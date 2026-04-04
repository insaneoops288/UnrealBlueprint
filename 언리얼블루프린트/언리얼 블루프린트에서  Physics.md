# **Unreal Engine Blueprint Physics Guide**

언리얼 엔진에서 물리 시뮬레이션을 구현하기 위해 반드시 알아야 할 핵심 개념과 블루프린트 활용법을 정리한 가이드입니다.

## **1\. 물리 시뮬레이션의 기본 설정**

블루프린트 내의 컴포넌트(주로 Static Mesh 또는 Collision)가 물리적으로 동작하게 하려면 다음 설정을 확인해야 합니다.

* **Simulate Physics**: 이 체크박스를 활성화해야 엔진의 물리 엔진(Chaos Physics)이 해당 객체를 제어합니다.  
* **Enable Gravity**: 중력 적용 여부를 결정합니다.  
* **Mass (kg)**: 질량을 설정합니다. 질량이 높을수록 더 큰 힘이 있어야 움직입니다.  
* **Collision Presets**: 'Physics Actor' 혹은 'BlockAll' 등으로 설정되어 있어야 다른 물체와 충돌합니다.

## **2\. 힘을 가하는 핵심 노드 (Force vs Impulse)**

물체를 움직이게 할 때 가장 많이 사용하는 두 가지 방식입니다.

### **Add Force (지속적인 힘)**

* **특징**: 매 프레임마다 가해지는 힘입니다. (뉴턴 단위)  
* **용도**: 추진기(Thruster), 바람, 부력 등 일정 시간 동안 계속 밀어줄 때 사용합니다.  
* **팁**: Tick 이벤트와 함께 사용하는 경우가 많습니다.

### **Add Impulse (즉각적인 충격)**

* **특징**: 한 번에 폭발적으로 가해지는 힘입니다. (속력 변화량)  
* **용도**: 총알에 맞았을 때, 폭발, 점프, 야구방망이 휘두르기 등.  
* **Vel Change**: 이 옵션을 체크하면 질량을 무시하고 즉시 속도를 변경합니다.

## **3\. 회전과 속도 제어**

### **Add Torque (토크)**

* 물체를 회전시키는 힘입니다. Add Force의 회전 버전이라고 생각하면 됩니다.

### **Set Physics Linear Velocity**

* 물체의 선형 속도를 강제로 고정합니다. 물리 법칙을 무시하고 특정 속도로 달리고 싶을 때 유용합니다.

## **4\. 충돌 이벤트 처리 (Collision Events)**

블루프린트에서 물리적 상호작용을 감지하는 두 가지 주요 이벤트입니다.

| 이벤트명 | 설명 | 필수 조건 |
| :---- | :---- | :---- |
| **OnComponentHit** | 두 물체가 'Block' 설정일 때 쾅 부딪히는 순간 발생 | Simulation Generates Hit Events 체크 |
| **OnComponentBeginOverlap** | 물체가 영역을 '통과'할 때 발생 (트리거) | Generate Overlap Events 체크 |

## **5\. 피직스 머티리얼 (Physics Material)**

물체의 "표면 성질"을 정의합니다. 블루프린트 노드는 아니지만, 물리 반응에 결정적인 역할을 합니다.

* **Friction (마찰력)**: 0이면 얼음판처럼 미끄럽고, 값이 높으면 끈적하게 멈춥니다.  
* **Restitution (탄성)**: 물체가 튀어오르는 정도입니다. 1이면 완벽하게 튕겨 나갑니다.

## **6\. 물리 제약 (Physics Constraints)**

두 물리 객체를 연결하는 "관절" 역할을 합니다.

* **사용 예**: 문 경첩(Hinge), 자동차 바퀴 현가장치, 쇠사슬 등.  
* 블루프린트에서 Physics Constraint Component를 추가하여 두 컴포넌트를 연결하고 회전/이동 범위를 제한할 수 있습니다.

## **7\. 최적화 팁**

1. **Sleep Family**: 움직이지 않는 물체는 물리 엔진이 '잠들게(Sleep)' 하여 CPU 소모를 줄입니다.  
2. **CCD (Continuous Collision Detection)**: 너무 빠른 물체(총알 등)가 벽을 뚫고 지나가는 것을 방지합니다. 성능 비용이 높으므로 필요한 경우에만 켭니다.  
3. **Substepping**: 물리 계산 주기를 더 쪼개서 정밀도를 높입니다. (Project Settings에서 설정)

### **자주 쓰는 워크플로우 예시**

1. **폭발 구현**: Radial Force Component를 배치하거나 Add Radial Impulse 노드를 호출합니다.  
2. **물건 집기**: Physics Handle Component를 사용하여 물체를 잡고 물리 법칙을 유지하며 이동시킵니다.