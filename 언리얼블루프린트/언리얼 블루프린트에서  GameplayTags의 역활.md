# **언리얼 엔진 GameplayTags의 역할과 활용**

**GameplayTags**는 단순한 '이름' 그 이상으로, 언리얼 엔진(특히 Gameplay Ability System, GAS)에서 데이터의 상태를 정의하고 분류하는 데 사용되는 계층적 명명 시스템입니다. Character.State.Stunned와 같이 점(.)으로 구분된 구조를 가집니다.

## **1\. 주요 역할 (The Core Roles)**

### **① 계층적 상태 관리 (Hierarchical State Management)**

GameplayTags의 가장 큰 특징은 **계층 구조**입니다.

* 예: Weapon.Ranged.Sniper라는 태그가 있다면, 이 태그는 Weapon과 Weapon.Ranged 하위에도 포함되는 것으로 간주될 수 있습니다.  
* 이를 통해 "현재 캐릭터가 무기(Weapon)를 들고 있는가?"라는 넓은 범위의 체크와 "스나이퍼를 들고 있는가?"라는 세부 체크를 동시에 효율적으로 처리할 수 있습니다.

### **② 시스템 간의 디커플링 (Decoupling)**

클래스 간의 직접적인 참조(Hard Reference)를 줄여줍니다.

* 특정 액터가 '불타는 상태'인지 확인하기 위해 해당 클래스의 내부 변수 bool bIsBurning에 접근할 필요가 없습니다.  
* 대신 액터에게 State.Burning 태그가 있는지 확인하면 됩니다. 이는 코드가 서로를 몰라도 소통할 수 있게 합니다.

### **③ 유연한 조건 검사 (Gameplay Tag Queries)**

단순히 태그가 있는지 확인하는 수준을 넘어, 복잡한 논리 연산을 지원합니다.

* "A 태그는 가지고 있지만, B 태그는 없어야 한다"와 같은 조건을 **Gameplay Tag Query**를 통해 블루프린트 노드 하나로 판별할 수 있습니다.

## **2\. Enum vs GameplayTags 비교**

| 기능 | Enum (열거형) | GameplayTags |
| :---- | :---- | :---- |
| **확장성** | 수정 시 컴파일이 필요하고 구조 변경이 어려움 | 데이터 에셋 기반으로 실시간 추가/변경 용이 |
| **중복 선택** | 하나만 선택 가능 (Flags 제외) | 여러 태그를 동시에 소유 가능 (Container) |
| **계층 구조** | 지원하지 않음 | 부모-자식 관계 지원 |
| **검색 속도** | 매우 빠름 (정수 비교) | 매우 빠름 (내부적으로 최적화된 해시 사용) |

## **3\. 블루프린트에서의 실제 활용 사례**

### **1\) 상태 이상 및 제어**

캐릭터가 '기절(Stunned)' 상태일 때 공격을 막고 싶다면:

* 공격 함수 시작 부분에 Does Container Have Tag? 노드를 배치하여 State.Stunned 태그가 있는지 확인합니다.

### **2\) 상호작용 필터링**

특정 문이 '열쇠'가 필요한 경우:

* 플레이어의 인벤토리에 Item.Key.Blue 태그가 있는지 체크하여 상호작용 여부를 결정합니다.

### **3\) 이펙트 및 사운드 제어**

* 캐릭터의 표면 재질에 따라 발소리를 다르게 내고 싶을 때:  
* Surface.Grass, Surface.Stone 등의 태그를 통해 사운드 에셋을 매칭합니다.

## **4\. 블루프린트 주요 노드**

* **Has Tag**: 특정 태그를 가지고 있는지 확인.  
* **Has Any Tags**: 나열된 태그 중 하나라도 가지고 있는지 확인 (OR 조건).  
* **Has All Tags**: 나열된 태그를 모두 가지고 있는지 확인 (AND 조건).  
* **Add Gameplay Tag**: 런타임에 태그 추가 (주로 Tag Container 변수 사용 시).  
* **Remove Gameplay Tag**: 소유한 태그 삭제.

## **5\. 설정 방법 요약**

1. **Project Settings** \> **Gameplay Tags**로 이동합니다.  
2. 태그를 직접 추가하거나 .ini 파일 또는 Data Table을 통해 관리할 수 있습니다.  
3. 블루프린트 변수 타입을 Gameplay Tag 또는 Gameplay Tag Container로 설정하여 사용합니다.

**팁:** 단순한 상태 체크라면 bool이나 Enum보다 **GameplayTags**를 습관적으로 사용하는 것이 프로젝트 규모가 커졌을 때 유지보수에 훨씬 유리합니다.