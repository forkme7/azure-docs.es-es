---
title: Elemento de interfaz de usuario SizeSelector de Azure | Microsoft Docs
description: Describe el elemento de la interfaz de usuario Microsoft.Compute.SizeSelector para Azure Portal.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: 3966de95233f32a09d4799630632c2bb6a490d78
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2018
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Elemento de interfaz de usuario Microsoft.Compute.SizeSelector
Control para seleccionar un tamaño para una o varias instancias de máquina virtual.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario
![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": []
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Comentarios
- `recommendedSizes` debe contener al menos un tamaño. El primer tamaño recomendado se utiliza como valor predeterminado.
- Si no hay un tamaño recomendado en la ubicación seleccionada, el tamaño se omite automáticamente. En su lugar, se utiliza el siguiente tamaño recomendado.
- Los tipos no especificados en `constraints.allowedSizes` se ocultan, mientras que los tipos no especificados en `constraints.excludedSizes` se muestran.
Tanto `constraints.allowedSizes` como `constraints.excludedSizes` son opcionales, pero no se pueden usar simultáneamente. Para determinar la lista de los tamaños disponibles, consulte la [lista de tamaños de máquina virtual disponibles para una suscripción](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).
- `osPlatform` debe especificarse y puede ser **Windows** o **Linux**. Se usa para determinar los costos de hardware de las máquinas virtuales.
- `imageReference` se omite para las imágenes propias, pero se proporciona para las imágenes de terceros. Se usa para determinar los costos de software de las máquinas virtuales.
- `count` se usa para establecer el multiplicador apropiado para el elemento. Admite un valor estático, como **2**, o un valor dinámico de otro elemento, como `[steps('step1').vmCount]`. El valor predeterminado es **1**.

## <a name="sample-output"></a>Salida de ejemplo
```json
"Standard_D1"
```

## <a name="next-steps"></a>Pasos siguientes
* Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).
* Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).
