---
title: "Publicación de un anuncio en un laboratorio de Azure DevTest Labs | Microsoft Docs"
description: Aprenda a agregar un anuncio a un laboratorio de Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 67a09946-4584-425e-a94c-abe57c9cbb82
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/29/2018
ms.author: v-craic
ms.openlocfilehash: 99b0938d5f4c8b022ead3473a0367de5d75cd6ff
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2018
---
# <a name="post-an-announcement-to-a-lab-in-azure-devtest-labs"></a>Publicación de un anuncio en un laboratorio de Azure DevTest Labs

Como administrador del laboratorio, puede publicar un anuncio personalizado en un laboratorio existente para notificar a los usuarios acerca de cambios recientes o adiciones en el laboratorio. Por ejemplo, puede que desee informar a los usuarios acerca de:

- Nuevos tamaños de máquinas virtuales que están disponibles
- Imágenes que están actualmente inutilizables
- Actualizaciones de directivas del laboratorio

Una vez publicado, el anuncio se muestra en la página de información general del laboratorio y el usuario puede seleccionarlo para obtener más detalles.

La característica de anuncios está pensada para usarla para notificaciones temporales.  Puede deshabilitar fácilmente un anuncio cuando este deja de ser necesario.

## <a name="steps-to-post-an-announcement-in-an-existing-lab"></a>Pasos para publicar un anuncio en un laboratorio existente

1. Inicie sesión en el [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Si es necesario, seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista. (Puede que su laboratorio ya aparezca en el panel, en **Todos los recursos**).
1. En la lista de laboratorios, seleccione aquel en el que desea publicar un anuncio.  
1. En el área **Introducción** del laboratorio, seleccione **Configuración y directivas**.  

    ![Botón Configuración y directivas](./media/devtest-lab-announcements/devtestlab-config-and-policies.png)

1. A la izquierda en **CONFIGURACIÓN**, seleccione **Lab announcement** (Anuncio del laboratorio).

    ![Botón de anuncio del laboratorio](./media/devtest-lab-announcements/devtestlab-announcements.png)

1. Para crear un mensaje para los usuarios de este laboratorio, establezca **Habilitado** en **Sí**.

1. Puede escribir una **fecha de expiración** para especificar una fecha y hora después de la cual ya no se mostrará el anuncio a los usuarios. Si no escribe una fecha de expiración, el anuncio permanece hasta que lo deshabilita.

   > [!NOTE]
   > Una vez que el anuncio expira, ya no se muestra a los usuarios pero sigue existiendo en el panel **Anuncio de laboratorio**. Puede modificarlo y volver a habilitarlo para que esté activo nuevamente.
   >
   >

1. Escriba un **título del anuncio** y el **texto del anuncio**.

   El título puede contener hasta 100 caracteres y se muestra al usuario en la página de información general del laboratorio. Si el usuario selecciona el título, se muestra el texto del anuncio.

   El texto del anuncio acepta marcado. A medida que escribe el texto del anuncio, puede ver el mensaje en el área de vista previa en la parte inferior de la pantalla.

    ![Pantalla de anuncio del laboratorio para crear el mensaje.](./media/devtest-lab-announcements/devtestlab-post-announcement.png)


1. Seleccione **Guardar** cuando el anuncio esté listo para publicarse.

Cuando ya no quiera mostrar este anuncio a los usuarios del laboratorio, vuelva a la página **Lab announcement** (Anuncio del laboratorio) y establezca **Habilitado** en **No**. Si especificó una fecha de expiración, el anuncio se deshabilita automáticamente en esa fecha y hora.

## <a name="steps-for-users-to-view-an-announcement"></a>Pasos para que los usuarios vean un anuncio

1. En [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), seleccione un laboratorio.

1. Si el laboratorio tiene un anuncio publicado en él, se muestra un aviso de información en la parte superior de la página de información general del laboratorio. Este aviso de información es el título del anuncio que se especificó cuando se creó este.

    ![Página de información general del anuncio del laboratorio](./media/devtest-lab-announcements/devtestlab-user-announcement.png)

1. El usuario puede seleccionar el mensaje para ver el anuncio completo.

    ![Más información sobre el anuncio del laboratorio](./media/devtest-lab-announcements/devtestlab-user-announcement-text.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Pasos siguientes
* Si cambia o establece una directiva de laboratorio, puede que quiera publicar un anuncio para informar a los usuarios. [Configuración de directivas y programaciones](devtest-lab-set-lab-policy.md) proporciona información acerca de cómo aplicar restricciones y convenciones en la suscripción mediante el uso de directivas personalizadas.
* Explore la [galería de plantillas de inicio rápido de Azure Resource Manager de DevTest Labs](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
