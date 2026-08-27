# 사용 사례 04 - 통합적이고 지능적인 데이터 인사이트를 위해 Fabric Data Agent를 Microsoft Foundry에 연결

### 소개

현대 조직은 여러 시스템에서 대량의 데이터를 생성하므로 비즈니스 사용자와 분석가가 통찰력에 빠르게 액세스하기가 어렵습니다. 데이터는 사일로에 저장되는 경우가 많으므로 정보를 추출, 분석, 해석하려면 기술 전문 지식이 필요합니다.

Microsoft Fabric 통합 데이터 플랫폼은 분석, 데이터 엔지니어링, 비즈니스 인텔리전스 기능을 단일 환경으로 통합하여 이러한 과제를 해결합니다. Azure AI Foundry의 에이전트 기반 AI 기능을 통합함으로써, 기업은 자연어와 자동화된 워크플로를 활용해 기업 데이터와 상호 작용하는 지능형 애플리케이션을 구축할 수 있습니다.

Unified Data Foundation Solution Accelerator 를 위한 Agentic Applications은 AI 기반 에이전트가 통합된 기업 데이터를 활용하여 질문에 답하고, 데이터 분석을 자동화하며, 기술 및 비기술 사용자 모두에게 인사이트를 제공하는 방법을 보여줍니다. 이러한 Agentic Applications은 작업을 조정하고 관련 데이터를 검색하며 상황에 맞는 응답을 생성함으로써, 더 빠른 의사결정과 운영 효율성 향상을 가능하게 합니다.

이 활용 사례에서 조직은 AI 에이전트를 사용하여 매출 실적, 고객 행동, 제품 트렌드와 같은 비즈니스 데이터를 분석할 수 있습니다. 사용자는 여러 데이터 세트를 수동으로 조회하는 대신, 자연어로 간단히 질문하고 시스템으로부터 즉시 실행 가능한 인사이트를 얻을 수 있습니다.

### 목표

이 사용 사례의 목적은 조직이 **agentic AI with a unified data foundation** 를 활용하여 데이터 접근성과 의사결정을 개선하는 방법을 보여주는 것입니다.

주요 목표는 다음과 같습니다:

### Microsoft Fabric에서 통합 데이터 기반 구축

- Lakehouse, Warehouse 및 semantic models을 포함하는, 거버넌스가 적용된
  Fabric 작업 영역을 생성합니다.

- 분석을 위해 엔터프라이즈 데이터셋을 로드하고 검증합니다.


**2. Fabric Data Agent** **빌드 및 구성**

- 자연어를 사용하여 데이터셋을 쿼리할 수 있는 **Fabric Data Agent** 를
  생성합니다.

- Ontology 리소스를 연결하고 기업별 쿼리를 지원하기 위한 에이전트 지침을
  정의합니다.


### 3. Azure 및 Foundry 구성 요소 배포

- Foundry 프로젝트, AI 서비스, 검색, 스토리지, 앱 서비스 등 Azure
  resources를 프로비저닝합니다.

- Azure Developer CLI(azd)를 통해 지원 구성 요소를 배포합니다.


### 4. Fabric Data Agent를 Microsoft Foundry에 연결하십시오.

- Foundry 내에서 AI 에이전트를 생성하거나 구성합니다.
- Workspace ID와 AI Skills ID를 사용하여 에이전트를 Microsoft Fabric에
  연결합니다.

- 에이전트가 Fabric 데이터를 해석하고 분석할 수 있도록 도메인별 지침을
  제공합니다.


### 5. Conversational Analytics 및 Automated Insights 활성화

- Foundry Playground에서 실제 비즈니스 쿼리로 에이전트를 테스트하세요.
- Fabric Lakehouse 데이터 세트를 활용한 자연어-데이터 검색 워크플로를
  시연합니다.

- 검사 합격/불합격률, 평균, 추세, 그룹별 요약 등의 인사이트를
  제공합니다.


**6. 엔드투엔드 Agentic Application Workflow** **시연**

- Foundry 에이전트, Fabric 데이터 소스, Azure 인프라를 통합하여 기능적인
  웹 애플리케이션을 구축하십시오.

- 지능형 데이터 상호작용, 자동화된 추론 및 인사이트 도출을 검증합니다.


### 솔루션 아키텍처

![Architecture Diagram](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image1.png)

이 솔루션은 Microsoft Fabric과 Microsoft Foundry를 결합하여 정형 데이터와 비정형 문서를 모두 활용해 질문에 답변할 수 있는 AI 솔루션을 구현합니다:
- **Microsoft** **Fabric**은 Lakehouse, Warehouse, 그리고 자연어를 SQL로
  변환하는 Fabric IQ 시맨틱 계층을 통해 데이터 계층을 제공합니다.

- **Microsoft Foundry**는 문서 검색을 위한 Foundry IQ와 두 가지 기능을
  조율하는 Orchestrator Agent를 포함한 AI 에이전트를 호스팅합니다.

- **Azure AI Services**가 언어 모델(GPT-4o-mini)과 임베딩을 구동합니다.
- **Azure AI Search** 는 의미론적 검색을 위해 문서 벡터를 저장합니다.


### 전제 조건

- **GitHub 계정: 본인의 GitHub 로그인 계정 정보를 갖추고 계셔야
  합니다. 계정이 없으시면 다음을 방문하여 계정을 생성해 주십시오: +++<https://github.com/signup?user_email=&source=form-home-signup+++>**


## 과제 0: GitHub 계정 만들기

이 작업에서는 이번 실습에서 사용한 것과 동일한 테넌트 자격 증명을 사용하여 새로운 **Github account** 을 생성합니다.

1. 다음 링크 +++<https://github.com/+++> 를 통해 GitHub로 이동한 다음, **Sign up**을 클릭하여 진행하세요.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image2.png)

1. 이제 새로운 GitHub 계정을 생성하려면 **email**, **password**, 그리고 고유한 **username**을 입력한 다음 **Continue**버튼을 클릭하세요.

    ![A screenshot of a login box AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image3.png)

1. 화면의 지시에 따라 **verification** **puzzle** 을 시작하세요. **Submit**을 클릭하세요.

1. 이메일로 받으신 **verification** **code** 를 입력하세요.

    ![A screenshot of a email form AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image4.png)

1. 이제 계정 정보로 GitHub에 로그인하고 **Sign in**을 클릭하세요.

    ![A screenshot of a login page AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image5.png)

1. GitHub에서 새 계정을 성공적으로 생성했습니다.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image6.png)


## 과제 1: Fabric workspace 만들기

이 작업에서는 Fabric 워크스페이스을 생성합니다. 이 워크스페이스에는 Lakehouse, Dataflows, Data Factory 파이프라인, notebook, Power BI 데이터 세트 및 보고서를 포함하여 이 Lakehouse 자습서에 필요한 모든 항목이 포함됩니다.

1. 웹 브라우저를 열고 주소 표시줄에 다음 URL을 입력하거나 붙여넣은 다음: +++<https://app.fabric.microsoft.com/+++> press the **Enter** 키를 누르고 계정 정보로 로그인하세요.

    

1. Fabric 홈 페이지에서 **+New workspace**타일을 선택합니다.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image7.png)

1. 오른쪽에 나타나는 **Create a workspace** 창에서 다음 세부 정보를 입력한 다음, **Apply** 버튼을 클릭합니다.

    

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image8.png)

    > \[!주석\] Lab Instant ID를 확인하려면 'Help'를 선택하고 Instant ID를
    > 복사하세요.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image9.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image10.png)

1. 배포가 완료될 때까지 기다리십시오. 완료까지 2~3분이 소요됩니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image11.png)


## 과제 2: Fabric workspace ID를 가져오세요.

솔루션을 빌드할 때 매개변수로 전달할 워크스페이스 ID가 필요합니다.

1. URL을 확인해 보세요- 워크스페이스 ID는 /groups/ 뒤에 표시되는 GUID입니다:

1. URL에서 **Workspace ID** (예: https://app.fabric.microsoft.com/groups/{workspace-id}/...) 를 복사하여 나중에 사용할 수 있도록 **Notepad**에 저장합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image12.png)


## 과제 3: 개방형 개발 환경

1. 브라우저를 열고 주소창으로 이동하여 다음 URL을 입력하거나 붙여넣으세요: +++<https://github.com/technofocus-pte/agnticapp-for-unified-data/tree/main+++>

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image13.png)

1. **fork**를 클릭하여 repo를 fork합니다. repo에 고유한 이름을 지정하고 **Create** **repo** 버튼을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image14.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image15.png)

1. **Code -\> Codespaces -\> Create Codespace on main**을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image16.png)

1. Codespaces 환경이 설정될 때까지 기다리세요. 설정이 완료되기까지 몇 분 정도 소요됩니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image17.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image18.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image19.png)


## 과제 4: 서비스를 프로비저닝하고 Azure 및 Fabric에 애플리케이션을 배포

1. 터미널에서 다음 명령을 실행합니다. 그러면 복사할 코드가 생성됩니다. 해당 코드를 복사한 후 Enter 키를 누르세요.

    `azd auth login`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image20.png)

1. 생성된 코드를 입력하여 인증할 수 있도록 기본 브라우저가 열립니다. 코드를 입력하고 **Next**을 클릭하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image21.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image22.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image23.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image24.png)

1. Azure에 로그인:

    `az login`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image25.png)

1. 생성된 코드를 입력하여 인증할 수 있도록 기본 브라우저가 열립니다. 코드를 입력하고 **Next**을 클릭하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image26.png)

1. Azure Subscription을 선택하세요

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image27.png)

1. Microsoft Cognitive Services 리소스 공급자를 등록합니다(구독에 아직 등록되지 않은 경우 필수).

    `az provider register --namespace Microsoft.CognitiveServices`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image28.png)

    \[!알림\] 왼쪽에 있는**Infra** 폴더로 이동한 다음, **main.bicep** 파일의**line 122** 열고*Lab Instance ID* 문자열을 @lab.LabInstance.Id 로 변경하십시오.

1. 모든 리소스 프로비저닝 및 배포:

    `azd up`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image29.png)

1. 아래 값을 선택하세요.

    - **Azure resources를 위한 환경을 생성하려면**,
    +++env@lab.LabInstance.Id+++ 를 입력하세요.

    - **사용할 Azure Subscription
    선택**: **@lab.CloudSubscription.Name**

    - **Location인프라 매개변수:** **ResourceGroup1의 Location를
    선택합니다.**

    - **리소스 그룹:** **@lab.CloudResourceGroup(ResourceGroup1).Name**


### 주석: 선택한 Azure 리전에서 Codespace 배포가 실패하면 배포 리전을 변경하고 배포를 다시 실행하십시오.

    azd env set AZURE_RESOURCE_LOCATION *{region}*

    예를 들어:

    azd env set AZURE_RESOURCE_LOCATION westus2

    지원 지역:
    - westus2
    - japaneast
    - swedencentral
    - northeurope 지역을 업데이트한 후 배포 단계를 다시 실행하십시오.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image30.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image31.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image32.png)


1. 이번 배포 과정에서는 계정의 리소스를 프로비저닝하고 샘플 데이터로 솔루션을 설정하는 데**7-10** 분이 소요됩니다.

1. 이제 배포가 완료되었습니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image33.png)

1. 가상 환경 만들기 및 활성화하기

    `python -m venv .venv`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image34.png)

1. **Visual Studio Code**의 왼쪽 상단 **menu icon** 을 클릭한 다음, **Terminal → New Terminal**로 이동하여 작업 공간에 new terminal창을 엽니다. ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image35.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image36.png)

1. 터미널에서 다음 명령어를 실행하여 필요한 Python 종속성을 설치하십시오.

    `pip install uv && uv pip install -r scripts/requirements.txt`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image37.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image38.png)

1. 터미널에서 다음 명령을 실행하세요. 복사할 코드가 생성됩니다. 해당 코드를 복사한 후 Enter 키를 누르세요.

    `az login`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image39.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image40.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image41.png)

1. 설정 프로세스를 계속 진행하려면 목록에서 **Azure subscription**을 선택하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image42.png)

1. azd 배포 결과에서 출력된 bash 스크립트를 실행합니다. 여기서 를 이전 단계에서 생성한 Fabric workspace ID로 대체하십시오. 스크립트는 다음과 같이 표시됩니다:

1. python scripts/00_build_solution.py --from 02 --fabric-workspace-id *{your-workspace-id}*

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image43.png)

1. Enter를 눌러 리소스 생성을 시작합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image44.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image45.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image46.png)


## 과제 5: Fabric Lakehouse 및 데이터 검토

1. +++<https://app.fabric.microsoft.com/+++> 워크스페이스으로 이동하세요

1. 리소스가 성공적으로 배포되었는지 확인하세요

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image47.png)

1. **Lakehouse**를 클릭하여 데이터가 성공적으로 로드되었는지 확인합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image48.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image49.png)

1. **Codespace**로 돌아가 에이전트를 테스트합니다.


## 과제 6: 에이전트를 테스트하세요.

1. 에이전트를 테스트하려면 터미널에서 다음 명령을 실행하십시오.

    `python scripts/08_test_agent.py`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image50.png)

1. 예시 질문을 입력하세요+++What is the average score from
    inspections?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image51.png)

    +++What constitutes a failed inspection?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image52.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image53.png)

    +++Do any inspections violate quality control standards in our
    Inspection Procedures?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image54.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image55.png)

1. **Ctrl+C** 를 눌러 프로세스를 취소하십시오.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image56.png)


## 과제 7: Fabric data agent만들기

1. Microsoft Fabric workspace +++<https://app.fabric.microsoft.com/+++> 으로 이동합니다.

1. "New item" 선택 → "Data Agent" 검색 → Data agent선택

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image57.png)

1. 이름으로 +++FabricDataAgent@lab.LabInstance.Id+++ 를 입력하고 **Create**를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image58.png)

1. 새 데이터 원본을 구성하려면 **Add data source**를 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image59.png)

1. 이번 워크숍을 위한 Ontology 리소스를 선택하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image60.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image61.png)

1. 상단 메뉴에서 **Agent instructions**를 클릭하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image62.png)

1. 아래의 에이전트 지침을 추가하십시오:

    +++You are a helpful assistant that can answer user questions using
    data. Support group by in GQL+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image63.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image64.png)

1. 상단 메뉴에서 Publish를 클릭하고 Publish를 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image65.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image66.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image67.png)

    \[!주석\] Ontology 설정에 최대 15분이 소요될 수 있으므로, 적절한 응답이 확인되지 않으면 잠시 후 다시 시도해 주십시오.

1. 에이전트를 테스트하려면 애플리케이션을 실행하고 샘플 질문을 입력하여 응답을 확인하십시오.

    +++How many tickets are high priority+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image68.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image69.png)

    +++What is the average score from inspections?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image70.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image71.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image72.png)

    `Show tickets grouped by status`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image73.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image74.png)

1. 나중에 사용할 수 있도록 **Workspace ID**와 **AISkills ID**를 **Notepad**에 저장하십시오.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image75.png)

1. 애플리케이션을 배포하고 실행하기 위해 **Codespace**로 돌아갑니다.


## 과제 8: 애플리케이션을 배포하고 실행하십시오.

1. 배포 전에 다음 명령을 실행하여 **AZURE_ENV_DEPLOY_APP** 환경 변수를 **true**로 설정하십시오.

    `azd env set AZURE_ENV_DEPLOY_APP true`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image76.png)

1. azd up 실행 - Azure resources를 프로비저닝합니다.

    `azd up`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image77.png)

1. 배포가 성공적으로 완료되면 웹 앱 URL을 복사합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image78.png)

1. 다음 명령을 실행하여 앱 권한을 설정합니다.

    `python scripts/00_build_solution.py --from 09`

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image79.png)

1. Enter 키를 눌러 구성을 시작합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image80.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image81.png)

1. 앱 URL을 클릭하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image82.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image83.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image84.png)


### 샘플 질문

시작하시는 데 도움이 되도록, 앱에서 물어볼 수 있는 몇 가지 **샘플 질문**를 준비했습니다:

소매 판매 분석 사용 사례:

`Show total revenue by year for last 5 years`.

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image85.png)

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image86.png)

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image87.png)

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image88.png)

\[!알림\] 표시된 날짜로 인해 응답이 확인되지 않을 수 있으니, 남은 실습 과정을 계속 진행해 주시기 바랍니다.

## 과제 9: Azure resources 확인 및 Fabric Lakehouse 데이터 검토

1. 브라우저를 열고 ++++++https://portal.azure.com+++/+++ 에 접속한 다음, 아래의 클라우드 슬라이스(cloud slice) 계정으로 로그인하세요.

1. **Resource groups**선택하기

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image89.png)

1. 할당된 **Resource group**을 클릭하세요.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image90.png)

1. 아래 리소스가 성공적으로 배포되었는지 확인하세요

    - Foundry
    - Foundry 프로젝트
    - 애플리케이션 인사이트
    - Search service
    - Azure Storage account
    - 앱 서비스
    - Azure Cosmos DB account

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image91.png)


## 과제 10: Microsoft Foundry Services의 Fabric data agent를 사용하세요

1. **Foundry** 선택하기

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image92.png)

1. 개요 창에서 **Go to Foundry portal** 을 클릭합니다. 그러면 Microsoft Foundry 포털로 이동합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image93.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image94.png)

    Foundry Portal로 이동한 후 왼쪽 메뉴에서 **Agents**를 선택하면 **pre created**에이전트를 확인할 수 있습니다. 만약 생성되어 있지 않다면, **+ New agent**옵션을 클릭하여 에이전트를 생성해 주십시오.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image95.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image96.png)

1. 새로 생성된 **agent**를 선택하면 오른쪽에 구성 창이 열립니다. 에이전트 이름을 +++Fabric Agent+++로 입력하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image97.png)

1. 동일한 에이전트 구성 창에서 아래로 스크롤하여 **Knowledge** 매개변수 항목의 **+ Add**를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image98.png)

1. **Add knowledge** 창에서 **Microsoft Fabric**을 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image99.png)

1. **+ Create connection** 을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image100.png)

1. **Task 7 \> Step 6**에서 저장한 **Workspace ID** 및 **AISkills ID**와 같은 사용자 지정 키를 입력합니다. 연결 이름을 **Fabric-aiskills**로 지정하고 **Connect**을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image101.png)

1. 지침 입력

    > **당신은 Microsoft Fabric에 저장된 검사 데이터를 분석하는 데이터 보조
    > 도구입니다.**


### Fabric Lakehouse 데이터 세트를 사용하여 검사 결과 및 점수에 관한 질문에 답하십시오. 이 데이터 세트에는 다음과 같은 열이 포함되어 있습니다.

### - inspection_id: 각 검사에 대한 고유 식별자

### - ticket_id: 검사 티켓과 관련된 식별자

### - result: 검사 결과 (합격 또는 불합격)

### - score: 검사에 부여된 수치 점수

### 데이터를 분석하고 요약하여 다음과 같은 인사이트를 제공할 수 있습니다:

### - 검사 총 횟수

### - 통과한 검사와 실패한 검사의 수

### - 평균, 최고 및 최저 점검 점수

### - 검사 결과의 배포

### - 검사나 티켓별 점수 추세

### 응답할 때:

### - 정확한 정보를 가져오려면 Fabric 데이터 소스를 사용하십시오.

### - 점검 결과를 바탕으로 명확한 요약과 통찰을 제공하십시오.

### - 적절한 경우, 합격과 불합격의 분포나 점수 비교를 보여주기 위해 막대 그래프나 원형 그래프와 같은 시각화 자료를 제안하십시오.

### - 답변은 간결하고 정확하며, 제공된 데이터셋만을 바탕으로 작성되어야 합니다.

![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image102.png)

1. 왼쪽 메뉴에서 **Agents**를 선택한 다음, **Fabric Agent**를 선택하고 **Try in playground**를 클릭하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image103.png)

1. 프롬프트를 입력할 수 있는 채팅 패널이 열립니다. 이제 에이전트는 연결된 문서와 데이터셋을 사용하여 응답합니다.

    샘플 프롬프트 -

    +++What constitutes a failed inspection?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image104.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image105.png)

    +++What is the total number of tickets in the system?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image106.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image107.png)

    +++Do any inspections violate quality control standards in our
    Inspection Procedures?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image108.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image109.png)


## 과제 11: 리소스를 삭제

1. 리소스를 삭제하려면 Azure 포털의 검색 창에 **Resource groups**을 입력하고, **Services** 아래에 있는 **Resource groups**으로 이동하여 클릭합니다.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image110.png)

1. 리소스 그룹 페이지에서 리소스 그룹을 선택합니다.

1. **Resource Group** 홈 페이지에서 **Fabric Capacity**를 제외한 모든 리소스를 선택한 다음, **Delete**를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image111.png)

1. 오른쪽에 나타나는 **Delete Resources** 창에서 **Enter "delete" to confirm deletion**입력란으로 이동한 다음, **Delete** 버튼을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image112.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image113.png)

1. Microsoft Fabric workspace +++<https://app.fabric.microsoft.com/+++> 으로 이동합니다.

1. 워크스페이스 이름 아래에 있는 ... 옵션을 선택하고 **Workspace settings**을 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image114.png)

1. **General을 선택하고 Remove this workspace.**

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image115.png)

1. 팝업으로 나타나는 경고 창에서 **Delete**를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image116.png)

1. 다음 실습을 진행하기 전에 워크스페이스가 삭제되었다는 알림을 기다리십시오.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2004/media/image117.png)


### 요약

이 사용 사례는 조직이 **Microsoft Fabric**과 **Microsoft Foundry**를 통합하여 **지능형 에이전트 기반 데이터 애플리케이션을** 구축하는 방법을 보여줍니다. 이 솔루션은 Fabric Lakehouse 및 Warehouse에 저장된 기업 데이터에 AI 기반 에이전트를 통해 접근하고 이를 분석할 수 있는 **통합 데이터 기반을** 마련합니다.

사용자는 **Fabric Data Agent**를 Foundry에 연결함으로써, 복잡한 SQL을 작성하거나 여러 데이터 소스를 수동으로 분석하는 **대신 자연어 쿼리를** 사용하여 기업 데이터셋을 활용할 수 있습니다. 이 AI 에이전트는 관련 데이터를 검색 및 분석하고, 평균, 추세, 요약, 그룹화된 결과 등의 인사이트를 생성합니다.

또한 이 솔루션은 AI 서비스, 검색, 스토리지, 웹 애플리케이션 등 지원 Azure 서비스를 프로비저닝하여 완벽한 **엔드투엔드 에이전트 기반 애플리케이션 아키텍처를** 구현합니다. 이를 통해 **조직은 구조화된 기업 데이터를 AI 기능**과 결합하여 대화형 분석 및 자동화된 인사이트를 제공할 수 있습니다.

전반적으로 이 사용 사례는 통합 데이터 플랫폼을 기반으로 구축된 **에이전트형 AI 애플리케이션이** 기술 및 비기술 사용자 모두를 위해 데이터 접근을 간소화하고, 분석을 가속화하며, 더 신속한 데이터 기반 의사결정을 지원하는 방식을 잘 보여줍니다.
