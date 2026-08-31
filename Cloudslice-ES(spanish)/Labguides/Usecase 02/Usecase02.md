## Caso de uso 02: Crear análisis de ventas con el conjunto de datos AdventureWorks mediante Fabric Data Agent

### Introducción

Contoso Analytics, un equipo de análisis para el sector minorista, está migrando sus flujos de trabajo de generación de informes a **Microsoft Fabric** para mejorar la accesibilidad a los datos para analistas y responsables de negocio. El equipo desea habilitar la exploración de datos mediante lenguaje natural para que los usuarios no técnicos puedan obtener insights sin necesidad de escribir consultas SQL ni navegar por paneles.

Para lograrlo, el equipo decide crear un **asistente inteligente de análisis** basado en un **Fabric Data Agent**. El primer paso de este proceso consiste en preparar los datos subyacentes en un **Fabric Lakehouse**. Como se describe en el tutorial de **Fabric Data Agent**, comienzan **creando y rellenando un Lakehouse**, que almacenará conjuntos de datos seleccionados del sector minorista, como transacciones de ventas, inventario de productos y perfiles de tiendas. Este Lakehouse sirve como la fuente de datos centralizada y gobernada para las tareas posteriores.

Una vez configurado el Lakehouse, el siguiente paso es hacerlo accesible para los sistemas conversacionales y las herramientas de automatización. El equipo lo consigue **creando un Fabric Data Agent** y agregando el Lakehouse como origen de datos conectado, lo que habilita un acceso seguro y gobernado a los datos. Esta configuración permite que el **Fabric Data Agent** comprenda y **consulte el contenido del Lakehouse**, estableciendo la base para crear experiencias de lenguaje natural en toda la organización.

Con el Lakehouse conectado mediante el Fabric Data Agent, Contoso ahora puede integrar el agente en aplicaciones de análisis, experiencias de Copilot y herramientas internas, lo que permite a los usuarios empresariales realizar consultas como *"Show me today’s sales for the south region"* o *"Identify the lowest stock products across all stores" y recibir al instante respuestas basadas en datos.*

### Objetivos

- Crear un espacio de trabajo de Microsoft Fabric y configurar el
  almacenamiento y los permisos.

- Crear un Fabric Lakehouse y cargar mediante programación los conjuntos
  de datos de AdventureWorks utilizando notebooks.

- Crear y configurar un Fabric Data Agent conectado a las tablas del
  Lakehouse.

- Mejorar las respuestas del agente mediante instrucciones y consultas
  de ejemplo.

- Publicar el agente y probarlo mediante programación a través de
  llamadas a API dentro de un notebook de Microsoft Fabric.

- Limpiar y eliminar el espacio de trabajo después de completar el
  laboratorio.


## **Tarea 0: Sincronizar la hora del entorno host**

1. En la máquina virtual (VM), escriba **Settings** en la barra de búsqueda. Luego, haga clic en **Settings**, en la sección **Best match.**

     ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image1.png)

1. En la ventana **Settings**, vaya a **Time & language** y haga clic en esta opción.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image2.png)

1. En la página **Time & language**, vaya a **Date & time** y haga clic en esta opción.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image3.png)

1. Desplácese hacia abajo hasta la sección **Additional settings** y, a continuación, haga clic en el botón **Sync now**. La sincronización puede tardar entre 3 y 5 minutos.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image4.png)

1. Cierre la ventana **Settings**.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image5.png)


## Tarea 1: Crear un espacio de trabajo de Microsoft Fabric

En esta tarea, configurará el entorno base mediante la creación de un espacio de trabajo de Microsoft Fabric que alojará el Lakehouse, los notebooks y el Fabric Data Agent. Este espacio de trabajo actúa como el contenedor central para todos los recursos utilizados a lo largo de este caso de uso.

1. Abra el navegador, escriba o pegue la siguiente URL +++https://app.fabric.microsoft.com/+++ en la barra de direcciones y, a continuación, presione **Enter**.![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image6.png)

1. En la ventana de **Microsoft Fabric**, escriba sus credenciales y haga clic en **Submit**.

    |  |   |
    |---|----|
    |Username	|+++@lab.CloudPortalCredential(User1).Username+++|
    |TAP	|+++@lab.CloudPortalCredential(User1).AccessToken+++|

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image7.png)

1. A continuación, en la ventana de **Microsoft**, escriba la contraseña y haga clic en **Sign in**.

     ![A login screen with a red box and blue text AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image8.png)

1. En la ventana **Stay signed in?**, haga clic en **Yes**.

1. Se le redirigirá a la página principal de **Power BI**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image9.png)

1. En la página principal de **Microsoft Fabric**, seleccione el mosaico **+New workspace**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image10.png)

1. En el panel **Create a workspace**, que aparece en el lado derecho, escriba la siguiente información y haga clic en **Apply**.

    | Property | Value |
    |---------|-------|
    | Name | +++Fabric Data agent-@lab.LabInstance.Id+++ |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | Small dataset storage format |
    | Template apps | Check **Develop template apps** |

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image11.png)

    **Nota:** Para encontrar el identificador de la instancia del laboratorio, seleccione **Help** y copie el **Instance ID**.

      ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image12.png)
      
      ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image13.png)

1. Espere a que finalice la implementación. Este proceso tarda entre 1 y 2 minutos en completarse.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image14.png)


## Tarea 2: Crear un Lakehouse con AdventureWorksLH

En esta tarea, creará un nuevo Lakehouse y lo poblará con las tablas de AdventureWorks mediante un notebook de Microsoft Fabric. El Lakehouse se convertirá en la base de datos estructurada que consultará el Fabric Data Agent.

1. Cree un nuevo Lakehouse haciendo clic en el botón **+New item** de la barra de navegación.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image15.png)

1. Haga clic en el mosaico **Lakehouse**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image16.png)

1. En el cuadro de diálogo **New lakehouse**, escriba +++AdventureWorksLH+++ en el campo **Name**. Luego, haga clic en **Create** y abra el nuevo **Lakehouse**.

    **Nota:** Asegúrese de eliminar el espacio antes de **AdventureWorksLH.**


      ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image17.png)
      
      ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image18.png)

1. Verá una notificación que indica que el **SQL endpoint se creó correctamente**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image19.png)

1. Cree un nuevo notebook en el espacio de trabajo donde desea crear el **Fabric Data Agent**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image20.png)

1. Reemplace el contenido de la celda con el siguiente código y, a continuación, haga clic en **▷ Run cell**, ubicado a la izquierda de la celda.

    ```
    import pandas as pd
    from tqdm.auto import tqdm
    base = "https://synapseaisolutionsa.z13.web.core.windows.net/data/AdventureWorks"
    
    # load list of tables
    df_tables = pd.read_csv(f"{base}/adventureworks.csv", names=["table"])
    
    for table in (pbar := tqdm(df_tables['table'].values)):
        pbar.set_description(f"Uploading {table} to lakehouse")
    
        # download
        df = pd.read_parquet(f"{base}/{table}.parquet")
    
        # save as lakehouse table
        spark.createDataFrame(df).write.mode('overwrite').saveAsTable(table)
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image21.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image22.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image23.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image24.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image25.png)


## Tarea 3: Crear un Fabric Data Agent

En esta tarea, creará un **Fabric Data Agent** y lo conectará al **Lakehouse**. Seleccionará las tablas de dimensiones y hechos necesarias para permitir que el agente responda una amplia variedad de consultas analíticas relacionadas con las ventas.

1. En el panel de navegación izquierdo, haga clic en **Fabric Data +++agent-@lab.LabInstance.Id+++**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image26.png)

1. En la página principal de **Microsoft Fabric**, seleccione **+New item.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image27.png)

1. En el cuadro de búsqueda **Filter by item type**, escriba +++data agent+++ y, a continuación, seleccione **Data agent**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image28.png)

1. En el campo **Data agent name**, escriba +++AI-agent+++ y haga clic en **Create**.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image29.png)

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image30.png)

1. En la página **AI-agent**, seleccione **Add a data source**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image31.png)

1. En la pestaña **OneLake catalog**, seleccione el Lakehouse **AI-Fabric_lakehouse** y, a continuación, haga clic en **Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image32.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image33.png)

1. A continuación, seleccione las tablas a las que desea que el AI skill tenga acceso.

    En este laboratorio, se utilizan las siguientes tablas:
    - DimCustomer
    - DimDate
    - DimGeography
    - DimProduct
    - DimProductCategory
    - DimPromotion
    - DimReseller
    - DimSalesTerritory
    - FactInternetSales
    - FactResellerSales

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image34.png)


## Tarea 4: Proporcionar instrucciones

En esta tarea, enriquecerá el Fabric Data Agent agregando preguntas en lenguaje natural y sus correspondientes consultas SQL. Estos ejemplos ayudan al agente a comprender el contexto específico del dominio y a generar consultas SQL más precisas para responder preguntas del mundo real.

1. Cuando formule las primeras preguntas utilizando las tablas seleccionadas, incluido **factinternetsales**, el Fabric Data Agent responderá las consultas de forma razonablemente precisa.

1. Por ejemplo, formule la siguiente pregunta +++What is the most sold product?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image35.png)

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image36.png)

1. Copie la pregunta y la consulta SQL, péguelas en un archivo de Notepad y, a continuación, guarde el archivo, ya que lo utilizará en las siguientes tareas.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image37.png)

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image38.png)

1. Seleccione **FactResellerSales**, escriba el siguiente texto y haga clic en el **ícono** **Submit**, como se muestra en la siguiente imagen.


    +++What is our most sold product?+++
    
    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image39.png)
    
    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image40.png)
    
    A medida que continúe probando consultas, agregue más instrucciones.

1. Seleccione **dimcustomer**, escriba el siguiente texto y haga clic en el **ícono Submit**.


    +++how many active customers did we have June 1st, 2013?+++
    
    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image41.png)
    
    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image42.png)

1. Copie todas las preguntas y consultas SQL, péguelas en un archivo de Notepad y, a continuación, guarde el archivo para utilizarlo en las siguientes tareas.

     ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image43.png)

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image44.png)

1. Seleccione **dimdate** y **FactInternetSales**, escriba el siguiente texto y haga clic en el **ícono Submit:**


    +++what are the monthly sales trends for the last year?+++
    
    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image45.png)
    
     ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image46.png)

1. Seleccione **dimproduct** y **FactInternetSales**, escriba el siguiente texto y haga clic en el **ícono Submit:**


    +++which product category had the highest average sales price?+++
    
     ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image47.png)
    
    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image48.png)
    
    Parte del problema es que "**active customer**" no tiene una definición formal. Agregar más instrucciones en el cuadro de texto Notes to the model podría ayudar, pero es posible que los usuarios formulen esta pregunta con frecuencia. Debe asegurarse de que la AI responda correctamente a esta pregunta..

1. La consulta correspondiente es moderadamente compleja, por lo que debe proporcionar un ejemplo. Para ello, seleccione el botón **Example queries** en el panel **Setup.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image49.png)

1. En la pestaña **Example queries**, seleccione **Add example**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image50.png)

1. En este paso, agregará consultas de ejemplo para el origen de **datos del Lakehouse** que creó anteriormente. Escriba la siguiente pregunta en el campo **Question:**


    +++What is the most sold product?+++
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image51.png)

1. Agregue la consulta 1 que guardó en el archivo de **Notepad:**

    ```
    SELECT TOP 1 ProductKey, SUM(OrderQuantity) AS TotalQuantitySold
    FROM [dbo].[factinternetsales]
    GROUP BY ProductKey
    ORDER BY TotalQuantitySold DESC
    ```

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image52.png)

1. Para agregar un nuevo campo de consulta, haga clic en **+Add.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image53.png)

1. Para agregar una segunda pregunta, escríbala en el campo **Question:**


    +++What are the monthly sales trends for the last year?+++
    
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image54.png)

1. Agregue la **consulta 3** que guardó en el archivo de **Notepad**:

    ```
    SELECT
        d.CalendarYear,
        d.MonthNumberOfYear,
        d.EnglishMonthName,
        SUM(f.SalesAmount) AS TotalSales
    FROM
        dbo.factinternetsales f
        INNER JOIN dbo.dimdate d ON f.OrderDateKey = d.DateKey
    WHERE
        d.CalendarYear = (
            SELECT MAX(CalendarYear)
            FROM dbo.dimdate
            WHERE DateKey IN (SELECT DISTINCT OrderDateKey FROM dbo.factinternetsales)
        )
    GROUP BY
        d.CalendarYear,
        d.MonthNumberOfYear,
        d.EnglishMonthName
    ORDER BY
        d.MonthNumberOfYear
    ```

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image55.png)

1. Para agregar un nuevo campo de consulta, haga clic en **+Add.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image56.png)

1. Para agregar una tercera pregunta, escríbala en el campo **Question**:

    +++Which product category has the highest average sales price?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image57.png)

1. Agregue la **consulta 4** que guardó en el archivo de **Notepad**:

    ```
    SELECT TOP 1
        dp.ProductSubcategoryKey AS ProductCategory,
        AVG(fis.UnitPrice) AS AverageSalesPrice
    FROM
        dbo.factinternetsales fis
    INNER JOIN
        dbo.dimproduct dp ON fis.ProductKey = dp.ProductKey
    GROUP BY
        dp.ProductSubcategoryKey
    ORDER BY
        AverageSalesPrice DESC
    ```

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image58.png)

1. Agregue todas las preguntas y consultas SQL que guardó en el Notepad y, a continuación, haga clic en **Export all**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image59.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image60.png)


## Tarea 5: Usar el Fabric Data Agent mediante programación

Ya se agregaron instrucciones y ejemplos al Data agent. A medida que continúe realizando pruebas, podrá agregar más ejemplos e instrucciones para mejorar aún más el AI skill. Trabaje con sus compañeros para comprobar si los ejemplos y las instrucciones proporcionados cubren los tipos de preguntas que desean realizar.

Puede utilizar el AI skill mediante programación desde un notebook de Microsoft Fabric. Para comprobar si el AI skill tiene una Published URL, siga estos pasos.

1. En la página del **Fabric Data Agent**, en la cinta **Home**, seleccione **Settings**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image61.png)

1. Antes de publicar el AI skill, este no dispone de una Published URL, como se muestra en la siguiente captura de pantalla.

1. Cierre la ventana AI Skill settings.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image62.png)

1. En la cinta **Home**, seleccione **Publish**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image63.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image64.png)

1. Haga clic en **View publishing details.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image65.png)

1. La Published URL del AI agent aparecerá, como se muestra en la siguiente captura de pantalla.

1. Copie la Published URL, péguela en un archivo de Notepad y, a continuación, guarde el archivo para utilizar esta información en los siguientes pasos.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image66.png)

1. En el panel de navegación izquierdo, seleccione **Notebook1**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image67.png)

1. Debajo del resultado de la celda, seleccione el ícono **+ Code** para agregar una nueva celda de código al notebook. Escriba el siguiente código en la nueva celda, reemplace la **URL** y, a continuación, haga clic en **▷ Run**. Revise la respuesta obtenida.

    +++%pip install "openai==1.70.0"+++

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image68.png)

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image69.png)

1. Debajo del resultado de la celda, seleccione el ícono **+ Code** para agregar una nueva celda de código al notebook. Escriba el siguiente código en la nueva celda, reemplace la **URL** y, a continuación, haga clic en **▷ Run**. Revise el resultado.

    +++%pip install httpx==0.27.2+++

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image70.png)

1. Debajo del resultado de la celda, seleccione el ícono **+ Code** para agregar una nueva celda de código al notebook. Escriba el siguiente código en la nueva celda, reemplace la **URL** y, a continuación, haga clic en **▷ Run**. Revise el resultado.

    ```
    import requests
    import json
    import pprint
    import typing as t
    import time
    import uuid
    
    from openai import OpenAI
    from openai._exceptions import APIStatusError
    from openai._models import FinalRequestOptions
    from openai._types import Omit
    from openai._utils import is_given
    from synapse.ml.mlflow import get_mlflow_env_config
    from sempy.fabric._token_provider import SynapseTokenProvider
     
    base_url = "https://<generic published base URL value"
    question = "What datasources do you have access to?"
    
    configs = get_mlflow_env_config()
    
    # Create OpenAI Client
    class FabricOpenAI(OpenAI):
        def __init__(
            self,
            api_version: str ="2024-05-01-preview",
            **kwargs: t.Any,
        ) - None:
            self.api_version = api_version
            default_query = kwargs.pop("default_query", {})
            default_query["api-version"] = self.api_version
            super().__init__(
                api_key="",
                base_url=base_url,
                default_query=default_query,
                **kwargs,
            )
        
        def _prepare_options(self, options: FinalRequestOptions) - None:
            headers: dict[str, str | Omit] = (
                {**options.headers} if is_given(options.headers) else {}
            )
            options.headers = headers
            headers["Authorization"] = f"Bearer {configs.driver_aad_token}"
            if "Accept" not in headers:
                headers["Accept"] = "application/json"
            if "ActivityId" not in headers:
                correlation_id = str(uuid.uuid4())
                headers["ActivityId"] = correlation_id
    
            return super()._prepare_options(options)
    
    # Pretty printing helper
    def pretty_print(messages):
        print("---Conversation---")
        for m in messages:
            print(f"{m.role}: {m.content[0].text.value}")
        print()
    
    fabric_client = FabricOpenAI()
    # Create assistant
    assistant = fabric_client.beta.assistants.create(model="not used")
    # Create thread
    thread = fabric_client.beta.threads.create()
    # Create message on thread
    message = fabric_client.beta.threads.messages.create(thread_id=thread.id, role="user", content=question)
    # Create run
    run = fabric_client.beta.threads.runs.create(thread_id=thread.id, assistant_id=assistant.id)
    
    # Wait for run to complete
    while run.status == "queued" or run.status == "in_progress":
        run = fabric_client.beta.threads.runs.retrieve(
            thread_id=thread.id,
            run_id=run.id,
        )
        print(run.status)
        time.sleep(2)
    
    # Print messages
    response = fabric_client.beta.threads.messages.list(thread_id=thread.id, order="asc")
    pretty_print(response)
    
    # Delete thread
    fabric_client.beta.threads.delete(thread_id=thread.id)
    ```

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image71.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image72.png)


## **Tarea 6: Eliminar los recursos**

1. En el panel de navegación izquierdo, seleccione su espacio de trabajo, +++AI-Fabric-@lab.LabInstance.Id+++. Se abrirá la vista de elementos del espacio de trabajo.

     ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image73.png)

1. Debajo del nombre del espacio de trabajo, seleccione el menú de tres puntos (...) y, a continuación, seleccione **Workspace settings**.

     ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image74.png)

1. Seleccione **Other** y, a continuación, seleccione **Remove this workspace**.

     ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image75.png)

1. En el mensaje de advertencia que aparece, haga clic en **Delete**.

     ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image76.png)
    
     ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2002/media/image77.png)


### Resumen

En este laboratorio, aprendió a aprovechar el potencial del análisis conversacional mediante Microsoft Fabric Data Agent. Configuró un Fabric Workspace, ingirió datos estructurados en un Lakehouse y configuró una AI skill para traducir preguntas en lenguaje natural a consultas SQL. También mejoró las capacidades del AI agent proporcionando instrucciones y ejemplos para perfeccionar la generación de consultas. Por último, llamó al agente de forma programática desde un Fabric notebook, lo que demostró una integración integral de AI.

Este laboratorio le permite hacer que los datos empresariales sean más accesibles, utilizables e inteligentes para los usuarios de negocio mediante el lenguaje natural y las tecnologías de generative AI.
