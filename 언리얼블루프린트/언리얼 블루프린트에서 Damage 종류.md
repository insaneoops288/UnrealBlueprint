# **언리얼 엔진 블루프린트 데미지 시스템 (Damage System)**

언리얼 엔진은 데미지를 가하는 쪽(Causer)과 받는 쪽(Victim) 사이의 통신을 단순화하기 위해 표준화된 데미지 이벤트를 제공합니다.

## **1\. 데미지를 가하는 함수 (Apply Damage)**

블루프린트에서 데미지를 입힐 때 사용하는 대표적인 함수 3가지입니다.

### **① Apply Damage (기본 데미지)**

가장 단순한 형태의 데미지입니다. "누가 누구에게 얼마만큼의 데미지를 주었다"는 정보만 전달합니다.

* **주요 파라미터**:  
  * Damaged Actor: 데미지를 받을 대상  
  * Base Damage: 가할 데미지 양  
  * Damage Causer: 데미지를 일으킨 액터 (예: 총기, 트랩)  
  * Damage Type Class: 데미지의 종류 (아래 2번 항목 참조)

### **② Apply Point Damage (지점 데미지)**

특정 부위나 방향성이 있는 데미지(예: 총알 발사)에 사용됩니다.

* **추가 파라미터**:  
  * Hit Direction: 데미지가 들어온 방향 (넉백 처리에 유용)  
  * Hit Info: Line Trace 등으로 얻은 상세 충돌 정보  
  * Shot From Instigator: 데미지를 발생시킨 주체

### **③ Apply Radial Damage (범위 데미지)**

폭발물처럼 특정 지점을 중심으로 반경 내의 모든 액터에게 데미지를 줄 때 사용합니다.

* **추가 파라미터**:  
  * Origin: 폭발 중심지  
  * Damage Radius: 데미지 반경  
  * Damage Falloff: 중심에서 멀어질수록 데미지가 줄어드는 정도

## **2\. 데미지 타입 클래스 (DamageType Class)**

DamageType은 데미지의 \*\*'성질'\*\*을 정의하는 데이터 클래스입니다. 예를 들어 "화염 데미지", "독 데미지", "낙하 데미지" 등을 구분할 수 있습니다.

* **사용 이유**: 데미지를 받는 액터가 "이 데미지가 화염인가?"를 판단하여 저항력을 적용하거나 특정 이펙트(불타는 효과)를 재생하기 위함입니다.  
* **생성 방법**: 콘텐츠 브라우저 \-\> 우클릭 \-\> Blueprint Class \-\> DamageType 검색 후 상속받아 생성.

## **3\. 데미지를 받는 이벤트 (Damage Events)**

데미지를 받는 액터의 블루프린트 그래프에서 호출하여 처리하는 이벤트들입니다.

| 이벤트명 | 설명 |
| :---- | :---- |
| **Event AnyDamage** | 모든 종류의 데미지(Point, Radial 포함)를 통합해서 처리할 때 사용합니다. |
| **Event PointDamage** | 특정 지점에 맞았을 때만 호출되며, 타격 부위(Bone Name) 정보를 활용할 수 있습니다. |
| **Event RadialDamage** | 폭발 범위 내에 있을 때 호출됩니다. |

## **4\. 실전 워크플로우 (예시)**

1. **DamageType 생성**: BP\_DamageType\_Fire 클래스를 만듭니다.  
2. **공격자**: Apply Damage 함수를 호출하면서 Damage Type Class에 BP\_DamageType\_Fire를 지정합니다.  
3. **피격자**: Event AnyDamage를 배치합니다.  
4. **로직 처리**:  
   * Damage Type 핀을 Get Class로 확인합니다.  
   * 만약 클래스가 Fire라면 데미지를 2배로 받거나 몸에 불이 붙는 파티클을 생성합니다.  
   * Health 변수에서 최종 데미지 값을 뺍니다.

## **5\. 고급 팁**

* **Instigator vs Damage Causer**:  
  * Instigator: 데미지를 명령한 주체 (보통 컨트롤러나 플레이어 캐릭터).  
  * Damage Causer: 실제 데미지를 입힌 물리적 매체 (발사체, 칼, 수류탄).  
* **Damage Falloff**: Apply Radial Damage with Falloff 함수를 사용하면 거리에 따른 데미지 감소 곡선을 더 세밀하게 제어할 수 있습니다.