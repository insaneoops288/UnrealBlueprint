# **MultiSphereTraceByObject 노드 이해하기**

언리얼 엔진의 **MultiSphereTraceByObject**는 보이지 않는 '구체'를 특정 경로로 이동시키며, 그 경로에 걸리는 지정된 타입의 **모든** 오브젝트 정보를 가져오는 노드입니다.

## **1\. 핵심 역할 (Core Role)**

이 노드의 이름은 기능을 명확히 설명합니다:

* **Multi**: 첫 번째로 부딪힌 물체에서 멈추지 않고, 시작점부터 끝점까지 경로상에 있는 모든 물체를 검출합니다.  
* **Sphere**: 얇은 선(Line)이 아니라, 일정한 반지름(Radius)을 가진 입체적인 구체 형태로 검사합니다.  
* **Trace**: 공간을 가로질러 검사(Sweep)를 수행합니다.  
* **ByObject**: 'Visibility'나 'Camera' 같은 채널이 아니라, 오브젝트의 속성(WorldStatic, Pawn, PhysicsBody 등)을 기준으로 필터링합니다.

## **2\. 주요 입력 파라미터 (Inputs)**

| 파라미터 | 설명 |
| :---- | :---- |
| **Start / End** | 트레이스가 시작되고 끝나는 3D 공간의 위치(Vector)입니다. |
| **Radius** | 트레이스에 사용할 구체의 크기입니다. 값이 클수록 더 넓은 범위를 검사합니다. |
| **Object Types** | 검출하고자 하는 오브젝트 타입의 배열입니다. (예: Pawn만 체크, 혹은 WorldDynamic만 체크) |
| **Trace Complex** | 참(True)일 경우 복잡한 충돌(메시 자체)을 계산하고, 거짓이면 단순한 충돌 캡슐/박스를 계산합니다. |
| **Actors to Ignore** | 검사에서 제외할 액터 리스트입니다. 보통 'Self(자기 자신)'를 여기에 넣습니다. |
| **Draw Debug Type** | 개발 중 트레이스가 어디를 지나가는지 시각적으로 보여줍니다. (None, For One Frame, For Duration 등) |

## **3\. 주요 출력 파라미터 (Outputs)**

| 파라미터 | 설명 |
| :---- | :---- |
| **Return Value** | 하나라도 충돌한 물체가 있다면 True, 없으면 False를 반환합니다. |
| **Out Hits** | 충돌한 모든 물체에 대한 정보(Hit Result 구조체)를 담은 \*\*배열(Array)\*\*입니다. |

## **4\. 실무 활용 사례**

1. **광역 데미지 (AOE Skill)**:  
   * 폭발이 일어났을 때, 폭발 지점(Start)과 약간 떨어진 지점(End) 사이의 구체 범위 안에 있는 모든 적(Pawn)을 찾아 데미지를 입힐 때 사용합니다.  
2. **아이템 습득 (Looting)**:  
   * 캐릭터 주변의 일정 범위 내에 있는 'Item' 타입의 오브젝트를 모두 찾아서 리스트업할 때 유용합니다.  
3. **근접 공격 판정**:  
   * 칼을 휘두를 때 칼의 궤적을 따라 두꺼운 구체 트레이스를 생성하여, 궤적에 걸린 모든 적을 한 번에 베는 판정을 만들 수 있습니다.

## **5\. LineTrace / SingleTrace와의 차이점**

* **Line vs Sphere**: Line은 정확한 조준이 필요하지만, Sphere는 범위 기반이라 판정이 후합니다.  
* **Single vs Multi**: Single은 가장 먼저 부딪힌 것 하나만 알려주지만, Multi는 관통하여 모든 적의 정보를 가져옵니다.  
* **ByChannel vs ByObject**: Channel은 시야(Visibility) 차단 여부를 따질 때 좋고, Object는 특정 종류의 물체들을 골라낼 때 적합합니다.

## **6\. 주의사항 (Performance)**

* Multi 트레이스는 Single보다 계산 비용이 높고, Sphere는 Line보다 무겁습니다.  
* 특히 Out Hits 배열을 루프(ForEachLoop)로 돌릴 때, 검출된 물체가 너무 많으면 프레임 드랍이 발생할 수 있으므로 Object Types를 최대한 좁게 설정하는 것이 좋습니다.