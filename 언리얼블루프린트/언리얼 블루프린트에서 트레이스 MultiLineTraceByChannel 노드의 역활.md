# **MultiLineTraceByChannel 노드 완벽 이해하기**

언리얼 엔진 블루프린트에서 MultiLineTraceByChannel은 레이캐스팅(Raycasting) 기술의 일종으로, 가상의 선을 그어 해당 선에 닿는 여러 개의 물체를 한꺼번에 찾아내는 역할을 합니다.

## **1\. 핵심 역할**

일반적인 LineTraceByChannel(Single Trace)은 선이 물체에 부딪히는 순간 멈추고 첫 번째 타겟 정보만 반환하지만, **MultiLineTraceByChannel**은 선이 끝나는 지점까지 경로상에 있는 **모든 유효한 충돌 정보**를 배열(Array) 형태로 반환합니다.

## **2\. 주요 입력 핀 (Inputs)**

| 입력 핀 | 설명 |
| :---- | :---- |
| **Start** | 트레이스(선)가 시작되는 세계 좌표 (![][image1]) 입니다. |
| **End** | 트레이스가 끝나는 세계 좌표입니다. |
| **Trace Channel** | 어떤 충돌 채널을 기준으로 검사할지 결정합니다 (예: Visibility, Camera, 또는 사용자 정의 채널). |
| **Actors to Ignore** | 검사에서 제외할 액터들의 목록(배열)입니다. 보통 자기 자신(Self)을 포함시킵니다. |
| **Draw Debug Type** | 개발 중 선이 제대로 그려지는지 시각적으로 확인하기 위한 옵션입니다 (None, For One Frame, For Duration, Persistent). |

## **3\. 주요 출력 핀 (Outputs)**

| 출력 핀 | 설명 |
| :---- | :---- |
| **Return Value** | 하나 이상의 충돌이 발생하면 True, 아무것도 맞지 않으면 False를 반환합니다. |
| **Out Hits** | 부딪힌 모든 물체들에 대한 정보를 담은 **Hit Result 구조체 배열**입니다. |

### **Hit Result 내부 주요 정보:**

* **Hit Actor**: 부딪힌 대상 액터.  
* **Location / Impact Point**: 충돌이 발생한 정확한 위치.  
* **Impact Normal**: 충돌 면의 법선 벡터 (튕겨 나가는 방향 등을 계산할 때 사용).  
* **Phys Mat**: 부딪힌 물체의 물리 재질.

## **4\. 작동 방식의 특이점 (Blocking vs Overlap)**

* **Blocking Hit**: 트레이스 채널 설정에서 'Block'으로 설정된 물체를 만나면 해당 지점의 정보가 기록됩니다.  
* **관통 여부**: MultiLineTrace는 기본적으로 경로 상의 모든 것을 훑지만, 만약 트레이스 설정이 **Blocking**으로 되어 있는 물체를 만나면 그 이후의 물체들도 감지할 수 있는지 여부는 엔진의 충돌 프리셋 설정에 따라 달라집니다. 일반적으로는 경로 끝까지 모든 Hit을 수집합니다.

## **5\. 주요 활용 사례**

1. **관통형 무기 (Piercing Shot)**  
   * 총알이 한 적을 뚫고 뒤에 있는 적까지 맞추어야 하는 경우.  
2. **범위 내 다수 대상 검사**  
   * 특정 방향으로 쏜 레이저가 닿는 모든 오브젝트의 이름을 가져오거나 상호작용할 때.  
3. **환경 분석 (Scanning)**  
   * 캐릭터 앞에서 특정 높이 범위 내에 장애물이 몇 개나 있는지, 어떤 종류가 있는지 한 번에 파악할 때.  
4. **복잡한 상호작용**  
   * 투명한 유리창(Overlap) 뒤에 있는 스위치(Block)를 동시에 감지해야 할 때.

## **6\. 사용 팁**

* Out Hits는 배열이므로, 반드시 **For Each Loop** 노드와 연결하여 각 충돌 정보를 하나씩 처리해야 합니다.  
* 성능 최적화를 위해 너무 긴 거리의 트레이스를 매 프레임(Tick) 호출하는 것은 피하는 것이 좋습니다.

*이 노드는 물리적인 '두께'가 없는 선을 사용하므로, 만약 일정한 두께를 가진 검사가 필요하다면 MultiSphereTraceByChannel이나 MultiBoxTraceByChannel을 고려해 보세요.*

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADkAAAAWCAYAAAB64jRmAAAChUlEQVR4Xu2WP2sUURTFd3ENkkZEQdll9+1sYbEEgiwqBNIJGgQLgyBEG79CIEIqP0GKICSkUwg2FulEso1WCYISMEQEEURJp62CJp6TvU/eHmbcyewYm/nBhTf33nfmvjfvz5RKBQUFBQX/nnKj0fik5pxbYBDtu2h3YStmr1ut1nkViQN9N5H/rtlsPrK+G/BdCXOoRU341yxntVqtjoY5KalAY9nXr8EDoig6ixfs0NgOY/B9RaFX0SyH/rSg/wxsHxoPNEbq9fokCrtVyqhPoHGD74B1NdYHi7Bibnsfnicw6Eth3mGBRmQFbGgM2nC7LfUfFmgsou6XWBknNdYHZmPMinmKxwoHh/au5mWgYro/QieKOocv+AI2F/qzAO030Lum/lisGNqqvTzzEgpxva2wD81TfO50Osfx3B048ynBAC+rLxFfDI2FaDwr0Htog+TBw8NujktV8/JgoK7rnXAHg8RjReNZwYl5BprbsO+wL8Pu8yS4Mqz2RDjD3CPPmcg9qgnDAM1F2C/YtMbywLYAP9I3jXnKCM7yU/vjGIO9r0nDYIN85fdlnnCA/naA3dQ4X34d9jFcQtZhL/WJNQAMrAW9XdiMxvLAVgg/TPxJjeBnHX1wnTwJ/Qom4YT64vCrAxM5rrG/wO3Dr35MAyHQnbBaV2IPS7z8oovfI/5uS1zfiF1gPKF/H663VP9cIWlA7pLVkPgXY3c5a+gfIJegdQ5tysfRvhcT/xDJ7579rbxFMe81Rtrt9kij90+qWlw5keYryJmF7cF+aow4m7g409yhgegzXhHqzwvoz6vvyEERj0s53qkCtw31/x8oYLpWq51Wfx7Yxb6OJX9HY4P4DRiRxvMhMl1OAAAAAElFTkSuQmCC>