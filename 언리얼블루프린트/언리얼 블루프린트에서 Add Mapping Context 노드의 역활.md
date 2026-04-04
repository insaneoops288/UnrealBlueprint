# **Add Mapping Context 노드의 역할과 활용**

언리얼 엔진 5의 **향상된 입력(Enhanced Input)** 시스템에서 Add Mapping Context 노드는 플레이어가 특정 입력 규칙 세트(Input Mapping Context, IMC)를 사용할 수 있도록 활성화하는 관문 역할을 합니다.

## **1\. 핵심 역할**

이 노드의 가장 주된 역할은 \*\*"어떤 입력(키, 버튼)이 어떤 행동(Action)을 유발할지 정의한 맵을 플레이어 컨트롤러에 등록하는 것"\*\*입니다.

* **활성화**: 단순히 프로젝트에 IMC(Input Mapping Context) 파일이 있다고 해서 바로 동작하지 않습니다. 이 노드를 통해 실행 시점에 등록해야만 키 입력이 먹히기 시작합니다.  
* **우선순위 부여**: 여러 개의 Mapping Context를 동시에 등록할 수 있으며, Priority 값을 통해 어떤 입력이 먼저 처리될지 결정할 수 있습니다.

## **2\. 노드의 주요 파라미터**

* **Target**: Enhanced Input Local Player Subsystem 객체가 들어갑니다. 보통 Get Local Player \-\> Get Enhanced Input Local Player Subsystem 단계를 거쳐 연결합니다.  
* **Mapping Context**: 활성화하고자 하는 Input Mapping Context 에셋을 선택합니다. (예: IMC\_Default, IMC\_Vehicle 등)  
* **Priority**: 정수(Integer) 값입니다. 숫자가 높을수록 우선순위가 높습니다.  
  * 예: '일반 걷기' IMC가 0순위이고, '인벤토리 창' IMC가 1순위라면, 인벤토리가 열렸을 때 같은 키(예: ESC)가 인벤토리 닫기로 먼저 작동하게 할 수 있습니다.

## **3\. 왜 이 노드를 사용하나요? (장점)**

1. **동적 입력 변경**: 게임 도중에 입력 방식을 쉽게 바꿀 수 있습니다.  
   * 캐릭터가 차에 타면 IMC\_Character를 제거(Remove Mapping Context)하고 IMC\_Vehicle을 추가하는 방식입니다.  
2. **모듈화**: 이동 관련 입력, 전투 관련 입력, UI 관련 입력을 별도의 IMC 파일로 쪼개어 관리하고 필요할 때만 조합해서 쓸 수 있습니다.  
3. **향상된 컨트롤**: 복잡한 입력 우선순위나 필터링을 코드 수정 없이 블루프린트 상에서 관리하기 편리합니다.

## **4\. 일반적인 사용 패턴 (Setup)**

보통 캐릭터(Pawn)의 **BeginPlay** 이벤트나 **Pawn Restarted** 시점에 다음과 같은 흐름으로 구성합니다.

1. **플레이어 컨트롤러 가져오기**: Get Controller  
2. **로컬 플레이어 서브시스템 참조**: Get Local Player Subsystem (Enhanced Input)  
3. **유효성 검사**: 서브시스템이 유효한지 Is Valid 확인.  
4. **컨텍스트 추가**: Add Mapping Context 노드 실행.

### **요약**

Add Mapping Context는 \*\*"지금부터 이 키보드/마우스 세팅을 사용하겠다"\*\*라고 엔진에 선언하는 노드라고 이해하시면 됩니다. UE5에서 입력을 구현할 때 가장 먼저 호출해야 하는 필수 단계입니다.