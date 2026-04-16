# **Unreal Engine Blueprint: Set Actor Location 노드 가이드**

Set Actor Location 노드는 월드 공간(World Space) 내에서 특정 액터(Actor)를 새로운 좌표(![][image1])로 즉시 이동시키는 역할을 합니다.

## **1\. 노드의 주요 기능**

이 노드는 액터의 루트 컴포넌트(Root Component) 위치를 지정된 좌표로 설정합니다. 캐릭터의 리스폰, 아이템의 배치, 특정 위치로의 순간이동 등을 구현할 때 필수적으로 사용됩니다.

## **2\. 입력 핀 (Input Pins) 상세 설명**

| 핀 이름 | 타입 | 설명 |
| :---- | :---- | :---- |
| **Target** | Actor Object Reference | 위치를 변경할 대상 액터입니다. 기본값은 Self입니다. |
| **New Location** | Vector | 액터가 이동할 월드 좌표값(![][image1])입니다. |
| **Sweep** | Boolean | **충돌 체크 여부**입니다. True일 경우 이동 경로상에 충돌체(Blocking)가 있으면 그 지점에서 멈춥니다. |
| **Teleport** | Boolean | **물리 물리 가속도 유지 여부**입니다. True일 경우 속도(Velocity)를 유지한 채 순간이동하며, 물리 엔진의 갑작스러운 튀어오름을 방지합니다. |

## **3\. 출력 핀 (Output Pins)**

* **Return Value (Boolean)**:  
  * 액터가 성공적으로 지정된 위치로 이동했으면 True를 반환합니다.  
  * Sweep이 켜져 있는 상태에서 장애물에 막혀 이동하지 못했다면 False를 반환합니다.

## **4\. 핵심 옵션 활용 팁**

### **Sweep (스윕)의 중요성**

* **Sweep이 꺼져 있을 때 (False)**: 액터가 벽이나 장애물을 뚫고 지정된 좌표로 그대로 들어갑니다. (완전한 순간이동)  
* **Sweep이 켜져 있을 때 (True)**: 이동 경로 사이에 다른 물리적 충돌체가 있다면 그 앞에 멈추게 됩니다. 캐릭터가 벽 안으로 들어가는 버그를 방지하고 싶을 때 유용합니다.

### **Teleport (텔레포트)의 중요성**

* 물리 엔진(Physics)이 활성화된 액터의 경우, 갑자기 위치를 바꾸면 속도가 급격히 변해 튕겨나가는 현상이 발생할 수 있습니다. Teleport 체크박스를 활성화하면 물리 엔진에 "이것은 의도적인 순간이동"임을 알려주어 연산 오류를 줄여줍니다.

## **5\. 자주 사용되는 상황**

1. **캐릭터 리스폰 (Respawn)**: 캐릭터가 죽었을 때 특정 체크포인트 좌표로 위치를 옮길 때 사용합니다.  
2. **포탈 이동 (Portal)**: 플레이어가 포탈에 닿았을 때 반대편 포탈의 좌표로 즉시 이동시킬 때 사용합니다.  
3. **오브젝트 배치**: 게임 시작 시 아이템이나 NPC를 특정 위치로 강제 이동시킬 때 활용합니다.  
4. **부착 해제 후 위치 조정**: 다른 액터에 붙어있던 액터가 떨어져 나올 때 특정 위치에 고정시키기 위해 사용합니다.

## **6\. 주의 사항**

* Set Actor Location은 **월드 좌표**를 기준으로 합니다. 만약 부모 액터를 기준으로 하는 상대적인 위치를 바꾸고 싶다면 Set Actor Relative Location 노드를 사용해야 합니다.  
* 매 프레임(Tick)마다 이 노드를 사용하여 부드러운 이동을 구현할 수도 있지만, 캐릭터의 경우에는 보통 Add Movement Input이나 Character Movement Component를 통해 이동하는 것이 물리 및 충돌 처리에 더 안정적입니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADkAAAAVCAYAAAD8dkbIAAACyklEQVR4Xu2WT6hMURzH78sToojmDc2fM/8iYjWliJ3FQ1jYWFBKSnaSp2wovRULNZJsLJWFl4WihKweOwvUQzavxOaVhfAyz+c3c868Mz/3mrlzh9X91q977+/7O99zfuf+zu/eIEiRIkWKFP8YxWJxizHmPbbg2bdCobBbeO4bivtYqVQ2ap0wEHsW++mPL5VKz3O53DoXk8/na/hnvZjO3HHAmlYzdtrpaL4FiB3YD+wdtsH5a7XaMp6n2IyL2Wx2pT+mT4ww/rqdfFKTAhIfR/+FbLjm+gVj6+h/xW74m9gFdnQFgQ8Jasqk1i0LPCcm9358HKC7C415bFp23OfK5bKBvy9X3x8XaJzHrgW91klyR+yO36nX60ttglfkXsfGgVdK85Kw8zPfenxTJLjdj48LdJajexOtsub+AJNlCXyLzWGXsUbSBB1YyCW7ga2StYk/wA7r2LhAew1JHuV2VHOhkEXIYqRB6NJKArN45l9zNHKygcxxPOhVXgMA7Z3a1wUC9mFNE3J+kiCTyaxC85nVfmoSnvMoSOmjPaf9HUBuxp7w6mdkMV4DGgqkOaC7wPX2sI6BD9vEZsQ014LrcvLd8htQ0G+d9wHTPgpN5tmjuaRg3WvlBclbDG1kUpYkdteRfgPiw7xVxw8C6YCm3Whm+Y7lNZ8Etok9xn6ZsEZmA+5h+32/64Zy9f0a0tnEtF+Dt1dB75MkKglrPgJLqtXq2N/i7afuliQY2sggylKikGe6iGDxAw73MuoPgje+iZjP8nZES/M+qIiDsmnYBc1FgdgTdkzUsRlhnROmfQQm5LnD4Dhmuv8pZRcOOZ7n0+LzecY8krrviAApO7hXdpIDPueA7in4L56W2Bu0tulYDdEk9jv2wXi/mg5ojyvdjunYxGAxJxHeq/3DgJQq2g3pE5r7nxhlEVdNj3IdFKIr+kF4uUbiNwQQ2amBN55DAAAAAElFTkSuQmCC>