## **用例02——使用 Fabric data agent构建 AdventureWorks dataset的销售分析**

**简介**
Contoso Analytics是一家零售洞察团队，正在将其报告工作流程转向
**Microsoft Fabric**
，以提升分析师和业务经理的data可访问性。團隊希望支持自然語言data探索，使非技術用戶無需編寫SQL或瀏覽儀錶盤即可獲得洞察。

为了实现这一目标，团队决定构建一个由 **Fabric Data
Agent**驱动的**智能分析助手**。该过程的第一步是在 **Fabric Lakehouse**
中准备底层数据。正如 Fabric data
agent教程中所述，他们首先**创建并填充一个
Lakehouse，**其中将保存精心整理的零售数据集，例如销售交易、产品库存和门店概况。該
Lakehouse 將作為下游任務的集中式數據源。

Lakehouse
搭建完成後，下一步是使其能夠被對話系統和自動化工具訪問。團隊通過**創建
Fabric Data Agent**並將 **Lakehouse
添加為其連接的data源來實現這一點**，從而實現對數據的安全、受控訪問。這種配置使Data
Agent能夠理解和查詢 Lakehouse
的內容，為在整個組織內構建自然語言體驗奠定了基礎。

通過Fabric Data
Agent連接Lakehouse，Contoso現在可以將代理集成到分析應用、Copilot體驗和內部工具中——賦能業務用戶提出諸如
*“向我展示今天南部地區銷售情況”* 或 *“識別所有門店庫存最低的產品”*
等問題，並即時獲得數據驅動的答案。

**目标**

- 创建一个 **Microsoft Fabric workspace**，配置存储和权限。

- 搭建一个 **Fabric Lakehouse**
  ，并用笔记本程序化加载AdventureWorks数据集。

- 创建并配置一个连接到Lakehouse表的**Fabric Data Agent**。

- 通過**指令和示例查詢**提升代理的回答。

- 發佈代理，並通過Fabric 筆記本內的 **API 調用程序測試。**

- 完成實驗後清理並刪除工作區。

## **任務0：同步主機環境時間**

1.  在你的 VM 里，点击 **Search bar**，输入 **Settings**，然后点击
    **Best match** 中的 **Settings**。

> ![A screenshot of a computer Description automatically
> generated](./media/image1.png)

2.  在 Settings 窗口中，点击 **Time & language**。

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  在 **“Time & language**”页面，点击 **“Date & time**”。

![A screenshot of a computer Description automatically
generated](./media/image3.png)

4.  向下滚动，进入 **“Additional settings**”部分，然后点击 **“Syn
    now**”按钮。同步需要3-5分钟。

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  关闭 **Settings** 窗口。

![A screenshot of a computer Description automatically
generated](./media/image5.png)

## 任務1： **創建Fabric工作區**

在這個任務中，你將通過創建一個Fabric工作區來搭建基礎環境，用於託管Lakehouse、筆記本和Data
Agent。該工作區作為所有使用場景中所有資產的中央容器。

1.  打开browser，进入地址栏，输入或粘贴以下URL：+++https://app.fabric.microsoft.com/+++，然后按下**Enter** 键。

![](./media/image6.png)

2.  在 **Microsoft Fabric**
    窗口中，输入你的凭证，然后点击**Submit**按钮。

    |  |   |
    |---|----|
    |Username	|+++@lab.CloudPortalCredential(User1).Username+++|
    |TAP	|+++@lab.CloudPortalCredential(User1).AccessToken+++|

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

3.  然后，在 **Microsoft** 窗口输入密码，点击**Sign in** 按钮。

> ![A login screen with a red box and blue text AI-generated content may
> be incorrect.](./media/image8.png)

4.  在 **Stay signed in?** 窗口，点击**“Yes”**按钮。

&nbsp;

5.  你将被引导到Power BI主页。

> ![](./media/image9.png)

6.  Fabric主页，选择 **+New workspace** 瓷砖。

> ![](./media/image10.png)

7.  在右侧的**Create a
    workspace** 面板中，输入以下细节，然后点击**“Apply**”按钮。

    | Property | Value |
    |---------|-------|
    | Name | +++Fabric Data agent-@lab.LabInstance.Id+++ |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | Small dataset storage format |
    | Template apps | Check **Develop template apps** |

![](./media/image11.png)

注意：要查找您的实验室instant ID，请选择“Help”并复制instant ID。

![A screenshot of a computer Description automatically
generated](./media/image12.png)

> ![](./media/image13.png)

8.  等待部署完成。完成大約需要1-2分鐘。

> ![](./media/image14.png)

## 任务2：用AdventureWorksLH创建lakehouse

这个任务会引导你创建新的Lakehouse，并用Fabric笔记本填充AdventureWorks表格。Lakehouse
成为Data Agent查询的结构化数据基础。

1.  点击导航栏中的 **+New item** 按钮创建新lakehouse。

![](./media/image15.png)

2.  点击“**Lakehouse**”瓷砖。

![](./media/image16.png)

3.  在“**New lakehouse**”对话框中，在“**Name**”字段中输入
    +++**AdventureWorksLH+++**，单击“**Create**”按钮，打开新的lakehouse。

**注意**：请确保在**AdventureWorksLH**之前删除空格

![](./media/image17.png)

![](./media/image18.png)

4.  你会看到一条通知，提示**Successfully created SQL endpoint**。

![](./media/image19.png)

5.  在你想创建 Fabric data agent的工作区创建一个新的笔记本。

> ![](./media/image20.png)

6.  用以下代碼更新該**單元格**的代碼，並點擊“**▷ Run
    cell** **”**左側的單元格。

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

![](./media/image21.png)

![](./media/image22.png)

![](./media/image23.png)

![](./media/image24.png)

![](./media/image25.png)

## 任務3：創建Data agent

在這個任務中，你將創建一個Fabric Data
Agent並將其連接到Lakehouse。您將選擇所需的維度表和事實表，使客服能夠回答各種與銷售相關的分析問題。

1.  现在，点击左侧导航面板上的**Fabric Data agent-XXXXXX**。

![](./media/image26.png)

2.  在**Fabric**主页，选择 **+New item。**

![](./media/image27.png)

3.  在**“Filter by item type”**搜索框中，输入 **+++data agent+++**
    并选择 **Data agent。**

![](./media/image28.png)

4.  输入 **+++AI-agent+++** 作为 Data agent名称，选择**Create**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

> ![](./media/image30.png)

5.  在 AI-agent页面中，选择 **Add a data source**。

> ![](./media/image31.png)

6.  在 **OneLake catalog** 标签页中，选择 **AI-Fabric_lakehouse
    lakehouse** 并选择 **Add**。

![](./media/image32.png)

![](./media/image33.png)

7.  然後你必須選擇你希望AI技能能訪問的表格。

本實驗室使用以下表格：

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

## 任務4：提供指令

在這裡，你將通過添加自然語言問題及其對應的SQL查詢來豐富Data
Agent。這些示例幫助代理理解領域特定上下文，並為現實世界查詢生成更準確的SQL響應。

1.  当你第一次用列出的表格选择 **factinternetsales** 来提问时，data
    agent会相当准确地回答。

2.  例如，对于问题 +++**What is the most sold product?+++**

![](./media/image35.png)

> ![](./media/image36.png)

3.  複製問題和SQL
    queries，粘貼到記事本，然後保存記事本，以便後續任務中使用這些信息。

![A screenshot of a computer Description automatically
generated](./media/image37.png)

![A screenshot of a computer Description automatically
generated](./media/image38.png)

4.  选择**FactResellerSales**，输入以下文字，点击下图所示的**Submit图标**。

**+++What is our most sold product?+++**

![A screenshot of a computer Description automatically
generated](./media/image39.png)

![A screenshot of a computer Description automatically
generated](./media/image40.png)

隨著你不斷嘗試查詢，應該添加更多指令。

5.  選擇**dimcustomer**，輸入以下文字，點擊**Submit圖標**

**+++how many active customers did we have June 1st, 2013?+++**

![A screenshot of a computer Description automatically
generated](./media/image41.png)

![A screenshot of a computer Description automatically
generated](./media/image42.png)

6.  把所有問題和SQL查詢複製出來，粘貼到記事本裡，然後保存記事本，方便後續任務中使用這些信息。

> ![A screenshot of a computer Description automatically
> generated](./media/image43.png)

![A screenshot of a computer Description automatically
generated](./media/image44.png)

7.  选择**dimdate，FactInternetSales**
    ，输入以下文字，点击**Submit图标：**

**+++what are the monthly sales trends for the last year?+++**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

> ![A screenshot of a computer Description automatically
> generated](./media/image46.png)

8.  选择**dimproduct，FactInternetSales**，输入以下文字，点击**Submit图标：**

**+++which product category had the highest average sales price?+++**

> ![A screenshot of a computer Description automatically
> generated](./media/image47.png)

![A screenshot of a computer Description automatically
generated](./media/image48.png)

問題的一部分在於“active
customer”沒有正式的定義。模型文本框備註中更多說明可能會有幫助，但用戶可能會經常問這個問題。你需要确保AI正确地处理这个问题。

7.  相关查询较为复杂，请从**Setup** 面板中选择**“Example
    queries**”按钮提供示例。

> ![](./media/image49.png)

8.  在“Example queries”标签中，选择**Add example。**

![](./media/image50.png)

9.  這裡，你應該為你創建的lakehouse
    data源添加示例查詢。在問題欄中添加以下問題：

**+++What is the most sold product?+++**

> ![](./media/image51.png)

10. 添加你保存在筆記本中的query1：

    ```
    SELECT TOP 1 ProductKey, SUM(OrderQuantity) AS TotalQuantitySold
    FROM [dbo].[factinternetsales]
    GROUP BY ProductKey
    ORDER BY TotalQuantitySold DESC
    ```
> ![](./media/image52.png)

11. 要添加新的查詢字段，請點擊 **+Add。**

> ![](./media/image53.png)

12. 在題目欄中補充第二個問題：

**+++What are the monthly sales trends for the last year?+++**

![](./media/image54.png)

13. 把你保存在筆記本裡的query3添加進去：

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

14. 要添加新的query字段，請點擊 **+Add。**

> ![](./media/image56.png)

15. 在題目欄中添加第三個問題：

+++Which product category has the highest average sales price?+++

![](./media/image57.png)

16. 把你保存在記事本裡的query4添加進去：

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
> ![](./media/image58.png)

17. 把你保存在Notepad的所有查询和SQL查询添加进去，然后点击“**Export
    all”**

> ![](./media/image59.png)

![](./media/image60.png)

## 任务5：程序化使用Data agent

指令和示例都被添加到了Data
agent中。随着测试的推进，更多的示例和说明可以进一步提升AI技能。和同事一起看看你是否提供了涵盖他们想问的问题的例子和说明。

你可以在Fabric
notebook中编程使用AI技能。用來判斷AI技能是否有已發佈的URL值。

1.  在数据代理 Fabric 页面，主 功能**Home** ribbon选择**Settings**。

> ![](./media/image61.png)

2.  在你發佈AI技能之前，它沒有發佈的URL值，如這張截圖所示。

3.  关闭AI技能设置。

> ![](./media/image62.png)

4.  在**Home ribbon**，选择 **Publish**。

> ![](./media/image63.png)
>
> ![](./media/image64.png)

5.  点击查看**View publishing details**

> ![](./media/image65.png)

6.  AI agent的公开URL显示在这张截图中。

7.  复制URL粘贴到notepad，然后保存notepad以便在后续步骤中使用这些信息。

> ![](./media/image66.png)

8.  在左側導航窗格選擇**Notebook1。**

> ![](./media/image67.png)

9.  使用單元格輸出下方的 **+
    Code** 圖標，向筆記本添加一個新的代碼單元格，輸入以下代碼並替換
    **URL**。點擊 **▷ Run**按鈕，查看輸出結果

+++%pip install "openai==1.70.0"+++

> ![](./media/image68.png)
>
> ![](./media/image69.png)

10. 使用單元格輸出下方的 **+
    Code**圖標，向筆記本添加一個新的代碼單元格，輸入以下代碼並替換
    **URL**。點擊 **▷ Run** 按鈕，查看輸出結果

+++%pip install httpx==0.27.2+++

> ![](./media/image70.png)

11. 使用單元格輸出下方的 **+
    Code** 圖標，向筆記本添加一個新的代碼單元格，輸入以下代碼並替換
    **URL**。點擊 **▷ Run** 按鈕，查看輸出結果

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
> ![](./media/image71.png)

![](./media/image72.png)

## **任务6：删除资源**

1.  選擇你的工作區，在左側導航菜單中選用**AI-Fabric-XXXX**。它會打開工作區的物品視圖。

> ![A screenshot of a computer Description automatically
> generated](./media/image73.png)

2.  选择...... 在工作区名称下选择选项，选择**Workspace settings**。

> ![A screenshot of a computer Description automatically
> generated](./media/image74.png)

3.  选择**Other**并**Remove this workspace。**

> ![A screenshot of a computer Description automatically
> generated](./media/image75.png)

4.  点击弹出的警告中“**Delete**”。

> ![A screenshot of a computer Description automatically
> generated](./media/image76.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image77.png)

**摘要：**

在本实验室中，你学习了如何利用Microsoft Fabric’s Data
Agent，释放对话式分析的力量。你配置了一个Fabric工作区，将结构化data导入lakehouse，并设置了一个AI技能将自然语言问题转换为SQL查询。你還通過提供指導和示例來優化查詢生成，增強了AI代理的能力。最后，你通过Fabric
notebook程序化调用了代理，展示了端到端的AI集成。该实验室通过自然语言和generative
AI技术，赋能您让企业数据对企业用户更易访问、更易用且更智能。
