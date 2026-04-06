# **Multi Line Trace By Object 노드 완벽 이해**

언리얼 엔진의 블루프린트에서 제공하는 Multi Line Trace By Object는 시작점(Start)과 끝점(End) 사이의 직선상에 있는 **특정 타입의 모든 오브젝트**를 탐지하여 배열 형태로 반환하는 노드입니다.

## **1\. 주요 역할 (Core Role)**

이 노드의 가장 큰 특징은 두 가지입니다:

* **Multi (다수 탐지):** 일반적인 Line Trace가 처음 부딪힌 물체 하나만 반환하고 멈추는 것과 달리, 경로 상에 있는 모든 유효한 물체를 뚫고 지나가며 전부 기록합니다.  
* **By Object (오브젝트 타입 기준):** '교전 채널(Collision Channel)'이 아닌, '오브젝트 타입(Object Type)'을 기준으로 필터링합니다. (예: WorldStatic, WorldDynamic, Pawn, PhysicsBody 등)

## **2\. 입력 항목 (Inputs)**

* **Start / End:** 레이저(선)가 시작되고 끝나는 3차원 좌표(Vector)입니다.  
* **Object Types:** 감지하고자 하는 오브젝트 타입들의 리스트입니다. (배열 형태) 여기에 포함되지 않은 타입의 물체는 무시하고 통과합니다.  
* **Trace Complex:** 체크하면 복잡한 콜리전(메시 자체)을 검사하고, 해제하면 단순한 캡슐/박스 콜리전을 검사합니다.  
* **Actors to Ignore:** 탐지에서 제외할 액터 리스트입니다. (보통 자기 자신을 제외할 때 사용)  
* **Draw Debug Type:** 디버그용 선을 화면에 그릴지 결정합니다. (For One Frame, For Duration 등)

## **3\. 출력 항목 (Outputs)**

* **Return Value:** 하나라도 부딪힌 물체가 있다면 True, 없으면 False를 반환합니다.  
* **Out Hits:** 가장 중요한 출력값으로, 부딪힌 모든 물체의 정보를 담은 **Hit Result 구조체 배열**입니다.

## **4\. Single Line Trace와의 차이점**

| 특징 | Single Line Trace | Multi Line Trace |
| :---- | :---- | :---- |
| **결과 수** | 단 하나 (가장 먼저 부딪힌 것) | 여러 개 (경로 상의 모든 것) |
| **관통 여부** | 첫 충돌 시 중단 | 끝점까지 관통하며 계속 탐색 |
| **데이터 타입** | 단일 Hit Result 구조체 | Hit Result 구조체 **배열** |

## **5\. 주요 활용 사례**

1. **관통탄 (Piercing Projectiles):** 총알이 여러 명의 적을 뚫고 지나가며 데미지를 입혀야 할 때.  
2. **범위 스캔:** 특정 방향으로 레이저를 쏘아 그 사이에 있는 아이템이나 상호작용 오브젝트를 모두 찾을 때.  
3. **복합 지형 분석:** 캐릭터 발 아래에 여러 겹의 레이어(예: 물, 투명 벽, 바닥)가 있을 때 이를 순차적으로 확인해야 하는 경우.

## **6\. 사용 시 주의사항**

* **성능:** 너무 많은 오브젝트를 동시에 추적하거나, 매 프레임(Tick)마다 너무 긴 거리의 Multi Trace를 수행하면 성능에 부담을 줄 수 있습니다.  
* **배열 처리:** 출력값이 배열이므로, 반드시 ForEachLoop 노드를 사용하여 각각의 Hit 결과를 처리해야 합니다.  
* **순서:** 반환되는 배열의 순서는 일반적으로 시작점(Start)에서 가까운 순서대로 정렬됩니다.

// 블루프린트 로직 예시 흐름  
1\. Start(카메라 위치)와 End(카메라 앞 1000유닛) 설정  
2\. Object Types에 'Pawn'과 'WorldDynamic' 추가  
3\. Multi Line Trace By Object 실행  
4\. Out Hits 결과값을 ForEachLoop에 연결  
5\. Loop 내부에서 'Break Hit Result'를 통해 부딪힌 액터에 데미지 전달 또는 효과 생성  
