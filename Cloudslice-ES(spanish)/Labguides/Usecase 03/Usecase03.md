## Caso de uso 03: Crear un Fabric Data Agent mediante Mirrored Azure SQL Database en Microsoft Fabric

### Introducción

Las organizaciones modernas requieren sistemas inteligentes que puedan analizar rápidamente los datos operativos y proporcionar información útil sin necesidad de realizar movimientos complejos de datos. En este caso de uso, se utiliza Microsoft Fabric para reflejar datos de una Azure SQL Database en un entorno de Fabric y crear un Fabric Data Agent capaz de consultar y analizar los datos reflejados.

El proceso comienza con la creación de una Azure SQL Database que contiene datos empresariales de ejemplo. A continuación, esta base de datos se refleja en Microsoft Fabric mediante Azure SQL Mirroring, lo que permite un acceso casi en tiempo real a los datos operativos dentro del espacio de trabajo de Fabric. Una vez que la base de datos reflejada está disponible, se configura un Fabric Data Agent para conectarse al origen de datos y responder consultas en lenguaje natural.

Este enfoque permite a los usuarios interactuar con los datos empresariales mediante agentes inteligentes, lo que facilita obtener información sobre el rendimiento de los productos, la distribución de clientes y las tendencias de ventas sin necesidad de escribir consultas SQL complejas.

### Objetivo

El objetivo de este laboratorio es demostrar cómo crear y configurar un Fabric Data Agent capaz de analizar datos operativos reflejados desde una Azure SQL Database.

Al completar este laboratorio, aprenderá a:
- Crear una **Azure SQL Database** con datos de ejemplo.
- Crear un **espacio de trabajo de Microsoft Fabric** para hospedar
  recursos de datos y análisis.

- Reflejar una **Azure SQL Database en Microsoft Fabric** mediante Azure
  SQL Mirroring.

- Configurar un **Fabric Data Agent** y conectarlo a la base de datos
  reflejada.

- Consultar los datos mediante **prompts en lenguaje natural** para
  generar insights.

- Validar las respuestas del agente mediante preguntas analíticas de
  ejemplo.


## **Tarea 0: Sincronizar la hora del entorno host**

1. En la VM, vaya a la barra de búsqueda y selecciónela. Escriba **Settings** y, a continuación, seleccione **Settings** en **Best match**.

     ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image1.png)

1. En la ventana **Settings**, vaya a **Time & language** y selecciónelo.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image2.png)

1. En la página **Time & language**, vaya a **Date & time** y selecciónelo.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image3.png)

1. Desplácese hacia abajo hasta la sección **Additional settings** y, a continuación, seleccione el botón **Sync now**. El proceso de sincronización tardará entre 3 y 5 minutos.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image4.png)

1. Cierre la ventana **Settings**.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image5.png)


## Tarea 1: Crear una Azure SQL Database única

En esta tarea, creará una Azure SQL Database completamente configurada con datos de ejemplo. Implementará el esquema de ejemplo **AdventureWorksLT**, comprobará las tablas y preparará los detalles de conexión del servidor para reflejarlo posteriormente en Fabric.

1. Abra un explorador, vaya a +++https://portal.azure.com+++ e inicie sesión con la cuenta de Cloud Slice que se indica a continuación.

    |   |   |
    |---|---|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | TAP | +++@lab.CloudPortalCredential(User1).AccessToken+++ |

1. En la página principal de Azure Portal, seleccione el menú de Azure portal, representado por las tres líneas horizontales situadas en el lado izquierdo de la barra de comandos de Microsoft Azure. A continuación, seleccione **SQL database**.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image6.png)

1. Haga clic en **+ Create.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image7.png)

1. En la ventana **Create a storage account**, en la pestaña **Basics**, escriba los siguientes datos para crear una cuenta de almacenamiento y, a continuación, seleccione **Next: Networking**

    | Setting | Value  |
    |--------|----------------|
    | Subscription | @lab.CloudSubscription.Name |
    | Resource group | @lab.CloudResourceGroup(ResourceGroup1).Name |
    | Database name | +++sqldatabase@lab.LabInstance.Id+++ |
    | Server | Select **Create new** |
    | Server name | +++sqlserver@lab.LabInstance.Id+++ |
    | Location | @lab.CloudResourceGroup(ResourceGroup1).Location |
    | Authentication Method | **Use SQL Authentication** |
    | Server admin login | +++sqladmin+++ |
    | Password | +++password321!+++ |
    | Confirm password | +++password321!+++ |
    | Action | Click **OK** |

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image8.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image9.png)

1. En la sección **Compute + Storage**, seleccione **Configure database.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image10.png)

1. En **Service tier**, seleccione **Standard (Budget Friendly)** en la lista desplegable. En **DTU**, escriba **100** y, a continuación, seleccione **Apply**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image11.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image12.png)

1. En la pestaña **Networking**, seleccione **Public endpoint**, establezca **Allow Azure services and resources** en **Yes**, habilite **Add current client IP address** y, a continuación, seleccione **Next: Security**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image13.png)

1. En la página **Security**, después de revisar la configuración, seleccione **Next: Additional settings.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image14.png)

1. En la pestaña *Additional settings*, en *Use existing data*, seleccione **Sample**. Cuando se le solicite, elija **AdventureWorksLT**, seleccione **OK** y, a continuación, seleccione **Review + create** para continuar.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image15.png)

1. En la página **Review + create**, después de revisar la configuración, seleccione **Create**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image16.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image17.png)

1. En la ventana **Microsoft.SQLDatabase**, una vez completada la implementación, seleccione el botón **Go to resource**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image18.png)

1. En la página SQL database, seleccione **Query editor**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image19.png)

1. En **Query editor (preview)**, escriba **sqladmin** como inicio de sesión del servidor SQL y **password321!** como contraseña y, a continuación, seleccione **OK** para conectarse a la base de datos.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image20.png)

1. Asegúrese de que todas las tablas de ejemplo se hayan implementado correctamente.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image21.png)

1. Regrese a su **SQL Database**. Copie **Server name** (1) y **SQL Database name** (2), péguelos en un Bloc de notas y, a continuación, **guarde** el archivo para utilizar esta información en la siguiente tarea.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image22.png)

1. Seleccione **Home** para volver a la página principal.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image23.png)

1. Haga clic en **Resource groups**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image24.png)

1. Seleccione el grupo de recursos **ResourceGroup1**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image25.png)

1. Seleccione **SQL server.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image26.png)

1. Vaya a **Identity**, cambie el estado de **System assigned managed identity** a **On** y, a continuación, seleccione **Save** para aplicar el cambio.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image27.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image28.png)


## Tarea 2: Crear un espacio de trabajo de Fabric

En esta tarea, creará un espacio de trabajo de Fabric. El espacio de trabajo contiene todos los elementos necesarios para este tutorial de lakehouse, incluidos el lakehouse, los flujos de datos, las canalizaciones de Data Factory, los notebooks, los conjuntos de datos de Power BI y los informes.

1. Abra el navegador, vaya a la barra de direcciones y escriba o pegue la siguiente URL: +++https://app.fabric.microsoft.com/+++ presione **Enter** e inicie sesión con sus credenciales.

    |  |   |
    |---|----|
    |Username	|+++@lab.CloudPortalCredential(User1).Username+++|
    |TAP	|+++@lab.CloudPortalCredential(User1).AccessToken+++|

1. En la página principal de Fabric, seleccione el mosaico +**New workspace**.

     ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image29.png)

1. En el panel **Create a workspace**, que aparece en el lado derecho, escriba los siguientes datos y seleccione el botón **Apply**.

    | Property | Value |
    |---------|-------|
    | Name | +++FabricAgent-mirroringdatabase@lab.LabInstance.Id+++ |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | Small dataset storage format |
    | Template apps | Check **Develop template apps** |

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image30.png)

    **Nota:** Para encontrar el ID de la instancia del laboratorio, seleccione **Help** y copie el **Instance ID.**

     ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image31.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image32.png)

1. Espere a que finalice la implementación. Este proceso tarda entre 2 y 3 minutos.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image33.png)


## Tarea 3: Crear una solución para Mirror Data usando Azure SQL Mirroring

En esta tarea, conectará la Azure SQL Database a Microsoft Fabric mediante Azure SQL Mirroring. Seleccionará las tablas, creará la base de datos reflejada y comprobará que los datos se hayan sincronizado correctamente.

1. Cree un nuevo lakehouse seleccionando el botón **+New item** en la barra de navegación.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image34.png)

1. En el cuadro de búsqueda **Filter by keyword**, escriba +++Mirrored Azure SQL Database+++ y seleccione el elemento **Mirrored Azure SQL Database**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image35.png)

1. En la ventana **Choose a database** **connection to get started**, seleccione **Azure SQL database.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image36.png)

1. En la pestaña **Connection settings**, escriba los siguientes datos y seleccione el botón **Connect**.

    | Field | Value |
    |------|-------|
    | Server | SQL server URL saved in **Task 2 → Step 15** |
    | Database | +++sqldatabase@lab.LabInstance.Id+++ |
    | Username | +++sqladmin+++ |
    | Password | +++password321!+++ |

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image37.png)

1. En la ventana **Choose data**, seleccione **Select all** y, a continuación, seleccione el botón **Connect.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image38.png)

1. En la pestaña **Destination**, seleccione **Create mirrored database.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image39.png)

1. Seleccione **Refresh** para actualizar y ver los cambios más recientes.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image40.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image41.png)

1. En el menú de navegación de la izquierda, vaya a *+++FabricAgent-mirroringdatabase@lab.LabInstance.Id+++* y selecciónelo, como se muestra en la siguiente imagen.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image42.png)


## Tarea 4: Crear un Fabric Data Agent y conectar la base de datos reflejada

En esta tarea, creará un nuevo Fabric Data Agent y lo configurará para que utilice la Azure SQL Database reflejada como origen de datos. Este agente responderá a prompts en lenguaje natural utilizando los datos reflejados.

1. En la página principal de **Fabric**, seleccione **+New item**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image43.png)

1. En el cuadro de búsqueda **Filter by item type**, escriba +++data agent+++ y seleccione **Data agent.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image44.png)

1. Escriba +++FabricDataAgent @lab.LabInstance.Id+++ como nombre del **Data agent** y seleccione **Create.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image45.png)

1. Seleccione **Add data source** para configurar un nuevo origen de datos.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image46.png)

1. Seleccione el recurso **Mirrored database** correspondiente a este taller.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image47.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image48.png)


## Tarea 5: Probar el agente

Probará el Data agent formulándole preguntas analíticas, por ejemplo:
- *Which product categories generate the highest sales?*
- *List products with high list price but low sales volume.*
- *Which cities have the highest number of customers?*


Esto permite comprobar la capacidad del agente para comprender y responder a consultas de negocio.

1. Seleccione el esquema **SalesLT** para todas las tablas.

1. En el panel de consultas de su **Fabric Data Agent**, escriba la pregunta +++Which product categories generate the highest sales?+++ y seleccione el icono **Send** para ver la respuesta del agente.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image49.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image50.png)

1. Para probar el agente, ejecute la aplicación e introduzca las siguientes preguntas de ejemplo para comprobar las respuestas.

    ++++List products with high list price but low sales volume.+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image51.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image52.png)

    +++List the cities with the highest number of customers+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image53.png)

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image54.png)

1. Seleccione **Agent instructions** en el menú superior.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image55.png)

1. Seleccione **Publish** en el menú superior y, a continuación, seleccione **Publish**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image56.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image57.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image58.png)

1. Ahora, seleccione +++FabricAgent-mirroringdatabase@lab.LabInstance.Id+++ en el panel de navegación izquierdo.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image59.png)


## Tarea 6: Eliminar los recursos

1. Seleccione la opción de puntos suspensivos (...) situada debajo del nombre del espacio de trabajo y, a continuación, seleccione **Workspace settings**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image60.png)

1. Seleccione **General** y, a continuación, **Remove this workspace.**

     ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image61.png)

1. En la advertencia que aparece, seleccione **Delete**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image62.png)

1. Espere a recibir una notificación de que el **Workspace** se ha eliminado antes de continuar con el siguiente laboratorio.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image63.png)

1. Abra un navegador, vaya a +++https://portal.azure.com+++ e inicie sesión con la cuenta de cloud slice que se indica a continuación.

1. Para eliminar los recursos, escriba **Resource groups** en la barra de búsqueda del **Azure portal**, vaya a **Resource groups** en **Services** y selecciónelo.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image64.png)

1. En la página **Resource groups**, seleccione su grupo de recursos.

1. En la página principal del **Resource Group**, seleccione todos los recursos excepto **Fabric Capacity** y, a continuación, seleccione **Delete**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image65.png)

1. En el panel **Delete Resources** que aparece en el lado derecho, escriba **delete** en el campo **Enter "delete" to confirm deletion** y, a continuación, seleccione **Delete.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image66.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2003/media/image67.png)


### Resumen

En este laboratorio, creó correctamente una Azure SQL Database y reflejó sus datos en Microsoft Fabric mediante Azure SQL Mirroring. A continuación, configuró un Fabric Data Agent para conectarse a la base de datos reflejada y analizar los datos mediante consultas en lenguaje natural.

El agente pudo responder preguntas analíticas, como identificar las categorías de productos con mayores ventas, los productos con precios elevados pero bajo volumen de ventas y las ciudades con el mayor número de clientes. Esto demuestra cómo Microsoft Fabric puede integrar orígenes de datos operacionales con agentes inteligentes para simplificar la exploración de datos y obtener insights de negocio con mayor rapidez.

Este caso de uso pone de manifiesto el potencial de combinar Data Mirroring y AI-powered data agents para crear experiencias de datos interactivas e inteligentes dentro del ecosistema de Microsoft Fabric.
