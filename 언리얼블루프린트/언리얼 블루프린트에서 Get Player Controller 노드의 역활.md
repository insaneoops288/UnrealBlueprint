# **Get Player Controller 노드 완벽 이해하기**

언리얼 엔진에서 Get Player Controller 노드는 현재 게임을 실행 중인 로컬 사용자의 의사결정 본체인 **Player Controller**를 호출할 때 사용됩니다.

## **1\. 핵심 역할**

플레이어 컨트롤러는 게임 내에서 "플레이어의 영혼"과 같습니다. 캐릭터(Pawn)가 파괴되어도 컨트롤러는 남아 있으며, 다음과 같은 핵심 기능을 담당합니다.

* **입력 처리:** 마우스 클릭, 키보드 입력, 게임패드 입력을 해석합니다.  
* **카메라 제어:** 플레이어가 보는 시점(Camera Manager)을 관리합니다.  
* **UI/HUD 관리:** 화면에 메뉴를 띄우거나 위젯을 추가할 때 기준점이 됩니다.  
* **빙의(Possess):** 어떤 캐릭터나 차량을 조종할지 결정합니다.

## **2\. 주요 파라미터: Player Index**

노드 아래쪽에 있는 Player Index 정수 값은 어떤 플레이어를 가져올지 결정합니다.

* **Index 0:** 기본 플레이어(싱글 플레이어 게임에서는 항상 0번입니다).  
* **Index 1 이상:** 로컬 멀티플레이어(화면 분할 게임 등)에서 두 번째, 세 번째 플레이어를 지칭합니다.  
* **주의:** 온라인 네트워크 멀티플레이어에서는 각 클라이언트가 자신의 Index 0을 가집니다.

## **3\. 자주 사용하는 상황 (Common Use Cases)**

### **① 마우스 커서 표시**

게임 중 인벤토리를 열었을 때 마우스 커서가 보이게 설정하려면 이 노드가 필요합니다.

* Get Player Controller \-\> Set Show Mouse Cursor (True)

### **② UI 위젯 생성 및 출력**

화면에 UI를 띄울 때 "누구의 화면"에 띄울지 알려주어야 합니다.

* Create Widget 노드의 Owning Player 핀에 연결합니다.

### **③ 시점 전환 (Set View Target with Blend)**

캐릭터 시점에서 시네마틱 카메라나 다른 카메라로 화면을 바꿀 때 사용합니다.

* Get Player Controller \-\> Set View Target with Blend

### **④ 입력 모드 설정**

게임 플레이 모드와 UI 조작 모드를 전환할 때 사용합니다.

* Set Input Mode Game Only  
* Set Input Mode UI Only  
* Set Input Mode Game And UI

## **4\. 핵심 팁**

* **Pawn vs Controller:** Get Player Character는 몸(Mesh)을 가져오는 것이고, Get Player Controller는 그 몸을 움직이는 정신(Logic)을 가져오는 것입니다.  
* **유효성 검사:** 간혹 컨트롤러가 생성되기 전이나 파괴된 직후에 호출하면 오류가 날 수 있으므로, Is Valid 노드와 함께 사용하는 것이 안전합니다.

**요약하자면:** "플레이어의 입력, 시점, UI와 관련된 모든 명령은 이 노드를 통해 시작된다"라고 이해하시면 됩니다.