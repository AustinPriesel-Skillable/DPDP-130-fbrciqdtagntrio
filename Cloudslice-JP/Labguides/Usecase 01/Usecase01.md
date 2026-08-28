> ユースケース1 - セマンティクスからインサイトへ：Fabric IQ
> Ontology(オントロジー)とFabric Data Agentsの活用
>
> 紹介
>
> 最新のデータプラットフォームでは、企業は多くの場合、多様なデータソースと分析モデル間で意味を統一する、**ビジネス中心のセマンティックレイヤー**を必要とします。Microsoft
> Fabric IQ の*Ontology (プレビュー)* 能を使用すると、企業の概念
> (*Store、Products、SaleEvent*)
> とその**関係**を定義し、これらの定義をLakehouse、セマンティックモデル、イベントストリーム全体にわたる実際のデータにバインドすることで、このレイヤーを構築できます。
>
> このシナリオでは、複数の場所でアイスクリームを販売する架空の会社、**Lakeshore
> Retail**
> を取り上げます。サンプルデータを使用して、チュートリアルでは環境の設定方法と、店舗、製品、販売イベントなどのビジネス概念を捉えるオントロジーの構築方法を説明します。また、ストリーミングデータ（Eventhouse
> からの冷凍庫の温度など）をこれらの概念に接続することで、オントロジーがドメイン横断的な推論やクエリをサポートできるようにします。例えば、「*冷凍庫の温度が
> -18 °C
> を超えると、どの店舗のアイスクリームの売上が減少するか。*」といったクエリを実行できます。
>
> 目的

- Lakehouse、Eventhouse、Ontology（プレビュー版）などの必要なサービスを含む、Microsoft
  Fabricワークスペースの準備

- Store, Products,
  SaleEventとFreezerなどのコアエンティティタイプを定義することで、ビジネス中心のオントロジーの構築

- OneLakeテーブルから静的データ、Eventhouseから時系列データを取得し、  
  オントロジーエンティティに格納

- 実際のビジネスプロセスを表すために、エンティティ間に意味のある関係を構築（たとえば、StoreにはSaleEventがあり、StoreはFreezerを運用）。
- エンティティインスタンス、関係グラフ、クエリビルダーフィルターによる、  
  オントロジーの探索と検証

- オントロジーをFabric Data
  Agentと統合することで、自然言語によるクエリの有効化（プレビュー版）


# 演習1：環境設定

## タスク 1: Fabric ワークスペースの構築

> このタスクでは、Fabricワークスペースを構築します。このワークスペースには、Lakehouseチュートリアルに必要なすべてのアイテムが含まれています。具体的には、Lakehouse本体、データフロー、データファクトリパイプライン、ノートブック、Power
> BI  
> データセットとレポートです。

1. ブラウザを開き、アドレスバーに移動して、次のURLを入力または貼り付けます: +++https://app.fabric.microsoft.com/+++。 次に**Enter**キーを押して、資格情報を使用してサインイン

    | Credential | Value |
    |------------|-------|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | Password | +++@lab.CloudPortalCredential(User1).Password+++ |

    > 2\. Workspacesペインで、**+New workspace** タイルをクリック
    >
    > 3\. 右側に表示される**Create a
    > workspace** ペインに、以下の詳細を入力し、**Apply** ボタンをクリック

    | Setting | Value |
    |----------|----------|
    | Name | +++Fabric IQ Ontology@lab.LabInstance.Id+++|
    | Advanced | Under **License mode**, select **Fabric capacity** |
    | Default storage format | **Small dataset storage format** |


## タスク2：レイクハウスの構築

1. ナビゲーションバーの**+New item** ボタンをクリックして、新しいレイクハウスを構築

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image1.png)

1. 絞り込んで+++Lakehouse+++ タイルを選択

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image2.png)

1. **New lakehouse** ダイアログボックスで、**Name** フィールドに+++IQ_Lakehouse+++と入力し、レイクハウススキーマの選択を解除します。**Create**ボタンをクリックして、新しいレイクハウスを開く

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image3.png)
      
    ![A screenshot of a computer AI-generated content may be incorrect.](./media/image4.png)

1. **Successfully created SQL endpoint**という通知が表示

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image5.png)

## タスク3：サンプルデータの取り込み

1. **IQ_Lakehouse**ページで、**Get data in your lakehouse** セクションに移動し、下の画像に示すように**Upload files**をクリック

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image6.png)

1. Upload filesタブで、Filesの下にあるフォルダーをクリック

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image7.png)

1. VM 上の **C:\LabFiles\LabFiles** に移動し、**DimProducts.csv、DimStore.csv、FactSale.csv、Freezer.csv** ファイルを選択して、**Open** ボタンをクリック

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image8.png)

1. 次に、**Upload** ボタンをクリックし、ダイアログの**Upload files** ダイアログの「×」 アイコンを選択して閉じる

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image9.png)

    ![A screenshot of a upload box AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image10.png)

1. **Files**をクリックして「更新」を選択すると、ファイルが表示

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image11.png)

1. **Lakehouse** ページで、Explorerラー ペインの下にある**Files**を選択します。次に、マウスカーソルを **DimProducts.csv** ファイルの上に移動します。DimProducts.csv の横にある横三点リーダー (…) をクリックします。**Load Table**をクリックしてから、**New table**を選択

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image12.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image13.png)

1. **Load file to new table** ダイアログボックスで、**Load** ボタンをクリック

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image14.png)

1. **DimProducts**テーブルの作成に成功

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image15.png)

1. データをプレビューするには、**DimProducts** テーブルを選択

    注: データをプレビューするには、\[更新\] ボタンを複数回選択する必要がある場合がある

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image16.png)

1. 手順7～9を繰り返して、残りのファイルをテーブルにプッシュ

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image17.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image18.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image19.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image20.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image21.png)

   ![A screenshot of a computer AI-generated content may be incorrect.](./media/image22.png)

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image23.png)

1. 左側のナビゲーションバーから**Fabric IQ Ontology**を選択

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image24.png)


## タスク4：Eventhouseの準備

以下の手順に従って、デバイスのストリーミングデータファイルをEventhouseのKQLデータベースにアップロードします。

1. **Fabric IQ Ontology**のホームページで、**+New item** を選択し、**Eventhouse**を選択

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image25.png)

1. Eventhouse に +++TelemetryDataEH+++ という名前を付けて、**Create** ボタンをクリック

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image26.png)

1. Eventhouseは準備が整い次第開く

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image27.png)

1. KQLデータベースの名前を選択して開く

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image28.png)

1. **KQLデータベース**の下部リボンで、**Get data**をクリックし、**Local file**を選択して、ローカルシステムからデータベースにファイルをアップロード

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image29.png)

1. データを新しいテーブルに取り込むためのターゲットオプションを選択し、+ New tableをクリックして、テーブル名として +++FreezerTelemetry+++ と入力

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image30.png)
  
    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image31.png)

1. アップロード先のテーブルを選択し、ファイルをドラッグアンドドロップするか、*Browse for files*をクリックしてデータをアップロード

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image32.png)

1. VM 上の **C:\LabFiles\Lab1** に移動し、**FreezerTelemetry.csv** ファイルを選択して、**Open** ボタンをクリック

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image33.png)

1. **Next**ボタンをクリック

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image34.png)

1. 次に、**Finish** ボタンをクリック

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image35.png)

1. データ取り込みが完了された上、**Close**をクリック

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image36.png)

1. 処理が完了すると、KQLデータベースに**FreezerTelemetry**テーブルが表示

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image37.png)

1. 左側のナビゲーションペインで**Fabric IQ Ontology**を選択

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image38.png)


# 演習2： OneLakeからオントロジーを構築

## タスク 1: オントロジー (プレビュー) アイテムの作成

1. Fabricワークスペースで、**+ New item**を選択します。**Ontology (preview)**アイテムを検索して選択
   
    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image39.png)

1. オントロジーの**Name**フィールド に+++RetailSalesOntology+++と入力し、**Create**を選択  

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image40.png)

    ヒント**：オントロジー名には、数字、文字、アンダースコアを含めることができます。スペースやハイフンは使用しないでください。

1. オントロジーは準備が整い次第開く

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image41.png)

    次に、Lakehouseテーブルのデータに基づいて、エンティティタイプ、データバインディングとリレーションシップを作成します。


## タスク2：エンティティ型とデータバインディングを作成

まず、エンティティ型を作成します。エンティティ型は、ビジネスにおけるオブジェクトの種類を表します。このステップでは、*Store*, *Products*,と*SaleEvent*の3つのエンティティ型を作成します。エンティティタイプを作成したら、**IQ_Lakehouse**
レイクハウステーブルのソースデータ列をバインドして、それらのプロパティを作成します。

### 最初のエンティティ型（Store）の追加

1. 上部のリボンまたは構成キャンバスの中央から、**Add entity type**を選択

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image42.png)

1. エンティティ型名に+++ **Store** +++と入力し、**Add Entity Type**を選択

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image43.png)

1. *Store* エンティティ型が構成キャンバスに追加され、**Entity type configuration** ペインが表示

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image44.png)

1. 設定キャンバスで、エンティティ名の横にある「...」を選択し、**Bind data**を選択

      ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image45.png)

1. **Add data binding \> Lakehouse table**を選択

      ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image46.png)

1. 次に、データソースを選択します。IQ_Lakehouse レイクハウスを選択し、「**Next**」をクリック

      ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image47.png)

1. **dimstore** テーブルを選択し、**Select**をクリック

      ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image48.png)

1. ソーステーブルのフィールドがデータバインディング構成に反映されます。構成ページの以下のセクションを確認：

    - **エンティティ型キー**：取り込まれたデータの各レコードを一意に識別するために使用できるフィールド（または複数のフィールド）を指定
    - **バインディングの選択**：バインディングのデータを保持するソーステーブルを  
    識別

    - **エンティティ型のキーマッピング**：ソースデータテーブル内で、エンティティ型キープロパティに対応する列を指定します。ソースデータから文字列型と整数型の列をエンティティ型キーとして選択できま。選択した列を組み合わせることで、レコードを一意に識別可能
    - **プロパティ**：ソースデータから、*Store* エンティティ型のプロパティとして表現される列を一覧表示します。**Source
    column** 側には*dimstore* テーブルの列が自動的に入力され、**Property name** 側にはオントロジー内の*Store* エンティティ型における対応するプロパティ名が一覧表示されます。このチュートリアルでは、既定のプロパティ名をそのまま使用
    
      ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image49.png)


1. 設定画面上部の**Define entity type key** を選択

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image50.png)

1. プロパティリストから**StoreId**を選択し、**Save**を選択

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image51.png)

1. データバインディングを保存

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image52.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image53.png)

1. エンティティ型が正常に更新されたことを確認したら、**Cancel** を選択して設定オプションを閉じる

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image54.png)

1. エンティティ型の詳細情報の**Configure** ページが表示されます。このページには、エンティティ型のプロパティやデータバインディングなど、重要な情報が表示されます。設定済みのデータバインディングを確認

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image55.png)

1. **Home**を選択し、設定キャンバスに戻り、新しいエンティティ型を追加

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image56.png)


### その他のエンティティ型（Products, SaleEvent）の追加

1. **Store **エンティティ型を作成した時と同じ手順に従って、以下の表に記載されているエンティティ型を作成してください。各エンティティには、ソーステーブルのデフォルト列との静的データバインディングが設定

    | Entity Type Name | Source Table in IQ_Lakehouse | Entity Type Key |
    |------------------|------------------------------|-----------------|
    | +++Products+++<br><br>**Note:** Use the plural form **Products** to avoid conflict with the GQL reserved word **PRODUCT**. | **dimproducts** | **ProductId** |
    | +++SaleEvent+++ | **factsales** | **SaleId** |

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image57.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image58.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image59.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image60.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image61.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image62.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image63.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image64.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image65.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image66.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image67.png)

1. **Home**を選択して設定キャンバスに戻り、**SaleEvent**エンティティ型を追加

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image68.png)

1. 完了すると、これらのエンティティ型が**Entity Types** ペインに一覧表示

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image69.png)


## タスク3：関係タイプを作成

> 次に、データ内の文脈的なつながりを表すために、エンティティ型間の関係型を作成します。

# StoreからのSaleEvent

1. エクスプローラーから**SaleEvent**エンティティタイプを選択

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image70.png)

1. メニューリボンから**Add relationship** を選択

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image71.png)

1. 以下の関係タイプの詳細を入力し、**Add relationship type**を選択

    - **Relationship type name**: +++from+++
    - **Source entity type**: *SaleEvent*
    - **Target entity type**: *Store*

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image72.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image73.png)


1. 関係がセマンティックキャンバスに追加されました。それを選択して、関係の詳細設定を開きます。設定ページの各セクションを確認：

    - **Origin entity type**:
    発生元エンティティ（この場合は**SaleEvent**）の詳細を一覧表示

    - **Relationship type：**類関係タイプの詳細を設定
    - **Target entity type**:
    対象エンティティ（この場合は**Store **）の詳細を一覧表示

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image74.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image75.png)


1. 中央部分に、以下の詳細を入力してください。

1. **Mapping table：**利用可能なデータソースを参照し、**factsales** テーブルを選択しします。このソースデータ内のテーブルには、*Store* と*SaleEvent* の両方のエンティティを識別する情報が含まれているため、ストアとセールイベントの エンティティを関連付けることができます。このテーブルの各行は、ID によってストアとセールイベントを参照

1. **Matched SaleEvent**: 販売ID: SaleIdを選択します。この設定では、SaleEventエンティティで定義されたキープロパティと値が一致する、リレーションシップソースデータテーブルの列を指定します。この場合、リレーションシップデータソースとエンティティデータソースの両方がfactsalesテーブルを使用するため、同じ列（SaleId）を選択します。

1. **Matched Store**: **StoreId**: **StoreId**を選択します。この設定では、リレーションシップソースデータテーブル（*factsales* \> StoreId）内の列のうち、*Store*エンティティ（*dimstore* \> StoreId）で定義されたキープロパティと値が一致する列を指定します。チュートリアルデータでは、両方のテーブルで列名は同じ（StoreId）です。

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image76.png)

    重要：エンティティタイプのキープロパティに一致する、正しい一致列を選択してください。

1. 保存関係タイプを確認してください。関係タイプが正常に更新されたことを確認したら、**Cancel** を選択して設定オプションを閉じます。

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image77.png)  
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image78.png)  
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image79.png)

これで最初のリレーションシップが作成され、ソーステーブルのデータにバインドされました。次のセクションに進み、別のリレーションシップタイプを作成してください。


### **SaleEventで販売されたProducts**

1. **Home**を選択すると、新しいエンティティタイプを追加できる設定キャンバスに 戻る

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image80.png)
    
1. 最初の関係タイプで使用した手順と同じ手順に従って、次の表に記載されている 詳細を持つ **SaleEvent** エンティティタイプから 2 番目の関係を作成

    | Relationship Type Name | Origin Entity Type | Target Entity Type | Mapping Table | Matched SaleEvent: SaleId | Matched Products: ProductId |
    |------------------------|-------------------|-------------------|---------------|--------------------------|----------------------------|
    | `sold` | `SaleEvent` | `Products` | `factsales` | `SaleId` | `ProductId` |

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image81.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image82.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image83.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image84.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image85.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image86.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image87.png)


# 演習3：追加データでオントロジーを充実

この演習では、新しい*Freezer*エンティティタイプを追加することで、オントロジーを拡張します。このエンティティタイプは、より多くのドメインコンテキストを追加し、リアルタイムの運用情報を反映する時系列データのプロパティを導入します。

注記

静的データと時系列データの両方において、データをバインドせずにプロパティを作成し、後でデータをバインドすることも、プロパティを作成してデータをバインドする手順を一度に行うことも可能です。この記事では、両方の方法について説明します。

最後に、店舗とその冷凍庫間のつながりを表す新しい関係タイプを作成します。

## タスク 1:エンティティ タイプ「Freezer」を作成し、プロパティを追加

以下の手順に従って、*Freezer* エンティティタイプを作成し、プロパティを追加してください。プロパティはまだデータにバインドされていません。

1. 上部のリボンから**Add entity type** を選択します。エンティティタイプの名前として+++Freezer+++と入力し、**Add entity type** を選択

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image88.png)
  
    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image89.png)

1. **Explorer**でFreezerエンティティタイプを選択した状態で、上部のリボンから**View entity type details**を選択

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image90.png)

1. エンティティタイプの詳細情報を表示する**Configure** ページが開きます。このページには、エンティティタイプのプロパティやデータバインディングなど、重要な情報が表示

    **Manage property bindings** を展開し、**Add properties**を選択

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image91.png)

1. 以下のプロパティを追加して、**Save**を選択

    | Name | Property Type |
    |------|---------------|
    | `FreezerId` | `String` |
    | `Model` | `String` |
    | `minSafeTempC` | `Double` |
    | `StoreId` | `String` |

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image92.png)

    注記：プロパティ名は、すべてのエンティティタイプにおいて一意である必要があります。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image93.png)

1. どのデータソースにも紐付けられず、プロパティがConfigure ページに追加
   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image94.png)


## タスク2：静的データをプロパティにバインド

次に、*Freezer*エンティティ型に作成したプロパティに静的データをバインドします。

1. **Manage property bindings** を展開し、**Add binding and properties**を選択

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image95.png)

1. **Add data binding \> Lakehouse table**を選択

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image96.png)

1. データソースを選択

    - **IQ_Lakehouse**のレイクハウスを選択し、**Next**を選択
    - **freezer** テーブルを選択して、**Selectをクリック**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image97.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image98.png)


1. ソーステーブルのフィールドがデータバインディング構成に反映されます。構成ページの以下のセクションを確認

    - **Entity type
    key：**取り込まれたデータの各レコードを一意に識別するために使用できるフィールド（または複数のフィールド）を特定

    - **Binding
    selection：**バインディングのデータを保持するソーステーブルを特定

    - **Entity type key mapping**: ソースデータテーブル内で、エンティティタイプキープロパティに対応する列を指定します。ソースデータから文字列型と整数型の列をエンティティタイプキーとして選択できます。選択した列を組み合わせることで、レコードを一意に識別可能

    - **Properties**: ソースデータの列と、それに対応する**Freezer**エンティティタイプのプロパティを一覧表示します。**Source
    column** 側にはFreezerテーブルの列が自動的に入力され、**Property name** 側にはオントロジー内のFreezerエンティティタイプの対応するプロパティ名が表示されます。このチュートリアルでは、既定のプロパティ名を使用

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image99.png)


1. 設定画面上部の**Define entity type key** を選択します。プロパティリストからFreezerIdを選択し、**Save**を選択

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image100.png)
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image101.png)

1. データバインディングを**保存します**。エンティティタイプが正常に更新されたことを確認したら、**Cancel** を選択して設定オプションを閉じる

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image102.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image103.png)


## タスク3：時系列データを追加のプロパティにバインド

> 次に、新しいプロパティを作成し、それらに時系列データを単一のデータバインディング操作で紐付けることで、**Freezer**エンティティに時系列データを追加します。

1. **Configure** ページで、**Manage property bindings** を展開し、**Add binding and properties**を 再度選択して、バインディング構成を再度開く

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image104.png)

1. **Binding selection**で、**Add data binding**を展開し、**Eventhouse table or materialized view**を選択

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image105.png)

1. データソースを選択

    1)  **TelemetryDataEH**イベントハウスを選択し、**Add**を選択
         ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image106.png)

    3)  **FreezerTelemetry** テーブルを選択して**追加**
         ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image107.png)

1. 設定に**Timeseries data** セクションが表示されます。**Timestamp column**でtimestampを選択

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image108.png)

1. **Properties** セクションまでスクロールすると、**StoreId**にエラーが表示されます。これは、StoreIdが既に静的データバインディングにバインドされているためです。ゴミ箱アイコンを使用して、重複しているプロパティを削除

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image109.png)

1. データバインディングを**保存します。**エンティティタイプが正常に更新されたことを確認したら、**Cancel** を選択して設定オプションを閉じます。

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image110.png)  
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image111.png)

1. **Freezer** の**Configure** ページに戻ると、エンティティタイプのプロパティが増えていることに気づきます。新しいプロパティは *FreezerTelemetry* データソースに バインド

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image112.png)

    > これで、*Freezer*エンティティには2つのデータバインディングができました。1つは*freezer*
    > レイクハウステーブルからの静的データ、もう1つは*FreezerTelemetry*
    > eventhouseテーブルからのストリーミングデータです。


## タスク4：関係タイプを追加

最後に、店舗とその冷凍庫間のつながりを表す新しい関係タイプを作成します。

**Create StoreはFreezerを運用**

1. **Configure**ページで、Manage relationshipsを展開し、**Add new relationship**を選択

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image113.png)

1. 以下の関係タイプの詳細を入力し、**Add relationship type**を選択

    1. **Relationship type name**: *operates*

    1. **Source entity type**: *Store*

    1. **Target entity type**: *Freezer*

       ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image114.png)

1. 関係が「**Relationships** 」セクションに追加されます。キャンバス上で**operates** 関係を選択して、関係の詳細設定を開きます。設定ページの各セクションを確認：

    - **Origin entity type**:
    発信元エンティティ（この場合は*Store* ）の詳細を一覧表示

    - **Relationship type：**関係タイプの詳細を設定
    - **Target entity type**:
    対象エンティティ（この場合は*Freezer* ）の詳細を表示

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image115.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image116.png)


1. 中央部分に、以下の詳細を入力：

    - **Mapping
    table：freezer** テーブルを選択します。このソースデータ内のテーブルには、*Store* と*Freezer*の両方のエンティティを識別する情報が含まれているため、店舗と冷凍庫のエンティティを関連付けることができます。このテーブルの各行は、IDによって店舗と冷凍庫を参照

    - **Matched Store**: **StoreId**:
    **StoreId**を選択します。この設定では、リレーションシップソースデータテーブル*（freezer \> StoreId*）内の列のうち、Storeエンティティ*（dimstore \> StoreId）*で定義されたキープロパティと値が一致する列を指定します。チュートリアルデータでは、両方のテーブルで列名は同じ*（StoreId）*

    - **Matched Freezer**: **FreezerId**: **FreezerId**
    を選択します。この設定では、リレーションシップソースデータテーブル内で、*Freezer* エンティティで定義されたキープロパティと値が一致する列を指定します。この場合、リレーションシップデータソースとエンティティデータソースの両方が *freezer* テーブルを使用するため、同じ列 (FreezerId) を選択

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image117.png)

    重要：エンティティタイプのキープロパティに一致する正しいソース列を選択してください。


1. 関係タイプを保存します。関係タイプが正常に更新されたことを確認したら、**Cancel** を選択して設定オプションを閉じる

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image118.png)  
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image119.png)

1. エンティティの**Configure** ページが表示され、更新された関係が**Relationships** セクションに引き続き確認可能

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image120.png)


# 演習4：オントロジーを表示する

この演習では、プレビュー機能を使用してオントロジーを探索します。エンティティ型をデータでインスタンス化するエンティティインスタンスを調べ、売上データとデバイスストリーミングデータにわたるグラフ状のコンテキストを探索します。

## タスク 1: インスタンス一覧と静的データを表示する

前のチュートリアル手順でエンティティタイプにデータをした際、オントロジーはソースデータ行に関連付けられたエンティティのインスタンスを自動的に作成しました。このセクションでは、プレビュー機能を使用してこれらのエンティティインスタンスを表示します。

1. オントロジーのHome設定キャンバスから始めます。**SaleEvent**エンティティタイプを選択し、上部のリボンから**View Entity Type details**を選択

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image121.png)

1. **Instances** タブを開きます。収益や販売数量など、**factsales** レイクハウステーブルからデータが入力された 6 つのエンティティ インスタンスが表示されていることを確認

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image122.png)


## タスク2：時系列データの表示

1. ページ左上にある、エンティティタイプ名の横にあるセレクターを使用して、**Freezer**エンティティタイプに切り替える

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image123.png)

1. **Overview** タブを開きます。デフォルトの時間の範囲である**Last 30 days** にはデータが含まれていないため、タブには空のグラフが表示

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image124.png)

1. 時間の範囲をデフォルトの「**Last 30 days** 」から、開始日時を **Fri Aug 01 2025**、**12:00 AM** 、終了日時を **Mon Aug 04 2025、12:00 AM** 、**Time granularity**  を **5 minutes** とするカスタム期間に変更

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image125.png)

1. 選択した時間枠内で、複数の**Freezer**エンティティインスタンスから表示されるようになった時系列データを観察

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image126.png)


## タスク3：オントロジーグラフを表示する

**Overview** タブには、ノードとエッジのグラフでオントロジーを視覚化するために使用する**Relationship graph**も含まれています。

1. エンティティタイプセレクターを使用して、**SaleEvent**エンティティタイプに切り替えます。**Relationship graph**タイルで、**Expand**を選択
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image127.png)

1. グラフビューが展開されます。SaleEventエンティティタイプから**Products**および**Store**への関係の詳細を確認

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image128.png)

1. エンティティタイプセレクターを使用して、**Store **エンティティタイプに切り替えます。その**relationship graph**を展開（**Expand**）

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image129.png)

1. グラフ上で、**Store** と **Freezer** および **SaleEvent** の関係を確認してください。次に、クエリ ビルダー リボンで **Run query**を選択します。この操作により既定のクエリが実行され、エンティティ インスタンスとその接続のグラフが表示

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image130.png)
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image131.png)  
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image132.png)


## タスク4：グラフインスタンスのクエリ

関係グラフビューでは、特定の条件を満たすエンティティインスタンスをオントロジーからクエリできます。クエリを作成するには、上部リボンの**Query builder** フィルターを使用します。
  
  ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image133.png)

まず、*パリ店で稼働しているすべての冷凍庫を表示する*というクエリを作成

1. *Store*エンティティのリレーションシップグラフで、クエリビルダーリボンから**Add filter \> Store \> StoreId** を選択します。フィルターを**StoreId = S-PAR-01**に設定します。この値は、パリ店のストアIDです
   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image134.png)
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image135.png)

1. **Components** セクションで、*SaleEvent*のチェックを外し、チェックされているフィールドが**Nodes \> Store**、**Nodes \> Freezer**、**Edges \> operates**のみになるようにする

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image136.png)

1. **Run query** を選択し、インスタンスグラフに*パリ*店に接続された2台の冷凍庫が表示されていることを確認

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image137.png)
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image138.png)

1. クエリ結果をクリアするには、**Clear query** を選択  
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image139.png)

    次のクエリを構築：*売上高が150を超える販売を行ったすべての店舗を表示*

1. **Add a node** を選択し、**SaleEvent** のノードを追加

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image140.png)

1. **Components** セクションで、 **Nodes \> Store** と**Edges \> from** の横にあるチェックボックスをオンにして、それらをグラフに追加

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image141.png)

1. クエリビルダーリボンから、**Add filter \> SaleEvent \> RevenueUSD**を選択します。フィルターを +++**RevenueUSD \> 150+++**に設定

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image142.png)  
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image143.png)

1. **Run query** を選択し、インスタンスグラフに、関連する販売イベントのフィルタ条件を満たす2つの店舗が表示されていることを確認します。グラフ内のノードを選択して、特定の販売イベントの詳細を取得可能

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image144.png)  

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image145.png)

    このプロセスにより、業務上の問題（特定の店舗における冷凍庫の温度上昇など）とビジネス成果（売上）を結びつける経路を検証することができます。


# 演習5：エージェントからオントロジーを取得

Ontology（プレビュー）は[Fabric data agent (preview)] (https://learn.microsoft.com/en-us/fabric/data-science/concept-data-agent) と統合され、自然言語で質問を行い、Ontologyの定義やバインディングに基づいた回答を得ることができます。

## タスク 1: オントロジー (プレビュー) ソースを使用してデータ エージェントを作成

以下の手順に従って、オントロジー（プレビュー）アイテムに接続する新しいデータエージェントを作成

1. 次に、左側のナビゲーションペインにある**Fabric IQ Ontology XX** をクリック

   ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image146.png)

1. **Fabric**のホームページで、**New item**を選択します。 Filter by item type 検索ボックスに+++data agent+++と入力し、Data agentを選択

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image147.png)

1. データエージェント名として+++RetailOntologyAgent+++と入力し、**Create**を選択

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image148.png)

1. **RetailOntologyAgent**ページで、**Add a data source**を選択

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image149.png)

1. OneLakeカタログタブで、**RetailSalesOntology**オントロジーを選択し、**Add**を選択

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image150.png)

    エージェントの準備が整うと、開きます。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image151.png)


## タスク2：エージェントへの指示を提供

**注記**：この手順は、クエリにおける集計に影響を与える既知の問題に対応するために追加

> 次に、エージェントにカスタム指示を追加

1. メニューリボンから**Agent instructions** を選択  
  
    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image152.png)

1. 入力ボックスの下部に+++ **Support group by in GQL**
    +++を追加します。この指示により、オントロジーデータ全体にわたる集計精度が向上  

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image153.png)

1. 指示は自動的に適用されます。必要に応じて、**Agent instructions** タブを閉じる


## タスク3：自然言語によるエージェントにクエリを送信

> 次に、自然言語による質問を使って、オントロジーを探索します。

1. 以下のテキストを入力し、下の画像に示されている**送信**アイコンをクリック

    **+++For each store, show any freezers operated by that store that ever had a humidity lower than 46 percent.+++**
    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image154.png)
    
      ![A screenshot of a chat AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image155.png)

1. 以下のテキストを入力し、下の画像に示されている送信アイコンをクリック

    **+++What is the top product by revenue across all stores?+++**
      ![A screenshot of a chat AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image156.png)
    
    ![A screenshot of a chat AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image157.png)

    応答では、単なるテーブルではなく、エンティティタイプ(*Store*, *Products*, *Freezer*）とその関係性が参照されていることに注意してください。

     ![Screenshot of the result of a query.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image158.png)

    **ヒント**：サンプルクエリの実行中にデータがないというエラーが表示された場合は、エージェントの初期化に十分な時間を確保するため、数分待ってから再度クエリを実行してください。
    
    データエージェントをさらに詳しく調べるには、自分でいくつかのプロンプトを試してみてください。


## タスク4：リソースの削除

1. 左側のナビゲーションメニューからワークスペース**Fabric IQ OntologyXX**を選択します。ワークスペースアイテムビューが開

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image159.png)

1. ワークスペース名の下にある「…」オプションを選択し、**Workspace settings**を選択

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image160.png)

1. General タブの一番下まで移動し、**Remove this workspace**を選択

   ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image161.png)
  
    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2001/media/image162.png)

    まとめ
    
    このユースケースでは、Microsoft Fabric IQ
    Ontology（プレビュー版）を使用して、現実世界のビジネス概念とその関係性を表す、接続されたセマンティックデータモデルを作成する方法を示します。構造化されたレイクハウスデータとストリーミング診断データを組み合わせることで、このオントロジーは、企業データの統一された、ビジネスに適したビューを提供します。
    
    エンティティ定義、データバインディング、およびリレーションシップモデリングを通じて、ユーザーは冷凍庫の温度や湿度といった運用シグナルが、売上や収益などのビジネス成果にどのように関連しているかを分析できます。このユースケースでは、オントロジーがFabricデータエージェントを介したグラフ探索と自然言語クエリをどのように強化し、ユーザーが基となるテーブルやスキーマを理解する必要なく、より深い洞察を可能にするかも示されています。
    
    全体として、このユースケースは、Fabric IQ
    Ontologyが運用データと分析を結びつけ、様々な分野におけるよりスマートな意思決定をどのように支援するかを示しています。
