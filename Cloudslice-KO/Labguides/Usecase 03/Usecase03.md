### 소개

현대 조직은 복잡한 데이터 이동 없이도 운영 데이터를 신속히 분석하고 의미 있는 인사이트를 제공할 수 있는 지능형 시스템을 필요로 합니다. 이 사용 사례에서는 Microsoft Fabric을 사용해 Azure SQL Database의 데이터를 Fabric 환경으로 미러링하고, 미러링된 데이터를 쿼리하고 분석할 수 있는 Fabric Data Agent를 생성합니다.

이 과정은 샘플 비즈니스 데이터를 포함하는 Azure SQL Database를 생성하는 것으로 시작됩니다. 이 데이터베이스는 Azure SQL Mirroring을 통해 Microsoft Fabric에 미러링되어, Fabric 워크스페이스 내에서 운영 데이터에 거의 실시간으로 접근할 수 있게 됩니다. Mirrored Database가 준비되면, 데이터 소스에 연결하고 자연어 질의에 응답하는 Fabric Data Agent가 구성됩니다.

이 방식은 사용자가 복잡한 SQL 쿼리를 작성하지 않고도 지능형 에이전트를 통해 기업 데이터와 상호작용하며 제품 성과, 고객 분포, 판매 추세에 대한 빠른 인사이트를 얻을 수 있게 합니다.

### 목표

이 실습의 목적은 Azure SQL Database에서 미러링된 운영 데이터를 분석할 수 있는 Fabric Data Agent를 구축하고 구성하는 방법을 보여주는 것입니다.

이 실습을 완료함으로써 다음을 배우게 됩니다:
- 샘플 데이터를 사용해 **Azure SQL Database** 를 생성합니다.
- 데이터와 분석 리소스를 호스팅할 **Microsoft Fabric workspace**를
  생성합니다.

- Azure SQL Mirroring을 사용해 **Azure SQL Database into Microsoft
  Fabric**에 미러링합니다.

- **Fabric Data Agent**를 구성하고 mirrored database에 연결합니다.
- **natural language prompts** 를 사용해 데이터를 질의하고 인사이트를
  도출합니다.

- 샘플 분석 질문을 사용해 에이전트의 응답을 검증합니다.


## **과제 0: 호스트 환경 시간 동기화**

1. VM에서 **Search bar**을 찾아 클릭한 다음, **Settings**를 입력하고 **Best match** 아래에 있는 **Settings**를 클릭하세요.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image1.png)

1. Settings 창에서 **Time & language** 를 찾아 클릭합니다.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image2.png)

1. **Time & language**페이지에서 **Date & time**을 찾아 클릭합니다.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image3.png)

1. 아래로 스크롤하여 **Additional settings** 섹션으로 이동한 다음, **Sync now** 버튼을 클릭하세요. 동기화에는 3~5분이 소요됩니다.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image4.png)

1. **Settings**창을 닫습니다.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image5.png)


## 과제 1: 단일 데이터베이스 만들기 - Azure SQL Database

이 작업에서는 샘플 데이터가 포함된, 모든 설정이 완료된 Azure SQL Database를 생성합니다. AdventureWorksLT 샘플 스키마를 배포하고 테이블을 확인한 다음, 추후 Fabric에서 미러링을 수행하는 데 필요한 서버 연결 정보를 준비하게 됩니다.

1. 브라우저를 열고 +++https://portal.azure.com+++에 접속한 다음, 아래의 cloud slice 계정으로 로그인하세요.

1. Azure Portal 홈 페이지에서 Microsoft Azure command bar왼쪽에 있는 가로줄 3개 모양의 **Azure portal menu** 를 클릭합니다. SQL 데이터베이스를 선택합니다.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image6.png)

1. **+ Create** 클릭하세요

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image7.png)

1. **Create a storage account**  창의 **Basics** 탭에서 아래 세부 정보를 입력하여 스토리지 계정을 만든 다음**, Next:Networking** 을 클릭합니다.

    

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image8.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image9.png)

1. Compute + Storage 섹션에서 **Configure database**를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image10.png)

1. Service tier 드롭다운에서 **Standard(Budget Friendly)를** 선택하고**, DTU에 100**을 입력한 뒤 **Apply**을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image11.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image12.png)

1. **Networking** 탭에서 **Public endpoint**를 선택하고, **Allow Azure services and resources**을 **Yes**로 설정한 다음, **Add current client IP address**를 활성화하고 **Next: Security\> Networking**을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image13.png)

1. **Security**페이지에서 내용을 검토한 후, **Next : Additional settings**을 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image14.png)

1. *Additional settings*탭에서 기존 데이터 사용 아래의 **Sample**을 선택하고, 메시지가 표시되면 **AdventureWorksLT**를 선택한 다음 **OK**을 클릭하고 **Review + create**을 선택하여 진행합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image15.png)

1. **Review + create** 페이지에서 내용을 검토한 후 **Create**를 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image16.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image17.png)

1. **Microsoft.SQLDatabase** 창에서 배포가 완료되면 **Go to resource**버튼을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image18.png)

1. SQL 데이터베이스 페이지에서 **Query editor**를 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image19.png)

1. **Query editor (preview)**,에서 SQL Server **login**에 **sqladmin**, **password**에 +++password321!**+++**를 입력한 다음, **OK**을 클릭하여 데이터베이스에 연결합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image20.png)

1. 모든 샘플 테이블이 성공적으로 배포되었는지 확인합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image21.png)

1. SQL 데이터베이스로 돌아갑니다. **Server name** (1)과 **SQL Database name** (2)을 복사하여 메모장에 붙여넣은 뒤, 다음 작업에서 이 정보를 사용할 수 있도록 메모장을 **Save**합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image22.png)

1. 메인 페이지로 돌아가려면 **Home**을 클릭하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image23.png)

1. **Resource groups**을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image24.png)

1. **Resource Group 1** 리소스 그룹을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image25.png)

1. **SQL server 선택**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image26.png)

1. Identity로 이동하여 System assigned managed identity 상태를 **On**으로 변경한 다음, **Save**을 클릭하여 변경 사항을 적용합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image27.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image28.png)


## 과제 2: Fabric 워크스페이스 만들기

##

이 작업에서는 Fabric 워크스페이스을 생성합니다. 이 작업 영역에는 Lakehouse, Dataflows, Data Factory 파이프라인, notebooks, Power BI 데이터 세트 및 보고서를 포함하여 이 Lakehouse 자습서에 필요한 모든 항목이 포함됩니다.

1. 브라우저를 열고 주소창에 다음 URL을 입력하거나 붙여넣으세요: +++https://app.fabric.microsoft.com/+++ **Enter** 키를 누르고 로그인 정보를 입력해 로그인합니다

    

1. Fabric 홈 페이지에서 **+New workspace**타일을 선택합니다.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image29.png)

1. 오른쪽에 나타나는 **Create a Workspace** 패널에서 다음 세부 사항을 입력하고 **Apply** 버튼을 클릭합니다.

    

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image30.png)

    주석: Lab Instant ID를 확인하려면 Help를 선택하고 Instant ID를 복사하세요.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image31.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image32.png)

1. 배포가 완료될 때까지 기다리세요. 완료하는 데 2~3분 정도 걸립니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image33.png)


## 과제 3: Azure SQL Mirroring을 사용하여 데이터를 미러링하는 솔루션 만들기만들기

이 작업에서는 Azure SQL Mirroring을 사용하여 Azure SQL Database를 Microsoft Fabric에 연결합니다. 테이블을 선택하고 mirrored database를 생성한 뒤, 데이터가 성공적으로 동기화되었는지 확인합니다.

1. 탐색 모음에서 **+New item** 버튼을 클릭하여 새 레이크하우스를 만듭니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image34.png)

1. **Filter by keyword** 검색 상자에 +++Mirrored Azure SQL Database+++를 입력하고 **Mirrored Azure SQL Database** 항목을 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image35.png)

1. **Choose a database connection to get started 창에서 Azure SQL Database를 선택합니다.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image36.png)

1. Connection settings 탭에서 아래 세부 정보를 입력한 후 Connect 버튼을 클릭하십시오.

    

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image37.png)

1. **Choose data** 창에서 **Select all**을 선택하고 **Connect** 버튼을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image38.png)

1. Destination **탭에서 Create mirrored database를 클릭합니다.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image39.png)

1. **Refresh** 을 클릭하여 최신 변경 사항을 업데이트하고 확인합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image40.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image41.png)

1. 아래 이미지와 같이 왼쪽 탐색 메뉴에서 *+++FabricAgent-mirroringdatabase@lab.LabInstance.Id+++*를 찾아 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image42.png)


## 과제 4: 데이터 에이전트를 생성하고 Mirrored Database를 연결합니다.

여기서 새로운 Fabric Data Agent를 생성하고, 미러링된 Azure SQL Database를 데이터 소스로 사용하도록 구성하게 됩니다. 이 에이전트는 미러링된 데이터를 활용하여 자연어 프롬프트에 응답합니다.

1. **Fabric** 홈 페이지에서 **+New item**을 선택하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image43.png)

1. **Filter by item type** 검색 상자에 +++data agent+++를 입력한 후 **Data agent**를 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image44.png)

1. 데이터 에이전트 이름으로 +++FabricDataAgent @lab.LabInstance.Id+++를 입력하고 **Create**를 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image45.png)

1. 새 데이터 원본을 구성하려면 **Add data source** 를 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image46.png)

1. 이번 워크숍을 위한 mirrored database리소스를 선택하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image47.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image48.png)


## 과제 5: 에이전트를 테스트하세요

다음과 같은 분석적인 질문을 통해 데이터 에이전트를 테스트하게 됩니다:
- *어떤 제품 카테고리가 가장 높은 매출을 올리나요?*
- *정가는 높지만 판매량은 낮은 제품 목록을 작성하십시오.*
- *고객 수가 가장 많은 도시는 어디입니까?*


이는 에이전트가 비즈니스 문의를 이해하고 응답할 수 있는 능력을 검증합니다.

1. 모든 테이블에 대해 **SalesLT** 스키마를 선택하십시오.

1. Fabric data agent의 쿼리 패널에 +++ **Which product categories
    generate the highest sales?+++**라는 질문을 입력하고 전송 아이콘을
    클릭하여 에이전트의 응답을 확인하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image49.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image50.png)

1. 에이전트를 테스트하려면 애플리케이션을 실행하고 샘플 질문을 입력하여 응답을 확인하십시오.

    ++++List products with high list price but low sales volume.+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image51.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image52.png)

    +++List the cities with the highest number of customers+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image53.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image54.png)

1. 상단 메뉴에서 **Agent instructions**를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image55.png)

1. 상단 메뉴에서 Publish를 클릭하고 **Publish**를 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image56.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image57.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image58.png)

1. 이제 왼쪽 탐색 창에서+++FabricAgent-mirroringdatabase@lab.LabInstance.Id+++ 를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image59.png)


## 과제 6: 리소스를 삭제

1. 작업 공간 이름 아래에 있는 ... 옵션을 선택하고 **Workspace settings**을 선택하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image60.png)

1. **General** 을 선택한 다음 **Remove this workspace.**

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image61.png)

1. 팝업으로 표시되는 경고 창에서 **Delete**를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image62.png)

1. 워크스페이스가 삭제되었다는 알림이 표시될 때까지 기다린 후, 다음 실습으로 진행하십시오.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image63.png)

1. 브라우저를 열고 +++https://portal.azure.com+++로 이동한 후, 아래에 있는 클라우드 슬라이스 계정으로 로그인하세요.

1. 리소스를 삭제하려면 Azure 포털의 검색 창에 **Resource groups**을 입력하고, **Services** 아래에 있는 **Resource groups** 으로 이동하여 클릭합니다.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image64.png)

1. 리소스 그룹 페이지에서 해당 리소스 그룹을 선택합니다.

1. **Resource Group**홈 페이지에서 **Fabric Capacity**를 제외한 모든 리소스를 선택한 다음, **Delete**를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image65.png)

1. 오른쪽에 나타나는 **Delete Resources** 창에서 **Enter “delete” to confirm deletion** 필드로 이동한 다음, **Delete** 버튼을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image66.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2003/media/image67.png)


### 요약

이번 실습에서 여러분은 Azure SQL Database를 성공적으로 생성하고, Azure SQL Mirroring을 사용하여 해당 데이터를 Microsoft Fabric으로 미러링했습니다. 그런 다음 Fabric Data Agent를 구성하여 미러링된 데이터베이스에 연결하고, 자연어 쿼리를 통해 데이터를 분석했습니다.

해당 에이전트는 판매 실적이 높은 제품군, 가격은 높지만 판매량은 저조한 제품, 그리고 고객 수가 가장 많은 도시를 파악하는 등의 분석적 질문에 답변할 수 있었습니다. 이는 Microsoft Fabric이 운영 데이터 소스와 지능형 에이전트를 통합하여 데이터 탐색을 간소화하고, 더 신속하게 비즈니스 인사이트를 도출할 수 있게 해준다는 점을 보여줍니다.

이 사용 사례는 Microsoft Fabric 생태계 내에서 **data mirroring and AI-powered data agents**를 결합하여 상호작용형의 지능적인 데이터 경험을 구현하는 강력한 역량을 보여줍니다.
