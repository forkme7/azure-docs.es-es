---
title: Creación de un área de trabajo en Azure Log Analytics | Microsoft Docs
description: Aprenda a crear un área de trabajo de Log Analytics para habilitar las soluciones de administración y la recopilación de datos en sus entornos tanto locales como en la nube.
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: magoedte
ms.openlocfilehash: e05d620a457edf427c946366a9caca471ac8531e
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2018
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Creación de un área de trabajo de Log Analytics en Azure Portal
En Azure Portal se puede configurar un área de trabajo de Log Analytics, que es un entorno de Log Analytics único con su propio repositorio de datos, sus propios orígenes de datos y sus propias soluciones.  Los pasos que se describen en este artículo son necesarios si se tiene intención de recopilar datos de los siguientes orígenes:

* Recursos de Azure de la suscripción
* Equipos locales supervisados por System Center Operations Manager
* Colecciones de dispositivos de System Center Configuration Manager 
* Datos de diagnóstico o registro de Azure Storage

Para otros orígenes, como las máquinas virtuales de Azure y los equipos Windows o Linux del entorno, consulte los temas siguientes:

*  [Recopilación de datos acerca de máquinas virtuales de Azure](log-analytics-quick-collect-azurevm.md) 
*  [Recopilación de datos de equipos Linux hospedados en el entorno](log-analytics-quick-collect-linux-computer.md)
*  [Recopilación de datos de equipos Windows hospedados en el entorno](log-analytics-quick-collect-windows-computer.md)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="log-in-to-azure-portal"></a>Iniciar sesión en Azure Portal
Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Crear un área de trabajo
1. En Azure Portal, haga clic en **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.<br><br> ![Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Haga clic en **Crear** y, a continuación, seleccione opciones para los elementos siguientes:

  * Proporcione un nombre para la nueva **Área de trabajo de OMS**, como *DefaultLAWorkspace*. 
  * Seleccione una **suscripción** a la que vincularlo en la lista desplegable si la opción predeterminada seleccionada no es adecuada.
  * Elija **Grupo de recursos** para usar un grupo de recursos existente o crear uno nuevo.  
  * Seleccione una **ubicación** disponible.  Para obtener más información, consulte en qué [regiones está disponible Log Analytics](https://azure.microsoft.com/regions/services/).
  * Si va a crear un área de trabajo en una nueva suscripción creada después del 2 de abril de 2018, esta utilizará automáticamente el plan de precios *Por GB* y la opción para seleccionar un plan de tarifas no estará disponible.  Si va a crear un área de trabajo para una suscripción existente creada antes del 2 de abril o para una suscripción asociada a una inscripción de EA existente, seleccione el plan de tarifa que prefiera.  Para obtener más información sobre planes concretos, consulte los [detalles de precios de Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-02.png)<br>  

3. Después de proporcionar la información necesaria en el panel **Área de trabajo de OMS**, haga clic en **Aceptar**.  

Mientras se comprueba la información y se crea el espacio de trabajo, puede realizar un seguimiento de su progreso en **Notificaciones** en el menú. 

## <a name="next-steps"></a>Pasos siguientes
Ahora que tiene un área de trabajo disponible, puede configurar la recopilación de datos de telemetría de supervisión, realizar búsquedas en el registro para analizar dichos datos y agregar una solución de administración que proporcione datos adicionales e información de los análisis. 

* Para habilitar la recopilación de datos de recursos de Azure con Azure Diagnostics o Azure Storage, consulte [Recopilación de registros y métricas de Azure para servicios de Log Analytics](log-analytics-azure-storage.md).  
* [Agregue System Center Operations Manager como origen de datos](log-analytics-om-agents.md) para recopilar datos de agentes que informan a su grupo de administración de Operations Manager y almacenarlos en el repositorio del área de trabajo de Log Analytics. 
* Conecte [Configuration Manager](log-analytics-sccm.md) para importar equipos que son miembros de colecciones en la jerarquía.  
* Revise las [soluciones de administración](/log-analytics-add-solutions.md) disponibles y cómo agregar o quitar una solución del área de trabajo.

