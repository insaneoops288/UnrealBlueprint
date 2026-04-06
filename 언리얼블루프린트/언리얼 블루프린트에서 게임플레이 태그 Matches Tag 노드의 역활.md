# **Gameplay Tag: Matches Tag 노드 완벽 이해하기**

언리얼 엔진의 블루프린트에서 Matches Tag 노드는 두 개의 **Gameplay Tag**를 서로 비교하여 불리언(True/False) 값을 반환하는 함수입니다.

## **1\. 기본 구조**

이 노드는 크게 세 가지 입력을 받습니다:

* **Tag to Check (확인할 태그):** 비교 대상이 되는 자식 후보 태그입니다.  
* **Parent Tag (부모 태그):** 기준이 되는 부모 태그입니다.  
* **Exact Match (정확히 일치):** 계층 구조를 무시하고 완전히 동일한 태그인지만 확인할지 결정하는 옵션입니다.

## **2\. 'Exact Match' 옵션에 따른 동작 차이**

이 노드의 가장 중요한 부분은 Exact Match 체크박스입니다.

### **Case A: Exact Match가 체크됨 (True)**

두 태그가 **문자 하나 틀리지 않고 완벽하게 일치**해야만 True를 반환합니다.

* 예: Weapon.Ranged.Rifle vs Weapon.Ranged.Rifle \-\> **True**  
* 예: Weapon.Ranged.Rifle vs Weapon.Ranged \-\> **False**

### **Case B: Exact Match가 체크되지 않음 (False \- 기본값)**

\*\*계층 구조(Hierarchy)\*\*를 인식합니다. Tag to Check가 Parent Tag와 같거나, 그 \*\*하위 태그(자식)\*\*라면 True를 반환합니다.

* 예: Weapon.Ranged.Rifle vs Weapon.Ranged \-\> **True** (Rifle은 Ranged의 자식이므로)  
* 예: Weapon.Ranged.Rifle vs Weapon \-\> **True**  
* 예: Weapon vs Weapon.Ranged \-\> **False** (부모는 자식에게 포함되지 않음)

## **3\. Matches Tag vs Has Tag (차이점)**

초보자들이 가장 많이 헷갈려 하는 부분입니다.

| 노드 이름 | 비교 대상 | 주 용도 |
| :---- | :---- | :---- |
| **Matches Tag** | **태그 vs 태그** | 단일 태그 변수끼리 비교할 때 사용 (예: 현재 들고 있는 무기 태그 확인) |
| **Has Tag** | **컨테이너(태그 묶음) vs 태그** | 액터나 컴포넌트가 특정 태그를 가지고 있는지 확인할 때 사용 (예: 캐릭터가 '중독' 상태인지 확인) |

## **4\. 실제 활용 사례**

### **① 데미지 타입 판별**

게임에 Damage.Fire, Damage.Fire.Explosion, Damage.Ice 같은 태그가 있다고 가정합시다.

* 화염 저항이 있는 몬스터에게 데미지가 들어올 때, Matches Tag를 사용하여 들어온 데미지 태그가 Damage.Fire(Parent Tag)를 매치하는지 확인합니다.  
* 이때 Exact Match를 끄면, 일반 화염(Fire)과 폭발 화염(Explosion) 모두를 한 번에 감지하여 저항 처리를 할 수 있습니다.

### **② 무기 분류 시스템**

UI에서 현재 장착된 무기 아이콘을 표시할 때 사용합니다.

* 무기 데이터에 Item.Equip.Weapon.Sword 태그가 있다면, Matches Tag로 Item.Equip.Weapon과 비교하여 이 아이템이 '무기' 카테고리에 속하는지 확인하고 로직을 실행합니다.

## **5\. 요약**

* **Matches Tag**는 두 태그 간의 **'부모-자식' 관계**를 확인하는 노드입니다.  
* **계층적 검사**가 필요할 때는 Exact Match를 끄고 사용하세요.  
* **특정 카테고리 전체**를 포괄하는 로직을 짤 때 매우 강력한 도구가 됩니다.