---
title: Transformaciones de flujo de datos de ejemplo posibles con Preparación de datos de Azure Machine Learning | Microsoft Docs
description: En este documento se proporciona un conjunto de ejemplos de transformaciones de flujo de datos posibles con la preparación de datos de Azure Machine Learning
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: aa213a3b1a8949f0fca5e4bbb7ec5a6a775ae6ec
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/19/2018
---
# <a name="sample-of-custom-data-flow-transforms-python"></a>Ejemplo de transformaciones de flujo de datos personalizados (Python) 
El nombre de la transformación en el menú es **Transform Dataflow (Script)** (Transformar flujo de datos [script]). Antes de leer este apéndice, vea la [Introducción a la extensibilidad de Python](data-prep-python-extensibility-overview.md).

## <a name="transform-frame"></a>Transformación de marco
### <a name="create-a-new-column-dynamically"></a>Crear una columna de forma dinámica 
Crea una columna de forma dinámica (**city2**) y reconcilia distintas versiones de San Francisco a una de la columna City existente.
```python
    df.loc[(df['city'] == 'San Francisco') | (df['city'] == 'SF') | (df['city'] == 'S.F.') | (df['city'] == 'SAN FRANCISCO'), 'city2'] = 'San Francisco'
```

### <a name="add-new-aggregates"></a>Agregar nuevos agregados
Crea un nuevo marco con el primer y último agregados calculados para la columna Score. Estos se agrupan por la columna **risk_category**.
```python
    df = df.groupby(['risk_category'])['Score'].agg(['first','last'])
```
### <a name="winsorize-a-column"></a>Winsorizar una columna 
Reformula los datos para cumplir una fórmula para reducir los valores atípicos de una columna.
```python
    import scipy.stats as stats
    df['Last Order'] = stats.mstats.winsorize(df['Last Order'].values, limits=0.4)
```

## <a name="transform-data-flow"></a>Transformación del flujo de datos
### <a name="fill-down"></a>Rellenar hacia abajo 

"Rellenar hacia abajo" requiere dos transformaciones. Se asume que tenemos unos datos como los de la tabla siguiente:

|Estado         |City       |
|--------------|-----------|
|Washington    |Redmond    |
|              |Bellevue   |
|              |Issaquah   |
|              |Seattle    |
|California    |Los Ángeles|
|              |San Diego  |
|              |San José   |
|Texas         |Dallas     |
|              |San Antonio|
|              |Houston    |

1. Cree una transformación "Add Column (Script)" (Agregar columna [script]) con el código siguiente:
```python
    row['State'] if len(row['State']) > 0 else None
```

2. Cree una transformación "Transform Data Flow (Script)" (Transformar flujo de datos [script]) que contenga el siguiente código:
```python
    df = df.fillna( method='pad')
```

Los datos ahora tienen un aspecto similar a la tabla siguiente:

|Estado         |newState         |City       |
|--------------|--------------|-----------|
|Washington    |Washington    |Redmond    |
|              |Washington    |Bellevue   |
|              |Washington    |Issaquah   |
|              |Washington    |Seattle    |
|California    |California    |Los Ángeles|
|              |California    |San Diego  |
|              |California    |San José   |
|Texas         |Texas         |Dallas     |
|              |Texas         |San Antonio|
|              |Texas         |Houston    |


### <a name="min-max-normalization"></a>Normalización mín. máx.
```python
    df["NewCol"] = (df["Col1"]-df["Col1"].mean())/df["Col1"].std()
```