# **Sphere Trace For Objects 노드 가이드**

언리얼 엔진 블루프린트에서 Sphere Trace For Objects는 특정 영역 내의 물체를 감지할 때 매우 유용한 노드입니다.

## **1\. 핵심 역할**

이 노드는 \*\*시작 지점(Start)\*\*에서 \*\*끝 지점(End)\*\*까지 \*\*반지름(Radius)\*\*을 가진 구체를 투사하여, 사용자가 지정한 \*\*오브젝트 타입(Object Types)\*\*과 충돌하는지 확인합니다.

* **범위 감지:** 선(Line)보다 넓은 범위를 체크하므로 "대충 조준해도 맞아야 하는" 판정에 유리합니다.  
* **필터링:** 특정 콜리전 채널(Visibility, Camera 등)이 아니라, 오브젝트의 물리적 성격(Pawn, WorldStatic, WorldDynamic 등)을 기준으로 필터링합니다.

## **2\. 주요 입력 파라미터 (Inputs)**

| 파라미터 | 설명 |
| :---- | :---- |
| **Start / End** | 구체 추적을 시작하고 끝낼 세계 좌표(Vector)입니다. |
| **Radius** | 추적할 구체의 크기입니다. 값이 클수록 더 넓은 범위를 체크합니다. |
| **Object Types** | 감지하려는 오브젝트 타입들의 배열입니다 (예: WorldDynamic, Pawn, PhysicsBody 등). |
| **Trace Complex** | 체크 시 복잡한 콜리전(메시 디테일)을 사용할지 여부입니다. 보통 성능을 위해 false로 둡니다. |
| **Actors to Ignore** | 추적에서 제외할 액터 목록입니다. 보통 '자기 자신(Self)'을 넣어 본인이 감지되는 것을 막습니다. |
| **Draw Debug Type** | 개발 중 추적 경로를 시각적으로 보여줍니다 (None, One Frame, For Duration 등). |

## **3\. 주요 출력 결과 (Outputs)**

* **Return Value:** 무언가에 부딪혔다면 true, 아무것도 발견하지 못했다면 false를 반환합니다.  
* **Out Hit (Hit Result):** 부딪힌 대상에 대한 상세 정보가 들어있는 구조체입니다.  
  * **Hit Actor:** 부딪힌 액터가 무엇인가?  
  * **Location / Impact Point:** 정확히 어디서 부딪혔는가?  
  * **Impact Normal:** 부딪힌 표면의 방향은 어디인가?  
  * **Distance:** 시작점에서 얼마나 떨어진 곳에서 부딪혔는가?

## **4\. 실전 활용 사례**

### **① 근접 공격 판정 (Melee Combat)**

칼을 휘두를 때 칼날의 궤적을 따라 구체 트레이스를 실행하면, 얇은 선보다 훨씬 관대한 히트 판정을 구현할 수 있습니다.

### **② 상호작용 시스템 (Interaction)**

플레이어 앞에 있는 아이템을 집을 때, 정확히 조준하지 않아도 근처에 있는 WorldDynamic 오브젝트를 쉽게 찾아낼 수 있습니다.

### **③ 폭발 데미지 체크**

특정 지점에서 아주 짧은 거리(Start와 End를 거의 같게 설정)로 구체 트레이스를 날려 주변에 있는 적(Pawn)들을 검색할 수 있습니다.

## **5\. Line Trace vs Sphere Trace**

| 특징 | Line Trace | Sphere Trace |
| :---- | :---- | :---- |
| **정밀도** | 매우 정밀함 (핀포인트) | 범위형 (관대함) |
| **용도** | 총기 사격, 레이저, 정확한 시선 체크 | 근접 공격, 광역 감지, 환경 조사 |
| **성능** | 가장 가벼움 | Line Trace보다는 무겁지만 여전히 효율적 |

## **💡 팁: 멀티 트레이스 (Multi Sphere Trace)**

만약 경로상에 있는 **모든** 물체를 다 찾고 싶다면 Sphere Trace For Objects 대신 **Multi Sphere Trace For Objects**를 사용하세요. 이 노드는 첫 번째 부딪힌 물체에서 멈추지 않고 경로상의 모든 결과를 배열로 반환합니다.