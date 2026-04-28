# **Event PointDamage 상세 가이드**

언리얼 엔진에서 데미지 시스템은 크게 세 가지 이벤트로 나뉩니다: AnyDamage, PointDamage, RadialDamage. 그 중 **Event PointDamage**는 총기류(Line Trace)나 화살처럼 특정 부위를 정확히 타격하는 공격을 처리하는 데 최적화되어 있습니다.

## **1\. Event PointDamage의 주요 역할**

이 이벤트는 Apply Point Damage 함수에 의해 호출되며, 피격자(Actor)의 블루프린트에서 다음과 같은 정보를 바탕으로 로직을 실행합니다.

### **핵심 매개변수 (Input Pins)**

* **Damage**: 가해진 데미지의 양 (Float).  
* **Damage Type**: 데미지의 성격 (예: 화염, 물리, 마법 등).  
* **Hit Location**: 실제로 데미지가 들어온 월드 좌표. (피격 이펙트 생성 시 유용)  
* **Hit Normal**: 맞은 표면의 법선 벡터. (도탄 효과나 혈흔 방향 결정 시 유용)  
* **Hit Component**: 맞은 액터 내부의 구체적인 컴포넌트.  
* **Bone Name**: **가장 중요한 핀 중 하나.** 스켈레탈 메시의 경우 어느 뼈(Head, Arm, Leg 등)를 맞았는지 알려줍니다.  
* **Shot From Direction**: 공격이 날아온 방향 벡터입니다. (피격 애니메이션 방향 결정 시 사용)  
* **Instigated By**: 공격을 지시한 컨트롤러 (PlayerController 등).  
* **Damage Causer**: 공격을 직접 수행한 액터 (총알, 무기 등).

## **2\. 주요 활용 사례**

### **① 부위별 데미지 판정 (Headshot)**

Bone Name을 확인하여 'head'인 경우 데미지를 2배로 적용하거나 즉사시키는 로직을 구현할 수 있습니다.

* **로직**: Bone Name \== head \-\> Damage \* 2

### **② 피격 위치 기반 이펙트 (Hit Effects)**

Hit Location과 Hit Normal을 사용하여 총을 맞은 바로 그 지점에 불꽃이 튀거나 피가 튀는 파티클을 생성합니다.

* **함수**: Spawn Emitter at Location 또는 Spawn System at Location (Niagara)

### **③ 물리적 반동 및 밀려남 (Knockback)**

Shot From Direction을 사용하여 캐릭터가 총을 맞았을 때 뒤로 움찔하거나 밀려나는 물리 효과를 구현합니다.

* **함수**: Add Impulse 또는 피격 애니메이션의 블렌드 스페이스 파라미터 조절.

## **3\. Event AnyDamage와의 차이점**

| 특징 | Event AnyDamage | Event PointDamage |
| :---- | :---- | :---- |
| **정밀도** | 낮음 (모든 데미지 수용) | 높음 (특정 지점 타격) |
| **위치 정보** | 없음 | **있음** (Hit Location) |
| **부위 정보** | 없음 | **있음** (Bone Name) |
| **주사용처** | 독 데미지, 추락사, 단순 체력 감소 | 총격전, 근접 무기 정밀 타격 |

## **4\. 실전 블루프린트 구성 팁**

1. **공격자 측**: Line Trace By Channel 등을 통해 충돌 정보를 얻은 후, Apply Point Damage 함수를 호출합니다. 이때 Hit Result를 통째로 연결할 수 있습니다.  
2. **피격자 측**: Event PointDamage를 배치하고, 먼저 Damage Type을 확인한 뒤 Bone Name에 따라 데미지 연산을 수행합니다.  
3. **주의사항**: Event PointDamage가 호출되면 Event AnyDamage도 **동시에 호출**됩니다. 따라서 두 이벤트를 한 블루프린트에 모두 사용할 경우 데미지가 중복 계산되지 않도록 주의해야 합니다. (보통은 하나만 선택해서 사용합니다.)

## **요약**

Event PointDamage는 \*\*"누가, 어디서, 어느 부위를, 어떤 각도로 쐈는가"\*\*에 대한 정답을 주는 이벤트입니다. FPS나 TPS 게임처럼 정밀한 타격 판정이 필요한 프로젝트라면 필수적으로 사용해야 합니다.