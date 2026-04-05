# **Gameplay Tags: Does Container Have Tag 노드 가이드**

언리얼 엔진의 Does Container Have Tag 노드는 **Gameplay Tag Container(태그 보관함)** 내에 특정 **Gameplay Tag**가 존재하는지 여부를 불리언(Boolean) 값으로 반환하는 함수입니다.

## **1\. 노드의 주요 역할**

이 노드는 단순히 "이 리스트에 이 아이템이 있는가?"를 묻는 것과 비슷하지만, 게임플레이 태그 특유의 \*\*계층 구조(Hierarchy)\*\*를 인식한다는 점이 핵심입니다.

* **입력 (Inputs):**  
  * **Tag Container:** 검사 대상이 되는 태그들의 집합입니다. (예: 캐릭터가 현재 가지고 있는 모든 상태 태그)  
  * **Tag:** 확인하고자 하는 특정 태그입니다. (예: Status.Debuff.Stun)  
  * **Match Exactly (선택 사항):** true일 경우 태그가 정확히 일치해야 하며, false일 경우 계층 구조를 허용합니다.  
* **출력 (Outputs):**  
  * **Return Value:** 태그가 컨테이너에 존재하면 True, 없으면 False를 반환합니다.

## **2\. 계층 구조와 매칭 로직**

게임플레이 태그는 Parent.Child.Grandchild 식의 계층 구조를 가집니다. Does Container Have Tag는 이 구조를 어떻게 처리할까요?

### **Case A: 일반적인 체크 (Exact Match \= False)**

컨테이너에 Status.Debuff.Stun이 들어있을 때:

* Status.Debuff를 검색하면? \-\> **True** (부모 태그는 자식 태그를 포함하는 개념으로 작동할 수 있음)  
* Status를 검색하면? \-\> **True**

### **Case B: 엄격한 체크 (Exact Match \= True)**

컨테이너에 Status.Debuff.Stun이 들어있을 때:

* Status.Debuff.Stun을 검색하면? \-\> **True**  
* Status.Debuff를 검색하면? \-\> **False** (정확히 일치하지 않기 때문)

## **3\. 왜 사용하는가? (장점)**

1. **문자열 비교보다 빠름:** 내부적으로 정수(Integer) 기반으로 동작하여 String Name 비교보다 성능이 훨씬 뛰어납니다.  
2. **유연성:** 상태 이상, 아이템 카테고리, 팀 구분 등을 처리할 때 Enum이나 Name보다 훨씬 유연하게 확장할 수 있습니다.  
3. **가독성:** Has Tag?라는 직관적인 질문을 통해 블루프린트 로직을 쉽게 파악할 수 있습니다.

## **4\. 실전 활용 예시**

### **예시 1: 데미지 처리 시스템**

캐릭터가 공격을 받았을 때, 캐릭터의 태그 컨테이너에 State.Invincible(무적) 태그가 있는지 확인합니다.

* 존재한다면 (True) \-\> 데미지 무시  
* 존재하지 않는다면 (False) \-\> 체력 감소

### **예시 2: 아이템 장착 조건**

플레이어가 무기를 장착하려고 할 때, 해당 무기의 태그 컨테이너에 Weapon.Type.Sword가 있는지 확인하여 전사 클래스인지 판별합니다.

## **5\. 유사한 노드와 차이점**

| 노드 이름 | 설명 |
| :---- | :---- |
| **Does Container Have Tag** | 컨테이너에 **단 하나의 특정 태그**가 있는지 확인 |
| **Has All Tags** | 컨테이너가 지정된 **여러 태그를 모두** 가지고 있는지 확인 (AND 조건) |
| **Has Any Tags** | 컨테이너가 지정된 **여러 태그 중 하나라도** 가지고 있는지 확인 (OR 조건) |

## **요약**

Does Container Have Tag는 캐릭터나 오브젝트의 \*\*상태를 쿼리(Query)\*\*할 때 가장 기본이 되는 노드입니다. 게임의 규칙(상태 이상, 권한, 속성 등)을 정의할 때 이 노드를 활용하면 하드코딩 없이 매우 유연한 시스템을 구축할 수 있습니다.