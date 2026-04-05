
언리얼 엔진에서 Add Gameplay Tag 노드는 **Gameplay Tag Container** 변수에 새로운 태그를 추가하거나, **Gameplay Ability System(GAS)** 환경에서 액터에게 특정 상태(State)를 부여할 때 사용됩니다.

## **1\. 주요 역할**

### **① 상태 정의 (State Definition)**

캐릭터나 오브젝트가 현재 어떤 상태인지 정의합니다.

* 예: 캐릭터가 기절했을 때 State.Stunned 태그를 추가하여, 다른 시스템이 이 태그를 보고 "아, 이 캐릭터는 현재 조작 불능이구나"라고 판단하게 합니다.

### **② 조건부 로직 필터링 (Conditional Logic)**

특정 태그가 있는지 여부에 따라 로직을 실행하거나 중단합니다.

* 예: Add Gameplay Tag로 Buff.FireResistance를 추가했다면, 데미지 계산 로직에서 Has Tag 노드로 확인하여 화염 데미지를 경감시킬 수 있습니다.

### **③ 계층적 분류 (Hierarchical Categorization)**

Gameplay Tag는 Parent.Child 형태의 계층 구조를 가집니다. Add Gameplay Tag를 통해 하위 태그(예: Weapon.Ranged.Rifle)를 추가하면 상위 태그(Weapon) 검색 시에도 해당 오브젝트가 감지됩니다.

## **2\. 노드 구조 및 입력 파라미터**

* **Target/Container**: 태그를 추가할 목적지입니다. 보통 Gameplay Tag Container 변수 타입이 연결됩니다.  
* **Tag**: 추가하고자 하는 구체적인 태그(예: Effect.Slow)를 선택합니다.

## **3\. 실무 활용 시나리오**

### **\[시나리오 A: 상태 이상 시스템\]**

1. 독 화살에 맞음 \-\> Add Gameplay Tag 노드로 State.Poisoned 추가.  
2. 매 초마다 Has Tag (State.Poisoned)를 체크하여 HP 감소.  
3. 해독제를 먹으면 Remove Gameplay Tag로 해당 태그 제거.

### **\[시나리오 B: 아이템 상호작용\]**

1. 문(Door) 오브젝트에 Add Gameplay Tag로 Requirement.BlueKey를 설정해둠.  
2. 플레이어가 상호작용할 때 플레이어의 인벤토리에 해당 태그가 있는지 비교하여 문을 열지 결정.

## **4\. Gameplay Ability System(GAS)에서의 역할**

GAS를 사용하는 경우, 이 노드는 단순히 변수를 수정하는 것을 넘어 훨씬 강력한 기능을 수행합니다.

* **Ability Bloack/Cancel**: 특정 태그가 추가되면 현재 실행 중인 능력이 취소되거나, 새로운 능력의 실행이 막히도록 설정할 수 있습니다.  
* **Attribute Modifiers**: 특정 태그의 존재 유무에 따라 스탯(공격력, 속도 등)이 실시간으로 변하도록 연동됩니다.

## **5\. 요약 및 주의사항**

* **가독성**: Boolean 변수(isStunned, isDead 등)를 수십 개 만드는 것보다 태그 시스템 하나로 관리하는 것이 가독성과 확장성 면에서 압도적으로 유리합니다.  
* **중복 처리**: Add Gameplay Tag는 동일한 태그를 여러 번 추가하더라도 컨테이너 내에서 중복을 허용하지 않거나(Set 구조), GAS 설정에 따라 중첩 횟수를 관리할 수 있습니다.  
* **성능**: 단순한 문자열 비교보다 빠르도록 최적화되어 있어 대규모 프로젝트에서도 안심하고 사용할 수 있습니다.