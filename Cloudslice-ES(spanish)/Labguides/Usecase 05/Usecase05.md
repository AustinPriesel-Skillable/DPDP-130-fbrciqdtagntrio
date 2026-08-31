## Caso de uso 05: Integrar Fabric Data Agent con Microsoft Teams para obtener información procesable y colaboración entre agentes mediante Copilot Studio

### Introducción

En el competitivo mercado digital actual, las empresas de comercio electrónico generan grandes volúmenes de datos a partir de las transacciones de los clientes, los catálogos de productos, las interacciones en el sitio web y los sistemas de pago. Extraer información significativa de estos datos es fundamental para mejorar la experiencia del cliente, optimizar las operaciones y aumentar los ingresos. Sin embargo, administrar y analizar grandes conjuntos de datos provenientes de múltiples fuentes puede ser complejo sin una plataforma de análisis unificada.

**Zava**, una empresa de comercio electrónico en rápido crecimiento, vende diversos productos de consumo a través de su plataforma en línea y procesa miles de pedidos cada día. La empresa recopila datos de distintos sistemas, incluidos la gestión de pedidos, los perfiles de clientes, el inventario de productos y las transacciones de pago. A medida que el negocio de Zava crece, la organización enfrenta desafíos para analizar estos datos de manera eficiente y proporcionar información en tiempo real a los equipos de negocio.

Para abordar estos desafíos, Zava implementa una solución moderna de análisis mediante **Microsoft Fabric**. Fabric proporciona una plataforma unificada que integra capacidades de ingeniería de datos, almacenamiento de datos, transformación de datos e inteligencia empresarial en un único entorno. Zava almacena sus datos de comercio electrónico, tanto sin procesar como procesados, en el Fabric Lakehouse, lo que permite una administración y un análisis de datos escalables.

Además, Zava aprovecha **Microsoft Fabric Data Agents** para mejorar la accesibilidad a los datos y la generación de información. **Fabric Agents** permiten que los usuarios de negocio y los analistas interactúen con los datos empresariales mediante consultas en lenguaje natural. En lugar de buscar manualmente en los informes o escribir consultas complejas, los usuarios simplemente pueden hacer preguntas como:
- “What are the top selling products this month?”
- “Which region generated the highest sales revenue?”
- “What is the customer order trend over the last quarter?”


El Fabric Agent recupera automáticamente los datos relevantes del Lakehouse y genera información, lo que ayuda a los equipos a comprender rápidamente el desempeño del negocio. Esta interacción inteligente mejora la productividad y permite una toma de decisiones más ágil en toda la organización.

Con esta solución, los usuarios de negocio, analistas y equipos de administración de Zava pueden explorar Data fácilmente, supervisar los indicadores clave de rendimiento y obtener real-time Insights sobre el rendimiento de las ventas, el comportamiento de los clientes y la demanda de productos. Al combinar Advanced Analytics con AI-powered Fabric Agents, Zava crea una plataforma de e-commerce Analytics escalable e inteligente que impulsa el crecimiento data-driven y la excelencia operativa.

### Objetivos

- Crear y configurar un **Fabric Data Agent** conectado a un semantic
  model de comercio electrónico.

- Ingerir y modelar datos en un **Fabric Lakehouse** y exponerlos
  mediante un semantic model.

- Mejorar la inteligencia del agente mediante **metaprompts** e
  instrucciones a nivel del agente.

- Conectar el **Fabric Data Agent** con **Copilot Studio** y habilitar
  la comunicación entre agentes.

- Publicar el agente de Copilot e integrarlo en Microsoft Teams para
  realizar análisis en tiempo real.

- Probar el flujo de extremo a extremo consultando información
  empresarial directamente desde Microsoft Teams.


# Ejercicio 1: Crear y configurar el Fabric Data Agent

## En este ejercicio, establecerá los componentes fundamentales en Microsoft Fabric. Creará un Workspace, configurará un Lakehouse, ingerirá conjuntos de datos de ejemplo en formato CSV, generará un semantic model y configurará un Fabric Data Agent capaz de responder preguntas analíticas. Esto proporciona la capa principal de inteligencia de datos que se utilizará durante el resto del laboratorio.

## Tarea 1: Crear un Fabric Workspace

En esta tarea, creará un Fabric Workspace. El Workspace contiene todos los elementos necesarios para este tutorial del Lakehouse, incluidos el Lakehouse, Dataflows, Data Factory Pipelines, los Notebooks, los conjuntos de datos de Power BI y los informes.

1. Abra el navegador, vaya a la barra de direcciones y escriba o pegue la siguiente dirección URL: +++https://app.fabric.microsoft.com/+++ ; luego presione **Enter** e inicie sesión con sus credenciales.

    |   |   |
    |---|---|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | TAP | +++@lab.CloudPortalCredential(User1).AccessToken+++ |

1. En la página principal de Fabric, seleccione el mosaico **+New workspace**.

     ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image1.png)

1. En el panel **Create a workspace** que aparece en el lado derecho, escriba los siguientes detalles y haga clic en el botón **Apply**.

    | Property | Value |
    |---------|-------|
    | Name | +++Fabric-Copilot-@lab.LabInstance.Id+++  |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | Small dataset storage format |
    | Template apps | Check **Develop template apps** |

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image2.png)

    **Nota:** Para encontrar el ID de la instancia del laboratorio, seleccione **Help** y copie el **Instance ID**.

     ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image3.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image4.png)

1. Espere a que se complete la implementación. Este proceso tarda entre 2 y 3 minutos.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image5.png)


## Tarea 2: Crear un Lakehouse e ingerir datos de ejemplo

En esta tarea, configurará un Lakehouse e ingerirá los datos de ejemplo de NYC Taxi junto con archivos CSV adicionales. Esto establece la base de su conjunto de datos sin procesar dentro de Fabric, lo que le permitirá realizar transformaciones y consultas más adelante.

1. Cree un Lakehouse nuevo haciendo clic en el botón **+New item** de la barra de navegación.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image6.png)

1. En el cuadro de búsqueda **Filter by item type**, escriba +++Lakehouse+++ y seleccione el elemento **Lakehouse.**

     ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image7.png)

1. En el cuadro de diálogo **New lakehouse**, escriba +++fabricagent_lakehouse+++ en el campo **Name**, haga clic en el botón **Create** y abra el nuevo **Lakehouse**.

    **Nota:** Asegúrese de eliminar cualquier espacio antes de **fabricagent_lakehouse**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image8.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image9.png)

1. Espere a que aparezca la notificación **Successfully created SQL endpoint**.

     ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image10.png)

1. En la página del **Lakehouse**, vaya a la sección **Get data in your lakehouse** y haga clic en **Upload files.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image11.png)

1. En la pestaña **Upload files**, haga clic en el icono de carpeta de la sección **Files.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image12.png)

1. Vaya a **C:\LabFiles** en la VM, seleccione los archivos **customers.csv**, **Orders_Data.csv** y **products.csv**, y haga clic en el botón **Open**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image13.png)

1. A continuación, haga clic en el botón **Upload** y cierre el panel.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image14.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image15.png)

1. En la sección **Files**, haga clic en **Refresh**. Los archivos aparecerán en la lista.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image16.png)

1. En la página del **Lakehouse**, en el panel **Explorer**, seleccione **Files**. A continuación, coloque el cursor sobre el archivo **Orders_Data.csv**. Haga clic en los puntos suspensivos (**…**) situados junto a **Orders_Data.csv**, seleccione **Load to Tables** y, después, **New table.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image17.png)

1. En el cuadro de diálogo **Load file to new table**, haga clic en el botón **Load**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image18.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image19.png)

1. Repita el mismo proceso para **customers.csv** y **products.csv** para convertirlos en tablas.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image20.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image21.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image22.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image23.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image24.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image25.png)

1. En el menú desplegable del **Lakehouse**, ubicado en la esquina superior derecha de la pantalla, seleccione **SQL analytics endpoint**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image26.png)

1. En la pestaña **Home** del **Lakehouse**, seleccione **New semantic model** y elija las tablas que desea agregar al **semantic model**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image27.png)

1. En el cuadro de diálogo **New semantic model**, escriba +++E-commerce Order Dataset+++ en el campo **Name**. A continuación, seleccione todas las tablas de la lista y haga clic en **Confirm** para crear el nuevo **semantic model**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image28.png)

1. En el menú de la izquierda, seleccione el icono del Workspace +++Fabric-Copilot-@lab.LabInstance.Id+++ y, a continuación, seleccione el nombre del Workspace.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image29.png)


## Tarea 3: Crear un Fabric Data Agent

1. En la página del Workspace +++Fabric-Copilot-@lab.LabInstance.Id+++, vaya al botón **+New item**, haga clic en él y, a continuación, seleccione **Data agent**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image30.png)

1. Proporcione el nombre <DataAgent_@lab.LabInstance.Id y haga clic en **Create.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image31.png)

1. Seleccione **Add data source** para configurar un nuevo origen de datos.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image32.png)

1. En los resultados, seleccione **E-commerce Order Dataset (Type: Semantic Model)**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image33.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image34.png)

1. Cuando formule las primeras preguntas con todas las tablas seleccionadas, el **Data agent** responderá de manera bastante precisa.

1. Por ejemplo, haga la siguiente pregunta +++Who are the top 10 customers by total purchase amount?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image35.png)

1. Ejecute la aplicación e ingrese las siguientes preguntas de ejemplo para verificar las respuestas:

    +++Which day has the highest sales?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image36.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image37.png)


## Tarea 4: Optimizar con metaprompts

1. En la sección **Setup**, localice el campo **Agent instructions**. (Como alternativa, también puede encontrar **Agent instructions** en la barra de navegación superior).

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image38.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image39.png)

1. Genere instrucciones a nivel del agente utilizando el siguiente metaprompt en el panel de prueba (**Test**) (a la derecha, donde aparece **Test the agent's responses**):

    ```
    Meta-Prompt: Generate Agent-Level Instructions:
     Analyze your available data sources and create agent-level instructions for yourself (max 15000 chars).
    
     Objective: {AGENT_OBJECTIVE}
     Users: {USER_PERSONA}
    
     Examine your data sources: list all sources, types, and primary use. Analyze domain, time coverage, and main themes.
    
     Generate instructions with:
     ## Objective
     ## Data Sources (list with priority)
     ## Key Terminology (infer from columns/measures)
     ## Response Guidelines
     Style: {RESPONSE_STYLE}
     ## Handling Common Topics (3-5 based on available data)
    
     Custom terms: {CUSTOM_TERMINOLOGY}
    ```

    Al utilizar este metaprompt, reemplace manualmente las variables del prompt con los siguientes valores **o** péguelos directamente en Test:
    - {AGENT_OBJECTIVE}: "E-commerce analytics agent for business
    intelligence"
  
    - {USER_PERSONA}: "Business analysts and sales teams"
    - {RESPONSE_STYLE}: "Clear summaries with data citations and trend
    analysis"
  
    - {CUSTOM_TERMINOLOGY}: Leave empty or add your domain-specific terms

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image40.png)


1. Pruebe el agente mejorado con consultas más complejas:

    +++How many orders are placed each day?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image41.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image42.png)

    +++Which products have the lowest stock levels?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image43.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image44.png)


## Tarea 5: Publicar el agente

1. Genere la descripción del agente utilizando el siguiente metaprompt en el panel de prueba (Test) de su Fabric Agent.

    ```
    Meta-Prompt: Generate Agent Description
    Create a 1-2 sentence description of yourself as a Fabric Data Agent (max 200 chars).
    
    Analyze your data sources and describe: what data domain you cover and what questions you answer.
    
    Example: "Fabric Data Agent for retail sales. Answers questions about revenue, products, customers, and orders"
    
    Output plain text only.
    ```

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image45.png)

1. Haga clic en **Publish** y pegue la descripción generada en el campo **Purpose and capabilities**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image46.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image47.png)


# Ejercicio 2: Ejercicio 2: Conectar Fabric Agent con Copilot Studio

## En este ejercicio, habilitará la comunicación entre Copilot Studio y el Fabric Agent. Creará un agente de Copilot, configurará su comportamiento, lo vinculará con el Fabric Data Agent y comprobará que ambos agentes colaboren para generar información más completa. Esto establece la comunicación entre agentes en distintas plataformas.

## Tarea 1: Crear el Copilot Studio Agent

1. Abra una nueva pestaña del navegador y vaya a +++https://copilotstudio.microsoft.com/+++.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image48.png)

1. En el panel de navegación izquierdo, seleccione **Agents.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image49.png)

1. Haga clic en el botón azul **+Create blank agent**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image50.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image51.png)

1. Haga clic en **Edit** para modificar la configuración.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image52.png)

1. Configure el agente con los siguientes valores:

    - **Name**: E-commerce RAG Agent
    - **Description**: An agent connected to a Microsoft Fabric data
    agent specializing in e-commerce business knowledge and support

    - En **Select your agent’s model**, seleccione **Claude Sonnet
      4.5**.

    - En **Instructions**, copie las instrucciones del siguiente bloque
    de código.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image53.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image54.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image55.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image56.png)


1. Haga clic en **Publish** en la esquina superior derecha.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image57.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image58.png)


## Tarea 2: Agregar el Fabric Agent como agente conectado para Copilot Studio

1. Después de crear el agente, vaya a la pestaña **Agents** y haga clic en **+Add agent.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image59.png)

1. Haga clic en **Connect to an external agent** y seleccione **Microsoft Fabric (preview).**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image60.png)

1. Si aparece *Connection: Not connected*, haga clic en la flecha desplegable junto a **Not connected** y seleccione **Create new connection**. Verifique que se muestre la dirección de correo electrónico de su cuenta y, a continuación, haga clic en **Next**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image61.png)

1. Haga clic en **Create** e inicie sesión con la misma cuenta utilizada para este laboratorio.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image62.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image63.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image64.png)

1. Seleccione su **Fabric Data Agent**.

    - Busque el nombre del agente que creó en **el Ejercicio 1 \ Tarea
    3**.

    - Haga clic para seleccionarlo.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image65.png)


1. Escriba el **nombre del agente como +++DataAgent-@lab.LabInstance.Id+++**, verifique la conexión y, a continuación, haga clic en **Add and configure** para continuar con la configuración del agente.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image66.png)

1. Haga clic en **Publish** para que el agente esté disponible para su uso.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image67.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image68.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image69.png)


## Tarea 3: Probar el Fabric Data Agent conectado

1. Pruebe la conexión del **Fabric Data Agent** con consultas progresivamente más complejas:

    +++What are the top 10 highest value orders?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image70.png)

1. Haga clic en **Allow** para conceder los permisos necesarios.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image71.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image72.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image73.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image74.png)

    **Nota:** El proceso de generación de la respuesta puede tardar entre **5 y 6 minutos** en completarse.

    +++What is the average price per category?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image75.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image76.png)


+++What percentage of orders use credit card vs PayPal vs debit card?+++

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image77.png)

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image78.png)

+++What is the revenue by payment method?+++

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image79.png)

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image80.png)

# Ejercicio 3: Conectar el Fabric Data Agent con Microsoft Teams

## En este ejercicio, publicará el agente de Copilot en Teams, lo que permitirá que los usuarios de negocio accedan a enterprise Data directamente desde su aplicación de colaboración. Validará la funcionalidad del agente ejecutando varias consultas de BI y observando las respuestas en real-time dentro de Teams.

## Tarea 1: Agregar capacidades de Copilot

1. En **E-commerce RAG Agent**, haga clic en el icono **+ (Add)** y seleccione **Channels** para configurar las opciones del canal del agente.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image81.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image82.png)

1. Seleccione **Teams and Microsoft 365 Copilot.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image83.png)

1. Haga clic en **Add Channel.**

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image84.png)

1. Seleccione **See agent in Teams** para abrir y probar el agente en **Microsoft Teams**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image85.png)

1. Haga clic en **Open Microsoft Teams.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image86.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image87.png)

1. Haga clic en **Sing in.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image88.png)

1. Escriba las credenciales proporcionadas para iniciar sesión y continuar.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image89.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image90.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image91.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image92.png)

1. Haga clic en **Add.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image93.png)

1. Una vez que la aplicación se haya agregado correctamente, haga clic en el botón *Open* para iniciar **E-commerce RAG Agent** en **Microsoft Teams**.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image94.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image95.png)


## Tarea 2: Probar el Fabric Data Agent conectado

1. Pruebe la conexión del **Fabric Data Agent** con consultas progresivamente más complejas:

    +++What is the revenue trend over time?+++
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image96.png)

1. Haga clic en **Allow** para conceder los permisos necesarios.

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image97.png)
    
     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image98.png)
    
    +++What are the top 10 highest value orders?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image99.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image100.png)

    +++Which payment method is used the most?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image101.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-ES(spanish)/Labguides/Usecase%2005/media/image102.png)


### Resumen

En este caso de uso, se mostró cómo una organización de comercio electrónico puede integrar **Microsoft Fabric Data Agents** con **Microsoft Teams** mediante **Copilot Studio** para proporcionar información en tiempo real, análisis en lenguaje natural y colaboración entre agentes. Al combinar una plataforma de análisis unificada (Microsoft Fabric) con conversational AI (Copilot Studio y Microsoft Teams), los usuarios de negocio pueden acceder de forma sencilla a tendencias de ventas, información sobre productos y comportamiento de los clientes sin necesidad de escribir consultas. La solución demuestra cómo los agentes de AI pueden recuperar datos desde Fabric Lakehouse, enriquecer las respuestas mediante **Agent instructions** y colaborar con otros agentes para optimizar los flujos de trabajo de inteligencia empresarial.
