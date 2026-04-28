# **Set Collision Response to Channel 노드 설명**

언리얼 엔진의 Set Collision Response to Channel 노드는 특정 컴포넌트(Static Mesh, Collision Shape 등)의 충돌 설정을 동적으로 제어하는 데 핵심적인 역할을 합니다.

## **1\. 노드의 주요 역할**

기본적으로 액터의 충돌 설정은 디테일(Details) 패널에서 미리 정의하지만, 게임 플레이 도중 상황에 따라 충돌 규칙을 바꿔야 할 때가 있습니다. 이 노드는 **"특정 채널에 대해서만"** 반응을 수정할 수 있게 해줍니다.

* **예:** 캐릭터가 평소에는 '문'을 통과하지 못하지만, 특정 아이템을 먹으면 '문' 채널에 대해 Ignore로 설정하여 통과하게 만듦.

## **2\. 입력 핀(Input Pins) 설명**

| 입력 핀 | 설명 |
| :---- | :---- |
| **Target** | 충돌 설정을 변경할 **Primitive Component**(Mesh, Sphere Collision 등)를 연결합니다. |
| **Channel** | 변경하고자 하는 특정 **Collision Channel**을 선택합니다. (예: WorldStatic, Pawn, Visibility, Camera 등) |
| **New Response** | 해당 채널에 대해 어떤 반응을 보일지 결정합니다. (Ignore, Overlap, Block 중 선택) |

## **3\. 세 가지 응답 유형 (Response Types)**

노드에서 선택 가능한 New Response의 의미는 다음과 같습니다.

### **① Ignore (무시)**

* **효과:** 해당 채널과 아무런 상호작용을 하지 않습니다.  
* **물리:** 그냥 통과합니다.  
* **이벤트:** Hit이나 Overlap 이벤트가 발생하지 않습니다.

### **② Overlap (겹침)**

* **효과:** 해당 채널의 물체를 통과하지만, 겹쳤다는 사실을 감지합니다.  
* **물리:** 물리적으로 막히지 않고 통과합니다.  
* **이벤트:** OnComponentBeginOverlap, OnComponentEndOverlap 이벤트가 발생합니다.

### **③ Block (차단)**

* **효과:** 해당 채널의 물체를 물리적으로 막습니다.  
* **물리:** 서로 통과할 수 없으며 충돌이 일어납니다.  
* **이벤트:** OnComponentHit 이벤트가 발생합니다. (단, 양쪽 물체 모두 Block 설정이어야 함)

## **4\. 실무 활용 사례**

1. **무적 상태 구현:**  
   * 캐릭터가 무적 아이템을 획득했을 때, Channel을 Pawn 혹은 Projectile로 설정하고 New Response를 Ignore로 변경하여 적의 공격을 통과시킴.  
2. **조건부 통과 구역:**  
   * 특정 팀만 통과할 수 있는 에너지 장벽을 만들 때, 캐릭터의 팀 속성에 따라 해당 장벽 채널을 Block 또는 Ignore로 변경.  
3. **발사체(Projectile) 로직:**  
   * 발사체가 발사된 직후에는 발사한 본인(Pawn)을 무시하도록 Ignore로 설정했다가, 일정 시간 후 다시 Block으로 변경하여 자신에게도 피해를 줄 수 있게 설정.

## **5\. 유사 노드와 차이점**

* **Set Collision Response to All Channels:** 모든 채널에 대한 반응을 한 번에 동일하게(예: 모두 Ignore) 바꿀 때 사용합니다.  
* **Set Collision Enabled:** 충돌 기능 자체를 켜거나 끌 때(No Collision, Query Only, Physics Only 등) 사용합니다.

## **💡 요약**

Set Collision Response to Channel은 \*\*"A라는 컴포넌트가 B라는 채널을 가진 물체를 만났을 때 통과할 것인가, 겹칠 것인가, 막힐 것인가?"\*\*를 코드(블루프린트)로 실시간 결정할 때 사용하는 노드입니다.