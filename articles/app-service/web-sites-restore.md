---
title: Restaurar una aplicación en Azure
description: Obtenga información sobre cómo restaurar la aplicación desde una copia de seguridad.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 4444dbf7-363c-47e2-b24a-dbd45cb08491
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 5c37a693e1594f9afe40effba45d7b05a68fd49c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2018
---
# <a name="restore-an-app-in-azure"></a>Restaurar una aplicación en Azure
En este artículo se muestra cómo restaurar una aplicación de [Azure App Service](../app-service/app-service-web-overview.md) de la que ha realizado previamente una copia de seguridad (consulte [Realizar una copia de seguridad de la aplicación en Azure](web-sites-backup.md)). Puede restaurar la aplicación con sus bases de datos vinculadas a petición a un estado anterior o crear una nueva aplicación basada en una copia de seguridad de la aplicación original. Azure App Service admite las siguientes bases de datos para copia de seguridad y restauración:
- [SQL Database](https://azure.microsoft.com/services/sql-database/)
- [Azure Database for MySQL](https://azure.microsoft.com/services/mysql)
- [Azure Database para PostgreSQL](https://azure.microsoft.com/services/postgres)
- [MySQL en aplicación](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

La restauración de las copias de seguridad está disponible para las aplicaciones que se ejecutan en el nivel **Estándar** y **Premium**. Para obtener más información sobre la escalación de su aplicación, vea [Escalación de una aplicación web en el Servicio de aplicaciones de Azure](web-sites-scale.md). El nivel **premium** permite realizar un mayor número de copias de seguridad diarias que se van a realizar en el nivel **estándar**.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Restaurar una aplicación de una copia de seguridad existente
1. En la página **Configuración** de la aplicación en Azure Portal, haga clic en la opción **Copias de seguridad** para mostrar la página **Copias de seguridad**. Haga clic en **Restaurar**.
   
    ![Elegir Restaurar ahora][ChooseRestoreNow]
2. En la página **Restaurar**, seleccione primero el origen de la copia de seguridad.
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    La opción **Copia de seguridad de aplicación** le muestra todas las copias de seguridad existentes de la aplicación actual y puede seleccionar una fácilmente.
    La opción **Almacenamiento** le permite seleccionar cualquier archivo ZIP de copia de seguridad de cualquier cuenta de Azure Storage y contenedor existente de su suscripción.
    Si está intentando restaurar una copia de seguridad de otra aplicación, use la opción **Almacenamiento** .
3. A continuación, especifique el destino de la restauración de la aplicación en **Destino de restauración**.
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > Si elige **Sobrescribir**, se borrarán y sobrescribirán todos los datos existentes de la aplicación actual. Antes de hacer clic en **Aceptar**, asegúrese de que es exactamente lo que desea.
   > 
   > 
   
    Puede seleccionar **Aplicación existente** para restaurar la copia de seguridad de la aplicación a otra aplicación en el mismo grupo de recursos. Antes de utilizar esta opción, debe haber creado primero otra aplicación en el grupo de recursos con una configuración de base de datos reflejada en la definida en la copia de seguridad de la aplicación. También puede crear una aplicación **Nueva** en la cual restaurar el contenido.

4. Haga clic en **OK**.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Descarga o eliminación de una copia de seguridad de una cuenta de almacenamiento
1. En la página principal **Examinar** de Azure Portal, seleccione **Cuentas de almacenamiento**. Se mostrará una lista de las cuentas de almacenamiento existentes.
2. Seleccione la cuenta de almacenamiento que contiene la copia de seguridad que desea descargar o eliminar. Se muestra la página de la cuenta de almacenamiento.
3. En la página de la cuenta de almacenamiento, seleccione el contenedor que quiere.
   
    ![Ver contenedores][ViewContainers]
4. Seleccione el archivo de copia de seguridad que quiere descargar o eliminar.
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. Haga clic en **Descargar** o **Eliminar**, dependiendo de lo que quiera hacer.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>Supervisar una operación de restauración
Para obtener detalles sobre si se ha realizado correctamente o no la operación de restauración de la aplicación, vaya a la página **Registro de actividades** de Azure Portal.  
 

Desplácese hacia abajo para buscar la operación de restauración deseada y haga clic para seleccionarla.

La página de detalles muestra la información disponible relacionada con la operación de restauración.

## <a name="automate-with-scripts"></a>Automatizar con scripts

Puede automatizar la administración de copias de seguridad con scripts, mediante la [CLI de Azure](/cli/azure/install-azure-cli) o [Azure PowerShell](/powershell/azure/overview).

Para obtener ejemplos, vea:

- [Ejemplos de la CLI de Azure](app-service-cli-samples.md)
- [Ejemplos de Azure PowerShell](app-service-powershell-samples.md)

<!-- ## Next Steps
You can backup and restore App Service apps using REST API. -->


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
