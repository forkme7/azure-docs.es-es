---
title: Contenido del SDK de Windows Universal Apps
description: Obtenga información sobre el contenido del SDK de Windows Universal Apps para Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 8fa1b701-1c2b-4aec-940c-06c974ef5405
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 7b520adcc6af24f7b092580ea82a787a120668bf
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2018
---
# <a name="windows-universal-apps-sdk-content"></a>Contenido del SDK de Windows Universal Apps
> [!IMPORTANT]
> Azure Mobile Engagement se retira el 31 de marzo de 2018. Esta página se eliminará poco después.
> 

En este documento se enumera y describe el contenido implementado por el SDK de la aplicación.

## <a name="the-resources-folder"></a>La carpeta `/Resources`
En esta carpeta se incluyen todos los recursos que necesita Mobile Engagement. Además, puede personalizarlas para adaptarlas a su aplicación.

* `EngagementConfiguration.xml` : el archivo de configuración de Mobile Engagement, donde puede personalizar la configuración de Mobile Engagement (cadena de conexión de Mobile Engagement, bloqueo de informes, etc.).

### <a name="html-folder"></a>Carpeta /html
* `EngagementNotification.html` : El diseño html de vista web `Notification` para banners en aplicación.
* `EngagementAnnouncement.html` : El diseño html de vista web `Announcement` para vistas intersticiales en aplicación.

### <a name="images-folder"></a>Carpeta /images
* `EngagementIconNotification.png`: el icono de marca que se muestra a la izquierda de una notificación, sustituya este por el icono de la marca.
* `EngagementIconOk.png`: el icono `Ok` de las páginas de contenido de Cobertura para el botón de acción o validación.
* `EngagementIconNOK.png`: el icono `NOK` que se usa cuando se deshabilita el botón de validación de las páginas de contenido de Cobertura.
* `EngagementIconClose.png`: el icono `Close` de las notificaciones y el contenido de Cobertura para el botón Descartar.

### <a name="overlay-folder"></a>Carpeta /overlay
* `EngagementPageOverlay.cs` : La página de superposición responsable de agregar la IU en aplicación de Engagement Reach a su hijo.

