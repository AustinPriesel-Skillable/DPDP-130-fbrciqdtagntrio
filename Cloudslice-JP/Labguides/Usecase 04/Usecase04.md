# ユースケース04 - Fabric Data AgentをMicrosoft Foundryに接続して、統合されたインテリジェントデータ分析を実現

### 紹介

現代の組織は複数のシステムにわたって大量のデータを生成するため、ビジネスユーザーやアナリストが迅速にインサイトを得ることが困難になっています。データはしばしばサイロ化された状態で保存されるため、情報の抽出、分析、解釈には専門的な技術知識が必要となります。

Microsoft Fabric統合データプラットフォームは、分析、データエンジニアリング、ビジネスインテリジェンスの機能を単一の環境に統合することで、この課題に対応します。Azure AI FoundryのエージェントベースのAI機能を統合することで、組織は自然言語と自動化されたワークフローを使用してエンタープライズデータと連携するインテリジェントなアプリケーションを構築できます。

「Unified Data Foundation Solution Accelerator」向けエージェント型アプリケーション・ソリューション・アクセラレーターは、AI搭載エージェントが統合エンタープライズを活用し、質問への回答、データ分析の自動化、そして技術系・非技術系双方のユーザーへのインサイト提供を行う仕組みを実証するものです。これらのエージェント型アプリケーションは、タスクの調整、関連データの取得、文脈に応じた回答の生成を行うことで、迅速な意思決定と業務効率の向上を実現します。

このユースケースでは、企業はAIエージェントを使用して、売上実績、顧客行動、製品トレンドなどのビジネスデータを分析できます。複数のデータセットを手動で照会する代わりに、ユーザーは自然言語で質問するだけで、システムから直接実用的なインサイトを受け取ることができます。

### 目的

このユースケースの目的は、組織が**統合データ基盤と連携したエージェント型AIを活用して**、データへのアクセスと意思決定を改善する方法を示すことです。主な目標は以下のとおりです。

### Microsoft Fabric に統合データ基盤の確立

- Lakehouse、Warehouse、およびセマンティックモデルを使用して、管理されたFabricワークスペースを作成します。
- 分析のために企業データセットをロードして検証します。


### 2. Fabric データエージェントの構築と構成

- 自然言語を使用してデータセットを照会できる**Fabric Data
  Agent**を作成します。

- Ontologyリソースを接続し、企業固有のクエリをサポートするエージェント指示を定義します。


### 3. AzureおよびFoundryコンポーネントのデプロイ

- Foundryプロジェクト、AIサービス、検索、ストレージ、アプリサービスなど、Azureリソースをプロビジョニングします。
- Azure Developer CLI ( azd )
  を使用してサポートコンポーネントをデプロイします。


### 4. Fabric Data Agent を Microsoft Foundry に接続

- Foundry内でAIエージェントを作成または構成します。
- Workspace IDとAI Skills IDを使用して、エージェントをMicrosoft
  Fabricにリンクします。

- エージェントがFabricデータを解釈および分析できるように、ドメイン固有の指示を提供します。


### 5. 会話型分析と自動インサイトを実現

- Foundry
  Playgroundで、実際のビジネスクエリを使ってエージェントをテストします。

- Fabric
  Lakehouseのデータセットを使用し、自然言語からデータ取得までのワークフローを実演します。

- 検査の合格率／不合格率、平均値、傾向、グループ化された要約などの洞察を提供します。


### 6. エンドツーエンドのエージェント型アプリケーションワークフローを紹介

- Foundryエージェント、Fabricデータソース、およびAzureインフラストラクチャを統合して、機能的なWebアプリケーションを構築します。
- インテリジェントデータインタラクション、自動推論、およびインサイト生成を検証します。


### ソリューションアーキテクチャ

![Architecture Diagram](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image1.png)

このソリューションは、Microsoft FabricとMicrosoft Foundryを組み合わせることで、構造化データと非構造化ドキュメントの両方を使用して質問に答えることができるAIソリューションを実現します。

- **Microsoft
  Fabricは**、Lakehouse、Warehouse、および自然言語からSQLへの変換を行うFabric IQセマンティックレイヤーを備えたデータレイヤーを提供します。

- **Microsoft Foundryは**、文書検索用のFoundry
  IQや、両方の機能をオーケストレーションするOrchestrator Agentなど、AIエージェントをホストしています。

- **Azure AI Services は**、言語モデル (GPT-4o-mini)
  と埋め込み機能を提供します。

- **Azure AI
  Searchは、**セマンティック検索のためにドキュメントベクトルを保存します。


### 前提条件

- **GitHubアカウント：GitHubのログイン認証情報が必要です。  
  アカウントを持っていない場合は、以下のリンクから作成してください。 +++
  <https://github.com/signup?user_email=&source=form-home-signup+++>**


## タスク0：GitHubアカウントを作成

このラボで使用したテナント認証情報と同じものを使用して、新しい**GitHubアカウントを作成します。**

1. <https://github.com/+++>を使用して GitHub にアクセスし、 **\[Sign
    up\]**をクリックします。

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image2.png)

1. それでは、新しいGitHubアカウントを作成するために、**メールアドレス**、**パスワード**、および固有の**ユーザー名**を入力し、 **「Continue」**ボタンをクリックします。

    ![A screenshot of a login box AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image3.png)

1. 画面の指示に従って、**検証用のパズル**を開始します。 **「Submit」**をクリックします。

1. メールで受け取った**検証コード**を入力します**。**

    ![A screenshot of a email form AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image4.png)

1. それでは、認証情報を使ってGitHubにサインインし、 **「Sign in」**をクリックします。

    ![A screenshot of a login page AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image5.png)

1. GitHubで新しいアカウントが正常に作成されました。

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image6.png)


## タスク 1: Fabric ワークスペースを作成

このタスクでは、Fabricワークスペースを作成します。このワークスペースには、 レイクハウス 、データフロー、Data Factoryパイプライン、ノートブック、Power BIデータセット、レポートなど、このレイクハウスチュートリアルに必要なすべてのアイテムが含まれています。

1. ブラウザを開き、アドレスバーに移動して、次のURLを入力または貼り付ける：+++
    <https://app.fabric.microsoft.com/+++>
    。**Enter**キーを押して、資格情報でサインインします。

    |   |   |
    |---|---|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | TAP | +++@lab.CloudPortalCredential(User1).AccessToken+++ |

1. Fabricのホームページで、 **「+** **New workspace**」タイルを選択します。

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image7.png)

1. 右側に表示される**「Create a workspace」ペイン**に、以下の詳細を入力し、 **「Apply」**ボタンをクリックします。

    | Property | Value |
    |---------|-------|
    | Name | +++Fabric agent@lab.LabInstance.Id+++  |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | Small dataset storage format |
    | Template apps | Check **Develop template apps** |

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image8.png)

    > \[!注\]
    > ラボのインスタントIDを確認するには、「Help」を選択してインスタントIDをコピーします。

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image9.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image10.png)

1. デプロイが完了するまでお待ちください。完了まで2～3分かかります。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image11.png)


## タスク2：Fabric  Workspace IDを取得

ソリューションを構築する際には、 Workspace IDをパラメータとして渡す必要があります。

1. URLを確認－ Workspace IDは、/groups/の後に現れるGUIDです。

1. URL（例：https://app.fabric.microsoft.com/groups/{workspace-id}/...）から **Workspace ID**をコピーし、後で使用するために**メモ帳に保存します。**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image12.png)


## タスク3：オープンな開発環境

1. ブラウザを開き、アドレスバーに移動して、次のURLを入力または貼り付ける：+++
    <https://github.com/technofocus-pte/agnticapp-for-unified-data/tree/main+++>

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image13.png)

1. **Fork**をクリックしてリポジトリをフォークします。リポジトリに一意の名前を付けて、 **「Create repo」**ボタンをクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image14.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image15.png)

1. **Code -\> Codespaces -\> Create Codespace on main**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image16.png)

1. Codespaces環境のセットアップが完了するまでお待ちください。セットアップには数分かかります。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image17.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image18.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image19.png)


## タスク4：サービスをプロビジョニングし、アプリケーションをAzureとFabricにデプロイ

1. ターミナルで以下のコマンドを実行してください。すると、コピーするコードが生成されます。そのコードをコピーしてEnterキーを押してください。

    `azd auth login`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image20.png)

1. デフォルトのブラウザが開き、生成されたコードを入力して認証を行います。コードを入力し、 **「Next」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image21.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image22.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image23.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image24.png)

1. Azureにログインする：

    `az login`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image25.png)

1. デフォルトのブラウザが開き、生成されたコードを入力して認証を行います。コードを入力し、 **「Next」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image26.png)

1. Azure サブスクリプションを選択してください

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image27.png)

1. Microsoft Cognitive Services リソース プロバイダーを登録します (サブスクリプションに既に登録されていない場合は必須です)。

    `az provider register --namespace Microsoft.CognitiveServices`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image28.png)

    >[!Alert] 左側の**Infra**フォルダーに移動し**、 main.bicep**ファイルの**122 行目**を開いて、 *Lab Instance ID文字列を*[+++@ mailto: +++@lab.labinstance.idlab.LabInstance.Id mailto: +++@lab.LabInstance.Id](mailto:@lab.LabInstance.Id)に変更します。

1. すべてのリソースをプロビジョニングしてデプロイ。

    `azd up`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image29.png)

1. 以下の値を選択してください。

    - **Azure リソースの環境を作成するには**、 +++env@lab.LabInstance.Id+++を入力します。
    - **使用するAzureサブスクリプションを選択**: **@lab.CloudSubscription.Name**
    - **「Location」インフラストラクチャパラメータ：** **ResourceGroup1 Locationを選択します**
    - **リソースグループ:** **@lab.CloudResourceGroup(ResourceGroup1).Name**


### 注：選択したAzureリージョンでCodespaceのデプロイが失敗した場合は、デプロイリージョンを更新してデプロイを再実行します。

azd env set AZURE_RESOURCE_LOCATION *{region}*

例：

azd env set AZURE_RESOURCE_LOCATION westus2

対応地域：

    - westus2
    - japaneast
    - swedencentral
    - northeurope リージョンを更新した後、デプロイメント手順を再度実行します


![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image30.png)

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image31.png)

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image32.png)

1. アカウントにリソースをプロビジョニングし、サンプルデータを使用してソリューションを設定するのに**7～10***分*かかります。

1. これで展開が完了しました

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image33.png)

1. 仮想環境を作成してアクティブ化します。

    `python -m venv . venv`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image34.png)

1. **Visual Studio Code**の左上の**メニューアイコンを使用して**、 **\[Terminal → New Terminal** **\]**に移動すると、ワークスペースに新しいターミナル ウィンドウが開きます。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image35.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image36.png)

1. ターミナルで以下のコマンドを実行して、必要なPythonの依存関係をインストールします。

    `pip install uv && uv pip install -r scripts/requirements.txt`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image37.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image38.png)

1. ターミナルで以下のコマンドを実行してください。すると、コピーするコードが生成されます。そのコードをコピーしてEnterキーを押してください。

    `az login`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image39.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image40.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image41.png)

1. セットアッププロセスを続行するには、リストから**Azureサブスクリプション**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image42.png)

1. azdデプロイメントの出力からbashスクリプトを実行します。 \<Fabric Workspace ID \>を、前の手順で作成したFabric  Workspace IDに置き換えます。スクリプトは次の通り：

    `python scripts/00_build_solution.py --from 02 --fabric-workspace-id <your-workspace-id>`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image43.png)

1. リソースの作成を開始するには、Enterキーを押します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image44.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image45.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image46.png)


## タスク5：Fabric Lakehouseとデータを確認

1. +++https://app.fabric.microsoft.com/+++ワークスペースに移動します。

1. リソースが正常にデプロイされたことを確認します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image47.png)

1. データが正常に読み込まれたことを確認するには、**Lakehouse**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image48.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image49.png)

1. エージェントをテストするには、**コードスペース**に戻ります。


## タスク6：エージェントのテスト

1. エージェントをテストするには、ターミナルで次のコマンドを実行します。

    `python scripts/08_test_agent.py`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image50.png)

1. サンプル質問を入力します +++What is the average score from inspections?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image51.png)

    +++What constitutes a failed inspection?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image52.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image53.png)

    +++Do any inspections violate quality control standards in our Inspection Procedures?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image54.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image55.png)

1. 処理をキャンセルするには、 **Ctrl+C**を押します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image56.png)


## タスク7：FabricData Agentを作成

1. Microsoft Fabricワークスペース[ +++https://app.fabric.microsoft.com/+++ にアクセスします。

1. 「New item」を選択 → 「Data Agent」を検索 → Data Agentを選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image57.png)

1. 名前[+++FabricDataAgent@lab.LabInstance.Id+++ を入力し、 **Create**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image58.png)

1. 新しいデータソースを設定するには、 **「Add data source** 」を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image59.png)

1. このワークショップで使用するOntologyリソースを選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image60.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image61.png)

1. 上部メニューから**「Agent instructions** **」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image62.png)

1. 以下のエージェント指示を追加します。

    +++You are a helpful assistant that can answer user questions using data. Support group by in GQL+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image63.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image64.png)

1. 上部メニューから「Publish」をクリックし、「Publish」を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image65.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image66.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image67.png)

    \[!注\] Ontologyの設定には最大15分かかる場合がありますので、良好な応答が見られない場合はしばらくしてから再度お試しください。

1. エージェントをテストするには、アプリケーションを実行し、サンプル質問を入力して応答を確認します。

    +++How many tickets are high priority+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image68.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image69.png)

    +++What is the average score from inspections?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image70.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image71.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image72.png)

    `Show tickets grouped by status`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image73.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image74.png)

1. ** Workspace ID**と**AISkills ID**をメモ帳に保存して、後で使用します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image75.png)

1. **Codespace**に戻り、アプリケーションをデプロイして起動します。


## タスク8：アプリケーションのデプロイと起動

1. デプロイ前に、以下のコマンドを実行して環境変数**AZURE_ENV_DEPLOY_APPをtrue**に設定します。

    `azd env set AZURE_ENV_DEPLOY_APP true`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image76.png)

1. azd up を実行- これにより Azureリソースがプロビジョニングされます

    `azd up`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image77.png)

1. デプロイが正常に完了したら、WebアプリのURLをコピーします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image78.png)

1. アプリの権限を設定するには、次のコマンドを実行します。

    `python scripts/00_build_solution.py --from 09`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image79.png)

1. 設定を開始するには、Enterキーを押します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image80.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image81.png)

1. アプリのURLをクリックします

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image82.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image83.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image84.png)


### サンプル問題

アプリを使い始めるにあたって、以下にアプリ内で質問できる**サンプル質問**をいくつかご紹介します。

小売売上分析のユースケース：

`Show total revenue by year for last 5 years`.

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image85.png)

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image86.png)

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image87.png)

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image88.png)

\[!警告\] 日付が入力されているため、応答が表示されない場合があります。ラボの残りの部分を続行してください。

## タスク 9: Azureリソースの検証とFabric Lakehouseデータの確認

1. ブラウザを開き、+++
    +++https://portal.azure.com+++/+++にアクセスします。以下のクラウドスライスアカウントでサインインします。

1. **Resource groups**を選択します

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image89.png)

1. 割り当てられた**Resource groups**をクリックします。

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image90.png)

1. 以下のリソースが正常にデプロイされたことを確認します。

    - Foundry
    - Foundry project
    - Application Insights
    - Search service
    - Azure Storage account
    - App Service
    - Azure Cosmos DB account

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image91.png)


## タスク 10: Microsoft Foundry ServicesからFabric Data Agentを使用

1. **Foundry** を選択します。![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image92.png)

1. Overviewペインで、 **「Go to Foundry portal」**をクリックします。これにより、Microsoft Foundryポータルに移動します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image93.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image94.png)

    Foundry Portalにアクセスしたら、左側のメニューから**「Agents」**を選択します。**既に構築済み**のエージェントが表示されます。構築されていない場合は、 **「+ New agent」**をクリックして構築します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image95.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image96.png)

1. 新しく作成した**エージェント**を選択すると、右側に設定ペインが開きます。エージェント名を「+++Fabric
    Agent+++」と入力します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image97.png)

1. 同じエージェント設定ペインで、下にスクロールして**「Knowledge」**パラメーターの**「+** **Add」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image98.png)

1. **「Add knowledge」**ペインで**「Microsoft Fabric」**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image99.png)

1. **「+ Create connection」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image100.png)

1. **タスク 7 \> ステップ 6**で保存した**ワークスペース ID**や**AISkills ID**などのカスタムキーを入力します。接続名を**Fabric- aiskillsと指定し**、 **\[Connect\]** をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image101.png)

1. 手順を入力します。

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

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image102.png)

1. **「Agents」**を選択し、次に「**Fabric Agent」**を選択します**。** エージェントをクリックし、 **「Try in playground」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image103.png)

1. チャットパネルが開きますので、そこに質問を入力してください。エージェントは、接続したドキュメントとデータセットに基づいて応答します。

    サンプルプロンプト -

    +++What constitutes a failed inspection?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image104.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image105.png)

    +++What is the total number of tickets in the system?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image106.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image107.png)

    +++Do any inspections violate quality control standards in our Inspection Procedures?+++


## タスク11：リソースを削除

1. 削除するには、 Azure ポータル検索バーに**「Resource groups」**と入力し、移動して**「Services」**の下にある**「Resource groups」**をクリックします。

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image108.png)

1. Resource groupsページで、リソースグループを選択します。

1. **Resource Group**のホームページで、**Fabric Capacity**以外のすべてのリソースを選択し、 **\[Delete\]**をクリックします。![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image109.png)

1. 右側に表示される**「Delete Resources** **」ペイン**で、 **「Enter "delete" to confirm deletion」+++フィールドに+++「Delete」**ボタンをクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image110.png)  
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image111.png)

1. Microsoft Fabricワークスペース[（https://app.fabric.microsoft.com/）](https://app.fabric.microsoft.com/+++)にアクセスします。

1. ワークスペース名の下にある「 **...」**オプションを選択し、 **「Workspace settings」**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image112.png)

1. **「General** **」**を選択し、 **「Remove this workspace」**を選択します。

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image113.png)

1. 表示された警告メッセージで**「Delete** **」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image114.png)

1. 次のラボに進む前に、ワークスペースが削除されたという通知が表示されるまでお待ちください。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2004/media/image115.png)


### まとめ

このユースケースでは、**Microsoft Fabric**と**Microsoft Foundryを**統合することで、組織が**インテリジェントエージェント駆動型データアプリケーションを**構築する方法を示します。このソリューションにより、Fabric LakehouseとWarehouseに保存されている企業データにAI搭載エージェントを通じてアクセスし、分析できる**統合データ基盤が**構築されます。

**Fabric Data Agentを**Foundryに接続することで、ユーザーは複雑なSQL文を記述したり、複数のデータソースを手動で分析したりする代わりに、**自然言語クエリ**を使用してエンタープライズデータセットとやり取りできます。AIエージェントは関連データを取得し、分析を実行して、平均値、傾向、概要、グループ化された結果などのインサイトを生成します。

このソリューションは、AIサービス、検索、ストレージ、Webアプリケーションなど、Azureのサポートサービスも提供し、**エンドツーエンドのエージェント型アプリケーションアーキテクチャを実現します。**これにより、組織は**構造化されたエンタープライズデータとAI機能**を組み合わせ、対話型分析と自動化されたインサイトを提供できるようになります。

全体として、このユースケースは、統合データプラットフォーム上に構築された**エージェント型AIアプリケーション**が、データアクセスを簡素化し、分析を加速させ、技術者と非技術者の両方にとってより迅速なデータ主導型の意思決定を支援できることを示している。
