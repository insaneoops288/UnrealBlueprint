# **Set Control Rotation 노드 가이드**

언리얼 엔진(Unreal Engine)에서 Set Control Rotation 노드는 플레이어의 \*\*'입력 방향(Control Rotation)'\*\*을 강제로 설정할 때 사용하는 매우 중요한 노드입니다.

## **1\. 핵심 역할**

Set Control Rotation은 **Player Controller**가 소유하고 있는 '컨트롤 회전값'을 직접 변경합니다.

* **컨트롤 회전(Control Rotation)이란?** 플레이어가 캐릭터를 조종할 때 "어디를 바라보고 있는가"를 나타내는 회전값입니다.  
* **시점 제어:** 주로 마우스나 조이스틱 입력을 통해 변경되지만, 이 노드를 사용하면 코드(블루프린트)로 시점을 강제 고정하거나 회전시킬 수 있습니다.

## **2\. 작동 원리**

보통 캐릭터(Pawn)의 카메라는 Use Pawn Control Rotation 옵션이 켜져 있습니다. 이 경우 카메라의 각도는 Player Controller의 Control Rotation 값을 그대로 따릅니다. 따라서 이 노드로 값을 수정하면 플레이어의 화면(시야)이 즉각적으로 돌아가게 됩니다.

## **3\. 주요 파라미터**

* **Target (Player Controller):** 이 노드는 'Player Controller' 객체를 대상으로 작동합니다. 따라서 보통 Get Player Controller 노드를 연결하여 사용합니다.  
* **New Rotation (Rotator):** 설정하고자 하는 새로운 회전값(Roll, Pitch, Yaw)입니다.

## **4\. Set Actor Rotation과의 차이점**

초보 개발자가 가장 많이 혼동하는 부분입니다.

| 기능 | Set Actor Rotation | Set Control Rotation |
| :---- | :---- | :---- |
| **대상** | 월드에 배치된 액터(몸체) | 플레이어 컨트롤러(조준 시점) |
| **용도** | 캐릭터의 몸 방향을 돌릴 때 | 플레이어의 카메라(시선)를 돌릴 때 |
| **영향** | Use Controller Rotation Yaw가 켜져 있으면 곧바로 다시 컨트롤 회전값으로 덮어씌워질 수 있음 | 시점의 근본적인 값을 바꾸므로 가장 확실하게 시선 제어 가능 |

## **5\. 주요 활용 사례**

1. **반동(Recoil) 구현:** 총을 쏠 때 시점이 위로 튀게 만들려면, 현재 Control Rotation에 특정 Pitch 값을 더해 이 노드로 재설정합니다.  
2. **카메라 리셋:** 시네마틱 연출 후 플레이어가 특정 방향을 보게 해야 할 때 사용합니다.  
3. **강제 시선 고정:** 특정 이벤트 발생 시 캐릭터가 특정 물체를 쳐다보게 만들 때(Find Look at Rotation 노드와 조합) 사용합니다.  
4. **리스폰 시 초기화:** 캐릭터가 사망 후 부활할 때 엉뚱한 곳을 보지 않도록 정면을 보게 설정합니다.

## **6\. 주의사항**

* **Target 연결 필수:** Target에 아무것도 연결하지 않으면 기본적으로 'Self'가 연결되는데, 만약 이 노드가 캐릭터 블루프린트 안에 있다면 오류가 발생하거나 작동하지 않습니다. 반드시 Get Player Controller를 연결해야 합니다.  
* **네트워크 환경:** 멀티플레이어 게임에서는 서버와 클라이언트 간의 회전 동기화 방식이 복잡할 수 있으므로, 소유권(Ownership)을 확인해야 합니다.

\# 사용 예시 (블루프린트 로직)  
\[Get Player Controller\] \-\> \[Target: Set Control Rotation\]  
\[Get Actor Rotation\] \-\> \[New Rotation\]

위와 같이 구성하면 플레이어의 시선이 즉시 액터가 바라보는 방향으로 정렬됩니다.