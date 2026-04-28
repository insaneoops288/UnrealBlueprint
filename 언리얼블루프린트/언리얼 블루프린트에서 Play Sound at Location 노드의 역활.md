# **Play Sound at Location 노드 상세 가이드**

## **1\. 노드의 핵심 역할**

Play Sound at Location은 월드 상의 특정 지점(![][image1] 좌표)에서 사운드를 즉시 재생합니다. 이 노드는 **"Fire and Forget(실행 후 방치)"** 방식으로 동작합니다. 즉, 사운드가 시작되면 엔진이 자동으로 관리를 시작하며, 재생이 끝나면 메모리에서 스스로 해제됩니다.

## **2\. 주요 파라미터(입력 핀) 설명**

* **Sound**: 재생할 사운드 에셋을 선택합니다 (Sound Wave 또는 Sound Cue).  
* **Location**: 사운드가 시작될 월드 좌표입니다. 보통 Get Actor Location 등을 연결합니다.  
* **Rotation**: 사운드의 방향을 설정합니다 (주로 스테레오/서라운드 사운드 큐에서 중요).  
* **Volume Multiplier**: 기본 볼륨에 곱해질 수치입니다. (1.0이 기본값)  
* **Pitch Multiplier**: 재생 속도 및 높낮이를 조절합니다. (1.0이 기본값)  
* **Start Time**: 사운드의 특정 시간대부터 재생하고 싶을 때 사용합니다.  
* **Attenuation Settings**: 거리에 따른 음량 감쇄(Attenuation) 설정을 적용합니다. 3D 입체 음향 효과를 내기 위해 필수적입니다.

## **3\. Play Sound at Location vs Spawn Sound at Location**

두 노드는 비슷해 보이지만 결정적인 차이가 있습니다.

| 특징 | Play Sound at Location | Spawn Sound at Location |
| :---- | :---- | :---- |
| **참조 반환** | 없음 (Return Value 없음) | **Audio Component**를 반환함 |
| **제어 가능 여부** | 재생 후 정지/일시정지 불가 | 재생 중 중지, 볼륨 변경 등 실시간 제어 가능 |
| **사용 사례** | 폭발음, 총소리, 충격음 (단발성) | 배경음악(BGM), 지속적인 엔진 소리, 불꽃 소리 |

## **4\. 실전 활용 팁**

1. **3D 입체감 구현**: Attenuation Settings가 설정되지 않으면 위치값에 상관없이 사용자 귀에 2D 사운드처럼 똑똑하게 들립니다. 거리감을 주려면 반드시 **Sound Attenuation** 에셋을 할당해야 합니다.  
2. **메모리 관리**: 참조를 남기지 않기 때문에 루핑(Looping) 사운드를 이 노드로 재생하면 **수동으로 끌 수 있는 방법이 없으므로** 주의해야 합니다. (루핑 사운드는 Spawn Sound at Location을 권장합니다.)  
3. **최적화**: 수많은 사운드가 한 위치에서 동시에 발생하면 성능에 영향을 줄 수 있으므로, 동시 재생 개수 제한(Concurrency) 설정을 사운드 에셋 자체에서 해두는 것이 좋습니다.

## **5\. 블루프린트 예시 흐름**

1. 캐릭터가 벽에 충돌함 (OnComponentHit 이벤트)  
2. Get Actor Location으로 충돌 위치 획득  
3. Play Sound at Location의 Location 핀에 연결  
4. Sound 항목에 '충돌 효과음' 에셋 선택  
5. 충돌할 때마다 해당 위치에서 소리 발생

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEEAAAAYCAYAAACldpB6AAADPElEQVR4Xu1WXYhMYRg+244QRRhj5++bv4jIxZQi7ig/4ULJBaWk5E5CSVFSxIUaob1xqSibC0VtyNVy54KtRUpb4oJyIQy7nsd83+47rzM758wZ2ovz1Ns58/485/ve733f+TwvRowYMWLEmBL5fH6FMeY1ZFzI11wut4F2vNeU7W2pVFqqefwA36OQHzK+UCg8yWQyC51PNputQD8qfCa+HQZY0zzEDslvQT5hf+/4dL+LxeJqHTsBOKyFfIe8gvQ5faVSmYnfAyA7nUql5siYgOhB/BW7iHPaSCAxm8H/lAeibUGB2Cr4v0CuyiRXq9UZ0PVDfkF2yZi/gBOZDaL7cBzjoqyaGzhG4bv0DwPwrgdHHTLEE5M2nIyB/S6fUh8W4DgBueyJdTIB2MsZ7gm249LWEgjYY0/sps0gE3CR79o3DESp1pkQp8f3lkA3gASskf5hAZ5Z4L0OrqJQ93DjNgHXAu8Bi0khaBjyGXIWUgsc3Ab2RJjgPy1hE3PPtCvRAAD3fGx0L14TTkde02iB/tB74CK5WA4wXbpRYCZnzgu0XoYJxjf2e0FKNCTAvc40DnKwoz0gcCtkzPj0bxQkk8m54HxsuR+ZiHOmFdhaTAAqY6SjOYPg5ZCHJOBixYDsCji8wDuO543QJRoAdsiO6ASw8gIlxE1p/m/LAemJPosK02g1DqqN2hYVWPcCHiCrQA9a6E7JgewLlj02fssFywGJi8tK7d8JOMFNYxCO4n88q+1RYIfsoPG5C9i93E6n04ukvgmW4A5km9S7ac6n1GtwMlO0XgMnUQLfeyaCCdH2Fugtl8uLp/KXlyGfQev+JmtKPwkYi2wBBB/RNpYP7HXYnskbmASyvAw+H3i65NJ2CVTUDiYVclLbWgG+B2xMq7aUd4GmyxArALrzsP30nW0w7jPNd3pmcaez4/dh6qQdMQ/Yd5KHZQ3bc7uI7dLmAN5DsH8UXJSX4FqlfTXICd9vkDdGXOUduDnF6yfDTIiO7Tqw2IP42Bat7wbYCuCu/ZeNREACi7xk2rRDpyAv+T3/dpgeYElCLnitBk809DIBqLRN2jCdkMACd3fzZimBBPTZWfUvEhzjN44qCQl6G6EUAAAAAElFTkSuQmCC>