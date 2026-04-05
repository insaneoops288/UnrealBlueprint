# **Has Any Tags 노드 가이드**

언리얼 엔진의 **Gameplay Tag** 시스템에서 Has Any Tags 노드는 조건문을 작성할 때 가장 빈번하게 사용되는 함수 중 하나입니다.

## **1\. 노드의 핵심 역할**

이 노드는 두 개의 태그 컨테이너(Tag Container)를 비교하여 **논리적 합집합(OR)** 관계의 확인을 수행합니다.

* **비교 로직**: "A 컨테이너 안에 B 컨테이너에 포함된 태그가 **단 하나라도** 들어있는가?"  
* **결과값**: 하나라도 일치하면 True, 일치하는 것이 전혀 없으면 False를 반환합니다.

## **2\. 주요 입력 및 출력 (Pins)**

| 핀 이름 | 타입 | 설명 |
| :---- | :---- | :---- |
| **Target** | Gameplay Tag Container (Self) | 태그를 검사할 대상 컨테이너 (보통 액터나 컴포넌트가 가진 태그들) |
| **Other Container** | Gameplay Tag Container | 찾고자 하는 태그들의 리스트 |
| **Return Value** | Boolean | 하나라도 포함되어 있으면 True, 아니면 False |

## **3\. Has All Tags와의 차이점**

가장 많이 헷갈리는 노드인 Has All Tags와 비교하면 이해가 빠릅니다.

* **Has Any Tags (OR)**: 제시된 태그 중 **하나만** 있어도 통과.  
  * 예: \[독, 화상, 기절\] 중 하나라도 걸려 있는가?  
* **Has All Tags (AND)**: 제시된 태그가 **전부 다** 있어야 통과.  
  * 예: \[은신 상태\]이면서 동시에 \[공격 중\]인가?

## **4\. 실전 활용 예시**

### **① 상태 이상 확인 (Combat System)**

캐릭터가 행동 불능 상태인지 확인할 때 유용합니다.

* Other Container에 State.Stun, State.Frozen, State.Dead를 넣습니다.  
* 캐릭터의 태그 중 위 셋 중 하나라도 있다면 공격을 못 하게 막는 로직을 짭니다.

### **② 아이템 필터링 (Inventory)**

플레이어가 특정 카테고리의 아이템을 장착할 수 있는지 확인할 때 사용합니다.

* 특정 장비 슬롯이 Item.Weapon.Melee 또는 Item.Weapon.Ranged 태그를 가진 아이템만 허용하도록 설정할 수 있습니다.

### **③ 팀 피아식별**

* 대상 액터가 Team.Enemy.Elite 또는 Team.Enemy.Boss 태그를 가지고 있는지 확인하여 보너스 데미지를 주는 로직에 활용합니다.

## **5\. 팁: 부모/자식 태그 관계**

Gameplay Tag는 계층 구조(Hierarchical)를 가집니다.

* 만약 Other Container에 State.Debuff가 있고, 캐릭터가 State.Debuff.Poison을 가지고 있다면, Has Any Tags는 이를 **True**로 인식합니다. (부모 태그는 자식 태그를 포함하는 것으로 간주됩니다.)

## **요약**

Has Any Tags는 \*\*"이것들 중에 뭐라도 있어?"\*\*라고 묻는 노드입니다. 복잡한 If 문을 여러 번 쓰는 대신, 태그 리스트 하나로 깔끔하게 조건을 체크할 수 있게 해줍니다.