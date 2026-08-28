## ユースケース03 - Microsoft Fabricでミラーリングされた Azure SQL Databaseを使用してFabric Data Agentを構築

### 紹介

現代の組織では、複雑なデータ移動を必要とせずに、オペレーショナルデータを迅速に分析し、有益な洞察を提供できるインテリジェントなシステムが求められています。このユースケースでは、Microsoft Fabricを使用してAzure SQL DatabaseからFabric環境にデータをミラーリングし、ミラーリングされたデータをクエリおよび分析できるFabric Data Agentを作成します。

このプロセスは、サンプルビジネスデータを含む Azure SQL Databaseを作成することから始まります。次に、このデータベースはAzure SQL Mirroringを使用してMicrosoft Fabricにミラーリングされ、Fabricワークスペース内のオペレーショナルデータに凖リアルタイムでアクセスできるようになります。ミラーリングされたデータベースが利用可能になったら、Fabric Data Agentが構成され、データソースに接続して自然言語クエリに応答します。

このアプローチにより、ユーザーはインテリジェントエージェントを介して企業データとやり取りできるようになり、複雑なSQLクエリを作成することなく、製品のパフォーマンス、顧客分布、販売動向に関するより迅速な洞察を得ることができます。

### 目的

このラボの目的は、 Azure SQL Database からミラーリングされたオペレーショナルデータを分析できる Fabric Data Agent を構築および構成する方法を示すことです。

この実習を完了することで、以下のことを学ぶことができます。

- **Azure SQL Database**を作成します。
- データおよび分析リソースをホストするための**Microsoft
  Fabricワークスペース**を作成します。

- **Azure SQL Mirroringを使用して、 Azure SQL Databaseを Microsoft
  Fabric に**ミラーリングします。

- **Fabric Data
  Agent**を設定し、ミラーリングされたデータベースに接続します。

- **自然言語のプロンプト**を使用してデータをクエリし、洞察を生成します。
- サンプル分析質問を用いて、エージェントの回答を検証します。


## **タスク0：ホスト環境の時刻を同期**

1. VM内で、**検索バーに移動してクリックし**、 **「Settings」と入力して、 「Best match」**の下にある**「Settings」**をクリックします。

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image1.png)

1. **Settingsウィンドウで「Time & language」**をクリックします。

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image2.png)

1. **「Time & language」ページ**で、**「Date & time」**に移動してクリックします。

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image3.png)

1. **「Additional settings」**セクションに移動し、 **「Syn now」**ボタンをクリックします。同期には3～5分かかります。

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image4.png)

1. **Settingsウィンドウ**を閉じます。

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image5.png)


## タスク 1: 単一データベースの作成 - Azure SQL Database

を含む完全に構成済みの Azure SQL Databaseを作成します。AdventureWorksLTサンプルスキーマをデプロイし、テーブルを確認し、後でFabricでミラーリングするためにサーバー接続の詳細を準備します。

1. ブラウザを開き、+++https://portal.azure.com+++ にアクセスして、以下のクラウドスライスアカウントでサインインします。

    |   |   |
    |---|---|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | TAP | +++@lab.CloudPortalCredential(User1).AccessToken+++ |

1. Azure ポータルのホーム ページから、 Microsoft Azure コマンド バーの左側にある3本の横線で表される**Azure ポータルメニュー**をクリックします。SQL データベースを選択します。

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image6.png)

1. **「+Create」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image7.png)

1. **「Create a storage account** **」**ウィンドウで、 **「Basics** **」**タブの下に以下の詳細を入力してストレージアカウントを作成し、 **「Next:Networking」**をクリックします。

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

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image8.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image9.png)

1. 「Compute + Storage」セクションで、 **「Configure database」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image10.png)

1. Service tier のドロップダウンメニューから**「Standard(Budget Friendly)」を選択し、DTU**に100を入力して**「Apply」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image11.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image12.png)

1. **「Networking」タブ**で**「Public endpoint」**を選択し、 **「Allow Azure services and resources」を「Yes」**に設定し、 **「Add current client IP address」**を有効にして、 **「Next: Security\>」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image13.png)

1. **Security**ページで、内容を確認後、 **「Next : Additional settings」を選択します。**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image14.png)

1. **「Additional settings」**タブで  「Use existing data」*の下にある**「Sample」を選択し**、プロンプトが表示されたら**「AdventureWorksLT」**を選択して**「OK」**をクリックし、次に**「Review + create」を選択して**続行します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image15.png)

1. **「Review + create」ページ**で、レビュー後、 **「Create」を選択します。**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image16.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image17.png)

1. **Microsoft.SQLDatabase**ウィンドウで、デプロイが完了したら、 **\[Go to resource\]**ボタンをクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image18.png)

1. SQLデータベースページで**Query editorを選択します**。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image19.png)

1. **Query editor (preview)**で、SQL Server の**ログイン名**として**sqladmin** 、**パスワード**として +++
    **password 321! ++**を入力し、
    **\[OK\]**をクリックしてデータベースに接続します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image20.png)

1. すべてのサンプルテーブルが正常にデプロイされていることを確認します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image21.png)

1. SQLデータベースに戻ります。**Server name** （1）と**SQL Database name+++（2）をコピーし、メモ帳に貼り付けてから、メモ帳を+++保存して、**次のタスクでその情報を使用します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image22.png)

1. **Home**をクリックしてメインページに戻ります。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image23.png)

1. **Resource groups**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image24.png)

1. **ResourceGroup1リソースグループ**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image25.png)

1. **SQLサーバー**を選択します

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image26.png)

1. 「Identity」に移動し、「System assigned managed identity」の状態を**「On」**に切り替えてから、 **「Save」**をクリックして変更を適用します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image27.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image28.png)


## タスク2：Fabricワークスペースを作成

このタスクでは、Fabricワークスペースを作成します。このワークスペースには、 レイクハウス、データフロー、Data Factoryパイプライン、ノートブック、Power BIデータセット、レポートなど、このレイクハウスチュートリアルに必要なすべてのアイテムが含まれています。

1. ブラウザを開き、アドレスバーに移動して、次のURLを入力または貼り付けます。 ++https://app.fabric.microsoft.com/+++。
    **Enter**キーを押して、資格情報でサインインします。

    |  |   |
    |---|----|
    |Username	|+++@lab.CloudPortalCredential(User1).Username+++|
    |TAP	|+++@lab.CloudPortalCredential(User1).AccessToken+++|

1. Fabricのホームページで、 **「+** **New workspace**」タイルを選択します。

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image29.png)

1. 右側に表示される**「Create a workspace」**ペインに、以下の詳細を入力し、 **「Apply」**ボタンをクリックします。

    | Property | Value |
    |---------|-------|
    | Name | +++FabricAgent-mirroringdatabase@lab.LabInstance.Id+++ |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | Small dataset storage format |
    | Template apps | Check **Develop template apps** |

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image30.png)

    注：ラボのインスタントIDを確認するには、「Help」を選択してインスタントIDをコピーします。

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image31.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image32.png)

1. デプロイが完了するまでお待ちください。完了まで2～3分かかります。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image33.png)


## タスク 3: Azure SQL Mirroringを使用してデータをミラーリングするソリューションを構築構築

このタスクでは、Azure SQL Mirroringを使用して Azure SQL Databaseを Microsoft Fabric に接続します。テーブルを選択し、ミラーリングされたデータベースを作成し、データが正常に同期されたことを確認します。

1. ナビゲーションバーの**「+** **New item」**ボタンをクリックして、新しいレイクハウスを作成します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image34.png)

1. **Filter by keyword**検索ボックスに**「+++Mirrored Azure SQL
    Database+++」**と入力し、「 **Mirrored Azure SQL
    Database」**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image35.png)

1. **「Choose a database connection to get started」**ウィンドウで、 **Azure SQL Database**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image36.png)

1. 接続設定タブで以下の詳細を入力し、「Connect」ボタンをクリックします。

    | Field | Value |
    |------|-------|
    | Server | SQL server URL saved in **Task 2 → Step 15** |
    | Database | +++sqldatabase@lab.LabInstance.Id+++ |
    | Username | +++sqladmin+++ |
    | Password | +++password321!+++ |

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image37.png)

1. **「Choose data」ウィンドウ**で**「Select all」**を選択し、 **「Connect」**ボタンをクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image38.png)

1. 「Destination」タブで、 **「Create mirrored database」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image39.png)

1. **「Refresh」**をクリックして、最新の変更内容を確認します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image40.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image41.png)

1. 左側のナビゲーションメニューで、下の画像に示すように、 ***「FabricAgent- +++mirroringdatabase@lab.LabInstance.Id+++ 」*に移動します。**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image42.png)


## タスク4： Data agent を作成し、ミラーリングされたデータベースに接続

ここでは、新しいFabric Data Agentを作成し、ミラーリングされたAzure SQL Databaseをデータソースとして使用するように構成します。このエージェントは、ミラーリングされたデータを使用して自然言語によるプロンプトに応答します。

1. **Fabric**のホームページで、 **「+** **New item」**を選択します**。**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image43.png)

1. **「Filter by item type」検索ボックス+++に+++「+++data
    agent+++」**と入力し、「**Data agent」を選択します。**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image44.png)

1. Data agent名として**+++FabricDataAgent @lab.LabInstance.Id+++を**入力し、 **\[Create\]**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image45.png)

1. **「Add data source」**を選択して、新しいデータソースを設定します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image46.png)

1. このワークショップで使用するミラーリングされたデータベースリソースを選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image47.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image48.png)


## タスク5：エージェントのテスト

Data Agent をテストするには、次のような分析的な質問をします。

- *どの製品カテゴリーが最も高い売上を生み出していますか。*
- *定価は高いが販売量が少ない商品の詳細を提供します。*
- *顧客数が最も多い都市はどこですか。*


これにより、エージェントがビジネス上の問い合わせを理解し、対応する能力を 証明します。

1. すべてのテーブルに対して**SalesLT**スキーマを選択します。

1. 「+++Which product categories generate the highest sales?+++」という質問を入力し、Sendアイコンをクリックしてエージェントの応答を表示します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image49.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image50.png)

1. エージェントをテストするには、アプリケーションを実行し、サンプル質問を入力して応答を確認します。

    ++++List products with high list price but low sales volume.+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image51.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image52.png)

    +++List the cities with the highest number of customers+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image53.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image54.png)

1. 上部メニューから**「Agent instructions」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image55.png)

1. 上部メニューから「**Publish**」をクリックし、 **「Publish」**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image56.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image57.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image58.png)

1. 次に、左側のナビゲーションペインにある**「FabricAgent - +++mirroringdatabase@lab.LabInstance.Id+++ 」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image59.png)


## タスク6：リソースを削除

1. ワークスペース名の下にある「 **...」**オプションを選択し、 **「Workspace settings」**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image60.png)

1. **「General」**を選択し、 **「Remove this workspace」**を選択します。

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image61.png)

1. 表示された警告メッセージで**「Delete」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image62.png)

1. 次のラボに進む前に、ワークスペースが削除されたという通知が表示されるまでお待ちください。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image63.png)

1. ブラウザを開き、+++https://portal.azure.com+++ にアクセスして、以下のクラウドスライスアカウントでサインインします。

1. リソースを削除するには、 Azure ポータル検索バーに**「Resource groups」**と入力し、**「Services」**の下にある**「Resource groups」**をクリックします。

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image64.png)

1. Resource groupsページで、リソースグループを選択します。

1. **Resource Group**のホームページで、**Fabric Capacity**以外のすべてのリソースを選択し、 **\[Delete\]**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image65.png)

1. 右側に表示される**「Delete Resources」**ペインで、 **「Enter “delete” to confirm deletion」+++フィールドに移動し、「+++Delete」**ボタンをクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image66.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2003/media/image67.png)


### まとめ

このラボでは、 Azure SQL Databaseを正常に作成し、Azure SQL Mirroringを使用してそのデータを Microsoft Fabric にミラーリングしました。次に、Fabric Data Agent を構成してミラーリングされたデータベースに接続し、自然言語クエリを使用してデータを分析しました。

このエージェントは、売れ行きの良い製品カテゴリ、高価格ながら販売数が少ない製品、顧客数が最も多い都市の特定といった、分析的な質問に回答することができました。これは、Microsoft Fabricがいかにしてオペレーショナルデータソースとインテリジェントエージェントを統合し、データ探索を簡素化して、より迅速なビジネスインサイトの獲得を可能にするかを示しています。

このユースケースは、Microsoft Fabricエコシステム内で、**データミラーリングとAI搭載 Data Agent**を組み合わせることで、対話型かつインテリジェントデータ体験を実現する力を示しています**。**
