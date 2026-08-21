## **用例 05 - 使用 Copilot Studio 将 Fabric Data Agent 与 Microsoft Teams 集成，以获得可操作的见解和代理间协作**

**简介**

在当今竞争激烈的数字市场中，电子商务公司从客户交易、产品目录、网站互动和支付系统中生成大量数据。從這些數據中提取有意義的洞察對於提升客戶體驗、優化運營和增加收入至關重要。然而，沒有統一的分析平臺，管理和分析來自多個來源的大型數據集可能會很複雜。

**Zava**是一家快速發展的電商公司，通過其在線平臺銷售各種消費品，每天處理數千筆訂單。公司從多個系統收集數據，包括訂單管理、客戶檔案、產品庫存和支付交易。隨著Zava業務的增長，組織在高效分析數據和向業務團隊提供實時洞察方面面臨挑戰。

为应对这些挑战，Zava 采用**了 Microsoft Fabric**
的现代化分析解决方案。Fabric提供了一个统一平台，将data engineering、data
storage、data转换和商业智能功能整合在单一环境中。Zava將原始和處理後的電子商務數據存儲在Fabric
Lakehouse中，實現可擴展的數據管理和分析。

此外，Zava 利用 **Microsoft Fabric Data Agents**
提升data可访问性和洞察生成。Fabric
Agents允许业务用户和分析师通过自然语言查询与企业数据交互。用戶無需手動搜索報告或編寫複雜查詢，只需提出以下問題：

- “本月最暢銷的產品是什麼？”

- “哪個地區銷售收入最高？”

- “上一個季度的客戶訂單趨勢如何？”

Fabric
Agent自動從Lakehouse獲取相關data並生成洞察，幫助團隊快速瞭解業務表現。這種智能互動提高了生產力，促進了跨部門的決策。

通過該解決方案，Zava的業務用戶、分析師和管理團隊可以輕鬆探索data，監控關鍵績效指標，並實時洞察銷售表現、客戶行為和產品需求。通過將先進分析與AI驅動的Fabric
Agents結合，Zava構建了一個可擴展且智能的電子商務分析平臺，支持數據驅動的增長和卓越運營。

**目标**

- 构建并配置**一**个连接到电子商务semantic模型的Fabric Data Agent。

- 在 Fabric Lakehouse 内部导入和建模data ，并通过semantic模型进行展示。

- 利用**meta‑prompts**和代理级指令增强代理的智能。

- 将Fabric Data Agent连接到 **Copilot Studio** ，并启用多代理通信。

- 发布 Copilot agent并将其集成到 **Microsoft Teams** 中进行实时分析。

- 通過直接在Teams中查詢業務洞察來測試端到端流程。

# 练习1：创建和配置Fabric Data Agent

## 在这个练习中，你将建立Microsoft Fabric的基础组件。你将创建一个工作区，搭建lakehouse，导入样本CSV datasets，生成semantic模型，并配置一个能够回答分析性问题的Fabric Data Agent。這為實驗室其他部分提供了核心數據智能層。

## 任務1：創建Fabric工作區

在這個任務中，你需要創建一個Fabric工作區。工作区包含了本 lakehouse
教程所需的所有内容，包括 lakehouse、数据流、Data Factory
管道、笔记本、Power BI datasets和报表。

1.  打開browser，進入地址欄，輸入或粘貼以下URL：+++https：//app.fabric.microsoft.com/+++
    按下**Enter**鍵，並用你的憑證登錄

    |   |   |
    |---|---|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | TAP | +++@lab.CloudPortalCredential(User1).AccessToken+++ |

2.  Fabric主页，选择 **+New workspace** 瓷砖。

    ![A screenshot of a computer Description automatically
    generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image1.png)

3.  在右侧的**Create a
    workspace** 面板中，输入以下细节，然后点击**“Apply**”按钮。

    | Property | Value |
    |---------|-------|
    | Name | +++Fabric-Copilot-@lab.labinstance.id+++  |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | Small dataset storage format |
    | Template apps | Check **Develop template apps** |

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image2.png)

    注意：要查找您的实验室instant ID，请选择“Help”并复制instant ID。

    ![A screenshot of a computer Description automatically
    generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image3.png)
    
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image4.png)

4.  等待部署完成。完成大約需要2-3分鐘。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image5.png)

## 任務2：創建一個lakehouse並導入樣本數據

在這個任務中，你需要搭建一個lakehouse，並獲取NYC出租車的樣本數據以及額外的CSV文件。这在
Fabric 中建立了你的原始dataset基础，方便你之后开始转换和查询。

1.  点击导航栏中的 **+New item** 按钮创建新lakehouse。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image6.png)

2.  在**“Filter by item type”**搜索框中，输入 +++Lakehouse+++
    并选择lakehouse项目。

    ![A screenshot of a computer AI-generated content may be
    incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image7.png)

3.  在“**New
    lakehouse** ”对话框中，在“**Name**”字段中输入+++fabricagent_lakehouse+++，单击“**Create**”按钮，打开新的lakehouse。

    **注意**：请务必在**fabricagent_lakehouse**前删除空格。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image8.png)
    
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image9.png)

4.  等待显示“**Successfully created SQL endpoint**”的通知。

    ![A screenshot of a computer AI-generated content may be
    incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image10.png)

5.  在**lakehouse** 页面，进入**“Get data in your
    lakehouse**”部分，点击**Upload files**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image11.png)

6.  在“Upload files”标签页中，点击Files下的文件夹

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image12.png)

7.  在VM上浏览到 **C：\LabFiles**，然后选择
    **customers.csv、Orders_Data.csv** 和 **products.csv** 文件，点击
    **Open** 按钮。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image13.png)

8.  然後點擊 **Upload **按鈕並關閉

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image14.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image15.png)

9.  點擊並選擇 **Files** 刷新。文件出现了。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image16.png)

10. 在**Lakehouse**页面，在Explorer面板下选择“Files”。不过，现在你的鼠标
    **Orders_Data.csv**文件。点击水平椭圆**（...）**
    旁边**Orders_Data.csv**。点击**“Load Table**”，然后选择**“New
    table**”。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image17.png)

11. 在**“Load file to new table**”对话框中，点击**Load** 按钮。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image18.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image19.png)

12. 对**customers.csv**和**products.csv**重复同样的过程
    ，将它们转换成表格。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image20.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image21.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image22.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image23.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image24.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image25.png)

13. 在屏幕右上角的 **Lakehouse** 下拉菜单中选择 **SQL analytics**。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image26.png)

14. 在 lakehouse **Home** 标签中，选择**“New semantic
    model**”，选择你想添加到 semantic 模型中的表格。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image27.png)

16. 在**New semantic model** 对话框中，输入 +++E-commerce Order
    Dataset+++，然后从表列表中选择**all** 表，选择**Confirm** 以创建新模型。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image28.png)

15. 在左侧菜单中，选择 **Fabric-Copilot-XXXX**
    工作区图标，然后选择工作区名称。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image29.png)

## 任务3：创建一个Fabric data agent

1.  在 **Fabric-Copilot-XXXX** 工作区页面，点击 **+New
    item** 按钮，然后选择 **Data agent**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image30.png)

2.  请提供
    [DataAgent\_@lab.LabInstance.Id](DataAgent_@lab.LabInstance.Id) 姓名
    并点击**Create**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image31.png)

3.  选择 **Add data source** 以配置新的data source。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image32.png)

4.  从结果中选择**E-commerce Order Dataset**（类型：Semantic Model）。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image33.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image34.png)

5.  当你第一次用列出的表格提问时，选择**all tables，** data
    agent会比较好地回答。

6.  例如，对于+++Who are the top 10 customers by total purchase
    amount?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image35.png)

7.  運行申請並輸入樣本問題以驗證回答。

    +++Which day has the highest sales?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image36.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image37.png)

## 任务4：利用Meta-Prompts进行优化

1.  在**Setup**部分，找到**Agent
    instructions** 字段。（或者，你也可以找到 导航栏顶部有**Agent
    instructions** 。）

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image38.png)
    >
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image39.png)

2.  在**test pane** 右侧（写着“**Test the agent's
    responses**”）中，使用这个meta-prompt生成代理级指令：

    > Meta-Prompt: 生成代理级指令:
    >
    > 分析你可用的data sources，并为自己创建代理级指令（最多15000字符）。
    >
    > 目标: {AGENT_OBJECTIVE}
    >
    > 用户: {USER_PERSONA}
    >
    > 檢查你的數據來源：列出所有來源、類型和主要用途。分析領域、時間覆蓋和主要主題。
    >
    > 生成指令:
    >
    > \## Objective
    >
    > \## Data Sources (list with priority)
    >
    > \## Key Terminology (infer from columns/measures)
    >
    > \## Response Guidelines
    >
    > Style: {RESPONSE_STYLE}
    >
    > \## Handling Common Topics (3-5 based on available data)
    >
    > Custom terms: {CUSTOM_TERMINOLOGY}

    使用该meta-prompt时，请根据以下数值手动替换prompt中的变量，**或者**将变量粘贴到测试中：

    - {AGENT_OBJECTIVE}: “商业智能电子商务分析代理”

    - {USER_PERSONA}: “业务分析师和销售团队”

    - {RESPONSE_STYLE}: “清晰的摘要，附有数据引用和趋势分析”

    - {CUSTOM_TERMINOLOGY}: 留空或添加你的领域特定术语

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image40.png)

8.  用更複雜的查詢測試增強版代理：

    > +++How many orders are placed each day?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image41.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image42.png)

    > +++Which products have the lowest stock levels?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image43.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image44.png)

## 任务5：发布Agent

1.  在你的 Fabric Agent 测试窗格内，使用这个元提示词生成代理描述

    > Meta-Prompt：生成代理描述
    >
    > 创建一个1-2句的描述，介绍你自己作为Fabric Data
    > Agent（最多200个角色）。
    >
    > 分析你的數據來源，描述你覆蓋的哪個數據領域以及你要回答哪些問題。
    >
    > 示例：“零售销售用的Fabric Data
    > Agent。回答有关收入、产品、客户和订单的问题。”
    >
    > 只输出纯文本。
    >
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image45.png)

2.  点击**Publish**，然后将生成的描述粘贴到目的和能力字段。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image46.png)
    >
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image47.png)

# 练习2：将Fabric Agent连接到Copilot Studio

## 该练习重点是使 Copilot Studio 能够与 Fabric Data Agent 通信。你将创建一个 Copilot agent，配置其行为，将其与 Fabric Agent关联，并确保两个代理协作以产生更丰富的洞察。这建立了跨平台的多智能体通信。

## 任务1：创建Copilot Studio Agent

1.  打開一個新的瀏覽器標簽頁，進入
    +++https://copilotstudio.microsoft.com/+++。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image48.png)

2.  在左侧导航中，选择 **Agents**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image49.png)

3.  点击蓝色的 **+Create blank agent**按钮。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image50.png)
    >
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image51.png)

4.  點擊 **Edit** 以修改設置。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image52.png)

5.  请用以下设置配置您的agent：

    - **Name**: 电子商务RAG Agent

    - **Description**: 一个连接到专注于电子商务业务知识和支持的Microsoft
    Fabric data agent的agent

    - 选择你经纪人的型号，选择 **Claude Sonnet 4.5**

    - **Instructions**: 复制下面代码块的说明

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image53.png)
    >
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image54.png)
    >
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image55.png)
    >
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image56.png)

1.  点击右上角的**Publish**。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image57.png)
    >
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image58.png)

## 任务2：将Fabric Agent添加为Copilot Studio的连接agent

1.  创建代理后，进入**Agents** 标签，点击 **+Add agent**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image59.png)

2.  点击**Connect to an external agent**，选择**Microsft Fabric
    (preview)。**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image60.png)

3.  如果显示 *Connection : Not connected，点击“Not
    connected”旁边的下拉菜单* ，选择**Create new
    connection**。确认它是否显示为你账户的邮箱，然后点击**“Next**”。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image61.png)

4.  點擊**Create**並使用本實驗室使用的同一個賬號登錄

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image62.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image63.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image64.png)

5.  选择您的 Fabric Data Agent

    - 找你在練習#1\>任務3中創建的代理名

    - 点击以选中它

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image65.png)

6.  输入 **agent名称** 为 +++DataAgent-@lab.LabInstance.Id+++，验证
    **connection**，然后点击 **Add and configure** ，继续agent设置。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image66.png)

7.  点击 **Publish** 以使agent可用

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image67.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image68.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image69.png)

## 任务3：测试连接的Fabric Data Agent

1.  通过渐进式查询测试Fabric Data Agent连接：

    > +++What are the top 10 highest value orders?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image70.png)

2.  点击 **Allow** 以授予所需的权限

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image71.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image72.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image73.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image74.png)

    **注意：** 響應生成過程可能需要 **5–6分鐘** 完成。

    +++What is the average price per category?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image75.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image76.png)

    > +++What percentage of orders use credit card vs PayPal vs debit
    > card?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image77.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image78.png)

    > +++What is the revenue by payment method?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image79.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image80.png)

# 練習3：將Fabric Data Agent連接到Teams中

## 在此練習中，您將向Teams發佈Copilot agent，使業務用戶能夠直接在其協作應用內訪問企業data。你將通過運行多個BI查詢並在Teams內部實時觀察響應來驗證代理的功能。

## 任务1：添加Copilot能力

1.  **E-commerce RAG Agent**中，点击 **+ (Add)**
    图标，选择**Channels**以配置agent渠道设置。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image81.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image82.png)

2.  选择**Teams and Microsoft 365 Copilot**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image83.png)

3.  点击Add Channel

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image84.png)

4.  选择 **See agent in Teams，**在 **Microsoft Teams**
    中打开并测试该agent。

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image85.png)

5.  点击**Open Microsoft Teams**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image86.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image87.png)

6.  点击**Sing in**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image88.png)

7.  輸入您提供的資質以便登錄並繼續

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image89.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image90.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image91.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image92.png)

8.  点击**Add**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image93.png)

9.  应用成功添加后，点击 *Open* 按钮，在 Microsoft Teams 中启动
    E‑commerce RAG Agent

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image94.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image95.png)

## 任务2：测试连接的Fabric Data Agent

1.  通过渐进式查询测试Fabric Data Agent连接：

    > +++What is the revenue trend over time?+++
    >
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image96.png)

2.  点击 **Allow** 以授予所需的权限

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image97.png)
    >
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image98.png)
    >
    > +++What are the top 10 highest value orders?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image99.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image100.png)

    +++Which payment method is used the most?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image101.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-CNT/Labguides/Usecase%2005/media/image102.png)

    **摘要**

    该用例聚焦于电子商务组织如何通过 **Copilot Studio**将**Microsoft Fabric
    Data Agents**与**Microsoft
    Teams**集成，以提供实时洞察、自然语言分析和多智能体协作。通过结合统一分析平台（Microsoft
    Fabric）与 conversational AI（Copilot
    Studio和Teams），业务用户无需编写查询即可无缝访问销售趋势、产品洞察和客户行为。该解决方案展示了
    AI agents如何从 Fabric Lakehouse
    获取data，利用指令丰富响应，并与其他代理协作，简化商业智能工作流程。
