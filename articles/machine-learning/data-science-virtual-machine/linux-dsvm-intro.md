---
title: Aprovisionamiento de una instancia de Data Science Virtual Machine de Linux CentOS en Azure | Microsoft Docs
description: Configure y cree Linux Data Science Virtual Machine en Azure para realizar análisis y aprendizaje automático.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: 3bab0ab9-3ea5-41a6-a62a-8c44fdbae43b
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 1a201974749acbbb9607e42e67d1935f437f9ca1
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2018
---
# <a name="provision-a-linux-centos-data-science-virtual-machine-on-azure"></a>Aprovisionamiento de una instancia de Data Science Virtual Machine de Linux CentOS en Azure

Linux Data Science Virtual Machine es una máquina virtual de Azure basada en CentOS que incluye una colección de herramientas preinstaladas. que se utilizan normalmente para hacer análisis de datos y tareas de aprendizaje automático. Los principales componentes de software que se incluyen son:

* Sistema operativo: distribución CentOS de Linux.
* Microsoft R Server Developer Edition
* Distribución de Anaconda Python (versiones 2.7 y 3.5), incluidas las bibliotecas de análisis de datos más conocidas
* JuliaPro : distribución protegida del lenguaje Julia con bibliotecas populares científicas y de análisis de datos
* Instancia independiente de Spark y Hadoop de un solo nodo (HDFS, Yarn)
* JupyterHub: servidor de Jupyter Notebook multiusuario compatible con kernels de R, Python, PySpark y Julia
* Explorador de Azure Storage
* Interfaz de la línea de comandos de Azure (CLI) para administrar recursos de Azure
* PostgresSQL Database
* Herramientas de aprendizaje automático
  * [Cognitive Toolkit](https://github.com/Microsoft/CNTK): kit de herramientas de software de Deep Learning de Microsoft Research.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): sistema de aprendizaje automático rápido que admite varias técnicas, como el aprendizaje en línea, el uso de hash, la clase AllReduce, las reducciones, learning2search y los aprendizajes activo e interactivo
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): herramienta que proporciona una implementación de árbol ampliada, rápida y precisa
  * [Rattle](http://rattle.togaware.com/) (sigla del inglés “R Analytical Tool To Learn Easily”, la herramienta de análisis de R para aprender fácilmente): herramienta que simplifica la introducción al análisis de datos y al aprendizaje automático en R con una exploración de datos basada en GUI y un modelado con generación automática de códigos en R
* Azure SDK en Java, Python, node.js, Ruby, PHP
* Bibliotecas en R y Python para usarlas en Azure Machine Learning y en otros servicios de Azure
* Editores y herramientas de desarrollo (RStudio, PyCharm, IntelliJ, Emacs, gedit, vi)


La ciencia de datos implica la iteración de una secuencia de tareas:

1. Buscar, cargar y preprocesar datos
2. Compilar y probar modelos
3. Implementar los modelos para consumirse en aplicaciones inteligentes

Los científicos de datos usan varias herramientas para realizar estas tareas. Puede llevar bastante tiempo encontrar las versiones del software adecuadas y, después, descargarlas, compilarlas e instalarlas.

Linux Data Science Virtual Machine puede facilitar esta carga considerablemente. Úsela para comenzar de inmediato su proyecto de análisis. Le permite trabajar en tareas en varios lenguajes, incluidos R, Python, SQL, Java y C++. Eclipse proporciona un IDE para desarrollar y probar el código muy fácil de usar. El Azure SDK incluido en la máquina virtual permite compilar aplicaciones con varios servicios en Linux para la plataforma en la nube de Microsoft. Además, tiene acceso a otros lenguajes como Ruby, Perl, PHP y node.js, que también están instalados previamente.

No hay ningún cargo de software para esta imagen de VM de ciencia de datos. Solo se pagan las cuotas de uso del hardware de Azure, que se calculan en función del tamaño de la máquina virtual que aprovisiona con la imagen de la máquina virtual. Puede encontrar más información sobre cuotas de proceso en la [página de listas de VM en Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Otras versiones de Data Science Virtual Machine
También hay una imagen de [Ubuntu](dsvm-ubuntu-intro.md) disponible, con la mayoría de las mismas herramientas que la imagen de CentOS más marcos de trabajo de aprendizaje profundo. También hay una imagen de [Windows](provision-vm.md) disponible.

## <a name="prerequisites"></a>requisitos previos
Antes de poder crear una Linux Data Science Virtual Machine, debe tener lo siguiente:

* **Una suscripción a Azure**: para conseguir una, vea [Obtención de una evaluación gratuita de Azure](https://azure.microsoft.com/free/).
* **Una cuenta de Azure Storage**: consulte la sección sobre [creación de una cuenta de Azure Storage](../../storage/common/storage-create-storage-account.md#create-a-storage-account) para crear una. Como alternativa, la cuenta de almacenamiento puede crearse como parte del proceso de creación de la máquina virtual si no quiere usar una cuenta existente.

## <a name="create-your-linux-data-science-virtual-machine"></a>Creación de su Linux Data Science Virtual Machine
A continuación le indicamos los pasos para crear una instancia de Linux Data Science Virtual Machine:

1. Navegue al listado de máquinas virtuales en [Azure Portal](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm).
2. Haga clic en **Crear** (botón ubicado en la parte inferior) para acceder al asistente.![configure-data-science-vm](./media/linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3. En las secciones siguientes se proporcionan las entradas para cada uno de los pasos del asistente (enumerados a la derecha de la figura anterior) que se siguieron para crear la Microsoft Data Science Virtual Machine. Estas son las entradas necesarias para configurar cada uno de estos pasos:
   
   a. **Aspectos básicos**:
   
   * **Nombre**: nombre del servidor de ciencia de datos que está creando.
   * **Nombre de usuario**: identificador de acceso de la primera cuenta.
   * **Contraseña**: contraseña de la primera cuenta (puede usar una clave pública SSH en lugar de una contraseña).
   * **Suscripción**: si tiene más de una suscripción, seleccione aquella en la que se creará y facturará la máquina. Debe tener privilegios de creación de recursos en esta suscripción.
   * **Grupo de recursos**: puede crear uno nuevo o usar un grupo que ya exista.
   * **Ubicación**: seleccione el centro de datos más adecuado. Normalmente, es el centro de datos que tenga la mayoría de los datos o el que esté más cerca de su ubicación física para disfrutar de un acceso más rápido a la red.
   
   b. **Tamaño**:
   
   * Seleccione uno de los tipos de servidor que cumpla sus requisitos funcionales y las restricciones de costo. Seleccione **Ver todo** para consultar más opciones de tamaños de máquina virtual.
   
   c. **Configuración**:
   
   * **Tipo de disco**: seleccione **Premium** si prefiere una unidad de estado sólido (SSD). De lo contrario, elija **Estándar**.
   * **Cuenta de almacenamiento**: puede crear una nueva cuenta de Azure Storage en su suscripción o usar una existente en la misma ubicación que ha elegido en el paso **Aspectos básicos** del Asistente.
   * **Otros parámetros**: en la mayoría de los casos, usará simplemente los valores predeterminados. Si se plantea utilizar valores no predeterminados, mueva el puntero sobre el vínculo informativo para obtener ayuda sobre los campos específicos.
   
   d. **Resumen**:
   
   * Compruebe que toda la información que ha especificado es correcta.
   
   e. **Comprar**:
   
   * Haga clic en **Comprar**para iniciar el aprovisionamiento. Se proporciona un vínculo a los términos de la transacción. La máquina virtual no tiene ningún cargo adicional más allá del proceso para el tamaño del servidor que eligió en el paso **Tamaño** .

El aprovisionamiento tardará entre 10 y 20 minutos. El estado del aprovisionamiento se muestra en el Portal de Azure.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Acceso a Linux Data Science Virtual Machine
Después de crear la máquina virtual, puede iniciar sesión en ella mediante SSH. Utilice las credenciales de la cuenta que haya creado en la sección **Aspectos básicos** del paso 3 para la interfaz de shell de texto. En Windows, puede descargar una herramienta de cliente SSH como [Putty](http://www.putty.org). Si prefiere un escritorio gráfico (X Windows System), puede usar el reenvío de X11 en Putty o instalar el cliente X2Go.

> [!NOTE]
> El cliente X2Go ha funcionado significativamente mejor que el reenvío de X11 durante las pruebas. Por lo tanto, se recomienda usar el cliente X2Go para la interfaz gráfica de escritorio.
> 
> 

## <a name="installing-and-configuring-x2go-client"></a>Instalación y configuración del cliente X2Go
La VM de Linux ya está provista del servidor X2Go y está preparada para aceptar conexiones de cliente. Para conectarse al escritorio gráfico de la VM de Linux, haga lo siguiente en el cliente:

1. Descargue e instale el cliente X2Go para su plataforma cliente desde [aquí](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Ejecute el cliente X2Go y seleccione **Nueva sesión**. Se abrirá una ventana de configuración con varias pestañas. Escriba los siguientes parámetros de configuración:
   * **Pestaña Sesión**:
     * **Host**: nombre de host o dirección IP de la Linux Data Science Virtual Machine.
     * **Inicio de sesión**: nombre de usuario en la máquina virtual Linux.
     * **Puerto SSH**: Déjelo en 22, el valor predeterminado.
     * **Tipo de sesión**: Cambie el valor a XFCE. Actualmente, la máquina virtual Linux solo admite el escritorio XFCE.
   * **Pestaña Multimedia**: puede desactivar la compatibilidad de sonido y la impresión en el cliente si no necesita usarlas.
   * **Carpetas compartidas**: Si quiere que los directorios de las máquinas cliente se monten en la VM de Linux, agregue en esta pestaña los directorios de máquina cliente que quiere compartir con la VM.

Una vez que inicie sesión en la máquina virtual mediante el cliente SSH o el escritorio gráfico XFCE a través del cliente X2Go, ya podrá empezar a usar las herramientas que están instaladas y configuradas en la máquina virtual. En XFCE, puede ver accesos directos del menú de aplicaciones e iconos de escritorio para muchas de las herramientas.

## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Herramientas instaladas en Linux Data Science Virtual Machine
### <a name="microsoft-r-server"></a>Microsoft R Server
R es uno de los lenguajes más conocidos para el análisis de datos y el aprendizaje automático. Si quiere usar R para el análisis, la máquina virtual incluye Microsoft R Server (MRS) con Microsoft R Open (MRO) y la biblioteca Math Kernel Library (MKL). MKL optimiza las operaciones matemáticas comunes en algoritmos de análisis. MRO es totalmente compatible con CRAN-R; cualquiera de las bibliotecas de R publicadas en CRAN puede instalarse en MRO. MRS ofrece el escalado y la operacionalización de los modelos de R en servicios web. Puede editar los programas de R en uno de los editores predeterminados, como RStudio, vi, Emacs o gedit. Si está usando el editor Emacs, tenga en cuenta que el paquete ESS de Emacs (Emacs Speaks Statistics), que simplifica el trabajo con los archivos de R en el editor Emacs, se ha instalado previamente.

Para iniciar la consola de R, simplemente escriba **R** en el shell. Esto le dirigirá a un entorno interactivo. Para desarrollar el programa de R, se suele usar un editor como Emacs, vi o gedit, y se ejecutan los scripts en R. Con RStudio, tendrá un entorno de IDE gráfico completo para desarrollar su programa de R.

También hay un script de R para que pueda instalar los [20 paquetes principales de R](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) si lo desea. Este script se puede ejecutar una vez que esté en la interfaz interactiva de R, a la que se puede acceder (como se mencionó anteriormente) escribiendo **R** en el shell.  

### <a name="python"></a>Python
Para el desarrollo con Python, se ha instalado Anaconda Python Distribution 2.7 y 3.5. Esta distribución contiene Python base, junto con aproximadamente 300 de los paquetes de matemáticas, ingeniería y análisis de datos más populares. Puede usar los editores de texto predeterminados. Además, puede usar Spyder, un IDE de Python que integra las distribuciones de Anaconda Python. Spyder necesita un escritorio gráfico o el reenvío de X11. Se proporciona un acceso directo a Spyder en el escritorio gráfico.

Ya que tenemos tanto Python 2.7 como 3.5, necesita activar específicamente la versión de Python (entorno conda) con la que quiera trabajar en la sesión actual. El proceso de activación establece la variable PATH de acuerdo con la versión deseada de Python.

Para activar el entorno conda Python 2.7, ejecute el siguiente comando en el shell:

    source /anaconda/bin/activate root

Python 2.7 se instalará en */anaconda/bin*.

Para activar el entorno conda Python 3.5, ejecute lo siguiente en el shell:

    source /anaconda/bin/activate py35


Python 3.5 se instalará en */anaconda/envs/py35/bin*.

Después, para invocar la sesión interactiva de Python, escriba **python** en el shell. Si se encuentra en una interfaz gráfica o tiene la configuración de reenvío de X11, puede escribir **pycharm** para iniciar el IDE de PyCharm Python.

Para instalar bibliotecas adicionales de Python, debe ejecutar el comando ```conda``` o ````pip```` en sudo y proporcionar la ruta de acceso completa del administrador de paquetes de Python (conda o pip) para instalar en el entorno correcto de Python. Por ejemplo: 

    sudo /anaconda/bin/pip install <package> #pip for Python 2.7
    sudo /anaconda/envs/py35/bin/pip install <package> #pip for Python 3.5
    sudo /anaconda/bin/conda install [-n py27] <package> #conda for Python 2.7, default behavior
    sudo /anaconda/bin/conda install -n py35 <package> #conda for Python 3.5


### <a name="jupyter-notebook"></a>Jupyter Notebook
La distribución de Anaconda también incluye un cuaderno de Jupyter Notebook, un entorno para compartir código y análisis. Se accede a Jupyter Notebook mediante JupyterHub. Inicie sesión con su nombre de usuario y contraseña de Linux local.

Se ha configurado previamente el servidor Jupyter Notebook con kernels de Python 2, Python 3 y R. Hay un icono del escritorio llamado "Jupyter Notebook" para iniciar el explorador y acceder al servidor de Jupyter Notebook. Si se encuentra en la máquina virtual a través de SSH o del cliente X2Go, también puede visitar [https://localhost:8000/](https://localhost:8000/) para acceder al servidor de Jupyter Notebook.

> [!NOTE]
> Continúe aunque aparezca una advertencia de certificado.
> 
> 

Puede acceder al servidor de Jupyter Notebook desde cualquier host. Solo tiene que escribir *https://\<<Nombre DNS o dirección IP de la máquina virtual\>:8000/*

> [!NOTE]
> El puerto 8000 se abre en el firewall de forma predeterminada cuando se aprovisiona la VM.
> 
> 

Hemos empaquetado algunos cuadernos de ejemplo (uno en Python y otro en R). Puede ver el vínculo a los ejemplos en la página principal del cuaderno después de que se autentique en Jupyter Notebook con el nombre de usuario y la contraseña de Linux local. Puede crear un nuevo cuaderno seleccionando **Nuevo** y, después, el kernel de lenguaje apropiado. Si no ve el botón **Nuevo**, haga clic en el icono de **Jupyter** en la parte superior izquierda para ir a la página principal del servidor de Notebook.

### <a name="apache-spark-standalone"></a>Apache Spark Standalone 
Una instancia independiente de Apache Spark está preinstalada en la DSVM de Linux para ayudarlo a desarrollar aplicaciones de Spark localmente antes de probarlas e implementarlas en clústeres de gran tamaño. Puede ejecutar programas PySpark mediante el kernel de Jupyter. Al abrir Jupyter y hacer clic en el botón **Nuevo**, verá una lista de los kernels disponibles. "Spark - Python" es el kernel de PySpark que le permite compilar aplicaciones Spark mediante el lenguaje Python. También puede usar un IDE de Python, como PyCharm o Spyder, para compilar el programa Spark. Como se trata de una instancia independiente, la pila de Spark se ejecuta dentro del programa de cliente que ejecuta la llamada. Esto permite que la solución de problemas sea más rápida y simple en comparación con el desarrollo en un clúster de Spark. 

Se proporciona un bloc de notas de PySpark de ejemplo en Jupyter que puede encontrar en el directorio "SparkML" dentro del directorio particular de Jupyter ($HOME/notebooks/SparkML/pySpark). 

Si programa en R para Spark, puede usar Microsoft R Server, SparkR o sparklyr. 

Antes de ejecutar en el contexto de Spark en Microsoft R Server, debe llevar a cabo un paso de configuración por una sola vez a fin de habilitar un HDFS de Hadoop de un solo nodo y una instancia de Yarn. De manera predeterminada, los servicios de Hadoop están instalados pero deshabilitados en la DSVM. Para habilitarlos, debe ejecutar los comandos siguientes como raíz la primera vez:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

Puede detener los servicios relacionados de Hadoop cuando no los necesite; para hacerlo, ejecute ````systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```` En el directorio `/dsvm/samples/MRS` hay disponible un ejemplo que demuestra cómo desarrollar y probar MRS en un contexto de Spark remoto (que es la instancia independiente de Spark en la DSVM). 

### <a name="ides-and-editors"></a>IDE y editores
Puede elegir varios editores de código. Estos incluyen vi/VIM, Emacs, gEdit, PyCharm, RStudio, Eclipse e IntelliJ. gEdit, Eclipse, IntelliJ, RStudio y PyCharm son editores gráficos y necesitará iniciar sesión en un escritorio gráfico para poder usarlos. Puede iniciar estos editores desde los accesos directos del menú de aplicaciones y el escritorio.

**VIM** y **Emacs** son editores basados en texto. En Emacs, hemos instalado un paquete de complementos denominado Emacs Speaks Statistics (ESS) que facilita el trabajo con R en el editor de Emacs. Puede obtener más información en [ESS](http://ess.r-project.org/).

**Eclipse** es un IDE extensible de código abierto que admite varios lenguajes. La edición de desarrolladores de Java es la instancia instalada en la VM. Hay complementos disponibles para algunos de los lenguajes más conocidos que pueden instalarse para ampliar el entorno. También disponemos de un complemento instalado en Eclipse denominado **Kit de herramientas de Azure para Eclipse**. Permite crear, desarrollar, probar e implementar aplicaciones de Azure mediante el entorno de desarrollo Eclipse que admite lenguajes como Java. También hay **Azure SDK para Java** que permite el acceso a diferentes servicios de Azure desde el interior de un entorno de Java. Puede obtener más información acerca del kit de herramientas de Azure para Eclipse en [Kit de herramientas de Azure para Eclipse](../../azure-toolkit-for-eclipse.md).

**LaTex** se instala mediante el paquete de texlive junto con un paquete [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) del complemento de Emacs, lo que simplifica la creación de los documentos LaTex con Emacs.  

### <a name="databases"></a>Bases de datos
#### <a name="postgres"></a>Postgres
La base de datos de código abierto **Postgres** está disponible en la máquina virtual con los servicios que se ejecutan y con initdb ya completado. Necesitará crear bases de datos y usuarios. Para obtener más información, consulte la [documentación de Postgres](https://www.postgresql.org/docs/).  

#### <a name="graphical-sql-client"></a>Cliente SQL gráfico
**SQuirrel SQL**, un cliente SQL gráfico, se proporciona para conectarse a bases de datos diferentes (Microsoft SQL Server, Postgres y MySQL) y ejecutar consultas SQL. Puede ejecutarlo desde una sesión de escritorio gráfico (con el cliente X2Go, por ejemplo). Para invocar a SQuirrel SQL, puede iniciarlo desde el icono del escritorio o ejecutar el siguiente comando en el shell.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Antes de usarlo por primera vez, configure los controladores y los alias de las bases de datos. Los controladores JDBC se ubican en:

*/usr/share/java/jdbcdrivers*

Para obtener más información, consulte [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Herramientas de línea de comandos para tener acceso a Microsoft SQL Server
El paquete de controladores ODBC para SQL Server también incluye dos herramientas de línea de comandos:

**bcp**: la utilidad bcp copia datos de forma masiva entre una instancia de Microsoft SQL Server y un archivo de datos en un formato especificado por el usuario. Asimismo, puede usarse para importar grandes cantidades de filas nuevas en tablas de SQL Server o exportar datos de tablas en archivos de datos. Para importar datos en una tabla, debe usar un archivo de formato creado para esa tabla o entender la estructura de la tabla y los tipos de datos que son válidos para sus columnas.

Puede encontrar más información en [Conexión con bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**sqlcmd**: la utilidad sqlcmd le permite introducir instrucciones Transact-SQL, procedimientos del sistema y archivos de script en el símbolo del sistema. Esta utilidad usa ODBC para ejecutar lotes de Transact-SQL.

Puede encontrar más información en [Conexión con sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

> [!NOTE]
> Existen algunas diferencias en esta utilidad entre las plataformas de Windows y Linux. Consulte la documentación de para obtener más información.
> 
> 

#### <a name="database-access-libraries"></a>Bibliotecas de acceso a las bases de datos
Hay bibliotecas disponibles en Python y en R para acceder a bases de datos.

* En R, el paquete **RODBC** o el paquete **dplyr** le permiten consultar o ejecutar instrucciones SQL en el servidor de bases de datos.
* En Python, la biblioteca **pyodbc** proporciona acceso a la base de datos con ODBC como la capa subyacente.  

Para acceder a **Postgres**:

* Desde R: use el paquete **RPostgreSQL**.
* Desde Python: use la biblioteca **psycopg2** .

### <a name="azure-tools"></a>Herramientas de Azure
En la VM se instalan las siguientes herramientas de Azure:

* **Interfaz de la línea de comandos de Azure**: la CLI de Azure permite crear y administrar recursos de Azure mediante comandos de shell. Para invocar las herramientas de Azure, escriba **azure help**. Para obtener más información, consulte la [página de documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Explorador de Microsoft Azure Storage**: explorador de Microsoft Azure Storage es una herramienta gráfica que se usa para examinar los objetos que haya almacenado en la cuenta de Azure Storage y cargar o descargar datos con los blobs de Azure como origen y destino. Puede acceder al Explorador de Storage desde el icono de acceso directo del escritorio. Puede invocarlo desde el símbolo del sistema del shell escribiendo **StorageExplorer**. Tiene que haber iniciado sesión desde un cliente X2Go o tener la configuración de reenvío de X11.
* **Bibliotecas de Azure**: a continuación, figuran algunas de las bibliotecas preinstaladas.
  
  * **Python**: las bibliotecas relacionadas con Azure en Python que están instaladas son **azure**, **azureml**, **pydocumentdb** y **pyodbc**. Las tres primeras bibliotecas permiten acceder a los servicios de Azure Storage, a Azure Machine Learning y a Azure Cosmos DB (una base de datos NoSQL en Azure). La cuarta biblioteca, pyodbc (junto con el controlador ODBC de Microsoft para SQL Server), permite el acceso a SQL Server, Azure SQL Database y Azure SQL Data Warehouse desde Python mediante una interfaz ODBC. Escriba **pip list** para ver todas las bibliotecas enumeradas. Asegúrese de ejecutar este comando en los entornos de Python 2.7 y 3.5.
  * **R**: las bibliotecas relacionadas con Azure en R que están instaladas son **AzureML** y **RODBC**.
  * **Java**: la lista de bibliotecas de Java para Azure se puede encontrar en el directorio **/dsvm/sdk/AzureSDKJava** de la máquina virtual. Las bibliotecas principales son Azure Storage y las API de administración, Azure Cosmos DB y los controladores JDBC para SQL Server.  

Puede acceder a [Azure Portal](https://portal.azure.com) desde el explorador Firefox instalado previamente. En Azure Portal, puede crear, administrar y supervisar los recursos de Azure.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning es un servicio en la nube totalmente administrado que permite crear, implementar y compartir soluciones de análisis predictivo. Puede crear sus experimentos y modelos desde Azure Machine Learning Studio. Puede acceder a él desde un explorador web en la máquina virtual de ciencia de datos si visita la página de [Microsoft Azure Machine Learning](https://studio.azureml.net).

Una vez que inicie sesión en Azure Machine Learning Studio, tendrá acceso a un lienzo de experimentación donde puede crear un flujo lógico para los algoritmos de aprendizaje automático. También tiene acceso a Jupyter Notebook hospedado en Azure Machine Learning, que puede funcionar perfectamente con los experimentos de Azure Machine Learning Studio. Haga que los modelos de aprendizaje automático que ha creado estén operativos encapsulándolos en una interfaz de servicio web. De este modo, los clientes pueden escribir en cualquier lenguaje para invocar predicciones a partir de los modelos aprendizaje automático. Para obtener más información, consulte la [documentación sobre Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/).

También puede crear los modelos en R o en Python en la máquina virtual y, después, implementarlos en la producción en Azure Machine Learning. Hemos instalado bibliotecas en R (**AzureML**) y en Python (**azureml**) para habilitar esta funcionalidad.

Para obtener información sobre cómo implementar modelos en R y en Python en Azure Machine Learning, consulte la sección [Diez cosas que puede hacer en Data Science Virtual Machine](vm-do-ten-things.md) (en concreto, la sección Compilación de modelos mediante R y Python, y hacer que estén operativos mediante Azure Machine Learning).

> [!NOTE]
> Estas instrucciones se han escrito para la versión de Windows de la VM de ciencia de datos. Sin embargo, la información proporcionada sobre la implementación de modelos en Azure Machine Learning es aplicable a la máquina virtual Linux.
> 
> 

### <a name="machine-learning-tools"></a>Herramientas de aprendizaje automático
La máquina virtual incluye algunas herramientas o algoritmos de aprendizaje automático que se han precompilado y preinstalado de forma local. Entre ellas se incluyen las siguientes:

* **Microsoft Cognitive Toolkit**: un kit de herramientas de Deep Learning.
* **Vowpal Wabbit**: algoritmo de aprendizaje rápido en línea
* **xgboost**: herramienta que proporciona los algoritmos de árbol ampliados y optimizados
* **Python**: Anaconda Python integra algoritmos de aprendizaje automático con bibliotecas como Scikit-learn. Puede instalar otras bibliotecas con el comando `pip install` .
* **R**: hay disponible una completa biblioteca de funciones de aprendizaje automático para R. Algunas de las bibliotecas preinstaladas son lm, glm, randomForest y rpart. Puede instalar otras bibliotecas si ejecuta el comando:
  
        install.packages(<lib name>)

A continuación, proporcionamos información adicional sobre las tres primeras herramientas de aprendizaje automático de la lista.

#### <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit
Se trata de un kit de herramientas de aprendizaje profundo de código abierto. Es una herramienta de línea de comandos (cntk) y ya se encuentra en la ruta PATH.

Para ver un ejemplo básico, ejecute los comandos siguientes en el shell:

    cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
    cntk configFile=lr_bs.cntk makeMode=false command=Train

Para obtener más información, consulte la sección sobre CNTK de [GitHub](https://github.com/Microsoft/CNTK) y la [wiki de CNTK](https://github.com/Microsoft/CNTK/wiki).

#### <a name="vowpal-wabbit"></a>Vowpal Wabbit
Vowpal Wabbit es un sistema de aprendizaje automático rápido que emplea varias técnicas, como el aprendizaje en línea, el uso de hash, la clase AllReduce, las reducciones, learning2search y el aprendizaje activo e interactivo.

Para ejecutar la herramienta en un ejemplo básico, haga lo siguiente:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Puede encontrar otras demostraciones más grandes en ese directorio. Para obtener más información sobre VW, consulte [esta sección de GitHub](https://github.com/JohnLangford/vowpal_wabbit) y la [wiki de Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>XGBoost
Se trata de una biblioteca que está diseñada y optimizada para algoritmos incrementados (de árbol). El objetivo de esta biblioteca es llevar los límites de cálculo de las máquinas a los puntos de conexión necesarios para proporcionar una ampliación de árboles a gran escala que sea escalable, portátil y precisa.

Se proporciona como una línea de comandos, así como una biblioteca de R.

Para usar esta biblioteca en R, puede iniciar una sesión interactiva de R (escribiendo simplemente **R** en el shell) y cargando la biblioteca.

Este es un ejemplo sencillo que puede ejecutar en el símbolo del sistema de R:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Aquí se muestran los comandos que se deben ejecutar en el shell para ejecutar la línea de comandos xgboost:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


Se escribe un archivo .model en el directorio especificado. Puede encontrar información sobre este ejemplo de demostración [en GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Puede obtener más información sobre xgboost en la [página de documentación de xgboost](https://xgboost.readthedocs.org/en/latest/) y en su [repositorio de GitHub](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rattle
Rattle (siglas del inglés **R** **A**nalytical **T**ool **T**o **L**earn **E**asily, la herramienta de análisis de R para aprender fácilmente) utiliza el modelado y la exploración de datos basados en GUI. Presenta resúmenes visuales y estadísticos de datos, transforma datos que pueden modelarse fácilmente, crea modelos supervisados y no supervisados a partir de los datos, presenta el rendimiento de los modelos gráficamente y puntúa los conjuntos de datos nuevos. También genera un código R replicando las operaciones en la interfaz de usuario que pueden ejecutarse directamente en R o usarse como un punto de inicio para un análisis posterior.

Para ejecutar Rattle, debe haber iniciado sesión en un escritorio gráfico. En el terminal, escriba ```R``` para entrar en el entorno de R. En el símbolo del sistema de R, escriba los siguientes comandos:

    library(rattle)
    rattle()

Se abrirá una interfaz gráfica con un conjunto de pestañas. Siga estos pasos de inicio rápido en Rattle para usar un conjunto de datos de meteorología de ejemplo y crear un modelo. En algunos de los pasos siguientes, se le solicitará que instale y cargue automáticamente cualquier paquete de R necesario que todavía no esté en el sistema.

> [!NOTE]
> Si no tiene acceso para instalar el paquete en el directorio del sistema (valor predeterminado), puede ver un símbolo del sistema en la ventana de la consola de R para instalar paquetes en su biblioteca personal. Si ve estos avisos, responda *y* .
> 
> 

1. Haga clic en **Ejecutar**.
2. Aparecerá un cuadro de diálogo que le preguntará si quiere usar el conjunto de datos de meteorología de ejemplo. Haga clic en **Yes** (Sí) para cargar el ejemplo.
3. Haga clic en la pestaña **Model** (Modelo).
4. Haga clic en **Execute** (Ejecutar) para crear un árbol de decisión.
5. Haga clic en **Draw** (Dibujar) para mostrar el árbol de decisión.
6. Haga clic en el botón de opción **Forest** (Bosque) y haga clic en **Execute** (Ejecutar) para crear un bosque aleatorio.
7. Haga clic en la pestaña **Evaluate** (Evaluar).
8. Haga clic en el botón de opción **Risk** (Riesgo) y haga clic en **Execute** (Ejecutar) para mostrar dos trazados de rendimiento de riesgo (acumulativo).
9. Haga clic en la pestaña **Log** (Registro) para mostrar el código en R generado para las operaciones anteriores
   (debido a un error de la versión actual de Rattle, debe insertar un carácter *#* delante de *Export this log* [Exportar este registro] en el texto del registro).
10. Haga clic en el botón **Export** (Exportar) para guardar el script de R en el archivo *weather_script.R* de la carpeta principal.

Puede salir de Rattle y R. Ahora puede modificar el script de R generado o usarlo tal como está y ejecutarlo en cualquier momento para repetir todo lo que se realizó en la interfaz de usuario de Rattle. Esta es una forma sencilla (especialmente para los principiantes en R) de realizar el análisis y el aprendizaje automático rápidamente en una interfaz gráfica sencilla mientras se genera automáticamente un código en R para modificar o aprender.

## <a name="next-steps"></a>Pasos siguientes
A continuación, mostramos cómo puede continuar con las tareas de aprendizaje y exploración:

* El tutorial [Ciencias de los datos en Linux Data Science Virtual Machine](linux-dsvm-walkthrough.md) muestra cómo llevar a cabo varias tareas comunes de ciencia de datos con la máquina virtual de ciencia de datos Linux aprovisionada aquí. 
* Explore las diversas herramientas de ciencia de datos en la máquina virtual de ciencia de datos probando las herramientas descritas en este artículo. También puede ejecutar *dsvm-more-info* en el shell de la máquina virtual para obtener un introducción básica y referencias para consultar más información sobre las herramientas instaladas en la máquina virtual.  
* Aprenda a crear soluciones analíticas completas mediante el uso sistemático del [proceso de ciencia de datos en equipo](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).
* Visite la [Galería de Cortana Analytics](http://gallery.cortanaanalytics.com) para ver ejemplos de aprendizaje automático y de análisis de datos con Cortana Analytics Suite.

