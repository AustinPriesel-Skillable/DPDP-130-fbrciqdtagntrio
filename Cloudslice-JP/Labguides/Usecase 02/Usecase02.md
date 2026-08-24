## ユースケース02 - Fabric Data Agentを使用して AdventureWorksデータセットによる販売分析を構築


**紹介**

小売業界のインサイト分析チームであるContoso
Analyticsは、アナリストやビジネスマネージャーのデータアクセス性を向上させるため、レポート作成ワークフローを**Microsoft
Fabric**に移行しています。同チームは、非技術系のユーザーでもSQLを記述したりダッシュボードを操作したりすることなくインサイトを得られるよう、自然‑言語によるデータ探索を可能にすることを目指しています。

これを実現するために、チームは**Fabric Data
Agent**を搭載した**インテリジェントな分析アシスタント**を構築することにしました。このプロセスの最初のステップは、
**Fabric Lakehouse**で基となるデータを準備することです。Fabric Data
Agentのチュートリアルで説明されているように、まず**Lakehouseを作成してデータを入力します。**このLakehouseには、販売取引、製品在庫、店舗プロファイルなどの厳選された小売データセットが格納されます。このLakehouseは、下流タスクのための統制された中央集約型データソースとして機能します。

Lakehouse
のセットアップが完了したら、次のステップは対話型システムや自動化ツールからアクセスできるようにすることです。チームは、
**Fabric Data Agent を構築し**、 **Lakehouse
を接続先データソースとして追加する**ことでこれを実現し、これにより、データへの安全で管理されたアクセスが可能になります。この構成により、Data
Agent は Lakehouse
のコンテンツを理解し、クエリを実行できるようになり、組織全体で自然言語によるエクスペリエンスを構築するための基盤が築かれます。‑

Fabric Data Agentを介してレイクハウスが接続されたことで、 Contoso
はエージェントを分析アプリケーション、Copilotエクスペリエンス、および社内ツールに統合できるようになりました。これにより、ビジネスユーザーは*「南部地域の今日の売上を表示します」*や「*全店舗で在庫が最も少ない商品を特定します」*といった質問を‑し、データ‑に基づいた回答を即座に受け取ることができます。

**目的**

- **Microsoft
  Fabricワークスペース**を作成し、ストレージとアクセス許可を設定します。

- **Fabric
  Lakehouse**を構築し、ノートブックを使用してAdventureWorksデータセットをプログラムで読み込みます。

- レイクハウステーブルに接続された**Fabric Data
  Agent**を作成し、構成します。

- **指示とサンプルクエリ**を使用して、エージェントの応答を改善します。

- エージェントを公開し、
  Fabricノートブック内で**API呼び出しを介してプログラム的にテストします。**

- 実験が完了したら、ワークスペースをクリーンアップして削除します。

## **タスク0：ホスト環境の時刻を同期する**

1.  VM内で、**検索バーに移動してクリックし**、
    **「Settings」**と入力して**、 「Best
    match」**の下にある**「Settings」**をクリックします。

> ![A screenshot of a computer Description automatically
> generated](./media/image1.png)

2.  **「Time & language」**に移動します。

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  **「Time & language」ページ**で、 **「Date & time」**に移動します。

![A screenshot of a computer Description automatically
generated](./media/image3.png)

4.  **「Additional settings」**セクションに移動し、 **「Syn
    now」**ボタンをクリックします。同期には3～5分かかります。

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  **Settings**ウィンドウを閉じます。

![A screenshot of a computer Description automatically
generated](./media/image5.png)

## タスク 1: **Fabric ワークスペースを作成**

このタスクでは、Lakehouse、ノートブック、およびData
AgentをホストするFabricワークスペースを作成することで、基盤となる環境をセットアップします。このワークスペースは、ユースケース全体で使用されるすべてのアセットの中央コンテナとして機能します。

1.  ブラウザを開き、アドレスバーに移動して、次のURL
    を入力または貼り付けます: ++https://app.fabric.microsoft.com/+++。
    そして**Enter**ボタンを押します。

![](./media/image6.png)

2.  **Microsoft Fabric**のウィンドウで、資格情報を入力し、
    **「Submit」**ボタンをクリックします。

    |  |   |
    |---|----|
    |Username	|+++@lab.CloudPortalCredential(User1).Username+++|
    |TAP	|+++@lab.CloudPortalCredential(User1).AccessToken+++|

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

3.  次に、 **Microsoft**のウィンドウでパスワードを入力し、「**Sign
    in** **」**ボタンをクリックします。

> ![A login screen with a red box and blue text AI-generated content may
> be incorrect.](./media/image8.png)

4.  **「Stay signed in?** **」**というウィンドウで、
    **「Yes** **」**ボタンをクリックします。

&nbsp;

5.  Power BIホームページに移動します。

> ![](./media/image9.png)

6.  Fabricのホームページで、 **「+New workspace**」タイルを選択します。

> ![](./media/image10.png)

7.  右側に表示される**「Create a
    workspace」**ペインに、以下の詳細を入力し、  
    **「Apply」**ボタンをクリックします。

    | Property | Value |
    |---------|-------|
    | Name | +++Fabric Data agent-@lab.LabInstance.Id+++ |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | Small dataset storage format |
    | Template apps | Check **Develop template apps** |

![](./media/image11.png)

注：ラボのインスタントIDを確認するには、「Help」を選択してインスタントIDをコピーします。

![A screenshot of a computer Description automatically
generated](./media/image12.png)

> ![](./media/image13.png)

8.  デプロイが完了するまでお待ちください。完了まで1～2分かかります。

> ![](./media/image14.png)

## タスク2： AdventureWorksLHを使用してレイクハウスの構築

このタスクでは、Fabricノートブックを使用して新しいレイクハウスを作成し、
AdventureWorksテーブルをレイクハウスに格納する手順を説明します。レイクハウスは、Data
Agentがクエリを実行する構造化データの基盤となります。

1.  ナビゲーションバーの**「+New
    item」ボタン**をクリックして、新しいレイクハウスを作成します。

![](./media/image15.png)

2.  「**Lakehouse**」のタイルをクリックします。

![](./media/image16.png)

3.  **New lakehouse** ダイアログボックスで、
    **\[Name** **\]**フィールドに+++ **AdventureWorksLH +++**と入力し、
    **\[Create\]**ボタンをクリックして新しいレイクハウスを開きます。

**注： AdventureWorksLH**の前のスペースを削除します。

![](./media/image17.png)

![](./media/image18.png)

4.  **「Successfully created SQL endpoint」**という通知が表示されます。

![](./media/image19.png)

5.  Fabric Data
    Agentを作成するワークスペースに、新しいノートブックを作成します。

> ![](./media/image20.png)

6.  **セル**内のコードを以下のコードに更新し、セルの左側に表示される**▷
    Run cell** をクリックします。

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

![](./media/image22.png)

![](./media/image23.png)

![](./media/image24.png)

![](./media/image25.png)

## タスク3：Data Agentの作成

このタスクでは、Fabric Data
Agentを作成し、レイクハウスに接続します。エージェントが幅広い販売‑関連の分析質問に回答できるように、必要なディメンションテーブルとファクトテーブルを選択します。

1.  次に、左側のナビゲーション ウィンドウにある**「Fabric Data
    agent-XXXXXX」**をクリックします。

![](./media/image26.png)

2.  **Fabricの**ホームページで、 **「+New item」**を選択します。

![](./media/image27.png)

3.  **「Filter by item type** **」**検索ボックスに**「+++data agent+++」**と入力し、「**Data agent**を選択します。

![](./media/image28.png)

4.  Data Agent名として**「+++AI-agent+++**と入力し、
    **「Create」**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

> ![](./media/image30.png)

5.  AIエージェントのページで、 **「Add a data source」**を選択します。

> ![](./media/image31.png)

6.  **OneLake catalog**タブで、 **AI-
    Fabric_lakehouseを選択し、「Add」**を選択します。

![](./media/image32.png)

![](./media/image33.png)

7.  次に、AIスキルがアクセスできるようにするテーブルを選択する必要があります。

この実験室では以下の表を使用：

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

![](./media/image34.png)

## タスク4：指示を与える

ここでは、自然言語による質問とそれに対応するSQLクエリを追加することで、Data
Agentの機能を強化します。これらの例は、エージェントがドメイン‑固有のコンテキストを理解し、実際のクエリに対してより正確なSQL応答を生成するのに役立ちます。

1.  リストされたテーブルを使用して最初に質問を行う場合、
    **factinternetsales**を選択すると、データ
    エージェントがそれらにかなり適切に回答します。

2.  例の質問：+++**What is the most sold product?+++**

![](./media/image35.png)

> ![](./media/image36.png)

3.  質問とSQLクエリをコピーしてメモ帳に貼り付け、メモ帳を保存して、今後のタスクでその情報を使用します。

![A screenshot of a computer Description automatically
generated](./media/image37.png)

![A screenshot of a computer Description automatically
generated](./media/image38.png)

4.  **FactResellerSales**を選択し、以下のテキストを入力して、下の画像に示すように**Submit**アイコンをクリックします。

**+++What is our most sold product?+++**

![A screenshot of a computer Description automatically
generated](./media/image39.png)

![A screenshot of a computer Description automatically
generated](./media/image40.png)

クエリの実験を続けるにつれて、指示をさらに追加していく必要があります。

5.  **dimcustomer**を選択し、以下のテキストを入力して**Submit**アイコンをクリックします。

**+++how many active customers did we have June 1st, 2013?+++**

![A screenshot of a computer Description automatically
generated](./media/image41.png)

![A screenshot of a computer Description automatically
generated](./media/image42.png)

6.  すべての質問とSQLクエリをコピーしてメモ帳に貼り付け、メモ帳を保存して、今後のタスクでその情報を使用します。

> ![A screenshot of a computer Description automatically
> generated](./media/image43.png)

![A screenshot of a computer Description automatically
generated](./media/image44.png)

7.  **dimdate、FactInternetSales**
    を選択し、以下のテキストを入力して**Submit**アイコンをクリックします。

**+++what are the monthly sales trends for the last year?+++**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

> ![A screenshot of a computer Description automatically
> generated](./media/image46.png)

8.  **dimproduct、FactInternetSales**を選択し、以下のテキストを入力して**Submit**アイコンをクリックします。

**+++which product category had the highest average sales price?+++**

> ![A screenshot of a computer Description automatically
> generated](./media/image47.png)

![A screenshot of a computer Description automatically
generated](./media/image48.png)

問題の一つは、「アクティブな顧客（active
customer）」に正式な定義がないことです。モデルテキストボックスの注釈に詳細な説明を追加すれば解決するかもしれませんが、ユーザーからこのような質問が頻繁に寄せられる可能性があります。AIがこの質問に正しく対応できるようにする必要があります。

7.  **Setup** ペインの**「Example
    queries** **」**ボタンを選択して例を示します。

> ![](./media/image49.png)

8.  「Example queries」タブで、「**Add example」**を選択します。

![](./media/image50.png)

9.  レイクハウスデータソースのExample
    queriesを追加する必要があります。質問フィールドに以下の質問を追加します。

**+++What is the most sold product?+++**

> ![](./media/image51.png)

10. メモ帳に保存したクエリ1を追加します。

    ```
    SELECT TOP 1 ProductKey, SUM(OrderQuantity) AS TotalQuantitySold
    FROM [dbo].[factinternetsales]
    GROUP BY ProductKey
    ORDER BY TotalQuantitySold DESC
    ```
> ![](./media/image52.png)

11. 新しいクエリフィールドを追加するには、
    **「+Add」をクリックします。**

> ![](./media/image53.png)

12. 質問欄に2つ目の質問を追加するには：

**+++What are the monthly sales trends for the last
year?+++**![](./media/image54.png)

13. メモ帳に保存したquery3を追加します。

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

> ![](./media/image55.png)

14. 新しいクエリフィールドを追加するには、
    **「+Add」をクリックします。**

> ![](./media/image56.png)

15. 質問欄に3つ目の質問を追加します。

+++Which product category has the highest average sales
price?+++![](./media/image57.png)

16. メモ帳に保存したquery4を追加します。

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
    ![](./media/image58.png)

17. メモ帳に保存したすべてのクエリとSQLクエリを追加し、「**Export
    all」**をクリックします。

> ![](./media/image59.png)

![](./media/image60.png)

## タスク5：Data Agentをプログラム的に使用

Data
Agentに手順と例が追加されました。テストが進むにつれて、より多くの例と手順を追加することで、AIのスキルをさらに向上させることができます。同僚と協力して、彼らが尋ねたい質問の種類を網羅する例と手順が提供されているかどうかを確認します。

Fabric ノートブック内で、プログラムによって AI スキルを使用できます。AI
スキルに公開済み URL の値があるかどうかを判断するために使用します。

1.  Data
    AgentFabricページで、**「Home** **」**リボンから**\[Settings\]**を選択します。

> ![](./media/image61.png)

2.  AIスキルを公開する前は、このスクリーンショットに示すように、公開されたURLの値は存在しません。

3.  AI Skill設定を閉じます。

> ![](./media/image62.png)

4.  **Home** リボンで、 **\[Publish\]**を選択します。

> ![](./media/image63.png)
>
> ![](./media/image64.png)

5.  **「View publishing details」**をクリックします。

> ![](./media/image65.png)

6.  このスクリーンショットに示すように、AI agent
    の公開URLが表示されます。

7.  URLをコピーしてメモ帳に貼り付け、メモ帳を保存します。その情報は、以降の手順で使用します。

> ![](./media/image66.png)

8.  左側のナビゲーション
    ウィンドウで**「Notebook1** **」**を選択します。

> ![](./media/image67.png)

9.  セル出力の下にある**「+
    Code」アイコン**を使用してノートブックに新しいコードセルを追加し、そこに次のコードを入力して**URLを**置き換えます。
    **▷ Run** ボタンをクリックして出力を確認します。

+++%pip install " openai ==1.70.0"+++

> ![](./media/image68.png)
>
> ![](./media/image69.png)

10. セル出力の下にある**「+
    Code** **」アイコン**を使用してノートブックに新しいコードセルを追加し、そこに次のコードを入力して**URL**を置き換えます。
    **▷ Run** ボタンをクリックして出力を確認します。　

+++%pip install httpx ==0.27.2+++

> ![](./media/image70.png)

11. セル出力の下にある**「+
    Code** **」アイコン**を使用してノートブックに新しいコードセルを追加し、そこに次のコードを入力して**URL**を置き換えます。
    **▷ Run** ボタンをクリックして出力を確認します。

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
     
    base_url = "https://<generic published base URL value>"
    question = "What datasources do you have access to?"
    
    configs = get_mlflow_env_config()
    
    # Create OpenAI Client
    class FabricOpenAI(OpenAI):
        def __init__(
            self,
            api_version: str ="2024-05-01-preview",
            **kwargs: t.Any,
        ) -> None:
            self.api_version = api_version
            default_query = kwargs.pop("default_query", {})
            default_query["api-version"] = self.api_version
            super().__init__(
                api_key="",
                base_url=base_url,
                default_query=default_query,
                **kwargs,
            )
        
        def _prepare_options(self, options: FinalRequestOptions) -> None:
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

![](./media/image71.png)

![](./media/image72.png)

## **タスク6：リソースを削除する**

1.  左側のナビゲーションメニューからワークスペース「
    **AI-Fabric-XXXX」を選択**します。ワークスペースアイテムビューが開きます。

> ![A screenshot of a computer Description automatically
> generated](./media/image73.png)

2.  ワークスペース名の下にある「 **...」オプション**を選択し、
    **「Workspace settings」**を選択します。

> ![A screenshot of a computer Description automatically
> generated](./media/image74.png)

3.  **「Other** **」**を選択し、**Remove this
    workspace**をクリックします。

> ![A screenshot of a computer Description automatically
> generated](./media/image75.png)

4.  表示された警告メッセージで**「Delete」**をクリックします。

> ![A screenshot of a computer Description automatically
> generated](./media/image76.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image77.png)

**まとめ：**

このラボでは、Microsoft Fabric のData
Agentを使用して会話型分析の力を引き出す方法を学びました。Fabric
ワークスペースを構成し、構造化データをLakehouseに取り込み、自然言語の質問を
SQL クエリに変換する AI
スキルを設定しました。また、クエリ生成を洗練するための手順と例を提供することで、AI
エージェントの機能を強化しました。最後に、Fabric
ノートブックからプログラムでエージェントを呼び出し、エンドツーエンドの
AI
統合を実証しました。このラボでは、自然言語技術や生成AI技術を活用し、ビジネスユーザーがエンタープライズデータをより容易に利用・活用し、そこからより高度な知見を得られるようにするための環境を構築できます。
