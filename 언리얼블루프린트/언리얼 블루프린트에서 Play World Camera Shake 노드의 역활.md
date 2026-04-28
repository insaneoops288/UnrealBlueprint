# **Play World Camera Shake 노드 완벽 가이드**

Play World Camera Shake는 언리얼 엔진에서 폭발, 거대한 발자국, 지진과 같이 **세계의 특정 위치에서 발생한 사건**이 플레이어의 카메라에 영향을 주어야 할 때 사용되는 노드입니다.

## **1\. 주요 역할**

일반적인 Play Camera Shake가 플레이어에게 직접적으로 흔들림을 부여한다면, Play World Camera Shake는 \*\*위치 기반(Location-based)\*\*입니다. 발생 지점에서 멀리 떨어질수록 흔들림의 강도가 약해지는 '감쇠(Attenuation)' 효과가 자동으로 적용됩니다.

## **2\. 입력 핀(Input Pins) 상세 설명**

| 핀 이름 | 설명 |
| :---- | :---- |
| **Shake Class** | 사용할 Camera Shake 블루프린트 클래스를 지정합니다. (흔들림의 패턴, 빈도, 강도 정의) |
| **Epicenter** | 흔들림이 시작되는 세계 공간의 중심 좌표(Vector)입니다. 주로 폭발 지점의 위치를 연결합니다. |
| **Inner Radius** | 흔들림이 **100% 강도로 유지되는** 반경입니다. 이 거리 안에 있으면 가장 강하게 흔들립니다. |
| **Outer Radius** | 흔들림이 **영향을 미치는 최대 반경**입니다. Inner Radius에서 이 거리까지 갈수록 흔들림이 서서히 약해집니다. |
| **Falloff** | 흔들림 강도가 Inner Radius에서 Outer Radius로 가면서 줄어드는 곡선의 형태(Exponent)를 결정합니다. (1.0이면 선형) |
| **Orient Shake towards Epicenter** | 흔들림의 방향이 중심점을 향하도록 할지 여부입니다. |

## **3\. Play Camera Shake vs Play World Camera Shake**

| 구분 | Play Camera Shake | Play World Camera Shake |
| :---- | :---- | :---- |
| **적용 방식** | 특정 플레이어 컨트롤러에 직접 적용 | 월드의 특정 위치에서 발생 |
| **거리 감쇠** | 없음 (어디에 있든 동일한 강도) | **있음** (거리에 따라 약해짐) |
| **주요 용도** | UI 피드백, 캐릭터 피격, 사격 반동 | 폭발, 환경적 진동, 거대 몬스터의 발자국 |

## **4\. 블루프린트 활용 예시: 폭발 효과**

폭발 액터(Actor) 내에서 다음과 같이 구성하는 것이 일반적입니다.

1. **Event Any Damage** 또는 폭발 커스텀 이벤트 발생.  
2. **Spawn Emitter at Location** (폭발 이펙트 생성).  
3. **Play Sound at Location** (폭발 사운드 재생).  
4. **Play World Camera Shake** 호출:  
   * **Shake Class**: 미리 만들어둔 CS\_Explosion 선택.  
   * **Epicenter**: Get Actor Location 연결.  
   * **Inner Radius**: 500.0  
   * **Outer Radius**: 2000.0  
5. **Apply Radial Damage** (실제 대미지 처리).

## **5\. 팁 및 주의사항**

* **Camera Shake 클래스 생성**: 이 노드를 쓰려면 먼저 CameraShakeBase를 상속받은 블루프린트 클래스를 만들어 흔들림의 디테일(Oscillation, Perlin Noise 등)을 설정해야 합니다.  
* **성능**: 월드 내의 모든 플레이어 카메라 매니저를 체크하여 범위 내에 있는지 계산하므로, 너무 남발하기보다는 의미 있는 연출에 사용하는 것이 좋습니다.  
* **플레이어 화면 고정**: 만약 거리에 상관없이 플레이어가 특정 아이템을 먹었을 때 화면을 흔들고 싶다면 이 노드가 아닌 Client Start Camera Shake를 사용하세요.