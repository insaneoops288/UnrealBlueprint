# **Get Actor Forward Vector 노드 상세 가이드**

언리얼 엔진 블루프린트에서 Get Actor Forward Vector는 액터의 **월드 좌표계 기준 정면 방향**을 계산하여 반환하는 함수입니다.

## **1\. 노드의 핵심 역할**

이 노드는 특정 액터가 월드 내에서 어느 방향을 향하고 있는지를 **단위 벡터(Unit Vector)** 형태로 알려줍니다.

* **방향 데이터:** 액터의 현재 회전(Rotation) 값을 기반으로 계산됩니다.  
* **단위 벡터:** 결과값의 길이는 항상 **1.0**입니다 (정규화된 상태).  
* **축 기준:** 언리얼 엔진에서 기본적으로 정면(Forward)은 **X축**을 의미합니다.

## **2\. 주요 특징**

* **World Space 기준:** 액터의 로컬 좌표가 아닌, 게임 세계 전체(World)에서의 방향을 반환합니다.  
* **회전값 반영:** 액터가 회전하면 이 벡터값도 실시간으로 변합니다. 예를 들어, 캐릭터가 동쪽을 보고 있다면 (1, 0, 0), 북쪽을 보고 있다면 (0, 1, 0)에 가까운 값을 가집니다.  
* **정규화(Normalized):** 길이가 1이므로, 이 값에 특정 숫자를 곱하면 해당 방향으로의 **거리**를 쉽게 계산할 수 있습니다.

## **3\. 자주 사용하는 활용 사례**

### **① 캐릭터 이동 (Movement)**

캐릭터를 '앞으로' 움직이게 할 때 사용합니다.

* Get Actor Forward Vector 값에 Speed 변수를 곱한 뒤, Add Actor World Offset이나 Add Movement Input 노드에 연결합니다.

### **② 라인 트레이스 (Line Trace / Raycasting)**

캐릭터 바로 앞에 무엇이 있는지 감지할 때 사용합니다.

* **시작점:** Get Actor Location  
* **끝점:** Get Actor Location \+ (Get Actor Forward Vector \* 감지 거리)

### **③ 발사체 생성 (Projectile Spawning)**

총알이나 마법 화살을 캐릭터가 보는 방향으로 날릴 때 사용합니다.

* Spawn Actor from Class로 발사체를 소환한 뒤, 해당 발사체의 속도(Velocity)를 Forward Vector \* 발사 속도로 설정합니다.

## **4\. 수학적 이해**

이 노드는 내부적으로 액터의 **Get Actor Rotation** 노드 값을 벡터로 변환한 것과 같습니다.

* Rotation \-\> Vector 변환과 결과값이 동일합니다.  
* 만약 액터의 '오른쪽'이나 '위쪽'이 필요하다면 각각 Get Actor Right Vector, Get Actor Up Vector를 사용하면 됩니다.

## **5\. 요약 주의사항**

* 이 노드는 \*\*위치(Location)\*\*를 주는 것이 아니라 \*\*방향(Direction)\*\*을 주는 것입니다.  
* 바닥에 떨어져 있는 아이템이 캐릭터 쪽을 보게 하거나, 특정 방향으로 밀쳐내는 로직을 짤 때 가장 먼저 떠올려야 하는 노드입니다.