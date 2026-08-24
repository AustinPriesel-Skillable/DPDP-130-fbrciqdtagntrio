# Caso de uso 04: Conectar Fabric Data Agent a Microsoft Foundry para obtener Insights unificados e inteligentes sobre los datos

**Introducción**

Las organizaciones modernas generan grandes volúmenes de datos en
múltiples sistemas, lo que dificulta que los usuarios empresariales y
los analistas accedan rápidamente a información valiosa. Con frecuencia,
los datos se almacenan en silos, lo que requiere conocimientos técnicos
para extraer, analizar e interpretar la información.

La plataforma de datos unificada Microsoft Fabric aborda este desafío al
reunir las capacidades de análisis, ingeniería de datos y business
intelligence en un único entorno. Al integrar las capacidades de AI
basadas en agentes de Azure AI Foundry, las organizaciones pueden crear
aplicaciones inteligentes que interactúan con los datos empresariales
mediante lenguaje natural y flujos de trabajo automatizados.

Agentic Applications for Unified Data Foundation Solution Accelerator
demuestra cómo los agentes AI-powered pueden aprovechar Unified Data
Foundation para responder preguntas, automatizar el análisis de Data y
proporcionar insights tanto a usuarios técnicos como no técnicos.

En este caso de uso, las organizaciones pueden utilizar agentes de AI
para analizar datos empresariales, como el rendimiento de las ventas, el
comportamiento de los clientes y las tendencias de los productos. En
lugar de consultar manualmente múltiples conjuntos de datos, los
usuarios pueden simplemente formular preguntas en lenguaje natural y
recibir información procesable directamente del sistema.

**Objetivo**

El objetivo de este caso de uso es demostrar cómo las organizaciones
pueden aprovechar **agentic AI con una base de datos unificada para
mejorar la accesibilidad a los datos** y la toma de decisiones. Los
objetivos principales incluyen:

1.  **Establecer una base de datos unificada en Microsoft Fabric**

- Crear un espacio de trabajo de Fabric gobernado con Lakehouse,
  Warehouse y modelos semánticos.

- Cargar y validar conjuntos de datos empresariales para su análisis.

**2. Crear y configurar un Fabric Data Agent**

- Crear un **Fabric Data Agent** capaz de consultar conjuntos de datos
  mediante lenguaje natural.

- Conectar recursos de Ontology y definir instrucciones para el agente
  que permitan responder consultas específicas de la organización.

**3. Implementar componentes de Azure y Foundry**

- Aprovisionar recursos de Azure, incluidos el proyecto de Foundry, AI
  Services, Search, Storage y App Services.

- Implementar los componentes de soporte mediante Azure Developer CLI
  (azd).

**4. Conectar Fabric Data Agent a Microsoft Foundry**

- Crear o configurar un AI agent en Foundry.

- Conectar el agente a Microsoft Fabric mediante Workspace ID y AI
  Skills ID.

- Proporcionar instrucciones específicas del dominio que permitan al
  agente interpretar y analizar los datos de Fabric.

**5. Habilitar Conversational Analytics y Automated Insights**

- Probar el agente en Foundry Playground con consultas empresariales
  reales.

- Demostrar flujos de trabajo de recuperación de datos mediante lenguaje
  natural utilizando conjuntos de datos de Fabric Lakehouse.

- Proporcionar Insights, como tasas de aprobación/rechazo de
  inspecciones, promedios, tendencias y resúmenes agrupados.

**6. Demostrar un flujo de trabajo de aplicaciones agentic de extremo a
extremo**

- Integrar el agente de Foundry, el origen de datos de Microsoft Fabric
  y la infraestructura de Azure en una aplicación web funcional.

- Validar las interacciones inteligentes con los datos, el razonamiento
  automatizado y la generación de Insights.

**Arquitectura de la solución**

![Architecture Diagram](./media/image1.png)

La solución combina Microsoft Fabric y Microsoft Foundry para crear una
AI solution capaz de responder preguntas utilizando tanto datos
estructurados como documentos no estructurados:

- **Microsoft Fabric** proporciona la capa de datos con Lakehouse,
  Warehouse y la capa semántica Fabric IQ para la traducción de lenguaje
  natural a SQL.

- **Microsoft Foundry** hospeda los AI agents, incluido Foundry IQ para
  la recuperación de documentos y Orchestrator Agent, que orquesta ambas
  capacidades.

- **Azure AI Services** proporciona los modelos de lenguaje
  (GPT-4o-mini) y los embeddings.

- **Azure AI Search** almacena vectores de documentos para la
  recuperación semántica.

**Requisitos previos**

- **Cuenta de GitHub:** Debe disponer de sus propias credenciales de
  inicio de sesión de GitHub.  
  **Si no tiene una cuenta, créela en:  
  +++<https://github.com/signup?user_email=&source=form-home-signup+++>**

## Tarea 0: Crear una cuenta de GitHub

En esta tarea, creará una nueva cuenta de **GitHub** utilizando las
mismas credenciales del tenant que ha usado en este laboratorio.

1.  Vaya a GitHub mediante el siguiente vínculo
    +++<https://github.com/+++> y haga clic en **Sign up** para
    continuar.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

2.  Para crear una nueva cuenta de GitHub, escriba una dirección de
    **correo electrónico**, una **contraseña** y un nombre de usuario
    único. A continuación, haga clic en **Continue.**

![A screenshot of a login box AI-generated content may be
incorrect.](./media/image3.png)

3.  Complete el **desafío de verificación** siguiendo las instrucciones
    que aparecen en pantalla. A continuación, haga clic en **Submit**.

4.  Escriba el **código de verificación** que recibió en su correo
    electrónico.

![A screenshot of a email form AI-generated content may be
incorrect.](./media/image4.png)

5.  Inicie sesión en GitHub con las credenciales que creó y haga clic en
    **Sign in.**

![A screenshot of a login page AI-generated content may be
incorrect.](./media/image5.png)

6.  Ha creado correctamente una nueva cuenta de GitHub.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

## Tarea 1: Crear un espacio de trabajo de Microsoft Fabric

En esta tarea, creará un espacio de trabajo de Microsoft Fabric. El
espacio de trabajo contiene todos los elementos necesarios para este
tutorial de Lakehouse, incluidos Lakehouse, flujos de datos, Data
Factory pipelines, notebooks, conjuntos de datos de Power BI e informes.

1.  Abra el explorador, vaya a la barra de direcciones, escriba o pegue
    la siguiente URL: +++<https://app.fabric.microsoft.com/+++>,
    presione **Enter** e inicie sesión con sus credenciales.

    |   |   |
    |---|---|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | TAP | +++@lab.CloudPortalCredential(User1).AccessToken+++ |

2.  En la página principal de Microsoft Fabric, seleccione el mosaico
    **+New workspace**.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

3.  En el panel **Create a workspace**, que aparece en el lado derecho,
    escriba la siguiente información y haga clic en **Apply**.

    | Property | Value |
    |---------|-------|
    | Name | +++Fabric agent@lab.LabInstance.Id+++  |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | Small dataset storage format |
    | Template apps | Check **Develop template apps** |

> ![](./media/image8.png)
>
> \[!Nota\] Para encontrar el identificador de la instancia del
> laboratorio, seleccione **Help** y copie el **Instance ID.**
>
> ![A screenshot of a computer Description automatically
> generated](./media/image9.png)
>
> ![](./media/image10.png)

4.  Espere a que finalice la implementación. Este proceso tarda entre 2
    y 3 minutos en completarse.

![](./media/image11.png)

## Tarea 2: Obtener el Workspace ID de Microsoft Fabric

Necesitará el Workspace ID para pasarlo como parámetro al compilar la
solución.

1.  Observe la URL. El Workspace ID es el GUID que aparece después de
    /groups/.

2.  Copie el **Workspace ID** de la URL (por ejemplo,
    https://app.fabric.microsoft.com/groups/{workspace-id}/...) y
    guárdelo en **Notepad** para usarlo más adelante.

![](./media/image12.png)

## Tarea 3: Abrir el entorno de desarrollo

1.  Abra el explorador, vaya a la barra de direcciones y escriba o pegue
    la siguiente URL:
    +++<https://github.com/technofocus-pte/agnticapp-for-unified-data/tree/main+++>

![](./media/image13.png)

2.  Haga clic en **Fork** para crear un *fork* del repositorio. Asigne
    un nombre único al repositorio y, a continuación, haga clic en
    **Create repository.**

![](./media/image14.png)

![](./media/image15.png)

3.  Haga clic en **Code -\> Codespaces -\> Create Codespace on main.**

![](./media/image16.png)

4.  Espere a que finalice la configuración del entorno de
    **Codespaces**. Este proceso tarda unos minutos en completarse.

![](./media/image17.png)

![](./media/image18.png)

![](./media/image19.png)

## Tarea 4: Aprovisionar servicios e implementar la aplicación en Azure y Microsoft Fabric

1.  Ejecute el siguiente comando en la **Terminal**. Se generará un
    código. Cópielo y, a continuación, presione **Enter**.

 +++azd auth login+++

![](./media/image20.png)

2.  Se abrirá el explorador predeterminado para que escriba el código
    generado y complete la verificación. Escriba el código y haga clic
    en **Next**.

![](./media/image21.png)

![](./media/image22.png)

![](./media/image23.png)

![](./media/image24.png)

3.  Inicie sesión en Azure.

+++az login+++

![](./media/image25.png)

4.  Se abrirá el explorador predeterminado para que escriba el código
    generado y complete la verificación. Escriba el código y haga clic
    en **Next.**

![](./media/image26.png)

5.  Seleccione su suscripción de Azure.

![](./media/image27.png)

6.  Registre el proveedor de recursos **Microsoft Cognitive Services**
    (si aún no está registrado en su suscripción).

+++az provider register --namespace Microsoft.CognitiveServices+++

![](./media/image28.png)

**\[!Alerta\]** En el panel izquierdo, vaya a la carpeta **Infra**, abra
el archivo **main.bicep** y, en la **línea 122**, reemplace el valor de
la cadena *Lab Instance ID* por <+++@lab.LabInstance.Id>+++.

7.  Aprovisione e implemente todos los recursos:

+++azd up+++

![](./media/image29.png)

8.  Seleccione los siguientes valores:

    - **Para crear un entorno para los recursos de Azure,**
      escriba <+++env@lab.LabInstance.Id>+++

    - **En Select an Azure Subscription to use,** seleccione
      **@lab.CloudSubscription.Name**

    - En el parámetro de infraestructura **Location**, seleccione la
      ubicación de **ResourceGroup1.**

    - En **Resource Group,** seleccione
      **@lab.CloudResourceGroup(ResourceGroup1).Name**

**\[!Nota\]** Si la implementación de **Codespaces** falla en la región
de Azure seleccionada, cambie la región de implementación y vuelva a
ejecutar la implementación.

azd env set AZURE_RESOURCE_LOCATION \<region\>

Por ejemplo:

azd env set AZURE_RESOURCE_LOCATION westus2

Regiones admitidas:

- westus2

- japaneast

- swedencentral

- northeurope 

> Después de actualizar la región, vuelva a ejecutar los pasos de
> implementación.

![](./media/image30.png)

![](./media/image31.png)

![](./media/image32.png)

10. La implementación tardará entre **7 y 10** minutos en aprovisionar
    los recursos en su cuenta y configurar la solución con datos de
    ejemplo.

11. La implementación ha finalizado.

![](./media/image33.png)

12. Cree y active un entorno virtual.

+++python -m venv .venv+++

![](./media/image34.png)

13. En **Visual Studio Code**, seleccione el **ícono de menú** ubicado
    en la esquina superior izquierda y, a continuación, vaya a
    **Terminal \> New Terminal** para abrir una nueva ventana de
    terminal en el espacio de trabajo.

![](./media/image35.png)

![](./media/image36.png)

14. Ejecute el siguiente comando en la terminal para instalar las
    dependencias de Python necesarias.

+++pip install uv && uv pip install -r scripts/requirements.txt+++

![](./media/image37.png)

![](./media/image38.png)

15. Ejecute el siguiente comando en la Terminal. Se generará un código.
    Cópielo y, a continuación, presione **Enter**.

+++az login+++

![](./media/image39.png)

![](./media/image40.png)

![](./media/image41.png)

16. Seleccione su **suscripción de Azure** de la lista para continuar
    con el proceso de configuración.

![](./media/image42.png)

17. Ejecute el script de bash que se muestra en la salida de la
    implementación de azd. Reemplace \<your-workspace-id\> por el Fabric
    Workspace ID que creó en los pasos anteriores. El script tendrá un
    aspecto similar al siguiente:

    `python scripts/00_build_solution.py --from 02 --fabric-workspace-id <your-workspace-id>`

![](./media/image43.png)

19. Presione **Enter** para comenzar a crear los recursos.

![](./media/image44.png)

![](./media/image45.png)

![](./media/image46.png)

## Tarea 5: Revise Fabric Lakehouse y Data

1.  Vaya a su espacio de trabajo en
    +++https://app.fabric.microsoft.com/+++.

2.  Asegúrese de que los recursos se hayan implementado correctamente.

![](./media/image47.png)

3.  Haga clic en **Lakehouse** para comprobar que los datos se hayan
    cargado correctamente.

![](./media/image48.png)

![](./media/image49.png)

4.  Regrese a **Codespaces** para probar el agente.

## Tarea 6: Probar el agente

1.  Para probar el agente, ejecute el siguiente comando en la terminal.

+++python scripts/08_test_agent.py+++

![](./media/image50.png)

2.  Escriba la siguiente pregunta de ejemplo: +++What is the average
    score from inspections?+++

![](./media/image51.png)

+++What constitutes a failed inspection?+++

![](./media/image52.png)

![](./media/image53.png)

+++Do any inspections violate quality control standards in our Inspection Procedures?+++

![](./media/image54.png)

![](./media/image55.png)

3.  Presione **Ctrl+C** para cancelar el proceso.

![](./media/image56.png)

## Tarea 7: Crear un Fabric Data Agent

1.  Vaya a su espacio de trabajo de Microsoft Fabric en
    +++https://app.fabric.microsoft.com/+++

2.  Seleccione **New item**, busque **Data Agent** y, a continuación,
    seleccione **Data agent.**

![](./media/image57.png)

3.  Escriba +++FabricDataAgent@lab.LabInstance.Id+++ como nombre y
    haga clic en **Create**.

![](./media/image58.png)

4.  Seleccione **Add data source** para configurar un nuevo origen de
    datos.

![](./media/image59.png)

5.  Seleccione el recurso Ontology correspondiente a este taller.

![](./media/image60.png)

![](./media/image61.png)

6.  En el menú superior, seleccione **Agent instructions**.

![](./media/image62.png)

7.  Agregue las siguientes instrucciones para el agente:

+++You are a helpful assistant that can answer user questions using data. Support group by in GQL+++

![](./media/image63.png)

![](./media/image64.png)

8.  En el menú superior, haga clic en **Publish** y, a continuación,
    seleccione nuevamente **Publish**.

![](./media/image65.png)

![](./media/image66.png)

![](./media/image67.png)

\[!Nota\] La configuración de Ontology puede tardar hasta 15 minutos. Si
no obtiene buenas respuestas, espere unos minutos y vuelva a intentarlo.

9.  Para probar el agente, ejecute la aplicación e introduzca la
    siguiente pregunta de ejemplo para comprobar la respuesta:

+++How many tickets are high priority+++

![](./media/image68.png)

![](./media/image69.png)

+++What is the average score from inspections?+++

![](./media/image70.png)

![](./media/image71.png)

![](./media/image72.png)

+++Show tickets grouped by status+++

![](./media/image73.png)

![](./media/image74.png)

10. Guarde el **Workspace ID** y el **AISkills ID** en Notepad para
    utilizarlos más adelante.

![](./media/image75.png)

11. Regrese a **Codespaces** para implementar e iniciar la aplicación.

## Tarea 8: Implementar e iniciar la aplicación

1.  Ejecute el siguiente comando para establecer la variable de entorno
    **AZURE_ENV_DEPLOY_APP** en **true** antes de la implementación.

+++azd env set AZURE_ENV_DEPLOY_APP true+++

![](./media/image76.png)

2.  Ejecute azd up. Este comando aprovisionará los recursos de Azure.

+++azd up+++

![](./media/image77.png)

3.  Una vez que la implementación haya finalizado correctamente, copie
    la **Web app URL**.

![](./media/image78.png)

4.  Ejecute el siguiente comando para configurar los permisos de la
    aplicación.

+++python scripts/00_build_solution.py --from 09+++

![](./media/image79.png)

5.  Presione **Enter** para iniciar la configuración.

![](./media/image80.png)

![](./media/image81.png)

6.  Haga clic en la Web app URL.

![](./media/image82.png)

![](./media/image83.png)

![](./media/image84.png)

**Preguntas de ejemplo**

Para ayudarle a comenzar, estas son algunas **preguntas de ejemplo** que
puede hacer en la aplicación:

Para el caso de uso de análisis de ventas minoristas:

+++Show total revenue by year for last 5 years+++.

![](./media/image85.png)

![](./media/image86.png)

![](./media/image87.png)

![](./media/image88.png)

\[!Alerta\] Es posible que no obtenga una respuesta debido a los datos
cargados. Si esto ocurre, continúe con el resto del laboratorio.

## Tarea 9: Verificar Azure Resources y revisar Fabric Lakehouse Data

1.  Abra un explorador, vaya
    a+++[https://portal.azure.com+++](https://portal.azure.com+++/) e
    inicie sesión con la cuenta de **Cloud Slice** que se proporciona a
    continuación.

2.  Seleccione **Resource groups.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image89.png)

3.  Haga clic en el **Resource group** que tiene asignado.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image90.png)

4.  Asegúrese de que los siguientes recursos se hayan implementado
    correctamente.

    - Foundry

    - Foundry project

    - Application Insights

    - Search service

    - Azure Storage account

    - App Service

    - Azure Cosmos DB account

![](./media/image91.png)

## Tarea 10: Utilizar el Fabric Data Agent desde Microsoft Foundry Services

1.  Seleccione **Foundry.**

![](./media/image92.png)

2.  En el panel **Overview**, haga clic en **Go to Foundry portal**.
    Esto lo dirigirá al portal de Microsoft Foundry.

![](./media/image93.png)

![](./media/image94.png)

Una vez que acceda al portal de Foundry, seleccione **Agents** en el
menú izquierdo. Verá un agente **creado previamente**. Si no aparece,
haga clic en **+ New agent** para crearlo.

![](./media/image95.png)

![](./media/image96.png)

3.  Seleccione el **agente** recién creado. Se abrirá un panel de
    configuración a la derecha. En **Agent name**, escriba **+++Fabric
    Agent+++**.

![](./media/image97.png)

4.  En el mismo panel de configuración del agente, desplácese hacia
    abajo y, en el parámetro **Knowledge**, haga clic en **+ Add**.

![](./media/image98.png)

5.  En el panel **Add knowledge**, seleccione **Microsoft Fabric**.

![](./media/image99.png)

6.  Haga clic en **+ Create connection.**

![](./media/image100.png)

7.  Escriba las claves personalizadas, como **Workspace ID** y
    **AISkills ID**, que guardó en la **Tarea 7 \> Paso 6**. Escriba
    **Fabric-aiskills** como nombre de la conexión y haga clic en
    **Connect**.

![](./media/image101.png)

8.  Escriba las siguientes instrucciones:

    ```
    You are a data assistant that analyzes inspection data stored in Microsoft Fabric.
    
    Use the Fabric Lakehouse dataset to answer questions about inspection results and scores. The dataset includes the following columns:
    - inspection_id: Unique identifier for each inspection
    - ticket_id: Identifier associated with the inspection ticket
    - result: Inspection outcome (Pass or Fail)
    - score: Numeric score assigned to the inspection
    
    You can analyze and summarize the data to provide insights such as:
    - Total number of inspections
    - Number of passed and failed inspections
    - Average, highest, and lowest inspection scores
    - Distribution of inspection results
    - Score trends across inspections or tickets
    
    When responding:
    - Use the Fabric data source to retrieve accurate information.
    - Provide clear summaries and insights based on the inspection results.
    - When appropriate, suggest visualizations such as bar charts or pie charts to show pass vs fail distribution or score comparisons.
    - Ensure answers are concise, accurate, and based only on the available dataset.
    ```

![](./media/image102.png)

9.  En el menú izquierdo, seleccione **Agents**. A continuación,
    seleccione el agente **Fabric Agent** y haga clic en **Try in
    playground**.

![](./media/image103.png)

10. Se abrirá un panel de chat en el que podrá escribir sus prompts. A
    partir de ese momento, el agente responderá utilizando los
    documentos y conjuntos de datos que haya conectado.

Prompts de ejemplo:

+++What constitutes a failed inspection?+++

![](./media/image104.png)

![](./media/image105.png)

+++What is the total number of tickets in the system?+++

![](./media/image106.png)

![](./media/image107.png)

+++Do any inspections violate quality control standards in our Inspection Procedures?+++

![](./media/image108.png)

![](./media/image109.png)

## Tarea 11: Eliminar los recursos

1.  Para eliminar los recursos, escriba **Resource groups** en la barra
    de búsqueda del Azure portal. A continuación, vaya a **Resource
    groups** en **Services** y haga clic en esa opción.

![A screenshot of a computer Description automatically
generated](./media/image110.png)

2.  En la página **Resource groups**, seleccione su **Resource group**.

3.  En la página principal del **Resource group**, seleccione todos los
    recursos excepto **Fabric Capacity** y, a continuación, haga clic en
    **Delete**.

![](./media/image111.png)

4.  En el panel **Delete Resources** que aparece en el lado derecho,
    vaya al campo **Enter "delete" to confirm deletion**, escriba
    **delete** y, a continuación, haga clic en **Delete**.

![](./media/image112.png)

![](./media/image113.png)

5.  Vaya a su espacio de trabajo de **Microsoft Fabric**
    en+++<https://app.fabric.microsoft.com/+++>.

6.  Seleccione **los tres puntos (...)** debajo del nombre del espacio
    de trabajo y, a continuación, seleccione **Workspace settings**.

![](./media/image114.png)

7.  Seleccione **General** y, a continuación, **Remove this workspace.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image115.png)

8.  Haga clic en **Delete** en la advertencia que aparece.

![](./media/image116.png)

9.  Espere a que aparezca una notificación indicando que el Workspace se
    ha eliminado antes de continuar con el siguiente laboratorio.

![](./media/image117.png)

**Resumen**

Este caso de uso demuestra cómo las organizaciones pueden crear
aplicaciones de datos inteligentes impulsadas por agentes mediante la
integración de Microsoft Fabric con Microsoft Foundry. La solución
establece una base de datos unificada en la que los datos empresariales
almacenados en Fabric Lakehouse y Warehouse pueden consultarse y
analizarse mediante agentes AI-powered.

Al conectar un Fabric Data Agent a Foundry, los usuarios pueden
interactuar con conjuntos de datos empresariales mediante consultas en
lenguaje natural, en lugar de escribir consultas SQL complejas o
analizar manualmente múltiples orígenes de datos. El AI agent recupera
los datos relevantes, realiza el análisis y genera insights, como
promedios, tendencias, resúmenes y resultados agrupados.

La solución también aprovisiona los servicios complementarios de Azure,
incluidos AI services, Search, Storage y aplicaciones web, lo que
permite una arquitectura completa de aplicaciones agentic de extremo a
extremo. Esto permite a las organizaciones combinar datos empresariales
estructurados con capacidades de AI para ofrecer análisis
conversacionales e insights automatizados.

En general, este caso de uso destaca cómo las aplicaciones de agentic AI
creadas sobre una plataforma de datos unificada pueden simplificar el
acceso a los datos, acelerar el análisis y respaldar una toma de
decisiones más rápida y basada en datos tanto para usuarios técnicos
como no técnicos.
