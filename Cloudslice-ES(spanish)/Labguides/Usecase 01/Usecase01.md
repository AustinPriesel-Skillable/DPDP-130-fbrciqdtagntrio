Caso de uso 1: De la semántica a los insights: Aprovechar Fabric IQ
Ontology con Fabric Data Agents

**Introducción**

En las plataformas de datos modernas, las empresas suelen necesitar una
capa semántica centrada en el negocio que unifique el significado entre
diversas fuentes de datos y modelos analíticos. La característica
Ontology (preview) de Microsoft Fabric IQ le permite crear esta capa
definiendo conceptos empresariales (como productos, tiendas y eventos) y
sus relaciones, y vinculando estas definiciones con datos reales de su
Lakehouse, modelos semánticos y flujos de eventos.

En este escenario, una empresa ficticia llamada Lakeshore Retail, que
vende helados en varias ubicaciones. Con datos de ejemplo, el tutorial
muestra cómo configurar el entorno y comenzar a crear una ontología que
capture conceptos empresariales como Store, Products y SaleEvent.
También conectará datos de streaming (como las temperaturas de los
congeladores desde Eventhouse) a estos conceptos para que la ontología
pueda admitir razonamiento y consultas entre dominios, por ejemplo:
“¿Qué Store tienen menores ventas de helados cuando la temperatura del
congelador supera los –18 °C?”

**Objetivos**

- Preparar un Microsoft Fabric Workspace con los servicios necesarios,
  incluidos Lakehouse, Eventhouse y Ontology (preview).

- Crear una ontología centrada en el negocio definiendo los tipos de
  entidad principales, como Store, Products, SaleEvent y Freezer.

- Vincular datos estáticos de tablas de OneLake y datos de series
  temporales de Eventhouse con las entidades de la ontología.

- Crear relaciones significativas entre entidades para representar
  procesos empresariales reales (por ejemplo, Store tiene SaleEvent y
  Store opera Freezer).

- Explorar y validar la ontología mediante instancias de entidades,
  gráficos de relaciones y filtros del generador de consultas.

- Habilitar consultas en lenguaje natural integrando la ontología con un
  Fabric Data Agent (preview).

# Ejercicio 1: Configuración del entorno

## Tarea 1: Crear un espacio de trabajo de Fabric

En esta tarea, creará un espacio de trabajo de Fabric. El espacio de
trabajo contendrá todos los elementos necesarios para este laboratorio
de Lakehouse, incluidos Lakehouse, los flujos de datos, los pipelines de
Data Factory, los notebooks, los conjuntos de datos de Power BI y los
informes.

1.  Abra el navegador, vaya a la barra de direcciones y escriba o pegue
    la siguiente URL: +++https://app.fabric.microsoft.com/+++; luego
    presione **Enter** e inicie sesión con sus credenciales.

[TABLE]

2.  En el panel Workspaces, haga clic en el mosaico **+New workspace.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

3.  En el panel **Create a workspace** que aparece en el lado derecho,
    escriba los siguientes valores y haga clic en el botón **Apply**.

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image2.png)
>
> ![](./media/image3.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image4.png)

## Tarea 2: Crear un Lakehouse

1.  Cree un nuevo Lakehouse haciendo clic en el botón **+New item** de
    la barra de navegación.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image5.png)

2.  Filtre por Lakehouse y seleccione el mosaico +++**Lakehouse**+++.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image6.png)

3.  En el cuadro de diálogo **New lakehouse**, escriba
    +++**IQ_Lakehouse**+++ en el campo **Name** y desactive **Lakehouse
    schemas**. Haga clic en el botón **Create** y abra el nuevo
    **Lakehouse**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image7.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image8.png)

4.  Verá una notificación con el mensaje **Successfully created SQL
    endpoint**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

## Tarea 3: Incorporar datos de ejemplo

1.  En la página **IQ_Lakehouse**, vaya a la sección **Get data in your
    lakehouse** y haga clic en **Upload files**, como se muestra en la
    siguiente imagen**.**

> ![](./media/image10.png)

2.  En la pestaña **Upload files**, haga clic en el icono de carpeta
    ubicado debajo de **Files**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image11.png)

3.  Vaya a **C:\LabFiles\LabFiles** en la VM, seleccione los archivos
    **DimProducts.csv**, **DimStore.csv**, **FactSale.csv** y
    **Freezer.csv**, y haga clic en el botón **Open**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image12.png)

4.  A continuación, haga clic en el botón **Upload** y cierre el cuadro
    de diálogo **Upload files** seleccionando el icono **X**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image13.png)
>
> ![A screenshot of a upload box AI-generated content may be
> incorrect.](./media/image14.png)

5.  Haga clic en **Refresh** en **Files**. Los archivos aparecerán.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image15.png)

6.  En la página **Lakehouse**, en el panel **Explorer**, seleccione
    **Files**. A continuación, coloque el cursor sobre el archivo
    **DimProducts.csv**. Haga clic en los puntos suspensivos (**…**)
    junto a **DimProducts.csv**, seleccione **Load to Tables** y,
    después, **New table.**

> ![](./media/image16.png)
>
> ![](./media/image17.png)

7.  En el cuadro de diálogo **Load file to new table**, haga clic en el
    botón **Load**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image18.png)

8.  La tabla **DimProducts** se creará correctamente.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

9.  Seleccione la tabla **DimProducts** para obtener una vista previa de
    los datos.

\[!note\] **Nota**: Es posible que deba seleccionar el botón **Refresh**
más de una vez para obtener una vista previa de los datos.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image20.png)

10. Repita los pasos 7 al 9 para cargar los archivos restantes en las
    tablas.

![](./media/image21.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

> ![](./media/image23.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image24.png)
>
> ![](./media/image25.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image26.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image27.png)

11. En la barra de navegación izquierda, seleccione **Fabric IQ
    Ontology**.

> ![](./media/image28.png)

## Tarea 4: Preparar el Eventhouse

Siga estos pasos para cargar el archivo de datos de streaming de
dispositivos en una base de datos KQL en Eventhouse.

1.  En la página principal de **Fabric IQ Ontology**, seleccione **+New
    item** y, a continuación, seleccione **Eventhouse**. 

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image29.png)

2.  Asigne el nombre +++**TelemetryDataEH**+++ al **Eventhouse** y haga
    clic en el botón **Create**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image30.png)

3.  El Eventhouse se abrirá cuando esté listo. ![A screenshot of a
    computer AI-generated content may be
    incorrect.](./media/image31.png)

4.  Abra la base de datos KQL seleccionando su nombre.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image33.png)

5.  En la cinta de opciones inferior de la **KQL database**, haga clic
    en **Get data** y, a continuación, seleccione **Local file** para
    cargar archivos desde el sistema local en la base de datos.  
    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image34.png)

6.  Seleccione la opción de destino para incorporar los datos en una
    nueva tabla, haga clic en **+ New table** y escriba
    +++**FreezerTelemetry**+++ como nombre de la tabla.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image35.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image36.png)

7.  Seleccione la tabla de destino y, a continuación, arrastre y suelte
    los archivos o haga clic en **Browse for files** para cargar los
    datos.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image37.png)

8.  Vaya a **C:\LabFiles\Lab1** en su VM, seleccione el archivo
    **FreezerTelemetry.csv** y haga clic en el botón **Open**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image38.png)

9.  Haga clic en el botón **Next.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image39.png)

10. A continuación, haga clic en el botón **Finish**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image40.png)

11. Espere a que se complete la **Data ingestion** y haga clic en
    **Close**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image41.png)

12. Cuando termine, la KQL database mostrará la tabla
    **FreezerTelemetry**:

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image42.png)

13. Seleccione **Fabric IQ Ontology** en el panel de navegación
    izquierdo.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image43.png)

# Ejercicio 2: Crear una Ontology a partir de OneLake

## Tarea 1: Crear un elemento Ontology (preview)

1.  En el espacio de trabajo de Fabric, seleccione **+ New item**.
    Busque y seleccione el elemento **Ontology (preview)**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image44.png)

2.  Escriba +++**RetailSalesOntology**+++ como nombre de la ontología y
    seleccione **Create**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image45.png)
>
> ** Sugerencia:** Los nombres de las ontologías pueden incluir números,
> letras y guiones bajos. No utilice espacios ni guiones.

3.  La **Ontology** se abrirá cuando esté lista.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image46.png)
>
> A continuación, cree tipos de entidad, vinculaciones de datos y
> relaciones basadas en los datos de las tablas de su Lakehouse.

## Tarea 2: Crear tipos de entidad y vinculaciones de datos

> Primero, cree los tipos de entidad. Los tipos de entidad representan
> tipos de objetos en una empresa. En este paso se crean tres tipos de
> entidad: *Store, Products* y *SaleEvent*. Después de crear los tipos
> de entidad, cree sus propiedades vinculando las columnas de los datos
> de origen de las tablas del Lakehouse ***IQ_Lakehouse.***

### Agregar el primer tipo de entidad (Store)

1.  En la cinta de opciones superior o en el centro del lienzo de
    configuración, seleccione **Add entity type**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image47.png)

2.  Escriba +++Store+++ como nombre del tipo de entidad y seleccione
    **Add Entity Type**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image48.png)

3.  El tipo de entidad *Store* se agrega al lienzo de configuración y el
    panel **Entity type configuration** estará visible.

> ![](./media/image49.png)

4.  En el lienzo de configuración, haga clic en los puntos suspensivos
    (**...**) situados junto al nombre de la entidad y seleccione **Bind
    data**.

> ![](./media/image50.png)

5.  Seleccione **Add data binding \> Lakehouse table**.

> ![](./media/image51.png)

6.  A continuación, elija el origen de datos. Seleccione el
    **Lakehouse** **IQ_Lakehouse** y haga clic en **Next**.

> ![](./media/image52.png)

7.  Seleccione la tabla **dimstore** y haga clic en **Select**.

![](./media/image53.png)

8.  Los campos de la tabla de origen se cargarán en la configuración de
    la vinculación de datos. Observe las siguientes secciones de la
    página de configuración:

- **Entity type key:** Identifica el campo (o los campos) que puede
  utilizarse para identificar de forma única cada registro de los datos
  incorporados.

- **Binding selection:** Identifica la tabla de origen que contiene los
  datos de la vinculación.

- **Entity type key mapping:** Identifica la columna (o las columnas) de
  la tabla de datos de origen que se asigna a la propiedad de clave del
  tipo de entidad. Puede seleccionar columnas de tipo cadena e integer
  de los datos de origen como clave del tipo de entidad. En conjunto,
  las columnas seleccionadas identifican de forma única un registro.

- **Properties:** Enumera las columnas de los datos de origen que se
  representarán como propiedades del tipo de entidad *Store*. La columna
  **Source** se completa automáticamente con las columnas de la tabla
  *dimstore*, mientras que la columna **Property name** muestra los
  nombres de las propiedades correspondientes del tipo de entidad
  *Store* dentro de la Ontology. Para este tutorial, mantenga los
  nombres de propiedad predeterminados.

![](./media/image54.png)

9.  En la parte superior de la configuración, seleccione **Define entity
    type key**.

![](./media/image55.png)

10. Seleccione **StoreId** en la lista de propiedades y haga clic en
    **Save**.

![](./media/image56.png)

11. **Guarde** la vinculación de datos.

![](./media/image57.png)

![](./media/image58.png)

12. Confirme que el tipo de entidad se haya actualizado correctamente y,
    a continuación, seleccione **Cancel** para cerrar las opciones de
    configuración.

![](./media/image59.png)

13. Verá la página **Configure** con los detalles del tipo de entidad.
    Esta página muestra información importante sobre el tipo de entidad,
    incluidas sus propiedades y vinculaciones de datos. Revise las
    vinculaciones de datos configuradas.

![](./media/image60.png)

14. Seleccione **Home** para volver al lienzo de configuración y agregar
    nuevos tipos de entidad.

![](./media/image61.png)

### Agregar los demás tipos de entidad (Products, SaleEvent)

15. Siga los mismos pasos que utilizó para el tipo de entidad **Store**
    para crear los tipos de entidad descritos en la siguiente tabla.
    Cada entidad tiene una vinculación de datos estática con las
    columnas predeterminadas de su tabla de origen.

[TABLE]

![](./media/image62.png)

> ![](./media/image63.png)

![](./media/image64.png)

![](./media/image65.png)

![](./media/image66.png)

![](./media/image67.png)

![](./media/image68.png)

![](./media/image69.png)

![](./media/image70.png)

![](./media/image71.png)

![](./media/image72.png)

16. Seleccione **Home** para volver al lienzo de configuración y agregar
    el tipo de entidad **SaleEvent**.

![](./media/image73.png)

![](./media/image74.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image75.png)
>
> ![](./media/image76.png)
>
> ![](./media/image77.png)
>
> ![](./media/image78.png)
>
> ![](./media/image79.png)
>
> ![](./media/image80.png)
>
> ![](./media/image81.png)
>
> ![](./media/image82.png)
>
> ![](./media/image83.png)
>
> ![](./media/image84.png)

17. Cuando termine, verá estos tipos de entidad enumerados en el panel
    **Entity Types**.

![](./media/image85.png)

## Tarea 3: Crear tipos de relación

A continuación, cree tipos de relación entre los tipos de entidad para
representar las conexiones contextuales de los datos.

### SaleEvent desde Store

1.  En el Explorer, seleccione el tipo de entidad **SaleEvent**.

> ![](./media/image86.png)

2.  En la cinta de opciones, seleccione **Add relationship**.

> ![](./media/image87.png)

3.  Escriba los siguientes detalles del tipo de relación y seleccione
    **Add relationship type**.

- **Relationship type name**: +++from+++

- **Source entity type**: *SaleEvent*

- **Target entity type**: *Store*

> ![](./media/image88.png)
>
> ![](./media/image89.png)

4.  La relación se agregará al lienzo semántico. Selecciónela para abrir
    la configuración de los detalles de la relación. Observe las
    siguientes secciones de la página de configuración:

- **Origin entity type:** Muestra los detalles de la entidad de origen
  **(SaleEvent** en este caso**).**

- **Relationship type:** Permite configurar los detalles del tipo de
  relación.

- **Target entity type:** Muestra los detalles de la entidad de destino
  **(Store** en este caso**)**.

> ![](./media/image90.png)
>
> ![](./media/image91.png)

5.  En la sección central, especifique los siguientes detalles:

&nbsp;

1.  **Mapping table**: **Examine los orígenes disponibles** y seleccione
    la tabla **factsales.** Esta tabla de los datos de origen puede
    vincular las entidades Store y SaleEvent, ya que contiene
    información de identificación para ambos tipos de entidad. Cada fila
    de esta tabla hace referencia a un almacén y a un evento de venta
    mediante su ID.

> ![](./media/image92.png)

2.  **Matched SaleEvent: SaleId**: Seleccione **SaleId**. Esta
    configuración especifica la columna de la tabla de datos de origen
    de la relación cuyos valores coinciden con la propiedad de clave
    definida en la entidad *SaleEvent*. En este caso, el origen de datos
    de la relación y el origen de datos de la entidad utilizan la misma
    tabla, *factsales*, por lo que se selecciona la misma columna
    (SaleId).

3.  **Matched Store: StoreId**: Seleccione **StoreId**. Esta
    configuración especifica la columna de la tabla de datos de origen
    de la relación (*factsales \>* StoreId) cuyos valores coinciden con
    la propiedad de clave definida en la entidad *Store* (*dimstore* \>
    StoreId). En los datos de este tutorial, el nombre de la columna es
    el mismo (StoreId) en ambas tablas.

> ![](./media/image93.png)

**  
 Importante:** Asegúrese de seleccionar las columnas **Matched**
correctas que correspondan a las propiedades de clave de los tipos de
entidad.

6.  **Guarde** el tipo de relación. Confirme que el tipo de relación se
    haya actualizado correctamente y, a continuación, seleccione
    **Cancel** para cerrar las opciones de configuración.

> ![](./media/image94.png)
>
> ![](./media/image95.png)
>
> ![](./media/image96.png)
>
> Ahora se ha creado la primera relación y se ha vinculado a los datos
> de la tabla de origen. Continúe con la siguiente sección para crear
> otro tipo de relación.

### **SaleEvent sold Products**

1.  Seleccione **Home** para volver al lienzo de configuración, donde
    podrá agregar nuevos tipos de relación.

![](./media/image97.png)

2.  Siga los mismos pasos que utilizó para el primer tipo de relación
    para crear un segundo tipo de relación desde el tipo de entidad
    **SaleEvent**, con los detalles descritos en la siguiente tabla.

[TABLE]

![](./media/image98.png)

![](./media/image99.png)

![](./media/image100.png)

![](./media/image101.png)

![](./media/image102.png)

![](./media/image103.png)

![](./media/image104.png)

# Ejercicio 3: Enriquecer la Ontology con datos adicionales

En este ejercicio, enriquecerá la Ontology agregando un nuevo tipo de
entidad ***Freezer***. Este tipo de entidad incorpora más contexto del
dominio e introduce propiedades para datos de series temporales, que
reflejan información operativa en tiempo real.

** Nota**

Tanto para datos estáticos como para datos de series temporales, puede
crear propiedades sin vincular datos y vincularlos posteriormente, o
crear las propiedades y vincular los datos a ellas en un solo paso. En
este artículo se muestran ambos enfoques.

Por último, creará un nuevo tipo de relación para representar la
conexión entre una tienda y sus congeladores.

## Tarea 1: Crear el tipo de entidad Freezer y agregar propiedades

Siga estos pasos para crear el tipo de entidad *Freezer* y agregarle
propiedades. Las propiedades aún no estarán vinculadas a los datos.

1.  Seleccione **Add entity type** en la cinta de opciones superior.
    Escriba +++**Freezer*+++*** como nombre del tipo de entidad y
    seleccione **Add Entity Type.**

> ![](./media/image105.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image106.png)

2.  Con el tipo de entidad **Freezer** seleccionado en el **Explorer**,
    seleccione **View entity type details** en la cinta de opciones
    superior.

> ![](./media/image107.png)

3.  Se abrirá la página **Configure** con los detalles del tipo de
    entidad. En esta página se muestra información importante sobre el
    tipo de entidad, incluidas sus propiedades y los vínculos con los
    datos.

Expanda **Manage property bindings** y seleccione **Add properties**.

![](./media/image108.png)

4.  Agregue las siguientes propiedades y seleccione **Save**.

[TABLE]

![](./media/image109.png)

**Nota:** Los nombres de las propiedades deben ser únicos en todos los
tipos de entidad.

![](./media/image110.png)

5.  Las propiedades se agregan a la página **Configure**, sin estar
    vinculadas a ningún origen de datos.

> ![](./media/image111.png)

## Tarea 2: Vincular datos estáticos a las propiedades

A continuación, vincule datos estáticos a las propiedades que creó para
el tipo de entidad *Freezer*.

1.  Expanda **Manage property bindings** y seleccione **Add binding and
    properties**.

> ![](./media/image112.png)

2.  Seleccione **Add data binding \> Lakehouse table**.

> ![](./media/image113.png)

3.  Elija el origen de datos.

- Seleccione el Lakehouse **IQ_Lakehouse** y, a continuación, seleccione
  **Next**.

- Seleccione la tabla **freezer** y seleccione **Select**.

> ![](./media/image114.png)
>
> ![](./media/image115.png)

4.  Los campos de la tabla de origen se incorporan a la configuración
    del vínculo de datos. Observe las secciones de la página de
    configuración:

- **Entity type key:** Identifica el campo (o los campos) que sirve como
  clave única para cada registro de los datos ingeridos.

- **Binding selection:** Identifica la tabla de origen que contiene los
  datos del vínculo.

- **Entity type key mapping:** Identifica la(s) columna(s) de la tabla
  de datos de origen que se asignan a la propiedad de clave del tipo de
  entidad. Puede seleccionar columnas de tipo cadena e integer de los
  datos de origen como clave del tipo de entidad. En conjunto, las
  columnas seleccionadas identifican de forma única un registro.

- **Properties**: Enumera las columnas de los datos de origen y las
  propiedades correspondientes del tipo de entidad **Freezer**. El lado
  **Source** se completa automáticamente con las columnas de la tabla
  **freezer**, y el lado **Property name** muestra los nombres de las
  propiedades correspondientes del tipo de entidad **Freezer** dentro de
  la **Ontology**. En este tutorial, mantenga los nombres de propiedad
  predeterminados.

![](./media/image116.png)

5.  Seleccione **Define entity type key** en la parte superior de la
    configuración. Seleccione **FreezerId** en la lista de propiedades
    y, a continuación, seleccione **Save**.

![](./media/image117.png)

![](./media/image118.png)

6.  **Guarde** el vínculo de datos. Confirme que el tipo de entidad se
    haya actualizado correctamente y, a continuación, seleccione
    **Cancel** para cerrar las opciones de configuración.

![](./media/image119.png)

![](./media/image120.png)

## Tarea 3: Vincular datos de series temporales a propiedades adicionales

A continuación, agregue datos de series temporales a la entidad
**Freezer**, creando nuevas propiedades y vinculando los datos de series
temporales a ellas en una única operación de vínculo de datos.

1.  En la página **Configure**, expanda **Manage property bindings** y
    seleccione nuevamente **Add binding and properties** para volver a
    abrir la configuración del vínculo.

![](./media/image121.png)

2.  En **Binding selection**, expanda **Add data binding** y seleccione
    **Eventhouse table or materialized view**.

![](./media/image122.png)

3.  Elija el origen de datos.

    1.  Seleccione el Eventhouse **TelemetryDataEH** y, a continuación,
        seleccione **Add**.

![](./media/image123.png)

2.  Seleccione la tabla **FreezerTelemetry** y, a continuación,
    seleccione **Add**.

![](./media/image124.png)

4.  En la configuración aparecerá una sección **Timeseries data**. En
    **Timestamp column**, seleccione **timestamp**.

![](./media/image125.png)

5.  Desplácese hasta la sección **Properties**, donde **StoreId**
    muestra un error porque ya está vinculado en el enlace de datos
    estáticos. Utilice el icono de la papelera para eliminar la
    propiedad duplicada.

![](./media/image126.png)

6.  **Guarde** el vínculo de datos. Confirme que el tipo de entidad se
    haya actualizado correctamente y, a continuación, seleccione
    **Cancel** para cerrar las opciones de configuración.

![](./media/image127.png)

![](./media/image128.png)

7.  De vuelta en la página **Configure** de *Freezer*, observe que ahora
    hay más propiedades del tipo de entidad y que las nuevas están
    vinculadas al origen de datos *FreezerTelemetry*.

![](./media/image129.png)

Ahora la entidad *Freezer* tiene dos vínculos de datos: uno con datos
estáticos de la tabla *freezer* del Lakehouse y otro con datos de
streaming de la tabla *FreezerTelemetry* del Eventhouse.

## Tarea 4: Agregar un tipo de relación

Por último, cree un nuevo tipo de relación para representar la conexión
entre una tienda y sus congeladores.

**Create Store operates Freezer**

1.  En la página **Configure**, expanda **Manage relationships** y
    seleccione **Add new relationship.**

![](./media/image130.png)

2.  Especifique los siguientes detalles del tipo de relación y
    seleccione **Add relationship type**.

    1.  **Relationship type name**: *operates*

    2.  **Source entity type**: *Store*

    3.  **Target entity type**: *Freezer*

![](./media/image131.png)

3.  La relación se agrega a la sección **Relationships**. Seleccione la
    relación **operates** en el lienzo para abrir la configuración de
    los detalles de la relación. Observe las secciones de la página de
    configuración:

- **Origin entity type:** Enumera los detalles de la entidad de origen
  **(Store** en este caso**).**

- **Relationship type:** Establece los detalles del tipo de relación.

- **Target entity type:** Enumera los detalles de la entidad de destino
  **(Freezer** en este caso**)**.

![](./media/image132.png)

![](./media/image133.png)

4.  En la sección central, especifique los siguientes detalles.

- **Mapping table:** Esta tabla de los datos de origen puede vincular
  las entidades *Store* y *Freezer*, ya que contiene información de
  identificación para ambos tipos de entidad. Cada fila de esta tabla
  hace referencia a una **Store** y a un Freezer mediante su ID.

- **Matched Store:** **StoreId:** Seleccione **StoreId.** Esta
  configuración especifica la columna de la tabla de datos de origen de
  la relación (*freezer* \> StoreId) cuyos valores coinciden con la
  propiedad clave definida en la entidad Store (*dimstore* \> StoreId).
  En los datos del tutorial, el nombre de la columna es el mismo
  (StoreId) en ambas tablas.

- **Matched Freezer: FreezerId:** Seleccione **FreezerId**. Esta
  configuración especifica la columna de la tabla de datos de origen de
  la relación cuyos valores coinciden con la propiedad clave definida en
  la entidad *Freezer*. En este caso, el origen de datos de la relación
  y el origen de datos de la entidad utilizan la misma tabla
  (*freezer*), por lo que se selecciona la misma columna (*FreezerId*).

![](./media/image134.png)

**  
 Importante:** Asegúrese de seleccionar las columnas de origen correctas
que coincidan con las propiedades clave del tipo de entidad.

5.  **Guarde** el tipo de relación. Confirme que el tipo de relación se
    haya actualizado correctamente y, a continuación, seleccione
    **Cancel** para cerrar las opciones de configuración.

![](./media/image135.png)

![](./media/image136.png)

6.  Verá la página **Configure** de la entidad, donde la relación
    actualizada permanece visible en la sección **Relationships**.

![](./media/image137.png)

# Ejercicio 4: **Ver la Ontology**

En este ejercicio, explorará la Ontology mediante la experiencia de
vista previa. Inspeccionará instancias de entidad que crean instancias
de los tipos de entidad con datos y explorará el contexto con forma de
grafo entre los datos de ventas y los datos de streaming de
dispositivos.

## Tarea 1: **Ver la lista de instancias y los datos estáticos**

Cuando vinculó datos a los tipos de entidad en los pasos anteriores del
tutorial, Ontology creó automáticamente instancias de esas entidades
asociadas a las filas de los datos de origen. En esta sección, utilizará
la experiencia de vista previa para ver esas instancias de entidad.

1.  Comience en el lienzo de configuración **Home** de **Ontology**.
    Seleccione el tipo de entidad **SaleEvent** y, en la cinta superior,
    seleccione **View Entity Type details.**

![](./media/image138.png)

2.  Abra la pestaña **Instances**. Compruebe que muestra seis instancias
    de entidad con datos rellenados a partir de la tabla **factsales**
    del Lakehouse, como ingresos y cantidades de unidades.

![](./media/image139.png)

## Tarea 2: Ver datos de series temporales

1.  En la esquina superior izquierda de la página, utilice el selector
    situado junto al nombre del tipo de entidad para cambiar al tipo de
    entidad **Freezer**.

![](./media/image140.png)

2.  Abra la pestaña **Overview**. La pestaña se carga con gráficos
    vacíos, porque el intervalo de tiempo predeterminado **Last 30
    days** no incluye ningún dato.

![](./media/image141.png)

3.  Actualice el intervalo de tiempo, reemplazando el valor
    predeterminado **Last 30 days** por un intervalo de fechas
    personalizado que comience el **viernes 1 de agosto de 2025 a las
    12:00 AM**, finalice el **lunes 4 de agosto de 2025 a las 12:00 AM**
    y tenga una **Time granularity** de **5 minutos**.

![](./media/image142.png)

4.  Observe los datos de series temporales que ahora son visibles para
    varias instancias de la entidad **Freezer** dentro del intervalo de
    tiempo que seleccionó.

![](./media/image143.png)

## Tarea 3: **Ver el gráfico de Ontology**

La pestaña **Overview** también contiene un gráfico **Relationship
graph**, que se utiliza para visualizar la ontología mediante un gráfico
de nodos y aristas.

1.  Utilice el selector de tipo de entidad para cambiar al tipo de
    entidad **SaleEvent**. En el mosaico **Relationship graph**,
    seleccione **Expand**.

> ![](./media/image144.png)

2.  Se abre la vista expandida del gráfico. Observe los detalles de las
    relaciones desde el tipo de entidad **SaleEvent** hacia **Products**
    y **Store**.

![](./media/image145.png)

3.  Utilice el selector de tipo de entidad para cambiar al tipo de
    entidad Store. Expanda su gráfico **Relationship graph**.

![](./media/image146.png)

4.  En el gráfico, observe las relaciones que **Store** tiene con
    **Freezer** y **SaleEvent**. Después, seleccione **Run query** en la
    cinta del generador de consultas. Esta acción ejecuta la consulta
    predeterminada y muestra un gráfico de instancias de entidad junto
    con sus conexiones.

![](./media/image147.png)

![](./media/image148.png)

![](./media/image149.png)

## Tarea 4: Consultar instancias del gráfico

En la vista del gráfico de relaciones, puede consultar Ontology para
buscar instancias de entidad que cumplan determinados criterios. Utilice
los filtros de **Query builder** en la cinta superior para crear
consultas.

![](./media/image150.png)

Primero, cree esta consulta: *Show all freezers that are operated in the
Paris store.*

1.  En el gráfico de relaciones de la entidad *Store*, seleccione **Add
    filter** \> **Store** \> **StoreId** en la cinta de **Query
    builder**. Configure el filtro con **StoreId = S-PAR-01**. Este
    valor corresponde al identificador de la tienda de *París*.

![](./media/image151.png)

![](./media/image152.png)

5.  En la sección **Components**, desactive *SaleEvent* para que los
    únicos campos seleccionados sean **Nodes** \> **Store**, **Nodes**
    \> **Freezer** y **Edges** \> **operates**.

![](./media/image153.png)

6.  Seleccione **Run query** y compruebe que el gráfico de instancias
    muestra dos freezers conectados a la tienda de *París*.

![](./media/image154.png)

![](./media/image155.png)

7.  Seleccione **Clear query** para borrar los resultados de la
    consulta.

![](./media/image156.png)

A continuación, cree esta consulta: *Show all stores that have made a
sale with a revenue greater than 150.*

8.  Seleccione **Add a node** y agregue un nodo para **SaleEvent.**

> ![](./media/image157.png)

9.  En la sección **Components**, seleccione las casillas situadas junto
    a **Nodes** \> **Store** y **Edges** \> **from** para agregarlos al
    gráfico.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image158.png)

10. En la cinta de **Query builder**, seleccione **Add filter** \>
    **SaleEvent** \> **RevenueUSD**. Configure el filtro con
    +++**RevenueUSD \> 150+++**.

![](./media/image159.png)

![](./media/image160.png)

11. Seleccione **Run query** y compruebe que el gráfico de instancias
    muestra dos tiendas que cumplen el filtro aplicado a los eventos de
    venta conectados. También puede seleccionar los nodos del gráfico
    para obtener detalles de los eventos de venta específicos.

![](./media/image161.png)

![](./media/image162.png)

Este proceso permite inspeccionar las rutas que conectan los problemas
operativos (como el aumento de la temperatura de los freezers en
determinadas tiendas) con los resultados del negocio (las ventas).

# Ejercicio 5: **Usar Ontology (preview) desde agentes**

Ontology (preview) se integra con [Fabric data agent
(preview)](https://learn.microsoft.com/en-us/fabric/data-science/concept-data-agent) para
permitirle formular preguntas en lenguaje natural y obtener respuestas
basadas en las definiciones y asociaciones de Ontology.

## Tarea 1: Crear un data agent con Ontology (preview) como origen

Siga estos pasos para crear un nuevo data agent que se conecte al
elemento Ontology (preview).

1.  Ahora, seleccione **Fabric IQ Ontology XX** en el panel de
    navegación izquierdo.

![](./media/image163.png)

2.  En la página principal de **Fabric**, seleccione **+New item**. En
    el cuadro de búsqueda **Filter by item type**, escriba +++**data
    agent**+++ y seleccione **Data agent**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image164.png)

3.  Escriba +++**RetailOntologyAgent**+++ como nombre del **Data agent**
    y seleccione **Create**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image165.png)

4.  En la página de **RetailOntologyAgent**, seleccione **Add a data
    source**.

> ![](./media/image166.png)

5.  En la pestaña **OneLake catalog,** seleccione la ontología
    **RetailSalesOntology** y, a continuación, seleccione **Add**.

> ![](./media/image167.png)
>
> Cuando el agente esté listo, se abrirá.
>
> ![](./media/image168.png)

## Tarea 2: Proporcionar instrucciones al agente

** Nota:** Este paso se agrega como respuesta a un problema conocido que
afecta a la agregación en las consultas**.**

> A continuación, agregue una instrucción personalizada al agente.

1.  Seleccione **Agent instructions** en la cinta.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image169.png)

2.  En la parte inferior del cuadro de entrada, agregue +++**Support
    group by in GQL**+++. Esta instrucción permite una mejor agregación
    de los datos de **Ontology.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image170.png)

3.  La instrucción se aplica automáticamente. Si lo desea, cierre la
    pestaña **Agent instructions**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image171.png)

## Tarea 3: Consultar el agente mediante lenguaje natural

> A continuación, explore Ontology mediante preguntas en lenguaje
> natural.

1.  Escriba el siguiente texto y seleccione el **icono Submit**, como se
    muestra en la siguiente imagen.

> **+++For each store, show any freezers operated by that store that
> ever had a humidity lower than 46 percent.+++**
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image172.png)

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image173.png)

2.  Escriba el siguiente texto y seleccione el **icono Submit**, como se
    muestra en la siguiente imagen.

> *+++What is the top product by revenue across all stores?+++*

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image174.png)

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image175.png)

> Observe que las respuestas hacen referencia a los tipos de entidad
> (*Store, Products, Freezer*) y a las relaciones entre ellos, no solo a
> las tablas sin procesar.
>
> ![Screenshot of the result of a query.](./media/image176.png)
>
> ** Sugerencia:** Si al ejecutar las consultas de ejemplo aparecen
> errores que indican que no hay datos, espere unos minutos para dar más
> tiempo al agente para inicializarse. Después, vuelva a ejecutar las
> consultas.
>
> Continúe explorando el data agent probando algunas consultas propias.

## Tarea 4: Eliminar recursos

1.  En el menú de navegación izquierdo, seleccione su espacio de
    trabajo, **Fabric IQ OntologyXX**. Se abrirá la vista de elementos
    del espacio de trabajo.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image177.png)

2.  Seleccione el botón de los tres puntos (...) situado debajo del
    nombre del espacio de trabajo y, a continuación, seleccione
    **Workspace settings**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image178.png)

3.  Desplácese hasta la parte inferior de la pestaña **General** y
    seleccione **Remove this workspace**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image179.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image180.png)

**Resumen**

Este caso de uso demuestra cómo Microsoft Fabric IQ Ontology (preview)
puede utilizarse para crear un modelo de datos semántico e
interconectado que representa conceptos empresariales del mundo real y
sus relaciones. Al combinar datos estructurados de Lakehouse con datos
de telemetría de streaming, la ontología proporciona una vista unificada
y orientada al negocio de los datos empresariales.

Mediante definiciones de entidades, enlaces de datos y modelado de
relaciones, los usuarios pueden analizar cómo las señales operativas,
como la temperatura o la humedad de los congeladores, se relacionan con
resultados empresariales como las ventas y los ingresos. El caso de uso
también muestra cómo las ontologías permiten la exploración de grafos y
las consultas en lenguaje natural mediante Fabric Data Agents, lo que
facilita la obtención de insights más profundos sin que los usuarios
necesiten comprender las tablas o los esquemas subyacentes.

En conjunto, este caso de uso muestra cómo Fabric IQ Ontology ayuda a
conectar los datos operativos con el análisis de datos, lo que favorece
una toma de decisiones más inteligente en distintos dominios.
