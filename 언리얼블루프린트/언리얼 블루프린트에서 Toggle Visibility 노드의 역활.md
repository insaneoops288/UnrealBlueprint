# **Toggle Visibility 노드 상세 가이드**

언리얼 엔진 블루프린트에서 Toggle Visibility 노드는 개발자가 오브젝트의 노출 상태를 간편하게 제어할 수 있게 돕는 유용한 도구입니다.

## **1\. 핵심 역할**

이 노드의 주요 기능은 \*\*상태 반전(Inversion)\*\*입니다.

* **현재 Visible(보임) 상태인 경우:** Hidden(숨김) 상태로 변경합니다.  
* **현재 Hidden(숨김) 상태인 경우:** Visible(보임) 상태로 변경합니다.

Set Visibility 노드와 달리, 현재 상태를 체크하기 위한 Branch나 Is Visible 함수를 따로 사용할 필요가 없어 로직이 간소화됩니다.

## **2\. 주요 파라미터 (Inputs)**

* **Target (Scene Component):** 가시성을 변경할 대상 컴포넌트입니다. (예: Static Mesh, Light, Particle System 등)  
* **Propagate to Children (Boolean):** \- True로 설정하면 해당 컴포넌트에 부착된 모든 하위(자식) 컴포넌트의 가시성도 함께 토글됩니다.  
  * False로 설정하면 타겟 컴포넌트 본인만 변경됩니다.

## **3\. 작동 원리 (Logic)**

논리적으로는 다음과 같은 연산을 수행한다고 볼 수 있습니다.

![][image1]즉, 불리언(Boolean) 값의 NOT 연산과 동일한 결과를 가져옵니다.

## **4\. 실전 활용 사례**

### **A. 손전등(Flashlight) 기능**

플레이어가 특정 키(예: 'F' 키)를 누를 때마다 캐릭터에 부착된 Spot Light 컴포넌트의 불을 켜거나 끌 때 사용합니다.

* **Input Action F** \-\> **Toggle Visibility (Target: SpotLight)**

### **B. UI 및 메뉴 토글**

인게임에서 인벤토리 창이나 일시정지 메뉴(Widget Component 등)를 띄우거나 닫을 때 유용합니다.

### **C. 레이저 가이드선**

조준 시에만 나타나는 레이저 포인터를 구현할 때, 버튼 입력에 따라 가이드라인 메시를 끄고 켤 수 있습니다.

## **5\. Set Visibility와의 비교**

| 특징 | Toggle Visibility | Set Visibility |
| :---- | :---- | :---- |
| **제어 방식** | 현재 상태를 반전시킴 | 특정 상태(True/False)를 강제함 |
| **복잡도** | 단순함 (조건문 불필요) | 상황에 따라 Branch 노드 필요 |
| **주요 용도** | On/Off 스위치 형태 | 특정 이벤트 발생 시 확실히 숨기거나 보일 때 |

## **6\. 주의사항**

1. **렌더링 비용:** 비록 보이지 않게 설정(Hidden)하더라도 해당 컴포넌트가 메모리에서 제거되는 것은 아닙니다. 단순히 렌더링 스레드에서 제외되는 것입니다.  
2. **콜리전(Collision):** 가시성을 토글한다고 해서 물리적 충돌(Collision)까지 자동으로 꺼지지는 않습니다. 충돌도 함께 제어하려면 Set Collision Enabled 노드를 병행해서 사용해야 합니다.
