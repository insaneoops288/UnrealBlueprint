# **Unreal Engine: Apply Point Damage 노드 상세 가이드**

Apply Point Damage는 특정 위치와 방향성을 가진 데미지를 특정 액터에게 전달할 때 사용되는 함수입니다. 단순히 체력을 깍는 것을 넘어, \*\*어디를 맞았는지(Hit Location)\*\*와 \*\*어느 방향에서 맞았는지(Shot From Direction)\*\*에 대한 정보를 함께 전달합니다.

## **1\. 주요 매개변수 (Inputs)**

이 노드를 사용할 때 입력해야 하는 주요 항목들은 다음과 같습니다.

| 매개변수 | 설명 |
| :---- | :---- |
| **Damaged Actor** | 데미지를 입을 대상 액터입니다. |
| **Base Damage** | 가할 기본 데미지 수치입니다. |
| **Shot From Direction** | 공격이 날아온 방향(Vector)입니다. 피격 애니메이션이나 물리 임펄스 계산에 사용됩니다. |
| **Hit Info** | Line Trace 등의 결과로 나온 Hit Result 구조체를 연결합니다. 타격 지점, 노멀, 맞은 뼈(Bone) 이름 등을 포함합니다. |
| **Damage Causer** | 데미지를 직접적으로 입힌 주체(예: 총알 액터 또는 무기)입니다. |
| **Instigated By** | 데미지 발생을 책임지는 컨트롤러(예: 플레이어 컨트롤러)입니다. 주로 점수 계산이나 킬 로그 생성 시 사용됩니다. |
| **Damage Type Class** | 데미지의 성격(예: 화염, 물리, 폭발 등)을 정의하는 클래스입니다. |

## **2\. 작동 방식 및 이벤트 흐름**

1. **공격 측 (Attacker):** Line Trace 등을 통해 적을 감지한 후 Apply Point Damage를 호출합니다.  
2. **엔진 내부:** 해당 액터의 TakeDamage 시스템을 가동시키며, 구체적인 정보를 포함한 OnTakePointDamage 이벤트를 발생시킵니다.  
3. **수비 측 (Victim):** 데미지를 입은 액터의 블루프린트에서 Event OnTakePointDamage 노드를 배치하여 반응을 구현합니다.

## **3\. Apply Point Damage의 장점**

* **부위별 데미지 판정:** Hit Info 내의 Hit Bone Name을 확인하여 헤드샷(Headshot)은 2배 데미지를 주는 등의 로직을 쉽게 짤 수 있습니다.  
* **물리 효과 적용:** 타격 지점 정보를 알고 있기 때문에, 캐릭터가 사망할 때 맞은 부위에서 뒤로 밀려나는 물리 효과(Ragdoll Impulse)를 정교하게 구현할 수 있습니다.  
* **데미지 타입 연동:** 특정 속성(예: 얼음 속성 공격)에 대한 저항력을 계산할 때 유용합니다.

## **4\. 다른 데미지 노드와의 비교**

* **Apply Damage:** 가장 기본적인 노드로, 위치 정보 없이 수치만 전달합니다. (예: 독 데미지, 허기 등)  
* **Apply Radial Damage:** 폭발처럼 특정 반경 내의 모든 대상에게 데미지를 줄 때 사용합니다. (예: 수류탄, 지뢰)  
* **Apply Point Damage:** 정확한 타격 지점이 중요한 직사화기 공격에 최적화되어 있습니다.

## **5\. 실전 활용 팁**

피격자(Victim)의 블루프린트에서 데미지를 처리할 때는 Event Any Damage 보다는 \*\*Event OnTakePointDamage\*\*를 사용하는 것이 좋습니다. 이 이벤트는 Shot Direction과 Hit Location을 직접 출력해주기 때문에 별도의 구조체 분리 없이도 즉시 피격 이펙트나 사운드를 타격 지점에 생성할 수 있습니다.