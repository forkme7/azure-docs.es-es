---
title: Copia de datos con Azure Data Lake Store como origen o destino mediante Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar datos desde cualquier almacén de datos de origen compatible a Azure Data Lake Store o desde Data Lake Store a cualquier almacén de receptor compatible mediante Data Factory.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: f346accd318689bbaa10080111d07e3fedd84556
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2018
---
# <a name="copy-data-to-or-from-azure-data-lake-store-by-using-azure-data-factory"></a>Copia de datos con Azure Data Lake Store como origen o destino mediante Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versión 1: Disponibilidad general](v1/data-factory-azure-datalake-connector.md)
> * [Versión 2: versión preliminar](connector-azure-data-lake-store.md)

En este artículo se resume el uso de la actividad de copia de Azure Data Factory para copiar datos con Azure Data Lake Store como origen o destino. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

> [!NOTE]
> Este artículo se aplica a la versión 2 de Data Factory, que actualmente se encuentra en versión preliminar. Si usa la versión 1 del servicio Data Factory, que está disponible con carácter general, vea el artículo sobre [la versión 1 del conector de Azure Data Lake Store](v1/data-factory-azure-datalake-connector.md).

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos desde cualquier almacén de datos de origen compatible a Azure Data Lake Store o desde Azure Data Lake Store a cualquier almacén de datos del receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector de Azure Data Lake Store admite las siguientes funcionalidades:

- Copia de los archivos mediante la autenticación de **entidad de servicio** o **identidad de servicio administrada (MSI)**.
- Copia de archivos tal cual, o bien análisis o generación de archivos con los [códecs de compresión y los formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md).

> [!IMPORTANT]
> Si copia los datos con Integration Runtime autohospedado, configure el firewall corporativo para permitir el tráfico saliente a `<ADLS account name>.azuredatalakestore.net` y a `login.microsoftonline.com/<tenant>/oauth2/token` en el puerto 443. El segundo es Azure Security Token Service (STS) con el que IR necesita comunicarse para obtener el token de acceso.

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas de Azure Data Lake Store.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Azure Data Lake Store:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type debe establecerse en: **AzureDataLakeStore**. | Sí |
| dataLakeStoreUri | Información sobre la cuenta de Azure Data Lake Store. Esta información adopta uno de los siguientes formatos: `https://[accountname].azuredatalakestore.net/webhdfs/v1` o `adl://[accountname].azuredatalakestore.net/`. | Sí |
| subscriptionId | Id. de suscripción de Azure al que pertenece la cuenta de Data Lake Store. | Necesario para el receptor |
| resourceGroupName | Nombre del grupo de recursos de Azure al que pertenece la cuenta de Data Lake Store. | Necesario para el receptor |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Puede usar los entornos Integration Runtime (autohospedado) (si el almacén de datos se encuentra en una red privada) o Azure Integration Runtime. Si no se especifica, se usará Azure Integration Runtime. |Sin  |

En las secciones siguientes puede consultar más propiedades y ejemplos de JSON para los distintos tipos de autenticación, respectivamente:

- [Uso de la autenticación de entidad de servicio](#using-service-principal-authentication)
- [Uso de la autenticación de identidades de servicio administradas](#using-managed-service-identity-authentication)

### <a name="using-service-principal-authentication"></a>Uso de la autenticación de entidad de servicio

Para usar la autenticación de entidad de servicio, registre una entidad de aplicación en Azure Active Directory (AAD) y concédale acceso a Data Lake Store. Consulte [Autenticación entre servicios](../data-lake-store/data-lake-store-authenticate-using-active-directory.md) para ver los pasos detallados. Anote los siguientes valores; los usará para definir el servicio vinculado:

- Identificador de aplicación
- Clave de la aplicación
- Id. de inquilino

>[!IMPORTANT]
> Asegúrese de que concede el permiso adecuado principal del servicio en Azure Data Lake Store:
>- **Como origen**, en el Explorador de datos -> Acceso, conceda al menos permiso de **lectura y ejecución** para enumerar y copiar los archivos en la carpeta y las subcarpetas o permiso de **lectura** para copiar un único archivo y optar por agregar como **una entrada de permiso de acceso y una entrada de permiso predeterminado**. No hay ningún requisito en el control de acceso de nivel de cuenta (IAM).
>- **Como receptor**, en el Explorador de datos -> Acceso, conceda al menos permiso de **escritura y ejecución** para crear elementos secundarios en la carpeta y optar por agregar como **una entrada de permiso de acceso y una entrada de permiso predeterminado**. Si usa Azure IR para copiar (tanto el origen como el receptor están en la nube), en el control de acceso de cuenta (IAM), conceda al menos el rol de **lector** para que Data Factory pueda detectar la región de Data Lake Store. Si desea evitar este rol de IAM, [cree un Azure IR](create-azure-integration-runtime.md#create-azure-ir) explícitamente con la ubicación de Data Lake Storage y realice la asociación en el servicio de Data Lake Storage vinculado como en el siguiente ejemplo.

Se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| servicePrincipalId | Especifique el id. de cliente de la aplicación. | Sí |
| servicePrincipalKey | Especifique la clave de la aplicación. Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |
| tenant | Especifique la información del inquilino (nombre de dominio o identificador de inquilino) en el que reside la aplicación. Para recuperarlo, mantenga el puntero del mouse en la esquina superior derecha de Azure Portal. | Sí |

**Ejemplo:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-managed-service-identity-authentication"></a>Uso de la autenticación de identidades de servicio administradas

Una factoría de datos puede asociarse con una [identidad de servicio administrada](data-factory-service-identity.md), que representa esta factoría de datos específica. Puede usar directamente esta identidad de servicio para la autenticación en Data Lake Store, de manera similar a como usa su propia entidad de servicio. Permite que esta fábrica designada tenga acceso y copie los datos desde y hacia Data Lake Store.

Para usar la autenticación de identidades de servicio administradas (MSI):

1. [Recupere la identidad de servicio de Data Factory](data-factory-service-identity.md#retrieve-service-identity) copiando el valor del id. de la aplicación de identidad de servicio que se genera con la factoría.
2. Conceda a la identidad de servicio acceso a Data Lake Store de la misma manera que lo hace para la entidad de servicio, con las notas siguientes.

>[!IMPORTANT]
> Asegúrese de conceder el permiso adecuado a la identidad de servicio de la factoría de datos en Azure Data Lake Store:
>- **Como origen**, en el Explorador de datos -> Acceso, conceda al menos permiso de **lectura y ejecución** para enumerar y copiar los archivos en la carpeta y las subcarpetas o permiso de **lectura** para copiar un único archivo y optar por agregar como **una entrada de permiso de acceso y una entrada de permiso predeterminado**. No hay ningún requisito en el control de acceso de nivel de cuenta (IAM).
>- **Como receptor**, en el Explorador de datos -> Acceso, conceda al menos permiso de **escritura y ejecución** para crear elementos secundarios en la carpeta y optar por agregar como **una entrada de permiso de acceso y una entrada de permiso predeterminado**. Si usa Azure IR para copiar (tanto el origen como el receptor están en la nube), en el control de acceso de cuenta (IAM), conceda al menos el rol de **lector** para que Data Factory pueda detectar la región de Data Lake Store. Si desea evitar este rol de IAM, [cree un Azure IR](create-azure-integration-runtime.md#create-azure-ir) explícitamente con la ubicación de Data Lake Storage y realice la asociación en el servicio de Data Lake Storage vinculado como en el siguiente ejemplo.

En Azure Data Factory, no es necesario especificar ninguna propiedad además de la información general de Data Lake Store en el servicio vinculado.

**Ejemplo:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre conjuntos de datos. En esta sección se proporciona una lista de las propiedades que el conjunto de datos de Azure Data Lake Store admite.

Para copiar datos con Azure Data Lake Store como origen o destino, establezca la propiedad type del conjunto de datos en **AzureDataLakeStoreFile**. Se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del conjunto de datos debe establecerse en: **AzureDataLakeStoreFile**. |Sí |
| folderPath | Ruta de acceso para el contenedor y la carpeta en el almacenamiento de archivos. Ejemplo: rootfolder/subfolder/ |Sí |
| fileName | Especifique el nombre del archivo en **folderPath** si quiere que la copia se haga con un archivo concreto como origen o destino. Si no especifica ningún valor para esta propiedad, el conjunto de datos apunta a todos los archivos de la carpeta.<br/><br/>Cuando fileName no se especifica para un conjunto de datos de salida y **preserveHierarchy** no se determina en el receptor de la actividad, la actividad de copia generará automáticamente el nombre de archivo con el siguiente formato: `Data.[activity run id GUID].[GUID if FlattenHierarchy].[format if configured].[compression if configured]`. Por ejemplo: `Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz`. |Sin  |
| formato | Si desea **copiar los archivos tal cual** entre los almacenes basados en archivos (copia binaria), omita la sección de formato en las definiciones de los conjuntos de datos de entrada y salida.<br/><br/>Si desea analizar o generar archivos con un formato concreto, se admiten los siguientes tipos de formato: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** y **ParquetFormat**. Establezca la propiedad **type** de formato en uno de los siguientes valores. Para más información, consulte las secciones [Formato de texto](supported-file-formats-and-compression-codecs.md#text-format), [Formato Json](supported-file-formats-and-compression-codecs.md#json-format), [Formato Avro](supported-file-formats-and-compression-codecs.md#avro-format), [Formato Orc](supported-file-formats-and-compression-codecs.md#orc-format) y [Formato Parquet](supported-file-formats-and-compression-codecs.md#parquet-format). |No (solo para el escenario de copia binaria) |
| compresión | Especifique el tipo y el nivel de compresión de los datos. Para más información, consulte el artículo sobre [códecs de compresión y formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Los tipos admitidos son **GZip**, **Deflate**, **BZip2** y **ZipDeflate**.<br/>Los niveles admitidos son **Optimal** y **Fastest**. |Sin  |

**Ejemplo:**

```json
{
    "name": "ADLSDataset",
    "properties": {
        "type": "AzureDataLakeStoreFile",
        "linkedServiceName":{
            "referenceName": "<ADLS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "datalake/myfolder/",
            "fileName": "myfile.csv.gz",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que el receptor y el origen de Azure Data Lake admiten.

### <a name="azure-data-lake-store-as-source"></a>Azure Data Lake Store como origen

Para copiar datos desde Azure Data Lake Store, establezca el tipo de origen de la actividad de copia en **AzureDataLakeStoreSource**. Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del origen de la actividad de copia debe establecerse en: **AzureDataLakeStoreSource**. |Sí |
| recursive | Indica si los datos se leen de forma recursiva de las subcarpetas o solo de la carpeta especificada. Tenga en cuenta que cuando recursive se establezca en true y el receptor sea un almacén basado en archivos, la carpeta o subcarpeta vacías no se copiarán ni crearán en el receptor.<br/>Los valores permitidos son: **True** (valor predeterminado) y **False** | Sin  |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromADLS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-store-as-sink"></a>Azure Data Lake Store como receptor

Para copiar datos a Azure Data Lake Store, establezca el tipo de receptor de la actividad de copia en **AzureDataLakeStoreSink**. Se admiten las siguientes propiedades en la sección **sink**:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del receptor de la actividad de copia debe establecerse en: **AzureDataLakeStoreSink**. |Sí |
| copyBehavior | Define el comportamiento de copia cuando el origen son archivos del almacén de datos basados en archivos.<br/><br/>Los valores permitidos son:<br/><b>- PreserveHierarchy (valor predeterminado)</b>: conserva la jerarquía de archivos en la carpeta de destino. La ruta de acceso relativa del archivo de origen que apunta a la carpeta de origen es idéntica a la ruta de acceso relativa del archivo de destino que apunta a la carpeta de destino.<br/><b>- FlattenHierarchy:</b> todos los archivos de la carpeta de origen están en el primer nivel de la carpeta de destino. Los archivos de destino tienen un nombre generado automáticamente. <br/><b>- MergeFiles</b>: combina todos los archivos de la carpeta de origen en un archivo. Si se especifica el nombre de archivo/blob, el nombre de archivo combinado sería el nombre especificado; de lo contrario, sería el nombre de archivo generado automáticamente. | Sin  |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyToADLS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataLakeStoreSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="recursive-and-copybehavior-examples"></a>Ejemplos de recursive y copyBehavior

En esta sección se describe el comportamiento resultante de la operación de copia para diferentes combinaciones de valores recursive y copyBehavior.

| recursive | copyBehavior | Estructura de carpetas de origen | Destino resultante |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la misma estructura que el origen:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5. |
| true |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File5 |
| true |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;El contenido de File1 + File2 + File3 + File4 + File 5 se combina en un archivo con un nombre de archivo generado automáticamente |
| false |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>No se selecciona la subcarpeta Subfolder1, que contiene los archivos File3, File4 y File5. |
| false |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File2<br/><br/>No se selecciona la subcarpeta Subfolder1, que contiene los archivos File3, File4 y File5. |
| false |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;El contenido de File1 + File2 se combina en un archivo con un nombre de archivo generado automáticamente. Nombre de archivo generado automáticamente para File1<br/><br/>No se selecciona la subcarpeta Subfolder1, que contiene los archivos File3, File4 y File5. |

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md##supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
