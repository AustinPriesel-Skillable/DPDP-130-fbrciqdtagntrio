# 使用案例04 - 将Fabric Data Agent连接到Microsoft Foundry，实现统一且智能的data insights

**简介**

现代组织在多个系统中生成大量数据，使业务用户和分析师难以快速获取洞察。Data常被存储在孤岛中，需要技术专长来提取、分析和解读信息。

Microsoft Fabric
統一數據平臺通過將分析、數據工程和商業智能能力整合到單一環境中，解決了這一挑戰。通過整合Azure
AI Foundry的基於代理的AI
capabilities，組織可以構建智能應用，利用自然語言和自動化工作流程與企業數據交互。

統一數據基礎代理應用解決方案加速器展示了AI驅動的代理如何利用統一的企業
data回答問題、自動化數據分析，並為技術和非技術用戶提供洞察。這些代理應用協調任務，檢索相關數據，生成上下文響應，從而實現更快的決策和提升運營效率。

在這種應用場景中，組織可以使用AI代理分析銷售表現、客戶行為和產品趨勢等業務data。用戶無需手動查詢多個數據集，只需用自然語言提問，就能直接從系統獲得可操作的見解。

**目標**

該用例的目標是展示組織如何利用具備**agentic
AI的統一數據基礎**，提升數據可訪問性和決策能力。主要目标包括：

**在 Microsoft Fabric 中建立统一 data基础**

- 创建一个带有Lakehouse、Warehouse和semantic模型的受控Fabric工作区。

- 加載並驗證企業datasets進行分析。

**2. 构建和配置 Fabric Data Agent**

- 创建一个**能**够使用自然语言查询数据集的**Fabric Data Agent。**

- 連接Ontology資源，定義代理指令以支持企業特定的查詢。

**3. 部署 Azure 和 Foundry 组件**

- 配置包括Foundry项目、AI服务、搜索、存储和应用服务在内的Azure
  resources。

- 通過 Azure 開發人員 CLI (azd) 部署支持組件

**4. 将 Fabric Data Agent连接到 Microsoft Foundry**

- 在Foundry中创建或配置AI agent。

- 通过Workspace ID和AI Skills ID将代理与Microsoft Fabric连接。

- 提供域特定指令，使代理能够解释和分析Fabric data。

**5. 啟用對話分析和自動化洞察**

- 用真实的业务查询测试Foundry Playground中的代理。

- 演示使用 Fabric Lakehouse datasets的自然语言到数据检索工作流程。

- 提供諸如檢查通過/不合格率、平均值、趨勢和分組總結等洞察。

**6. 展示端到端代理應用工作流程**

- 将Foundry agent、Fabric data
  source和Azure基础设施集成到一个功能齐全的Web应用中。

- 驗證智能數據交互、自動推理和洞察生成。

**解决方案架构**

![Architecture Diagram](./media/image1.png)

该解决方案结合了 Microsoft Fabric 和 Microsoft
Foundry，打造了一个能够通过结构化数据和非结构化文档回答问题的 AI
解决方案：

- **Microsoft Fabric** 提供数据层，包括 Lakehouse、Warehouse 和 Fabric
  IQ semantic层，用于自然语言到 SQL 的转换

- **Microsoft Foundry** 托管了这些 AI agents，包括用于文档检索的 Foundry
  IQ 和协调这两种功能的 Orchestrator Agent

- **Azure AI Services** 支持语言模型（GPT-4o-mini）和嵌入

- **Azure AI Search** 存储用于语义检索的文档向量

**前提条件**

- **GitHub賬戶：你需要擁有自己的GitHub登錄憑證。  
  如果您没有账户，请访问:  
  +++<https://github.com/signup?user_email=&source=form-home-signup+++>**

## 任務0：創建一個GitHub賬戶

在這個任務中，你需要創建一個新的**Github賬戶**，使用你在本實驗室使用的租戶憑證。

1.  点击此链接 +++<https://github.com/+++> 访问 GitHub，点击“**Sign
    up**”即可继续。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

2.  现在，要创建新的 GitHub 账户，输入
    **email**, **password** 和唯一**username** ，点击**Continue**按钮。

![A screenshot of a login box AI-generated content may be
incorrect.](./media/image3.png)

3.  按照屏幕上的指示開始**驗證謎題**。点击**Submit。**

4.  4\. 輸入您在郵件中收到的**驗證碼**。

![A screenshot of a email form AI-generated content may be
incorrect.](./media/image4.png)

5.  现在，请用你的凭证登录GitHub，点击**“Sign in”。**

![A screenshot of a login page AI-generated content may be
incorrect.](./media/image5.png)

6.  你已成功在GitHub上創建了一個新賬戶。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

## 任務1：創建Fabric工作區

在這個任務中，你需要創建一個Fabric工作區。工作区包含了本 lakehouse
教程所需的所有内容，包括 lakehouse、数据流、Data Factory
管道、notebooks、Power BI datasets和报表。

1.  打開瀏覽器，導航到地址欄，輸入或粘貼以下URL：+++<https://app.fabric.microsoft.com/+++>
    ,按下**Enter**鍵，並用你的憑證登錄

[TABLE]

2.  Fabric主页，选择 **+New workspace** 瓷砖。

![A screenshot of a computer Description automatically
generated](./media/image7.png)

3.  在右侧的**Create a
    workspace** 面板中，输入以下细节，然后点击**“Apply**”按钮。

[TABLE]

> ![](./media/image8.png)
>
> \[!注\] 要查找你的实验室instant ID，选择“Help”并复制instant ID。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image9.png)
>
> ![](./media/image10.png)

4.  等待部署完成。完成大約需要2-3分鐘。

![](./media/image11.png)

## 任务2：获取你的Fabric workspace ID

构建解决方案时，您需要将workspace ID 作为参数传递。

1.  看看 URL—— workspace ID 是出现在 /groups/ 之后的 GUID：

2.  从 URL 复制**Workspace ID**

> （例如，https://app.fabric.microsoft.com/groups/{workspace-id}/...）并保存在**Notepad**中以备后用

![](./media/image12.png)

## 任务3：开放开发环境

1.  打開browser，進入地址欄，輸入或粘貼以下URL：
    +++<https://github.com/technofocus-pte/agnticapp-for-unified-data/tree/main+++>

![](./media/image13.png)

2.  点击**fork** 即可fork repo。给repo命名唯一名称，然后点击**Create
    repo** 按钮。

![](./media/image14.png)

![](./media/image15.png)

3.  点击 **Code -\> Codespaces -\> Create Codespace on main**

![](./media/image16.png)

4.  等待 Codespaces 环境设置好。完全搭建只需几分钟

![](./media/image17.png)

![](./media/image18.png)

![](./media/image19.png)

## 任务4：配置服务并将应用部署到Azure和Fabric

1.  在終端上執行以下命令。它生成代碼來複製。複製代碼並按回車。

> +++azd auth login+++

![](./media/image20.png)

2.  默認browser打開，輸入生成的驗證碼。輸入代碼並點擊**Next**。

![](./media/image21.png)

![](./media/image22.png)

![](./media/image23.png)

![](./media/image24.png)

3.  登录Azure：

+++az login+++

![](./media/image25.png)

4.  默認browser打開，輸入生成的驗證碼。輸入代碼並點擊**Next**。

![](./media/image26.png)

5.  選擇您的Azure訂閱

![](./media/image27.png)

6.  注册 Microsoft
    Cognitive服务资源提供者（如果您订阅中尚未注册，必须注册）

+++az provider register --namespace Microsoft.CognitiveServices+++

![](./media/image28.png)

\[！alert\] 前往左侧的 **Infra** 文件夹，打开**第 122 行**的
**main.bicep** 文件 ，将*Lab Instance ID* 字符串切换为
<+++@lab.LabInstance.Id>+++。

7.  配置和部署所有资源：

+++azd up+++

![](./media/image29.png)

8.  选择以下数值。

    - **要為 Azure 資源創建環境**，請進入 <+++env@lab.LabInstance.Id>+++

    - **选择一个Azure订阅来使用**: **@lab.CloudSubscription.Name**

    - **“Location”基础设施参数：选择 ResourceGroup1 位置**

    - **Resource
      Group:** **@lab.CloudResourceGroup(ResourceGroup1).Name**

**注意：如果 Codespace 部署在所选的 Azure
区域失败，请更新部署区域并重新运行部署。**

azd env set AZURE_RESOURCE_LOCATION \<region\>

例如：:

azd env set AZURE_RESOURCE_LOCATION westus2

支持的地区:

- westus2

- japaneast

- swedencentral

- northeurope 更新区域后，重新执行部署步骤。

![](./media/image30.png)

![](./media/image31.png)

![](./media/image32.png)

10. 這次部署需要**7-10**
    *分鐘*，才能在賬戶中配置資源並設置解決方案並使用示例數據。

11. 现在部署完成了

![](./media/image33.png)

12. 创建并激活虚拟环境

+++python -m venv .venv+++

![](./media/image34.png)

13. 在 **Visual Studio Code**
    中使用左上角**的菜单图标**，然后导航到**“Terminal → New
    Terminal**”，在工作区中打开新的终端窗口

![](./media/image35.png)

![](./media/image36.png)

14. 在終端中執行以下命令安裝所需的 Python 依賴

+++pip install uv && uv pip install -r scripts/requirements.txt+++

![](./media/image37.png)

![](./media/image38.png)

15. 在Terminal上执行以下命令。它生成代码来复制。复制代码并按Enter。

+++az login+++

![](./media/image39.png)

![](./media/image40.png)

![](./media/image41.png)

16. 從列表中選擇您的**Azure訂閱**以繼續設置流程。

![](./media/image42.png)

17. 從azd部署的輸出中運行bash腳本。用前面步骤创建的Fabric workspace
    ID替换它。剧本将呈现如下样式：

18. python scripts/00_build_solution.py --from 02 --fabric-workspace-id
    \<your-workspace-id\>

![](./media/image43.png)

19. 按Enter開始創建資源

![](./media/image44.png)

![](./media/image45.png)

![](./media/image46.png)

## 任务5：审查Fabric Lakehouse和Data

1.  进入你的+++<https://app.fabric.microsoft.com/+++>工作区

2.  确保资源成功部署

![](./media/image47.png)

3.  點擊**Lakehouse** 確認數據已成功加載。

![](./media/image48.png)

![](./media/image49.png)

4.  返回**Codespace **测试该智能体。

## 任务6：测试代理

1.  要測試代理，請在終端中執行以下命令。

+++python scripts/08_test_agent.py+++

![](./media/image50.png)

2.  输入样题 +++What is the average score from inspections?+++

![](./media/image51.png)

+++What constitutes a failed inspection?+++

![](./media/image52.png)

![](./media/image53.png)

+++Do any inspections violate quality control standards in our
Inspection Procedures?+++

![](./media/image54.png)

![](./media/image55.png)

3.  按**Ctrl+C**取消該過程。

![](./media/image56.png)

## 任务7：创建Fabric data agent

1.  进入你的+++<https://app.fabric.microsoft.com/+++> Microsoft
    Fabric工作区

2.  选择“New item”→搜索“Data Agent”→选择Data Agent

![](./media/image57.png)

3.  输入名称 <+++FabricDataAgent@lab.LabInstance.Id>+++ 并点击**Create**

![](./media/image58.png)

4.  选择**Add data source**以配置新的数据源。

![](./media/image59.png)

5.  請選擇您的Ontology資源以參與本次工作坊

![](./media/image60.png)

![](./media/image61.png)

6.  点击顶部菜单中的 **Agent instructions**。

![](./media/image62.png)

7.  请添加以下agent instructions：

+++You are a helpful assistant that can answer user questions using
data. Support group by in GQL+++

![](./media/image63.png)

![](./media/image64.png)

8.  从顶部菜单点击“Publish”，然后选择“Publish”。

![](./media/image65.png)

![](./media/image66.png)

![](./media/image67.png)

\[!Note\] The Ontology set up may take up to 15 minutes so retry after
some time if you don't see good responses.

9.  To test the agent, run the application and enter the sample
    questions to verify the responses.

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

10. 将**Workspace ID**和**AISkills ID**保存在**Notepad** 中以备后用

![](./media/image75.png)

11. 返回**Codespace **部署并启动应用程序。

## 任務8：部署並啟動應用程序

1.  在部署前**，**执行以下命令将**AZURE_ENV_DEPLOY_APP**环境变量设置为
    **true**。

+++azd env set AZURE_ENV_DEPLOY_APP true+++

![](./media/image76.png)

2.  運行 azd up - 這將為 Azure 資源進行配置

+++azd up+++

![](./media/image77.png)

3.  部署成功完成後，複製網頁應用的URL。

![](./media/image78.png)

4.  請執行以下命令來設置應用權限

+++python scripts/00_build_solution.py --from 09+++

![](./media/image79.png)

5.  按Enter开始配置

![](./media/image80.png)

![](./media/image81.png)

6.  点击应用URL

![](./media/image82.png)

![](./media/image83.png)

![](./media/image84.png)

**示例問題**

為了幫助你入門，以下是你可以在應用中提出的一些**示例問題**：

针对Retail sales分析的用例：

+++Show total revenue by year for last 5 years+++.

![](./media/image85.png)

![](./media/image86.png)

![](./media/image87.png)

![](./media/image88.png)

\[!警報\] 由於日期已占卜，您可能看不到回復，請繼續進行實驗室其他部分。

## 任务9：验证Azure Resources并审查Fabric Lakehouse Data

1.  打開browser，進入+++[https://portal.azure.com+++](https://portal.azure.com+++/)，並用下面的Cloud
    Slice賬號登錄。

2.  选择**Resource groups**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image89.png)

3.  点击你指定的**Resource group**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image90.png)

4.  確保以下資源已成功部署

    - Foundry

    - Foundry project

    - Application Insights

    - Search service

    - Azure Storage account

    - App Service

    - Azure Cosmos DB account

![](./media/image91.png)

## 任务 10：使用 Microsoft Foundry Services 中的 Fabric data agent

1.  选择**Foundry**

![](./media/image92.png)

2.  在Overview面板中，点击“**Go to Foundry portal**”。这会引导你进入
    Microsoft Foundry 门户。

![](./media/image93.png)

![](./media/image94.png)

进入Foundry门户后，从左侧菜单选择“**Agents**”，你会看到已经**pre
created**的代理人。如果未创建，请点击 **+ New agent** 选项以创建。

![](./media/image95.png)

![](./media/image96.png)

3.  選擇新創建的**agent**，右側會打開一個配置面板。输入代理名称为
    +++Fabric Agent+++

![](./media/image97.png)

4.  在同一代理配置窗格中，向下滚动并单击 **+ Add** 為**Knowledge**参数。

![](./media/image98.png)

5.  在**Add knowledge**面板中，选择 **Microsoft Fabric**

![](./media/image99.png)

6.  Click on **+ Create connection**

![](./media/image100.png)

7.  输入你在**任务7\>第6步**保存的自定义密钥，比如 **Workspace
    ID** 和**AISkills ID**。输入连接名称为
    **Fabric-aiskills**，然后点击**Connect。**

![](./media/image101.png)

8.  输入说明

> **你是一个数据助手，负责分析存储在 Microsoft Fabric 中的检查data。**

**使用Fabric Lakehouse**
**dataset来回答关于检查结果和评分的问题。dataset包含以下列：**

**- inspection_id: 每次检查的唯一标识符**

**- ticket_id: 与检查单相关联的标识符**

**- result: 检查结果（通过或不通过）**

**- score: 分配给检查的数值评分**

**您可以分析和總結data，以提供如下見解：**

**- 檢查總數**

**- 通過和未通過檢查的數量**

**- 平均、最高和最低檢查分數**

**- 檢查結果的分配**

**- 檢查或工單的評分趨勢**

**回復時：**

**- 使用Fabric** **data源獲取準確信息。**

**- 根據檢查結果提供清晰的總結和見解。**

**-
在適當情況下，建議使用條形圖或餅圖等可視化工具，以顯示通過與不通過的分佈或分數比較。**

**- 确保答案简洁、准确，且仅基于可用dataset。**

![](./media/image102.png)

9.  从左侧菜单选择**“Agents**”，然后选择**Fabric
    Agent** agent，点击**“Try in playground**”。

![](./media/image103.png)

10. 會打開一個聊天面板，你可以在那裡輸入prompts。客服現在會根據你連接的文檔和datasets進行響應。

示例提示——

+++What constitutes a failed inspection?+++

![](./media/image104.png)

![](./media/image105.png)

+++What is the total number of tickets in the system?+++

![](./media/image106.png)

![](./media/image107.png)

+++Do any inspections violate quality control standards in our
Inspection Procedures?+++

![](./media/image108.png)

![](./media/image109.png)

## 任务11：删除资源

1.  要删除资源，请 在 Azure 门户搜索栏输入 **Resource
    groups**，然后在**Services**中导航并点击**Resource groups**。

![A screenshot of a computer Description automatically
generated](./media/image110.png)

2.  在Resource groups页面，选择你的资源组。

3.  在**Resource Group**主页，选择除 **Fabric
    Capacity**外的所有资源，然后点击**Delete**。

![](./media/image111.png)

4.  在右侧出现的**“Delete
    Resources**”面板中，点击**输入“delete”确认删除**字段，然后点击**Delete** 按钮

![](./media/image112.png)

![](./media/image113.png)

5.  进入你的+++<https://app.fabric.microsoft.com/+++> Microsoft
    Fabric工作区

6.  选择...... 在工作区名称下选择选项，选择**Workspace settings**。

![](./media/image114.png)

7.  选择**“General**”并**Remove this workspace。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image115.png)

8.  点击弹出的警告中“**Delete**”。

![](./media/image116.png)

9.  等待工作區被刪除的通知後，再進入下一個實驗室。

![](./media/image117.png)

**摘要**

这一用例展示了组织如何通过集成 **Microsoft Fabric** 与 **Microsoft
Foundry
来构建智能的代理驱动数据应用**。该解决方案建立了**统一的数据基础**，使存储在
Fabric Lakehouse 和 Warehouse 中的企业数据可以通过 AI
驱动的代理访问和分析。

通过将 **Fabric Data Agent **连接到
Foundry，用户可以使用**自然语言查询**与企业数据集交互，而无需编写复杂的
SQL
或手动分析多个数据源。AI代理檢索相關data，進行分析，並生成諸如平均值、趨勢、總結和分組結果等洞察。

該解決方案還支持Azure服務，包括AI服務、搜索、存儲和Web應用，實現完整的**端到端代理應用架構**。這使得組織能夠將**結構化企業數據與
AI能力**結合，提供對話式分析和自動化洞察。

總體而言，該用例凸顯了基於統一數據平臺構建的**agentic
AI應用**如何簡化數據訪問、加速分析，並支持技術用戶和非技術用戶更快的數據驅動決策。
