# **CInterp To 노드 상세 가이드**

언리얼 엔진 블루프린트에서 CInterp To (Color Interpolate To) 노드는 두 색상 사이를 부드럽게 전환하는 데 사용됩니다. 주로 조명 색상 변경, UI 요소의 색상 애니메이션, 캐릭터의 상태 변화에 따른 색상 변경 등에 활용됩니다.

## **1\. 노드 구조 및 입력 핀(Input Pins)**

이 노드는 매 프레임 실행되는 Tick 이벤트나 Timeline과 함께 사용되어야 효과가 나타납니다.

* **Current (Linear Color):** 현재의 색상 값입니다. 보통 현재 변수에 저장된 색상을 연결합니다.  
* **Target (Linear Color):** 도달하고자 하는 목표 색상입니다.  
* **Delta Time (Float):** 이전 프레임에서 현재 프레임까지 걸린 시간입니다. 보통 Event Tick의 Delta Seconds를 연결합니다.  
* **Interp Speed (Float):** 보간 속도입니다. 값이 높을수록 목표 색상에 더 빠르게 도달합니다.

## **2\. 주요 역할과 특징**

### **① 자연스러운 색상 전환**

단순히 색상을 교체하는 것이 아니라, 프레임마다 Current에서 Target으로 조금씩 이동한 값을 반환합니다. 이를 통해 끊김 없는 부드러운 애니메이션을 구현할 수 있습니다.

### **② 감속(Easing) 효과**

CInterp To는 기본적으로 목표값에 가까워질수록 속도가 느려지는 특성을 가집니다. 즉, 시작은 빠르고 끝은 부드럽게 멈추는 자연스러운 연출이 가능합니다.

## **3\. Lerp(Linear Interpolate)와의 차이점**

가장 많이 혼동하는 노드가 Lerp입니다.

| 특징 | Lerp (Linear Interpolate) | CInterp To |
| :---- | :---- | :---- |
| **제어 방식** | Alpha 값 (0\~1)에 따라 위치 결정 | 속도(Speed)와 시간(Delta Time)에 따라 결정 |
| **주요 사용처** | 타임라인(Timeline)과 함께 사용 | 이벤트 틱(Event Tick)과 함께 사용 |
| **로직** | 시작점과 끝점이 고정된 상태에서 진행률로 계산 | 현재값에서 목표값을 향해 매 순간 추적하며 이동 |

## **4\. 실전 사용 예시 (조명 색상 바꾸기)**

1. **변수 생성:** CurrentLightColor라는 이름의 Linear Color 변수를 만듭니다.  
2. **이벤트 구성:** Event Tick 노드를 배치합니다.  
3. **CInterp To 연결:**  
   * Current 핀에 CurrentLightColor 변수를 연결합니다.  
   * Target 핀에 원하는 목표 색상(예: 빨간색)을 설정합니다.  
   * Delta Time에 Event Tick의 Delta Seconds를 연결합니다.  
   * Interp Speed에 2.0 정도의 값을 입력합니다.  
4. **결과 적용:** 노드의 출력(Return Value)을 다시 CurrentLightColor 변수에 세팅(Set)하고, 이 값을 조명의 Set Light Color 노드에 전달합니다.

## **5\. 관련 노드: CInterp To Constant**

만약 "가까워질수록 느려지는 효과" 없이 **일정한 속도**로 색상을 바꾸고 싶다면 CInterp To Constant 노드를 사용하세요. 이 노드는 시작부터 끝까지 설정된 속도 그대로 선형적으로 움직입니다.

## **요약**

* **CInterp To**는 **색상 전용 보간 노드**입니다.  
* **Tick** 이벤트에서 사용하며, 목표 색상을 향해 **부드럽게(가속/감속 포함)** 변합니다.  
* 조명, 머티리얼 파라미터, UI 색상 변경 등 **동적인 색상 연출**에 필수적인 노드입니다.