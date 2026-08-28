## ユースケース05 - Copilot Studioを使用して、Fabric Data AgentをMicrosoft Teamsと統合し、実用的なインサイトとエージェント間のコラボレーションを実現

### 紹介

今日の競争の激しいデジタル市場において、eコマース組織は顧客取引、商品カタログ、ウェブサイト上でのやり取り、決済システムなどから膨大な量のデータを生成しています。これらのデータから有益な洞察を引き出すことは、顧客体験の向上、業務の最適化、収益の増加に不可欠です。しかし、複数のソースから得られる大規模なデータセットを管理・分析するには、統合された分析プラットフォームがなければ複雑な作業となります。

**Zava**は、オンラインプラットフォームを通じて様々な消費財を販売し、毎日数千件の注文を処理しています。同社は、注文管理、顧客プロファイル、製品在庫、決済取引など、様々なシステムからデータを収集しています。事業の拡大に伴い、Zavaはこれらのデータを効率的に分析し、ビジネスチームにリアルタイムのインサイトを提供することにおいて課題に直面しています。

これらの課題に対処するため、 Zavaは**Microsoft Fabric**を使用した最新の分析ソリューションを導入しています。Fabricは、データエンジニアリング、データストレージ、データ変換、ビジネスインテリジェンス機能を単一の環境に統合する統一プラットフォームを提供します。Zavaは、生データと処理済みeコマースデータをFabric Lakehouseに保存することで、拡張性の高いデータ管理と分析を実現しています。

さらに、 Zavaは**Microsoft Fabric Data Agent**を活用して、データへのアクセス性とインサイト生成を強化しています。Fabric Data Agentを使用すると、ビジネスユーザーやアナリストは自然言語クエリを使用してエンタープライズデータとやり取りできます。レポートを手動で検索したり、複雑なクエリを作成したりする代わりに、ユーザーは次のような質問をするだけで済みます。

- 「今月、最も売れている商品は何ですか」
- 「どの地域が最も高い売上高を記録しましたか」
- 「過去四半期における顧客からの注文動向はどうなっていますか」


Fabric AgentはLakehouseから関連データを自動的に取得し、洞察を生成することで、チームがビジネスパフォーマンスを迅速に把握できるよう支援します。このインテリジェントな連携により、生産性が向上し、部門横断的な意思決定が迅速化されます。

このソリューションにより、 Zavaのビジネスユーザー、アナリスト、経営陣は、データを容易に探索し、重要業績評価指標を監視し、販売実績、顧客行動、製品需要に関するリアルタイムの洞察を得ることができます。Zavaは、高度分析とAIを活用したFabric Agentsを組み合わせることで、データ主導の成長と卓越した業務運営を支える、拡張性と知性を備えたEコマース分析プラットフォームを構築しています。

### 目的

- Eコマースのセマンティックモデルに接続する**Fabric Data
  Agent**を構築・構成します。

- **Fabric
  Lakehouse**内でデータを取り込み、モデル化し、セマンティックモデルを通じて公開します。

- **メタ‑プロンプト**とエージェントレベルの指示‑を使用して、エージェントの知能を向上します。
- Fabric Data Agentを**Copilot
  Studioに接続し**、複数‑エージェント間の通信を有効にします。

- Copilotエージェントを公開し、 **Microsoft
  Teams**に統合してリアルタイム分析を実現します。

- Teams内でビジネスインサイトを直接クエリすることで、エンドツーエンドのフローをテストします。‑


# 演習1：ファブリックデータエージェントの作成と構成

## この演習では、Microsoft Fabric の基盤となるコンポーネントを確立します。ワークスペースを作成し、 レイクハウスをセットアップし、サンプル CSV データセットを取り込み、セマンティックモデルを生成し、分析クエリに回答できる Fabric Data Agent を構成します。これにより、ラボの残りの部分全体で使用されるコアとなるデータ インテリジェンス レイヤーが提供されます。

## タスク 1: Fabricワークスペースを作成

このタスクでは、Fabricワークスペースを作成します。このワークスペースには、 レイクハウス、データフロー、Data Factoryパイプライン、ノートブック、Power BIデータセット、レポートなど、このレイクハウスチュートリアルに必要なすべてのアイテムが含まれています。

1. ブラウザを開き、アドレスバーに移動して、次のURLを入力または貼り付ける： ++https://app.fabric.microsoft.com/+++
    。**Enter**キーを押して、資格情報でサインインします。

    |   |   |
    |---|---|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | TAP | +++@lab.CloudPortalCredential(User1).AccessToken+++ |

1. Fabricのホームページで、 **「+ New workspace** 」タイルを選択します。

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image1.png)

1. 右側に表示される**「Create a workspace」ペイン**に、以下の詳細を入力し、 **「Apply」**ボタンをクリックします。

    | Property | Value |
    |---------|-------|
    | Name | +++Fabric-Copilot-@lab.LabInstance.Id+++  |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | Small dataset storage format |
    | Template apps | Check **Develop template apps** |

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image2.png)

    注：ラボのインスタントIDを確認するには、「Help」を選択してインスタントIDをコピーします。

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image3.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image4.png)

1. デプロイが完了するまでお待ちください。完了まで2～3分かかります。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image5.png)


## タスク2：レイクハウスを作成し、サンプルデータを取り込む

レイクハウスを設定し、 NYC Taxi のサンプルデータと追加のCSVファイルを取り込みます。これにより、Fabric内に生データセットの基盤が構築され、後で変換やクエリを開始できるようになります。

1. ナビゲーションバーの**「+** **New item」**ボタンをクリックして、新しいレイクハウスを作成します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image6.png)

1. **「Filter by item type」検索ボックス**に+++Lakehouse+++と入力し、レイクハウスのアイテムを選択します。

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image7.png)

1. **New lakehouse**ダイアログボックスで、\[**Name** \]フィールドに+++
    **fabricagent_lakehouse** +++と入力し、
    **\[Create**\]ボタンをクリックして新しいレイクハウスを開きます。

    **注: fabricagent_lakehouse**の前のスペースを削除してください。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image8.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image9.png)

1. **「Successfully created SQL endpoint」**という通知が表示されるまでお待ちください。

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image10.png)

1. **レイクハウス**ページで、 **「Get data in your lakehouse」**セクションに移動し、 **「Upload files」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image11.png)

1. 「Upload files」タブで、「Files」の下にあるフォルダーをクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image12.png)

1. VM 上の**C:\LabFiles**フォルダを参照し、 **customers.csv、Orders_Data.csv** 、 **products.csv**ファイルを選択して**、 \[Open\]**ボタンをクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image13.png)

1. 次に、**Upload**ボタンをクリックして閉じます。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image14.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image15.png)

1. **「Files」**をクリックして「更新」を選択すると、ファイルが表示されます。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image16.png)

1. **Lakehouse**ページで、Explorerペインの下にあるFilesを選択します。次に、マウスカーソルを「Orders_Data.csv」ファイルに合わせます。「Orders_Data.csv」の横にある横方向の省略記号（…）をクリックします。「**Load Table」**に移動し、クリックします。次に**「New table」**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image17.png)

1. **「Load file to new table** **」**ダイアログボックスで、「**Load** **」**ボタンをクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image18.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image19.png)

1. **customers.csv**と**products.csv**についても同様の手順を繰り返して、テーブル形式に変換します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image20.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image21.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image22.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image23.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image24.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image25.png)

1. 画面右上の**Lakehouse**ドロップダウンメニューから、 **SQL analytics endpoint**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image26.png)

1. レイクハウスから **Home**タブで**「New semantic model」**を選択し、セマンティックモデルに追加するテーブルを選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image27.png)

1. **New semantic model**ダイアログで+++ **E-commerce Order Dataset**
    +++と入力し、テーブルのリストからすべてのテーブルを選択して「**Confirm**」をクリックすると、新しいモデルが作成されます。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image28.png)

1. 左側のメニューから、 +++Fabric-Copilot-@lab.LabInstance.Id+++ワークスペースアイコンを選択し、次にワークスペース名を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image29.png)


## タスク3：Fabricデータエージェントを作成

1. +++Fabric-Copilot-@lab.LabInstance.Id+++ワークスペースページで、移動して**「+New item」**ボタンをクリックし、次に**「Data agent」**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image30.png)

1. <DataAgent_@lab.LabInstance.Id>という名前を入力し、 **「Create」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image31.png)

1. 新しいデータソースを設定するには、 **「Add data source」**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image32.png)

1. 検索結果から**「E-commerce Order Dataset** （タイプ：セマンティックモデル）」を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image33.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image34.png)

1. リストされたテーブルで最初に質問すると、**すべてのテーブルを選択すると**、データエージェントが適切に回答できます。

1. 例えば、+++Who are the top 10 customers by total purchase amount?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image35.png)

1. アプリケーションを実行し、サンプル質問を入力して回答を確認します。

    +++Which day has the highest sales?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image36.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image37.png)


## タスク4：メタプロンプトによる最適化

1. **Setup**セクションで、「**Agent instructions」**フィールドに移動します**。（**または、上部のナビゲーションバーからも**Agent instructions** を確認できます。）

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image38.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image39.png)

1. **テストペイン**（右側の**「Test the agent's responses」**と表示されている部分）にあるこのメタプロンプトを使用して、エージェントレベルの指示を生成します。

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

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image40.png)

1. より複雑なクエリを使用して、強化されたエージェントをテストします。

    +++How many orders are placed each day?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image41.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image42.png)

    +++Which products have the lowest stock levels?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image43.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image44.png)


## タスク5：エージェントの公開

1. Fabric Agent のテスト ペイン内でこのメタ プロンプトを使用してエージェントの説明を生成します。

    > Meta-Prompt: Generate Agent Description
    >
    > Create a 1-2 sentence description of yourself as a Fabric Data Agent
    > (max 200 chars).
    >
    Analyze your data sources and describe: what data domain you cover and what questions you answer.

    Example: "Fabric Data Agent for retail sales. Answers questions about revenue, products, customers, and orders"

    Output plain text only.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image45.png)

1. **「Publish」**をクリックし、生成された説明文を「purpose and capabilities」フィールドに貼り付けます。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image46.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image47.png)


# 演習2：Fabric AgentとCopilot Studioの接続

## この演習では、Copilot Studio が Fabric Data Agent と通信できるようにすることに重点を置きます。Copilot エージェントを作成し、その動作を設定し、Fabric Agent にリンクし、両方のエージェントが連携してより詳細なインサイトを生成するようにします。これにより、プラットフォーム間でのマルチエージェント通信が確立されます。

## タスク1： Copilot Studioエージェントの作成

1. 新しいブラウザタブを開き、 ++https://copilotstudio.microsoft.com/+++にアクセスします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image48.png)

1. 左側のナビゲーションで**「Agents」**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image49.png)

1. 青色の「 **+Create blank agent** **」**ボタンをクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image50.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image51.png)

1. 設定を変更するには、 **「Edit」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image52.png)

1. エージェントを以下の設定で構成します。

    - **Name**: E-commerce RAG Agent
    - **Description**: An agent connected to a Microsoft Fabric data
    agent specializing in e-commerce business knowledge and support

    - エージェントのモデルで**Claude Sonnet 4.5**を選択します。
    - **Instructions:** Copy the instructions from the code block below

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image53.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image54.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image55.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image56.png)


1. 右上隅の**「Publish」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image57.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image58.png)


## タスク2：Copilot Studioに接続エージェントとしてFabric Agentを追加

1. エージェントの作成後、 **「Agents** **」**タブに移動し、 **「+Add agent」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image59.png)

1. **「Connect to an external agent」**をクリックし、 **「Microsft Fabric (preview)」**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image60.png)

1. *「Connection : Not connected* *」*と表示された場合は、 *「Not connected」*の横にあるドロップダウンをクリックして「**Create new connection」**を選択します。表示されているメールアドレスがご自身のアカウントのメールアドレスであることを確認し、「**Next」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image61.png)

1. **「Create」**をクリックし、このラボで使用したのと同じアカウントでサインインします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image62.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image63.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image64.png)

1. Fabric Data Agentを選択します

    - 演習1＞タスク3で作成したエージェント名を探します。
    - クリックして選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image65.png)


1. エージェント名を**「+++ DataAgent -@ lab.LabInstance.Id +++** 」と入力し、**接続**を確認してから、 **「Add and configure」**をクリックしてエージェントの設定を進めます。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image66.png)

1. エージェントを使用可能にするには、 **「Publish」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image67.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image68.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image69.png)


## タスク3：接続されたFabric Data Agentのテスト

1. 段階的なクエリを使用して、Fabric Data Agent の接続をテスト：

    +++What are the top 10 highest value orders?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image70.png)

1. 必要な権限を付与するには、「**Allow」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image71.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image72.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image73.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image74.png)

    **注：**応答生成処理には**5～6分**かかる場合があります。

    +++What is the average price per category?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image75.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image76.png)

    > +++**What percentage of orders use credit card vs PayPal vs debit
    > card?**+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image77.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image78.png)

    +++What is the revenue by payment method?+++


# 演習3： Fabric Data AgentをTeamsに接続

## この演習では、Copilot エージェントを Teams に公開し、ビジネスユーザーがコラボレーションアプリ内から直接エンタープライズデータにアクセスできるようにします。また、いくつかの BI クエリを実行し、Teams 上でリアルタイムの応答を確認することで、エージェントの機能を検証します。

## タスク1：Copilotの機能を追加

1. **E-commerce RAG Agent**で、 **\[+\]（Add）**アイコンをクリックし、 \[**Channels\]**を選択してエージェントチャネルの設定を構成します。![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image79.png)
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image80.png)

1. **TeamsとMicrosoft 365 Copilot**を選択します。![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image81.png)

1. 「Add Channel」をクリックします![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image82.png)

1. **Microsoft Teams**でエージェントを開いてテストするには、\[ **See agent in Teams\]**を選択します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image83.png)

1. **Open Microsoft Teams**をクリックします。![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image84.png)  
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image85.png)

1. **「Sign in」**をクリックします。![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image86.png)

1. 提供された認証情報を入力してサインインします。![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image87.png)  
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image88.png)  
  
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image89.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image90.png)

1. **「Add」**をクリックします。
1. ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image91.png)

1. アプリが正常に追加されたら、 *「Open」*ボタンをクリックして*、* Microsoft Teams でE コマース RAG Agentを起動します。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image92.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image93.png)


## タスク2：接続されたFabric Data Agentのテスト

1. 段階的なクエリを使用して、Fabric Data Agent の接続をテストします。

    +++What is the revenue trend over time?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image94.png)

1. 必要な権限を付与するには、「**Allow」**をクリックします。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image95.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image96.png)

    +++What are the top 10 highest value orders?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image97.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image98.png)

    +++Which payment method is used the most?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image99.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-JP/Labguides/Usecase%2005/media/image100.png)


### まとめ

このユースケースでは、Eコマース組織が**Copilot Studio**を介して**Microsoft Fabric Data Agents**と**Microsoft Teams**を統合し、リアルタイムのインサイト、自然言語による分析、およびマルチエージェント連携を実現する方法に焦点を当てます。統合分析プラットフォーム（Microsoft Fabric）と対話型AI（Copilot StudioおよびTeams）を組み合わせることで、ビジネスユーザーはクエリを記述することなく、売上トレンド、製品の分析情報、顧客行動といった情報にシームレスにアクセスできるようになります。本ソリューションは、AIエージェントがFabric Lakehouseからデータを取得し、指示に基づいて回答を充実させ、さらに他のエージェントと連携してビジネスインテリジェンスのワークフローを効率化する仕組みを示しています。
