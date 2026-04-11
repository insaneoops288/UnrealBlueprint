# **Get World Rotation 노드 완벽 가이드**

언리얼 엔진 블루프린트에서 Get World Rotation 노드는 씬 컴포넌트(Scene Component)나 액터의 **절대적인 회전값**을 가져오는 데 사용됩니다.

## **1\. 핵심 역할**

이 노드는 타겟이 되는 오브젝트가 게임 세계의 절대 좌표축(World X, Y, Z)을 기준으로 현재 어느 각도로 회전해 있는지를 **Rotator(로테이터)** 형식으로 반환합니다.

## **2\. 반환 데이터 (Rotator)**

결과값인 Rotator는 세 가지 축의 회전 정보를 담고 있습니다:

* **Roll (X축 회전):** 좌우로 기울어짐  
* **Pitch (Y축 회전):** 위아래로 끄덕임  
* **Yaw (Z축 회전):** 좌우로 회전 (도리도리)

## **3\. World Rotation vs Relative Rotation**

가장 헷갈리기 쉬운 부분입니다.

| 구분 | Get World Rotation | Get Relative Rotation |
| :---- | :---- | :---- |
| **기준** | **세계 절대 좌표계** (항상 고정) | **부모 컴포넌트** (부모를 기준 0으로 잡음) |
| **상황** | 부모가 회전하더라도, "실제 세계 기준" 어디를 보는지 알 때 | 부모에 대해 "상대적으로" 얼마나 돌아갔는지 알 때 |

### **예시:**

* 자동차(부모)가 90도 회전한 상태이고, 자동차 안의 운전자(자식)가 정면을 보고 있다면:  
  * **Relative Yaw:** 0도 (자동차 기준으로는 정면)  
  * **World Yaw:** 90도 (세계 기준으로는 자동차와 함께 돌아간 상태)

## **4\. 주요 사용 사례**

### **A. 발사체 스폰 (Spawning Projectiles)**

캐릭터가 총을 쏠 때, 총구(Muzzle) 컴포넌트의 Get World Rotation을 사용하여 총알이 날아갈 정확한 세계 방향을 설정합니다.

### **B. 카메라 방향 정렬**

캐릭터가 바라보는 방향(Camera World Rotation)에 맞춰 캐릭터의 몸을 회전시키거나, 특정 UI 요소를 항상 카메라를 향하게 할 때 사용합니다.

### **C. 벡터 계산 (Get Forward Vector)**

Get World Rotation 결과값을 Get Forward Vector 노드에 연결하면, 해당 오브젝트가 세계 기준으로 **앞으로 나아가는 방향 벡터**를 구할 수 있습니다.

## **5\. 노드 사용 팁**

1. **Target 입력:** 이 노드는 Scene Component를 타겟으로 받습니다. 액터 전체의 회전을 알고 싶다면 Get Actor Rotation 노드를 사용하는 것이 더 직관적입니다.  
2. **분할(Split):** 노드의 핀에서 우클릭하여 Split Struct Pin을 선택하면 Roll, Pitch, Yaw 값을 개별 플로트(Float) 값으로 직접 제어할 수 있습니다.