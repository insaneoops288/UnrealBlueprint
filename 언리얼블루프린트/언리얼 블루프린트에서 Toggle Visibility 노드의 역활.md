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

[image1]: <data:image/png;base64,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>