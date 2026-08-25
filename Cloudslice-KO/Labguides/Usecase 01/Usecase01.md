사용 사례 1- 의미론에서 인사이트로: Fabric Data Agents와 함께 Fabric IQ
Ontology 활용하기

**소개**

현대 데이터 플랫폼에서는 기업들이 다양한 데이터 소스와 분석 모델 전반에
걸쳐 의미를 통합하는 **비즈니스 중심의 시맨틱 계층**이 필요한 경우가
많습니다. Microsoft Fabric IQ의 Ontology (Preview) 기능을 사용하면
**기업 개념**(예: 제품, 매장, 이벤트 등)과 **그 관계를** 정의하고,
이러한 정의를 Lakehouse, Semantic Model 및 Event Stream 전반의 실제
데이터에 바인딩함으로써 이 계층을 구축할 수 있습니다.

이 시나리오에서는 여러 매장에서 아이스크림을 판매하는 가상의 회사인
‘**Lakeshore Retail’**을 다룹니다. 이 튜토리얼은 샘플 데이터를 활용하여
환경을 설정하고, ‘매장(Store)’, ‘제품(Products)’, ‘판매
이벤트(SaleEvent)’와 같은 비즈니스 개념을 반영하는 Ontology를 구축하는
방법을 보여줍니다. 또한 Eventhouse에서 제공하는 냉동고 온도와 같은
스트리밍 데이터를 이러한 개념에 연결하여, Ontology가 **도메인 간 추론 및
쿼리를** 지원할 수 있도록 합니다. 예를 들어, “냉동고 온도가 –18 °C
이상으로 상승할 때 아이스크림 판매량이 감소하는 매장은 어디인가?”와 같은
쿼리를 처리할 수 있습니다.

**목표**

- Lakehouse, Eventhouse, Ontology(Preview)를 포함한 필수 서비스를 갖춘
  Microsoft Fabric workspace를 준비합니다.

- 매장, 제품, 판매 이벤트, 냉동고와 같은 핵심 엔터티 유형을 정의하여
  비즈니스 중심의 Ontology를 구축합니다.

- OneLake 테이블의 ind 정적 데이터와 Eventhouse의 시계열 데이터를
  Ontology 엔터티에 연결합니다.

- 실제 비즈니스 프로세스를 나타내기 위해 엔터티 간의 의미 있는 관계를
  생성합니다(예: 매장은 판매 행사를 가지며, 매장은 냉동고를 운영함).

- 엔티티 인스턴스, 관계 그래프, 쿼리 빌더 필터를 사용하여 Ontology를
  탐색하고 검증하십시오.

- Ontology를 Fabric Data Agent(Preview)와 통합하여 자연어 쿼리 기능을
  활성화합니다.

# 연습 1: 환경 설정

## 과제 1: Fabric workspace만들기

이 작업에서는 Fabric workspace를 생성합니다. 이 워크스페이스에는
lakehouse, dataflows, Data Factory pipelines, notebooks, Power BI
데이터셋, 보고서 등 이 lakehouse 튜토리얼에 필요한 모든 항목이 포함되어
있습니다.

1.  브라우저를 열고 주소창에 다음 URL을 입력하거나 붙여넣으세요:
    +++https://app.fabric.microsoft.com/+++ 그리고 **엔터 키를** 누른 뒤
    로그인 정보를 입력해 로그인하세요

[TABLE]

2.  워크스페이스 창에서 **+New workspace** 타일을 클릭합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

3.  오른쪽에 나타나는 **워크스페이스 만들기** 창에서 다음 내용을 입력한
    후 **적용** 버튼을 클릭합니다.

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image2.png)
>
> ![](./media/image3.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image4.png)

## 과제 2: lakehouse을 만들기

1.  네비게이션 바에서 **+New item** 버튼을 클릭하여 새 레이크하우스를
    만드세요.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image5.png)

2.  필터에서 **+++Lakehouse+++** 타일을 선택하세요.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image6.png)

3.  **New lakehouse** 대화 상자에서 **Name** 필드에 **+++IQ_Lakehouse
    +++**를 입력하고 lakehouse schemas의 **unselect**을 합니다.
    **Create** 버튼을 클릭한 다음 new lakehousse를 엽니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image7.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image8.png)

4.  **Successfully created SQL endpoint**라는 알림이 표시됩니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

## 과제 3: 샘플 데이터를 가져오기

1.  **IQ_Lakehouse** 페이지에서 **Get data in your lakehouse** 섹션으로
    이동한 후, **Upload files as shown in the below image** 를
    클릭하세요.

> ![](./media/image10.png)

2.  파일 업로드 탭에서 파일 아래에 있는 폴더를 클릭합니다

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image11.png)

3.  가상 머신(VM)에서 **C:\LabFiles\LabFiles** 폴더로 이동한 다음,
    **DimProducts.csv, DimStore.csv, FactSale.csv**및
    **Freezer.csv** 파일을 선택하고 **Open** 버튼을 클릭하십시오.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image12.png)

4.  그런 다음 **Upload** 버튼을 클릭하고, 대화상자에서 **X** 아이콘을
    선택해 **Upload files** 대화상자를 닫으세요.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image13.png)
>
> ![A screenshot of a upload box AI-generated content may be
> incorrect.](./media/image14.png)

5.  **Files**에서 새로 고침을 클릭하고 선택하세요. 파일이 나타납니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image15.png)

6.  **Lakehouse** 페이지에서 탐색기 창 아래에 있는 **Files**을
    선택합니다. 이제 **DimProducts.csv** 파일 위에 마우스를 올려보세요.
    **DimProducts.csv** 옆에 있는 가로 타원 아이콘**(…)**을 클릭하세요.
    탐색하여 **Load Table**을 클릭한 다음, **New table**을 선택하세요.

> ![](./media/image16.png)
>
> ![](./media/image17.png)

7.  **Load file to new table** 대화 상자에서 **Load** 버튼을 클릭합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image18.png)

8.  이제 **DimProducts** 테이블이 성공적으로 생성되었습니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

9.  **DimProducts** 테이블을 선택하여 데이터를 미리 봅니다.

\[!주석\]**주석**: 데이터를 미리 보려면 **Refresh** 버튼을 한 번 이상
클릭해야 할 수도 있습니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image20.png)

10. 7~9단계를 반복하여 나머지 파일을 테이블에 입력합니다.

![](./media/image21.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

> ![](./media/image23.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image24.png)
>
> ![](./media/image25.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image26.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image27.png)

11. 왼쪽 탐색 모음에서 **Fabric IQ Ontology**를 선택합니다.

> ![](./media/image28.png)

## 과제 4: eventhouse준비하기

Eventhouse에서 KQL 데이터베이스에 디바이스 스트리밍 데이터 파일을
업로드하려면 다음 단계를 따르세요.

1.  **Fabric IQ Ontology** 홈 페이지에서 **+New item**을 선택한 다음
    **Eventhouse**를 선택하세요.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image29.png)

2.  Eventhouse 이름을 +++ **TelemetryDataEH** +++로 지정하고 **Create**
    버튼을 클릭합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image30.png)

3.  eventhouse 는 준비되면 열립니다![A screenshot of a computer
    AI-generated content may be incorrect.](./media/image31.png)

4.  KQL 데이터베이스의 이름을 선택하여 열어 주세요.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image33.png)

5.  **KQL database**의 하단 리본에서 **Get data**를 클릭한 다음, **Local
    file**을 선택하여 로컬 시스템의 파일을 데이터베이스로 업로드합니다.
    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image34.png)

6.  데이터를 새 테이블로 수집하기 위한 대상 옵션을 선택하고, + New
    table을 클릭한 다음, 테이블 이름을 +++ **FreezerTelemetry** +++로
    입력합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image35.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image36.png)

7.  대상 테이블을 선택한 다음, 파일을 드래그 앤 드롭하거나 *Browse for
    files*를 클릭하여 데이터를 업로드합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image37.png)

8.  VM에서 **C:\LabFiles\Lab1** 로 이동한 다음,
    ***FreezerTelemetry*.csv**파일을 선택하고 **Open** 버튼을
    클릭합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image38.png)

9.  **Next** 버튼을 클릭합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image39.png)

10. 그런 다음 **Finish** 버튼을 클릭합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image40.png)

11. 데이터 수집이 완료될 때까지 기다린 후 **Close**를 클릭합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image41.png)

12. 완료되면 KQL 데이터베이스에 **FreezerTelemetry** 테이블이
    표시됩니다:

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image42.png)

13. 왼쪽 탐색 창에서 **Fabric IQ Ontology**를 선택합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image43.png)

# 연습 2: OneLake에서 ontology 구축하기

## 과제 1: ontology (preview) 항목 만들기

1.  Fabric workspace에서 **+ New item**을 선택합니다.
    **Ontology(preview)** 항목을 검색하여 선택합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image44.png)

2.  Ontology **Name**에 +++ **RetailSalesOntology** **+++**를 입력하고
    **Create** 선택하세요.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image45.png)
>
> ** 팁:** Ontology 이름에는 숫자, 문자, 그리고 밑줄(\_)이 포함될 수
> 있습니다. 공백이나 대시는 사용하지 마세요.

3.  Ontology는 준비가 되면 열립니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image46.png)
>
> 다음으로, lakehouse 테이블의 데이터를 기반으로 엔터티 타입, 데이터
> 바인딩, 관계를 만드세요.

## 과제 2: 엔터티 타입과 데이터 바인딩 만들기

> 먼저, 엔터티 유형을 생성합니다. 엔터티 유형은 비즈니스 내 객체의
> 유형을 나타냅니다. 이 단계에는 세 가지 엔터티 유형이 있습니다:
> *Store*, *Products* 및 *SaleEvent*. 엔터티 유형을 만든 후에는
> IQ_Lakehouse lakehouse 테이블에 원본 데이터 컬럼을 바인딩하여 속성을
> 생성하세요.

### 첫 번째 엔터티 유형(상점) 추가

1.  상단 리본 메뉴나 구성 캔버스 중앙에서 **Add entity type** 를
    선택하세요.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image47.png)

2.  엔터티 유형 이름에 +++ **Store** +++를 입력하고 **Add Entity
    Type**을 선택하세요.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image48.png)

3.  *Store* 엔터티 유형이 구성 캔버스에 추가되고, **Entity type
    configuration** 창이 표시됩니다.

> ![](./media/image49.png)

4.  구성 캔버스에서 엔터티 이름 옆의 ...을 선택하고 **Bind data**를
    선택합니다.

> ![](./media/image50.png)

5.  **Add data binding \> Lakehouse table**을 선택합니다.

> ![](./media/image51.png)

6.  다음으로 데이터 소스를 선택합니다. **IQ_Lakehouse** lakehouse를
    선택하고 **Next**을 선택합니다.

> ![](./media/image52.png)

7.  **dimstore** 테이블을 선택하고 **Select**를 선택합니다.

![](./media/image53.png)

8.  소스 테이블의 필드가 데이터 바인딩 구성을 채웁니다. 설정 페이지의 각
    섹션을 살펴보세요:

- **엔터티 유형 키**: 수집된 데이터의 각 레코드를 고유하게 식별하는 데
  사용할 수 있는 필드(들)를 식별합니다.

- **구속 선택**: 바인딩에 필요한 데이터를 보유한 원본 테이블을
  식별합니다.

- **엔터티 유형 키 매핑**: 원본 데이터 테이블에서 엔터티 타입 키 속성에
  매핑되는 열(들)을 식별합니다. 엔터티 타입 키로 사용할 소스 데이터의
  문자열 및 정수형 컬럼을 선택할 수 있습니다. 선택한 컬럼들이 함께
  레코드를 고유하게 식별합니다.

- **특성**: *Store* 엔터티 유형의 속성으로 표현될 원본 데이터의 열을
  나열합니다. **Source column** 항목은 *dimstore*  테이블의 열로 자동
  채워지며, **Property name** 항목에는 온톨로지 내 *Store* 엔터티 유형의
  해당 속성 이름이 표시됩니다. 이 튜토리얼에서는 기본 속성 이름을 그대로
  사용하십시오.

![](./media/image54.png)

9.  구성 상단에서 **Define entity type key**를 선택합니다.

![](./media/image55.png)

10. 속성 목록에서 **StoreId**를 선택하고 **Save**을 선택합니다.

![](./media/image56.png)

11. 데이터 바인딩을 **Save**합니다

![](./media/image57.png)

![](./media/image58.png)

12. 엔터티 유형이 성공적으로 업데이트되었는지 확인한 다음, **Cancel** 를
    선택하여 구성 옵션을 닫습니다.

![](./media/image59.png)

13. 엔티티 유형 세부 정보의 **Configure**페이지가 표시됩니다. 이
    페이지에는 속성 및 데이터 바인딩을 포함하여 해당 엔티티 유형에 대한
    중요한 정보가 나타납니다. 구성된 데이터 바인딩을 확인하십시오.

![](./media/image60.png)

14. **Home**을 선택하여 구성 캔버스로 돌아가 새 엔터티 유형을
    추가합니다.

![](./media/image61.png)

### 다른 엔티티 유형(Products, SaleEvent) 추가

15. **Store** 엔터티 유형에 대해 수행했던 것과 동일한 단계를 따라 다음
    표에 설명된 엔터티 유형을 생성하십시오. 각 엔터티는 원본 테이블의
    기본 열을 사용하는 정적 데이터 바인딩을 갖습니다.

[TABLE]

![](./media/image62.png)

> ![](./media/image63.png)

![](./media/image64.png)

![](./media/image65.png)

![](./media/image66.png)

![](./media/image67.png)

![](./media/image68.png)

![](./media/image69.png)

![](./media/image70.png)

![](./media/image71.png)

![](./media/image72.png)

16. **Home**을 선택하여 구성 캔버스로 돌아간 다음, **SaleEvent** 엔터티
    유형을 추가합니다.

![](./media/image73.png)

![](./media/image74.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image75.png)
>
> ![](./media/image76.png)
>
> ![](./media/image77.png)
>
> ![](./media/image78.png)
>
> ![](./media/image79.png)
>
> ![](./media/image80.png)
>
> ![](./media/image81.png)
>
> ![](./media/image82.png)
>
> ![](./media/image83.png)
>
> ![](./media/image84.png)

17. 작업을 완료하면 **Entity Types** 창에 해당 엔터티 유형들이 나열된
    것을 볼 수 있습니다.

![](./media/image85.png)

## 과제 3: 관계 유형 생성

다음으로, 데이터 내의 맥락적 연결을 나타내기 위해 엔티티 유형 간의 관계
유형을 생성하십시오.

### 스토어에서 SaleEvent

1.  탐색기에서 **SaleEvent** 엔터티 유형을 선택합니다.

> ![](./media/image86.png)

2.  메뉴 리본에서 **Add relationship**를 선택합니다.

> ![](./media/image87.png)

3.  다음 관계 유형 세부 정보를 입력하고 **Add relationship type**를
    선택합니다.

- **관계 유형 이름**: +++from+++

- **소스 엔터티 유형**: *SaleEvent*

- **대상 엔티티 유형**: *Store*

> ![](./media/image88.png)
>
> ![](./media/image89.png)

4.  관계가 시맨틱 캔버스에 추가됩니다. 해당 관계를 선택하여 관계 세부
    정보 설정 화면을 엽니다. 설정 페이지의 각 섹션을 확인해 보십시오:

- **Origin 엔터티 유형**: 원본 엔티티(이 경우 **SaleEvent**)의 세부
  정보를 나열합니다.

- **관계 유형**: 관계 유형의 세부 사항을 설정합니다.

- **대상 기업 유형**: 대상 엔티티(이 경우 **Store**)의 세부 정보를
  나열합니다.

> ![](./media/image90.png)
>
> ![](./media/image91.png)

5.  중간 섹션에 다음 세부 정보를 입력하십시오.

&nbsp;

1.  **Mapping Table**: **Browse available sources**  \` **factsales**\`
    테이블을 선택하십시오. 소스 데이터에 포함된 이 테이블은 *Store*과
    *SaleEvent*엔터티를 서로 연결할 수 있는데, 이는 두 엔터티 유형을
    식별하는 정보를 모두 담고 있기 때문입니다. 이 테이블의 각 행은 ID를
    통해 특정 store 및 sale event를 참조합니다.

> ![](./media/image92.png)

2.  **Matched SaleEvent: SaleId**: **SaleId**선택. 이 설정은 *SaleEvent*
    엔터티에 정의된 키 속성과 값이 일치하는 관계 원본 데이터 테이블의
    열을 지정합니다. 이 경우 관계 데이터 원본과 엔터티 데이터 원본이
    모두 *factsales* 테이블을 사용하므로, 동일한 열(SaleId)을 선택하게
    됩니다.

3.  **Matched Store: StoreId**: **StoreId**선택.이 설정은 관계의 원본
    데이터 테이블(factsales \> StoreId)에서 *Store* 엔터티(dimstore \>
    StoreId)에 정의된 키 속성과 값이 일치하는 열을 지정합니다. 튜토리얼
    데이터의 경우, 두 테이블 모두에서 해당 열의 이름이 StoreId로
    동일합니다.

> ![](./media/image93.png)

**  
 중요:** 엔티티 유형의 키 속성과 일치하는 올바른 **Matched** 열을
선택해야 합니다.

6.  관계 유형을 **Save**합니다. 관계 유형이 성공적으로 업데이트되었는지
    확인한 다음, **Cancel**를 선택하여 구성 옵션을 닫습니다.

> ![](./media/image94.png)
>
> ![](./media/image95.png)
>
> ![](./media/image96.png)
>
> 이제 첫 번째 관계가 생성되어 소스 테이블의 데이터에 연결되었습니다.
> 다음 섹션으로 넘어가 또 다른 유형의 관계를 생성해 보세요.

### **SaleEvent 판매된 Products**

1.  **Home**을 선택하여 새 엔터티 유형을 추가할 수 있는 구성 캔버스로
    돌아갑니다.

![](./media/image97.png)

2.  첫 번째 관계 유형을 생성할 때와 동일한 단계를 따라, 다음 표에 설명된
    세부 정보를 포함하는 **SaleEvent** 엔터티 유형 기반의 두 번째 관계를
    생성하십시오.

[TABLE]

![](./media/image98.png)

![](./media/image99.png)

![](./media/image100.png)

![](./media/image101.png)

![](./media/image102.png)

![](./media/image103.png)

![](./media/image104.png)

# 연습 3:추가 데이터로 ontology를 풍부하게 하세요

이 실습에서는 새로운 ***Freezer*** 엔티티 유형을 추가하여 ontology를
확장합니다. 이 엔티티 유형은 도메인 맥락을 보강하고, 실시간 운영 정보를
반영하는 시계열 데이터용 속성을 도입합니다.

** 주석**

정적 데이터와 시계열 데이터 모두에 대해 데이터를 바인딩하지 않고 속성을
생성한 다음 나중에 데이터를 바인딩하거나, 속성을 생성하고 데이터를 한
번에 바인딩할 수 있습니다. 이 문서에서는 두 가지 접근 방식을 모두
설명합니다.

마지막으로, 매장과 freezers 간의 연결을 나타내는 새로운 관계 유형을
생성합니다.

## 과제 1: Freezer 엔티티 유형을 생성하고 속성을 추가하세요.

다음 단계에 따라 *Freezer* 엔터티 유형을 만들고 여기에 속성을
추가하세요. 속성은 아직 데이터에 바인딩되지 않은 상태입니다.

1.  상단 리본에서 **Add entity type**를 선택합니다. 엔티티 유형 이름으로
    +++**Freezer**+++를 입력하고 **Add entity type**를 선택합니다.

> ![](./media/image105.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image106.png)

2.  **Explorer**에서 Freezer 엔터티 유형을 선택한 다음, 상단 리본에서
    **View entity type details** 를 선택합니다.

> ![](./media/image107.png)

3.  엔터티 유형 세부 정보의 **Configure**페이지가 열립니다. 이
    페이지에는 속성 및 데이터 바인딩을 포함하여 해당 엔터티 유형에 대한
    중요한 정보가 표시됩니다.

**Manage property bindings** 를 확장하고 **Add properties**를
선택합니다.

![](./media/image108.png)

4.  다음 속성을 추가하고 **Save**을 선택합니다.

[TABLE]

![](./media/image109.png)

**주석:** 속성 이름은 모든 엔터티 유형에 걸쳐 고유해야 합니다.

![](./media/image110.png)

5.  속성들이 데이터 소스에 바인딩되지 않은 **Configure** 페이지에
    추가됩니다.

> ![](./media/image111.png)

## 과제 2: 정적 데이터를 프로퍼티에 바인딩하기

다음으로, *Freezer* 엔터티 유형에서 생성한 속성에 정적 데이터를
바인딩합니다.

1.  **Manage property bindings** 를 확장하고 **Add binding and
    properties**를 선택합니다.

> ![](./media/image112.png)

2.  **Add data binding \> Lakehouse table**선택

> ![](./media/image113.png)

3.  데이터 소스를 선택하세요.

- **IQ_Lakehouse** lakehouse를 선택하고 **Next**을 선택합니다.

- **freezer** 테이블을 선택하고 **Select**을 누르세요.

> ![](./media/image114.png)
>
> ![](./media/image115.png)

4.  원본 테이블의 필드가 데이터 바인딩 구성에 채워집니다. 구성 페이지의
    각 섹션을 살펴보십시오:

- **Entity type key**: 수집된 데이터의 각 레코드를 고유하게 식별하는 데
  사용할 수 있는 필드(또는 필드들)를 식별합니다.

- **Binding selection**: 바인딩에 사용할 데이터를 포함하는 원본 테이블을
  식별합니다.

- **Entity type key mapping**: 엔터티 유형의 키 속성에 매핑되는 원본
  데이터 테이블의 열을 지정합니다. 원본 데이터에서 문자열(string) 또는
  정수(integer) 형식의 열을 엔터티 유형 키로 선택할 수 있습니다. 선택한
  열들을 조합하여 레코드를 고유하게 식별하게 됩니다.

- **Properties**: 소스 데이터의 열과 **Freezer** 엔터티 유형의 해당
  속성을 나열합니다. **Source column** 쪽은 **freezer** 테이블의 열로
  자동 채워지며, **Property name** 쪽에는 온톨로지 내 **Freezer** 
  엔터티 유형의 해당 속성 이름이 표시됩니다. 이 튜토리얼에서는 기본 속성
  이름을 그대로 사용하십시오.

![](./media/image116.png)

5.  구성 상단에서 **Define entity type key**를 선택합니다. 속성 목록에서
    FreezerId를 선택한 다음 **Save**을 선택합니다.

![](./media/image117.png)

![](./media/image118.png)

6.  데이터 바인딩을 **Save**합니다. 엔터티 유형이 성공적으로
    업데이트되었는지 확인한 다음, **Cancel**를 선택하여 구성 옵션 창을
    닫습니다.

![](./media/image119.png)

![](./media/image120.png)

## 과제 3: 시계열 데이터를 추가 속성에 바인딩

다음으로, 새로운 속성을 생성하고 단일 데이터 바인딩 작업을 통해 해당
속성에 시계열 데이터를 바인딩함으로써 **Freezer** 엔티티에 시계열
데이터를 추가합니다.

1.  **Configure** 페이지에서 **Manage property bindings**를 확장하고
    **Add binding and properties**를 다시 선택하여 바인딩 구성 화면을
    다시 엽니다.

![](./media/image121.png)

2.  **Binding selection**에서 **Add data binding** 를 확장하고
    **Eventhouse table or materialized view**를 선택합니다.

![](./media/image122.png)

3.  데이터 소스를 선택하세요.

    1.  **TelemetryDataEH** eventhouse를 선택하고 **Add**를 선택합니다.

![](./media/image123.png)

2.  **FreezerTelemetry** 테이블을 선택하고 **Add**.

![](./media/image124.png)

4.  구성에 **Timeseries data** 섹션이 나타납니다. **Timestamp column**로
    timestamp를 선택하십시오.

![](./media/image125.png)

5.  **Properties**섹션으로 스크롤을 내리면 **StoreId** 항목에 오류가
    표시되는데, 이는 정적 데이터 바인딩에 이미 바인딩되어 있기
    때문입니다. 휴지통 아이콘을 사용하여 중복된 속성을 삭제하세요.

![](./media/image126.png)

6.  데이터 바인딩을 **Save**합니다. 엔터티 유형이 성공적으로
    업데이트되었는지 확인한 다음, **Cancel**를 선택하여 구성 옵션 창을
    닫습니다.

![](./media/image127.png)

![](./media/image128.png)

7.  다시 *Freezer*의 **Configure** 페이지로 돌아가 보면, 엔티티 유형
    속성이 늘어났으며 새로 추가된 속성들이 *FreezerTelemetry* 데이터
    소스에 바인딩되어 있음을 확인할 수 있습니다.

![](./media/image129.png)

이제 *Freezer* 엔티티는 두 가지 데이터 바인딩을 갖습니다. 하나는
*freezer* lakehouse 테이블의 정적 데이터와 연결되고, 다른 하나는
*FreezerTelemetry* eventhouse 테이블의 스트리밍 데이터와 연결됩니다.

## 과제 4: 관계 유형 추가

마지막으로, store과 freezers 간의 연결을 나타내는 새로운 관계 유형을
생성하십시오.

**Create Store는 Freezer를 운영**

1.  **Configure** 페이지에서 Manage relationships를 확장하고 **Add new
    relationship**를 선택합니다.

![](./media/image130.png)

2.  다음 관계 유형 세부 정보를 입력하고 **Add relationship type**를
    선택합니다.

    1.  **Relationship type name**: *operates*

    2.  **Source entity type**: *Store*

    3.  **Target entity type**: *Freezer*

![](./media/image131.png)

3.  관계가 **Relationships** 섹션에 추가됩니다. 캔버스에서 **operates**
    관계를 선택하여 관계 세부 정보 구성 화면을 엽니다. 구성 페이지의 각
    섹션을 확인해 보십시오:

- **Origin entity type**: 원본 엔티티(이 경우 Store)의 세부 정보를
  나열합니다.

- **Relationship type**: 관계 유형의 세부 사항을 설정합니다.

- **Target entity type**: 대상 엔티티(이 경우 *Freezer*)의 세부 정보를
  나열합니다.

![](./media/image132.png)

![](./media/image133.png)

4.  중간 섹션에 다음 세부 정보를 입력하십시오.

- **Mapping table**: **freezer** 테이블을 선택하십시오. 이 테이블은
  *Store*과 *Freezer* 엔터티를 식별하는 정보를 모두 포함하고 있어, 원본
  데이터 내에서 두 엔터티를 서로 연결할 수 있습니다. 이 테이블의 각 행은
  ID를 통해 특정 store과 freezer를 참조합니다.

- **Matched Store: StoreId**: **StoreId**선택. 이 설정은 Store
  엔터티(dimstore \> StoreId)에 정의된 키 속성과 값이 일치하는 관계 원본
  데이터 테이블(freezer \> StoreId)의 열을 지정합니다. 튜토리얼 데이터의
  경우, 두 테이블 모두에서 열 이름이 StoreId로 동일합니다.

- **Matched Freezer: FreezerId**: **FreezerId**선택. 이 설정은 *Freezer*
  엔터티에 정의된 키 속성과 값이 일치하는, 관계 소스 데이터 테이블의
  열을 지정합니다. 이 경우 관계 데이터 소스와 엔터티 데이터 소스 모두
  *freezer* 테이블을 사용하므로, 동일한 열(FreezerId)을 선택하게 됩니다.

![](./media/image134.png)

**  
 중요:** 엔터티 유형 키 속성과 일치하는 올바른 소스 열을 선택해야
합니다.

5.  관계 유형을 **Save**합니다. 관계 유형이 성공적으로 업데이트되었는지
    확인한 다음, **Cancel**를 선택하여 구성 옵션을 닫습니다.

![](./media/image135.png)

![](./media/image136.png)

6.  엔티티의 **Configure** 페이지가 표시되며, 여기서 업데이트된 관계가
    **Relationships** 섹션에 계속 표시됩니다.

![](./media/image137.png)

# 연습 4: **Ontology 보기**

이 연습에서는 미리 보기 환경을 사용하여 온톨로지를 탐색해 보세요.
데이터를 사용하여 엔티티 유형을 인스턴스화하는 엔티티 인스턴스를
검사하고, 판매 및 장치 스트리밍 데이터 전반에 걸쳐 그래프 형태의
컨텍스트를 살펴보세요.

## 과제 1: **인스턴스 목록 및 정적 데이터 보기**

이전 튜토리얼 단계에서 엔터티 유형에 데이터를 바인딩했을 때, ontology는
원본 데이터 행과 연결된 해당 엔터티의 인스턴스를 자동으로 생성했습니다.
이 섹션에서는 미리 보기 기능을 사용하여 해당 엔터티 인스턴스를
확인합니다.

1.  Ontology의 홈 구성 캔버스에서 시작합니다. **SaleEvent** 엔터티
    유형을 선택하고 상단 리본에서 **View Entity Type details**를
    선택합니다.

![](./media/image138.png)

2.  **Instances** 탭을 엽니다. **factsales** lakehouse 테이블에서 수익과
    단위 수치 같은 데이터가 채워진 6개의 엔터티 인스턴스가 표시되는지
    확인하세요.

![](./media/image139.png)

## 과제 2: 시계열 데이터 보기

1.  페이지 왼쪽 상단에서 엔티티 유형 이름 옆의 선택 도구를 사용하여
    **Freezer** 엔티티 유형으로 전환하십시오.

![](./media/image140.png)

2.  **Overview** 탭을 엽니다. 기본 기간 설정인 **Last 30 days** 에는
    데이터가 포함되어 있지 않으므로, 탭이 열릴 때 차트는 비어 있는
    상태로 표시됩니다.

![](./media/image141.png)

3.  시간 범위를 기본값인 **Last 30 days**에서, **2025년 8월 1일 금요일
    오전 12:00**에 시작하여 **2025년 8월 4일 월요일 오전 12:00**에
    종료되며, **시간 세분도**가 **5분인** 사용자 지정 날짜 범위로
    업데이트하십시오.

![](./media/image142.png)

4.  선택한 시간 창 내에서 여러 **Freezer** 엔티티 인스턴스에서 이제
    표시되는 시계열 데이터를 확인하십시오.

![](./media/image143.png)

## 과제 3: **ontology graph보기**

**Overview** 탭에는 **Relationship graph**도 포함되어 있으며, 이를 통해
온톨로지를 노드와 에지로 구성된 그래프로 시각화할 수 있습니다.

1.  엔터티 유형 선택기를 사용하여 **SaleEvent** 엔터티 유형으로
    전환합니다. **Relationship graph** 타일에서 **Expand**을 선택하세요.

> ![](./media/image144.png)

2.  그는 graph 뷰를 확장했다 열림**. SaleEvent** 엔터티 유형과
    **Products** 및 **Store** 간의 관계 세부 사항을 확인하세요.

![](./media/image145.png)

3.  엔터티 유형 선택기를 사용하여 **Store** 엔터티 유형으로 전환합니다.
    그 **relationship graph**를 확장하십시오.

![](./media/image146.png)

4.  Graph에서 **Store**가 **Freezer** 및 **SaleEvent**와 맺고 있는
    관계를 관찰하세요. 그다음 쿼리 빌더 리본에서 **Run query**을
    선택하세요. 이 작업은 기본 쿼리를 실행하고 엔터티 인스턴스와 그
    연결을 그래프로 보여줍니다

![](./media/image147.png)

![](./media/image148.png)

![](./media/image149.png)

## 과제 4: Query Graph 인스턴스

Relationship graph뷰에서는 특정 조건을 만족하는 엔터티 인스턴스를
온톨로지에서 조회할 수 있습니다. 상단 리본의 **Query builder** 필터를
사용해 쿼리를 작성하세요.

![](./media/image150.png)

*먼저, 다음 쿼리를 작성하세요: Paris store에서 운영되는 모든 freezers를
보여 주세요.*

1.  Store 엔터티의 관계 그래프에서 쿼리 빌더 리본에서 **Add filter \>
    Store \> StoreId**를 선택합니다. 필터를 **StoreId = S-PAR-01**로
    설정하세요. 이 값은 *Paris* store의 Store ID입니다.

![](./media/image151.png)

![](./media/image152.png)

5.  **Components**섹션에서 SaleEvent의 체크를 해제하여 체크된 필드가
    **Nodes \> Store, Nodes \> Freezer,** 그리고 **Edges \> operates**만
    남도록 합니다.

![](./media/image153.png)

6.  **Run query**를 선택하고 인스턴스 그래프에 *Paris* store과 연결된 두
    대의 freezers가 표시되는지 확인하세요.

![](./media/image154.png)

![](./media/image155.png)

7.  쿼리 결과를 지우려면 **Clear query**를 선택합니다.

![](./media/image156.png)

*다음으로, 이 쿼리를 작성하세요: 매출이 150을 초과하는 판매를 한 모든
매장을 표시하세요.*

8.  **Add a node를 선택한 다음 SaleEvent용 노드를 추가하세요.**

> ![](./media/image157.png)

9.  **Components** 섹션에서 **Nodes \> Store** and **Edges \> from**
    옆의 박스를 체크해 그래프에 추가하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image158.png)

10. 쿼리 빌더 리본 메뉴에서 **Add filter \> SaleEvent \> RevenueUSD**를
    선택합니다**.** 필터를 +++**RevenueUSD \> 150+++**로 설정합니다.

![](./media/image159.png)

![](./media/image160.png)

11. **Run query**을 선택하고, 인스턴스 그래프에 연결된 판매 이벤트에
    대한 필터 조건을 충족하는 두 개의 스토어가 표시되는지 확인합니다.
    또한 그래프에서 노드를 선택하여 특정 판매 이벤트에 대한 세부 정보를
    확인할 수도 있습니다.

![](./media/image161.png)

![](./media/image162.png)

이 프로세스를 통해 운영상의 문제(예: 특정 매장의 냉동고 온도 상승)와
비즈니스 성과(매출)를 연결하는 경로를 파악할 수 있습니다.

# 연습 5: **에이전트에서 ontology를 가져오기**

Ontology (preview) 는 [Fabric data agent
(preview)](https://learn.microsoft.com/en-us/fabric/data-science/concept-data-agent) 와
연동되어, 사용자가 자연어로 질문을 하면 온톨로지의 정의와 바인딩을
기반으로 한 답변을 얻을 수 있게 해줍니다.

## 과제 1: Ontology (preview) 소스로 Data Agent 생성하기

Ontology (preview) 항목에 연결되는 새 데이터 에이전트를 만들려면 다음
단계를 따르세요.

1.  이제 왼쪽 네비게이션 창에서**Fabric IQ Ontology XX** 를 클릭하세요.

![](./media/image163.png)

2.  **Fabric** 홈 페이지에서 **+New item**을 선택합니다. 항목 유형별
    필터 검색창에 +++**data agent**+++를 입력한 후 데이터 에이전트를
    선택하세요

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image164.png)

3.  Data Agent 이름으로 **+++RetailOntologyAgent+++** 를 입력하고
    **Create**를 선택합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image165.png)

4.  In **RetailOntologyAgent** 페이지에서, **Add a data source**를
    선택합니다

> ![](./media/image166.png)

5.  OneLake 카탈로그 탭에서 **RetailSalesOntology** Ontology를 선택한
    다음 **Add**를 선택합니다.

> ![](./media/image167.png)
>
> 에이전트가 준비되면 열립니다.
>
> ![](./media/image168.png)

## 과제 2: 에이전트 지침 제공

** 주석:** 이 단계는 쿼리 집계에 영향을 미치는 알려진 문제에 대응하여
추가되었습니다.

> 다음으로, 에이전트에 사용자 지정 지시를 추가합니다.

1.  메뉴 리본에서 **Agent instructions** 을 선택합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image169.png)

2.  입력 상자 하단에 +++**Support group by in GQL**+++를 추가하세요. 이
    명령을 사용하면 온톨로지 데이터 전반에 걸쳐 더 효과적인 집계 처리가
    가능합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image170.png)

3.  지침이 자동으로 적용됩니다. 원한다면**Agent instructions** 탭을
    닫으세요.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image171.png)

## 과제 3: 자연어를 사용하는 쿼리 에이전트

> 다음으로 자연어 질문을 통해 ontology를 탐색해 보세요.

1.  아래 이미지에 표시된 것처럼 다음 텍스트를 입력하고 **Submit
    icon** 을 클릭하세요.

> **+++For each store, show any freezers operated by that store that
> ever had a humidity lower than 46 percent.+++**
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image172.png)

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image173.png)

2.  아래 이미지에 표시된 것처럼 다음 텍스트를 입력하고 **Submit
    icon** 을 클릭하세요.

> *+++What is the top product by revenue across all stores?+++*

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image174.png)

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image175.png)

> 응답이 단순한 테이블뿐 아니라 엔터티 타입
> (*Store*, *Products*, *Freezer*) 과 그 관계를 참조한다는 점에
> 주목하세요.
>
> ![Screenshot of the result of a query.](./media/image176.png)
>
> ** 팁:** 예제 쿼리를 실행하는 도중 데이터가 없다는 오류가 뜨면,
> 에이전트가 초기화될 시간을 더 줄 수 있도록 몇 분 정도 기다리세요. 그런
> 다음, 쿼리를 다시 실행하세요.
>
> 직접 몇 가지 프롬프트를 시도해 보면서 데이터 에이전트를 계속 탐색해
> 보세요.

## 과제 4: 리소스 정리

1.  왼쪽 내비게이션 메뉴에서 작업 공간인 **Fabric IQ OntologyXX** 를
    선택하세요. 작업 공간 항목 뷰를 엽니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image177.png)

2.  작업 공간 이름 아래에 있는 ... 옵션을 선택하고 **Workspace
    settings**을 선택합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image178.png)

3.  General탭의 맨 아래로 이동하여 **Remove this workspace**를
    선택합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image179.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image180.png)

**요약**

이 사용 사례는 Microsoft Fabric IQ Ontology (preview) 를 활용하여 실제
비즈니스 개념과 그 관계를 나타내는 연결된 시맨틱 데이터 모델을 생성하는
방법을 보여줍니다. 이 ontology는 구조화된 lakehouse 데이터와 스트리밍
telemetry 데이터를 결합함으로써, 기업 데이터에 대한 통합적이고 비즈니스
친화적인 뷰를 제공합니다.

엔터티 정의, 데이터 바인딩, 관계 모델링을 통해 사용자는 냉동고 온도나
습도 같은 운영 신호가 매출과 수익 같은 비즈니스 결과와 어떻게 연관되는지
분석할 수 있습니다. 또한 이 사례는 ontology가 Fri Aug 01 2025 Agent를
통해 Graph 탐색과 자연어 질의를 지원하며, 사용자가 기본 테이블이나
스키마를 이해하지 않아도 더 깊은 인사이트를 얻을 수 있음을 보여줍니다.

전반적으로 이 사례는 Fabric IQ Ontology가 운영 데이터와 분석을 연결해
다양한 분야에서 더 현명한 의사결정을 지원하는 방식을 보여줍니다.
