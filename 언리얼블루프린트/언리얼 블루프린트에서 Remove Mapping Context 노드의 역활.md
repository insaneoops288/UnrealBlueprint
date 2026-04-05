# **Remove Mapping Context 노드의 역할과 이해**

언리얼 엔진 5(UE5) 이상에서 표준이 된 **향상된 입력(Enhanced Input)** 시스템에는 \*\*IMC(Input Mapping Context)\*\*라는 개념이 있습니다. Remove Mapping Context는 이 IMC를 플레이어의 입력 하위 시스템에서 제거하는 역할을 합니다.

## **1\. 핵심 역할**

Remove Mapping Context 노드는 특정 \*\*입력 매핑 컨텍스트(Input Mapping Context, IMC)\*\*를 'Enhanced Input Local Player Subsystem'에서 등록 해제합니다. 이 노드가 실행되면 해당 IMC 안에 정의된 모든 키 바인딩과 입력 액션이 더 이상 작동하지 않게 됩니다.

## **2\. 주요 사용 사례**

게임 플레이 중 상황에 따라 입력 체계를 완전히 바꿔야 할 때 주로 사용합니다.

* **상태 전환:** 캐릭터가 걷다가 '탈것(자동차, 말 등)'에 탔을 때, 기존의 걷기용 키 바인딩(WASD)을 제거하고 탈것용 키 바인딩을 적용하기 위해 사용합니다.  
* **UI/메뉴 오픈:** 게임 메뉴나 인벤토리를 열었을 때, 캐릭터가 움직이지 않도록 전투/이동 관련 IMC를 제거하고 UI 전용 IMC를 추가합니다.  
* **컨트롤 일시 중지:** 컷신이나 대화 창이 떴을 때 플레이어의 조작을 막기 위해 현재 활성화된 IMC를 모두 제거할 수 있습니다.

## **3\. 작동 원리 (Workflow)**

이 노드는 단독으로 쓰이기보다 보통 다음과 같은 흐름으로 사용됩니다.

1. **Get Local Player Subsystem:** 현재 플레이어 컨트롤러에서 'Enhanced Input Local Player Subsystem'을 가져옵니다.  
2. **Remove Mapping Context:** 제거하고 싶은 특정 IMC 에셋을 선택하여 연결합니다.  
3. **(선택 사항) Add Mapping Context:** 필요하다면 새로운 상황에 맞는 다른 IMC를 추가합니다.

## **4\. 노드 입력 핀 설명**

* **Target:** Enhanced Input Local Player Subsystem 객체가 연결되어야 합니다.  
* **Mapping Context:** 제거하고자 하는 Input Mapping Context 에셋을 지정합니다.

## **5\. Add Mapping Context와의 차이점**

| 기능 | Add Mapping Context | Remove Mapping Context |
| :---- | :---- | :---- |
| **목적** | 새로운 키 바인딩 세트를 활성화 | 기존 키 바인딩 세트를 비활성화 |
| **우선순위** | Priority 값을 설정하여 중첩 가능 | 단순히 목록에서 제거 (우선순위 무관) |
| **결과** | 지정된 키를 누르면 입력 액션이 발생함 | 지정된 키를 눌러도 아무 반응이 없음 |

## **6\. 주의사항**

* **Target 연결:** 반드시 **Player Controller**를 통해 **Local Player Subsystem**을 참조해야 합니다. (서버 전용 컨트롤러에는 이 시스템이 존재하지 않기 때문입니다.)  
* **존재 확인:** 제거하려는 IMC가 현재 추가되어 있지 않더라도 이 노드를 호출한다고 해서 에러(Crash)가 발생하지는 않지만, 논리적인 흐름상 필요한 시점에만 호출하는 것이 좋습니다.

**요약하자면:** Remove Mapping Context는 "이제 이 입력 뭉치(IMC)는 더 이상 쓰지 않겠다"고 시스템에 선언하는 '입력 비활성화' 노드입니다.