# **Unreal Engine Blueprint: Is Input Key Down 노드 가이드**

Is Input Key Down 노드는 특정 시점에 플레이어가 특정 키(키보드, 마우스, 게임패드 버튼 등)를 누르고 있는지 여부를 확인하는 **상태 확인(Polling)** 노드입니다.

## **1\. 노드의 주요 역할**

이 노드의 핵심은 \*\*"지금 이 순간 키가 눌려 있는가?"\*\*에 대해 True 또는 False 값(Boolean)을 반환하는 것입니다.

### **입력 (Inputs)**

* **Target**: Player Controller 객체입니다. 어떤 플레이어의 입력을 확인할지 결정합니다. 보통 Get Player Controller 노드를 연결합니다.  
* **Key**: 확인하고자 하는 특정 키를 선택합니다 (예: F, Left Shift, Left Mouse Button 등).

### **출력 (Outputs)**

* **Return Value**: 키가 눌려 있다면 True, 떼어져 있다면 False를 반환합니다.

## **2\. 주요 사용처**

### **A. 매 프레임 상태 확인 (Tick Event)**

이벤트 노드(Input Action 등)를 사용하지 않고, Event Tick에서 매 프레임마다 특정 키의 상태를 체크하여 지속적인 동작을 수행할 때 사용합니다.

* **예시**: 특정 키를 누르는 동안 캐릭터가 기를 모으거나, 서서히 이동 속도가 빨라지는 로직.

### **B. 보조 키(Modifier Key) 확인**

다른 액션이 수행될 때 특정 키가 함께 눌려 있는지 검사합니다.

* **예시**: '공격' 액션이 실행될 때 Left Shift가 눌려 있다면 '강공격'을, 아니면 '약공격'을 수행하도록 분기 처리.

### **C. 커스텀 함수 및 매크로 내부**

입력 이벤트 외부에서 현재의 입력 상태가 로직의 조건이 되어야 할 때 매우 유용합니다.

## **3\. 입력 이벤트(Input Action)와의 차이점**

| 특징 | Input Action (Events) | Is Input Key Down (Polling) |
| :---- | :---- | :---- |
| **작동 방식** | 키를 누르거나 뗄 때 '한 번' 발생 (Event-driven) | 호출되는 시점의 상태를 '즉시' 확인 (State-check) |
| **효율성** | 입력이 있을 때만 실행되어 성능에 유리함 | 매 프레임 체크할 경우 CPU 자원을 소모함 |
| **용도** | 점프, 상호작용, 발사 등 일회성 액션 | 지속적인 가속, 모드 전환, 복합 키 검사 |

## **4\. 사용 시 주의사항**

1. **Player Controller 연결 필수**: 이 노드는 플레이어 컨트롤러를 타겟으로 하므로, Get Player Controller가 연결되지 않으면 제대로 작동하지 않습니다.  
2. **입력 모드 영향**: UI 전용 모드(Set Input Mode UI Only)일 경우 게임 월드에서의 키 입력 상태를 제대로 가져오지 못할 수 있습니다.  
3. **성능 고려**: Event Tick에 연결하여 너무 많은 Is Input Key Down을 사용하는 것보다, 언리얼의 \*\*향상된 입력 시스템(Enhanced Input System)\*\*을 활용하는 것이 유지보수와 성능 면에서 권장됩니다.

## **5\. 블루프린트 구성 예시**

1. Event Tick 노드 배치  
2. Is Input Key Down 노드 배치  
3. Target에 Get Player Controller 연결  
4. Key를 'Left Shift'로 설정  
5. Return Value를 Branch 노드에 연결하여 True일 때 달리기 로직 수행