# **Get Character Trajectory 노드 상세 분석**

언리얼 엔진의 Get Character Trajectory 노드는 캐릭터의 \*\*과거 이동 경로(History)\*\*와 \*\*미래 예상 경로(Prediction)\*\*를 계산하여 데이터화하는 역할을 합니다. 이는 현대적인 애니메이션 기법인 '모션 매칭'을 구현하기 위한 필수적인 데이터 소스입니다.

## **1\. 핵심 역할: 궤적(Trajectory) 생성**

이 노드는 캐릭터의 현재 위치, 속도, 가속도 및 플레이어의 입력을 기반으로 캐릭터가 어떻게 움직여 왔고, 앞으로 어떻게 움직일지를 나타내는 **궤적 데이터**를 생성합니다.

* **과거 데이터 (History):** 캐릭터가 이전 프레임들에서 실제로 이동한 경로입니다.  
* **미래 데이터 (Prediction):** 현재 입력값(이동 스틱 방향 등)과 캐릭터의 물리적 상태를 기반으로 향후 수 초 내에 이동할 것으로 예상되는 경로입니다.

## **2\. 주요 출력 데이터 (Character Trajectory Structure)**

노드에서 출력되는 데이터 구조체는 일반적으로 다음과 같은 정보를 포함하는 여러 개의 '샘플 포인트'로 구성됩니다.

| 항목 | 설명 |
| :---- | :---- |
| **Position (위치)** | 특정 시점(![][image1])에서의 캐릭터 상대 위치 |
| **Rotation (회전)** | 특정 시점(![][image1])에서의 캐릭터가 바라보는 방향 |
| **Accumulated Seconds (시간)** | 현재를 기준(![][image2])으로 과거(![][image3]) 또는 미래(![][image4])의 시간 초 |

## **3\. 왜 필요한가? (모션 매칭과의 관계)**

전통적인 애니메이션 방식은 '걷기', '뛰기', '멈추기' 등의 상태(State)를 정의하고 이를 전환(Transition)하는 방식이었습니다. 하지만 **모션 매칭**은 다음과 같은 프로세스를 따릅니다.

1. **데이터베이스 탐색:** 캐릭터의 현재 이동 궤적(Trajectory)과 가장 유사한 궤적을 가진 애니메이션 조각을 애니메이션 데이터베이스에서 찾습니다.  
2. **포즈 선택:** Get Character Trajectory가 제공하는 미래 예측 선과 가장 잘 일치하는 애니메이션 포즈를 실시간으로 선택하여 재생합니다.

즉, 이 노드가 제공하는 \*\*'미래의 경로'\*\*가 있어야 시스템이 "아, 캐릭터가 곧 오른쪽으로 급커브를 틀겠구나"라는 것을 알고 그에 맞는 애니메이션(예: 오른쪽으로 몸을 기울이며 도는 동작)을 골라낼 수 있습니다.

## **4\. 주요 파라미터 (Input Pins)**

* **Character Movement Component:** 캐릭터의 물리 정보(속도, 가속도, 마찰력 등)를 참조하기 위해 연결합니다.  
* **Max History/Prediction Seconds:** 과거와 미래를 각각 몇 초까지 계산할지 결정합니다.  
* **Sample Rate:** 궤적을 얼마나 촘촘하게(몇 개의 점으로) 구성할지 결정합니다.

## **5\. 활용 이점**

1. **자연스러운 방향 전환:** 입력 급변 시 애니메이션이 끊기지 않고 궤적을 따라 부드럽게 이어집니다.  
2. **애니메이션 워핑(Warping)과의 시너지:** 계산된 궤적에 맞춰 애니메이션의 발 위치나 골반 회전을 미세하게 조정하여 발 미끄러짐(Foot Sliding) 현상을 방지합니다.  
3. **지능적인 반응:** 단순한 속도 값뿐만 아니라 '경로'를 보기 때문에 훨씬 복잡한 기동을 자연스럽게 표현할 수 있습니다.

### **요약**

Get Character Trajectory 노드는 \*\*"캐릭터가 어디서 왔고 어디로 갈 것인가"\*\*를 데이터로 만드는 노드입니다. 이를 통해 엔진은 수많은 애니메이션 데이터 중 현재 상황에 가장 적합한 동작을 실시간으로 추론하고 선택할 수 있게 됩니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAYAAAAVCAYAAABljp99AAAAoUlEQVR4XmNgGLpAXl4+CYh3S0tLC8MFFRQUOICCW0EYxIZLAFXJAAWfAHErWEBUVJQHyJGUk5MLBdK/gaojFBUVxUFGxAMFZgHxfSD+CcRLgXgSiebDANB8F6DgLxCNIgEUrALi50AJJbggkvl7xMXFuY2NjVmB7C4GKSkpESDjKsx8IB0EVFwAYjMCGY1AgTtAeiWIDdKFbKQACMMFcAEA+2MqgcEcd2MAAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAgAAAAVCAYAAAB7R6/OAAAAyklEQVR4XmNgGFZARkaGU15ePgqIZwFxl6KiojpcUklJiR8ouBuIm0VFRXkUFBQMgOxrQBwMViAnJ1cO5JwG0oIwTUB+NBBfB0kKgiSBuhbCjQQCWVlZU6D4F8IKgIQmEL9FVwDUaAwU/wpn4FPgC2T8x6kASHjiVUCMFUpAxnOcCkAhB2QcAOKtQEUcSApcgGK/YJwYIH4EFFCEyjMC2c1AfALMMzY2ZgUqmA4U2A80JQAqeRUUJ1ANEF3ASFMDKgwBxqQdSBNIEAB3CEQxM7lC8AAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAVCAYAAACkCdXRAAAAzklEQVR4XmNgGAWjgKaAUV5evhVdkCwgJSUlAjTsKro4WUBWVtYUaNg3dHGSgIKCggDQEEkgLgXiT+jyRAMtLS02oAFFcnJy84H0axBGV0MygIUXEE8CCxgbG7MqKiqKQ52MFysrK4sBtTDDDIOFFxBHgwWAfjcAcmYRg4FqJwKxAswwIDMdKP4WiDVhYuQCUPpaCsSngWEniC5JEkAKrznociQDkNegXgSFFyO6PElARkaGE2jQIiA+A8Rr0OXJAYzS0tLCoqKiPOgSFAEAjGwxPCvXN2MAAAAASUVORK5CYII=>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAVCAYAAACkCdXRAAABBElEQVR4XmNgGAWjgKaAUV5evhVdEA4UFRXFgQpmiYuLc6PLoQMpKSkRoNqr6OJwAJSUVFBQWCgqKsqDLocOZGVlTYHqv6GLwwExhgHlBUDqgLgUiD+hy8MBIcO0tLTYgGqK5OTk5gPp1yCMrgYOCBkGA7DwAuJJYAEk58IxMAL0gfRqGRkZFXQ5ZAtg4QXE0Qyg2AIyqoF4FhpeCsT3od5Al0uGGQZ0SDqQ/xaINWFiGECeOG+C0hfI0tNASwXRJeGAGMOQwmsOuhwKIMYwkNegXowGchnR5eGAGMOAkcMJVLcIiM8A8Rp0eTggxjAoYJSWlhbGqw4kCQzUUCCTBV0OFwAAUdVFutxBe9cAAAAASUVORK5CYII=>