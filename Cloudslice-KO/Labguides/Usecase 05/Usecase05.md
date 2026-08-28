### 소개

오늘날 경쟁이 치열한 디지털 시장에서 전자상거래 기업은 고객 거래, 제품 카탈로그, 웹사이트 상호작용, 결제 시스템 등을 통해 방대한 양의 데이터를 생성합니다. 이러한 데이터에서 유의미한 인사이트를 도출하는 것은 고객 경험을 개선하고, 운영을 최적화하며, 매출을 증대시키는 데 필수적입니다. 하지만 통합된 분석 플랫폼이 없다면 다양한 출처에서 발생하는 대규모 데이터 세트를 관리하고 분석하는 일이 복잡해질 수 있습니다.

급성장하는 전자상거래 기업인 **Zava**는 온라인 플랫폼을 통해 다양한 소비자 제품을 판매하며 매일 수천 건의 주문을 처리하고 있습니다. 이 회사는 주문 관리, 고객 프로필, 제품 재고, 결제 내역 등 다양한 시스템에서 데이터를 수집합니다. 사업이 확장됨에 따라 Zava는 이러한 데이터를 효율적으로 분석하고 비즈니스 팀에 실시간 인사이트를 제공하는 데 있어 어려움에 직면해 있습니다.

이러한 과제를 해결하기 위해 Zava는 **Microsoft Fabric**을 활용한 최신 분석 솔루션을 도입했습니다. Fabric은 데이터 엔지니어링, 데이터 저장, 데이터 변환 및 비즈니스 인텔리전스(BI) 기능을 단일 환경에서 통합적으로 제공하는 플랫폼입니다. Zava는 원본 및 가공된 이커머스 데이터를 Fabric Lakehouse에 저장함으로써 확장 가능한 데이터 관리와 분석을 구현하고 있습니다.

또한, Zava는 **Microsoft Fabric Data Agent**를 활용하여 데이터 접근성과 인사이트 도출 역량을 강화합니다. Fabric Data Agent를 사용하면 비즈니스 사용자와 분석가가 자연어 쿼리를 통해 기업 데이터와 상호 작용할 수 있습니다. 사용자는 보고서를 일일이 찾아보거나 복잡한 쿼리를 작성하는 대신, 다음과 같이 간단히 질문할 수 있습니다:
- “이번 달에 가장 많이 팔린 제품은 무엇인가요?”
- “어떤 지역에서 가장 높은 매출액이 발생했습니까?”
- “지난 분기 고객 주문 추세는 어떠한가요?”


Fabric Agent는 Lakehouse에서 관련 데이터를 자동으로 가져와 인사이트를 생성함으로써, 팀이 비즈니스 성과를 신속하게 파악할 수 있도록 돕습니다. 이러한 지능형 상호작용은 생산성을 높이고 부서 간의 신속한 의사결정을 가능하게 합니다.

이 솔루션을 통해 Zava의 비즈니스 사용자, 분석가 및 경영진은 데이터를 손쉽게 탐색하고 핵심 성과 지표(KPI)를 모니터링하며, 매출 실적, 고객 행동, 제품 수요에 대한 실시간 인사이트를 확보할 수 있습니다. Zava는 고급 분석 기술과 AI 기반 Fabric Agent를 결합하여, 데이터 기반의 성장과 운영 효율성을 뒷받침하는 확장 가능하고 지능적인 이커머스 분석 플랫폼을 구축합니다.

### 목표

- 전자상거래 시맨틱 모델에 연결된 **Fabric Data Agent**를 구축하고
  구성합니다.

- **Fabric Lakehouse** 내에서 데이터를 수집 및 모델링하고, 이를 의미
  체계 모델을 통해 노출합니다.

- **meta‑prompts**와 에이전트 수준의 지침을 활용하여 에이전트의 지능을
  강화하십시오.

- Fabric Data Agent를 **Copilot Studio**에 연결하고 다중 에이전트 통신을
  활성화하십시오.

- Copilot 에이전트를 게시하고 실시간 분석을 위해 **Microsoft Teams**에
  통합합니다.

- Teams에서 비즈니스 인사이트를 직접 쿼리하여 엔드투엔드 흐름을
  테스트합니다.


# 연습 1: Fabric Data Agent 생성 및 구성

## 이 실습에서는 Microsoft Fabric의 기반이 되는 구성 요소를 설정합니다. workspace을 생성하고, lakehouse를 구축하며, 샘플 CSV 데이터 세트를 수집하고, 시맨틱 모델을 생성한 뒤, 분석 관련 질문에 답변할 수 있는 Fabric Data Agent를 구성합니다. 이를 통해 이후 실습 과정 전반에서 활용될 핵심 데이터 인텔리전스 계층을 마련하게 됩니다.

## 과제 1: Fabric workspace 만들기

이 과제에서는 Fabric 워크스페이스을 생성합니다. 이 워크스페이스에는 Lakehouse, Dataflows, Data Factory 파이프라인, notebook, Power BI 데이터 세트 및 보고서를 포함하여 이 Lakehouse 자습서에 필요한 모든 항목이 포함됩니다.

1. 브라우저를 열고 주소창으로 이동한 다음, 다음 URL을 직접 입력하거나 붙여넣으세요: +++https://app.fabric.microsoft.com/+++ **Enter** 키를 누른 후 로그인 정보를 입력하여 로그인하세요

    

1. Fabric 홈페이지에서 **+New workspace** 타일을 선택합니다.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image1.png)

1. 오른쪽에 나타나는 **Create a workspace** 창에서 다음 세부 정보를 입력한 다음, **Apply** 버튼을 클릭합니다.

    

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image2.png)

    주석: Lab Instant ID를 확인하려면 Help를 선택하고 Instant ID를 복사하세요.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image3.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image4.png)

1. 배포가 완료될 때까지 기다리십시오. 완료까지 2~3분이 소요됩니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image5.png)


## 과제 2: lakehouse를 생성하고 샘플 데이터를 수집

이 과제에서는 lakehouse를 설정하고 NYC 택시 샘플 데이터와 추가 CSV 파일을 수집합니다. 이를 통해 Fabric 내에 원시 데이터 세트 기반을 구축하여, 추후 데이터 변환 및 쿼리 작업을 시작할 수 있게 됩니다.

1. 탐색 모음에서 **+New item** 버튼을 클릭하여 새 레이크하우스를 만듭니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image6.png)

1. **Filter by item type** 검색 상자에 +++Lakehouse+++를 입력하고 lakehouse 항목을 선택합니다.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image7.png)

1. **New lakehouse** 대화 상자의 **Name** 필드에 +++fabricagent_lakehouse+++를 입력하고, **Create** 버튼을 클릭하여 새 레이크하우스를 엽니다.

    \[!주석\]**주석**: **fabricagent_lakehouse**앞의 공백을 반드시 제거하세요.

    fabricagent_lak

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image8.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image9.png)

1. **Successfully created SQL endpoint**라는 알림이 표시될 때까지 기다리십시오.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image10.png)

1. **Lakehouse** 페이지에서 **Get data in your lakehouse** 섹션으로 이동한 다음, **Upload files**를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image11.png)

1. Upload files 탭에서 Files 아래에 있는 폴더를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image12.png)

1. VM에서 **C:\LabFiles** 로 이동한 다음, **customers.csv, Orders_Data.csv**및 **products.csv** 파일을 선택하고 **Open** 버튼을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image13.png)

1. 그런 다음, **Upload** 버튼을 클릭하고 닫으십시오.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image14.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image15.png)

1. **Files**를 클릭하고 Refresh를 선택합니다. 파일이 나타납니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image16.png)

1. **Lakehouse** 페이지에서 Explorer 창 아래에 있는 Files를 선택합니다. 이제 마우스를 **Orders_Data.csv** 파일에 올립니다. **Orders_Data.csv** 옆에 있는 수평 점 세 개**(…)**를 클릭합니다. **Load Table**로 이동하여 클릭한 다음, **New table**을 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image17.png)

1. **Load file to new table** 대화 상자에서 **Load** 버튼을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image18.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image19.png)

1. **customers.csv**와 **products.csv**에 대해서도 동일한 과정을 반복하여 표로 변환하십시오.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image20.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image21.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image22.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image23.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image24.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image25.png)

1. 화면 오른쪽 상단의 **Lakehouse** 드롭다운 메뉴에서 **SQL analytics endpoint**를 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image26.png)

1. lakehouse의 **Home** 탭에서 **New semantic model** 을 선택하고, 의미 체계 모델에 추가할 테이블을 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image27.png)

1. **New semantic model** 대화 상자에 +++E-commerce Order Dataset+++를 입력한 다음, 테이블 목록에서 **all** 테이블을 선택하고 **Confirm**을 눌러 새 모델을 생성합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image28.png)

1. 왼쪽 메뉴에서 +++Fabric-Copilot-@lab.LabInstance.Id+++ 작업 영역 아이콘을 선택한 다음, 작업 영역 이름을 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image29.png)


## 과제 3: Fabric data agent 만들기

1. +++Fabric-copilot-@lab.labinstance.id+++ 작업 영역 페이지에서 +New item 버튼을 찾아 클릭한 다음**, **Data agent를 선택합니다.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image30.png)

1. 이름을 <DataAgent_@lab.LabInstance.Id> 로 지정하고**Create**를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image31.png)

1. 새 데이터 원본을 구성하려면 **Add data source**를 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image32.png)

1. 결과에서 **E-commerce Order Dataset ** (유형: Semantic Model)을 선택하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image33.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image34.png)

1. 나열된 표를 사용하여 처음 질문할 때 **all tables**를 선택하면, 데이터 에이전트가 꽤 잘 답변해 줍니다.

1. 예를 들어, +++Who are the top 10 customers by total purchase amount?+++ 라는 질문의 경우

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image35.png)

1. 애플리케이션을 실행하고 예시 질문을 입력하여 응답을 확인하십시오.

    +++Which day has the highest sales?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image36.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image37.png)


## 과제 4: Meta-Prompts로 최적화하기

1. **Setup** 섹션에서 **Agent instructions** 필드를 찾습니다. (또는 상단 탐색 모음에서 **Agent instructions** 을 찾을 수도 있습니다.)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image38.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image39.png)

1. **테스트 창**(오른쪽의 **Test the agent's responses**라고 표시된 영역)에서 이 메타 프롬프트를 사용하여 에이전트 수준의 지침을 생성하세요.

    > Meta-Prompt: 에이전트 수준 명령어 생성:
    >
    > 사용 가능한 데이터 소스를 분석하고, 자신을 위한 에이전트 수준의 지침을
    > 작성하십시오(최대 15,000자).
    >
    Objective: {AGENT_OBJECTIVE}

    Users: {USER_PERSONA}

    데이터 소스를 검토하십시오. 모든 소스, 유형 및 주된 용도를 나열하고, 도메인, 시간적 범위, 주요 주제를 분석하십시오.

    명령어를 생성합니다:

    \## Objective

    \## Data Sources (list with priority)

    \## Key Terminology (infer from columns/measures)

    \## Response Guidelines

    Style: {RESPONSE_STYLE}

    \## Handling Common Topics (3-5 based on available data)

    Custom terms: {CUSTOM_TERMINOLOGY}

    이 meta-prompt를 사용할 때는 아래 값에 따라 프롬프트 내의 변수를 수동으로 교체하거나, **해당** 내용을 Test에 붙여넣으세요:
    - {AGENT_OBJECTIVE}: "비즈니스 인텔리전스를 위한 이커머스 분석 에이전트"
    - {USER_PERSONA}: "비즈니스 분석가 및 영업팀"
    - {RESPONSE_STYLE}: "데이터 출처 및 추세 분석을 포함한 명확한 요약"
    - {CUSTOM_TERMINOLOGY}: 비워두거나 도메인별 용어를 추가

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image40.png)


1. 더 복잡한 쿼리로 개선된 에이전트를 테스트합니다:

    +++How many orders are placed each day?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image41.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image42.png)

    +++Which products have the lowest stock levels?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image43.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image44.png)


## 과제 5: Agent 게시

1. Fabric Agent의 테스트 창에서 이 meta-prompt를 사용하여 에이전트 설명을 생성하세요.

    > Meta-Prompt: Agent 설명 생성
    >
    > Fabric Data Agent로서 자신에 대한 설명을 1~2문장으로 작성하세요(최대
    > 200자).
    >
    > 데이터 소스를 분석하고, 어떤 데이터 영역을 다루며 어떤 질문에 답하는지
    > 설명하십시오.
    >
    > 예시: "소매 판매용 Fabric Data Agent입니다. 매출, 제품, 고객 및 주문
    > 관련 질문에 답변합니다. "
    >
    > 출력은 일반 텍스트만 가능합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image45.png)

1. **Publish**를 클릭하고 생성된 설명을 purpose and capabilities 필드에 붙여넣으세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image46.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image47.png)


# 연습 2: Fabric Agent를 Copilot Studio에 연결하기

## 이 실습은 Copilot Studio가 Fabric Data Agent와 통신할 수 있도록 설정하는 데 중점을 둡니다. Copilot 에이전트를 생성하고 그 동작을 구성한 뒤 Fabric 에이전트와 연결하여, 두 에이전트가 협력해 더 풍부한 인사이트를 도출할 수 있도록 합니다. 이를 통해 플랫폼 간의 멀티 에이전트 통신이 구현됩니다.

## 과제 1:Copilot Studio Agent 만들기

1. 새 브라우저 탭을 열고  +++https://copilotstudio.microsoft.com/+++로 이동합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image48.png)

1. 왼쪽 탐색 영역에서 **Agents**를 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image49.png)

1. 파란색 **+Create blank agent** 버튼을 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image50.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image51.png)

1. 설정을 변경하려면 **Edit** 을 클릭하십시오.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image52.png)

1. 이 설정들로 에이전트를 구성하십시오:

    - **이름**: 이커머스 RAG 에이전트
    - **설명**: 전자상거래 비즈니스 지식 및 지원에 특화된 Microsoft
    Fabric data agent와 연결된 에이전트

    - 에이전트 모델을 선택하고 **Claude Sonnet 4.5**를 선택하세요.
    - **지침**: 아래 코드 블록에서 지침을 복사하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image53.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image54.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image55.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image56.png)


1. 우측 상단의 **Publish**를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image57.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image58.png)


## 과제 2: Copilot Studio의 연결된 에이전트로 Fabric agent 추가하기

1. **에이전트 생성 후 Agents 탭으로 이동하여 +Add agent를 클릭합니다.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image59.png)

1. **Connect to an external agent**을 클릭하고 **Microsft Fabric (preview)** 을 선택합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image60.png)

1. *Connection이라고 표시된다면 :* 연결되지 않음으로 표시되어 있다면, 연결되지 않음 옆의 드롭다운 메뉴를 클릭하고 **Create new connection**를 선택하세요. 계정의 이메일 주소가 제대로 표시되는지 확인한 후 **Next**을 클릭하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image61.png)

1. **Create**를 클릭하고 이 실습에 사용한 것과 동일한 계정으로 로그인합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image62.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image63.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image64.png)

1. Fabric Data Agent를 선택합니다.

    - 연습 \#1\>과제 3에서 생성한 에이전트 이름을 찾으십시오.
    - 클릭하여 선택하세요

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image65.png)


1. **agent name**을 +++DataAgent-@lab.LabInstance.Id+++로 입력하고 **connection**을 확인한 다음, **Add and configure**을 클릭하여 에이전트 설정을 진행합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image66.png)

1. 에이전트를 사용할 수 있도록 하려면 **Publish**를 클릭하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image67.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image68.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image69.png)


## 과제 3: 연결된 Fabric Data Agent 테스트

1. 점진적 쿼리를 사용하여 Fabric Data Agent 연결 테스트:

    +++What are the top 10 highest value orders?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image70.png)

1. **Allow**을 클릭하여 필요한 권한을 부여합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image71.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image72.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image73.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image74.png)

    **주석:** 응답 생성 과정이 완료되는 데 **5~6분**이 소요될 수 있습니다.

    +++What is the average price per category?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image75.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image76.png)

    > +++**What percentage of orders use credit card vs PayPal vs debit
    > card?**+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image77.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image78.png)

    +++What is the revenue by payment method?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image79.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image80.png)


# 연습 3: Fabric Data Agent를 Teams에 연결.

## 이 실습에서는 Copilot 에이전트를 Teams에 게시하여, 비즈니스 사용자가 협업 앱 내에서 직접 기업 데이터에 액세스할 수 있도록 합니다. 또한 몇 가지 BI 쿼리를 실행하고 Teams 내에서 실시간 응답을 확인하여 에이전트의 기능을 검증하게 됩니다.

## 과제 1: Copilot 기능 추가

1. **E-commerce RAG Agent**에서 **+ (Add)** 아이콘을 클릭하고 **Channels**를 선택하여 에이전트 채널 설정을 구성합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image81.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image82.png)

1. **Teams and Microsoft 365 Copilot** 선택

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image83.png)

1. Add Channel를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image84.png)

1. **Microsoft Teams**에서 에이전트를 열고 테스트하려면 **See agent in Teams**를 선택하세요.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image85.png)

1. **Open Microsoft Teams** 를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image86.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image87.png)

1. **Sign in**을 클릭

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image88.png)

1. 제공된 자격 증명을 입력하여 로그인하고 계속 진행하십시오.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image89.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image90.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image91.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image92.png)

1. **Add**를 클릭합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image93.png)

1. 앱이 성공적으로 추가되면 Open 버튼을 클릭하여 Microsoft Teams에서 E-commerce RAG 에이전트를 실행합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image94.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image95.png)


## 과제 2: 연결된 Fabric Data Agent 테스트

1. 점진적 쿼리를 사용하여 Fabric Data Agent 연결을 테스트합니다:

    +++What is the revenue trend over time?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image96.png)

1. **Allow** 을 클릭하여 필요한 권한을 부여합니다.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image97.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image98.png)

    +++What are the top 10 highest value orders?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image99.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image100.png)

    +++Which payment method is used the most?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image101.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntrio/refs/heads/main/Cloudslice-KO/Labguides/Usecase%2005/media/image102.png)


### 요약

이 사용 사례는 전자상거래 기업이 **Copilot Studio**를 통해 **Microsoft Fabric Data Agents**와 **Microsoft Teams**를 통합함으로써 실시간 인사이트, 자연어 기반 분석 및 다중 에이전트 협업을 구현하는 방법을 중점적으로 다룹니다. 통합 분석 플랫폼(Microsoft Fabric)과 대화형 AI(Copilot Studio 및 Teams)를 결합하면, 비즈니스 사용자는 별도의 쿼리 작성 없이도 매출 동향, 제품 인사이트 및 고객 행동 정보를 원활하게 확인할 수 있습니다. 또한 이 솔루션은 AI 에이전트가 Fabric Lakehouse에서 데이터를 가져오고, 지침을 활용해 응답 내용을 보강하며, 다른 에이전트와 협력하여 비즈니스 인텔리전스 워크플로를 효율화하는 과정을 보여줍니다.
