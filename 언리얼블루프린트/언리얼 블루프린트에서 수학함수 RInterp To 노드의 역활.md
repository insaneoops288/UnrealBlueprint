# **Unreal Engine: RInterp To 노드 이해하기**

RInterp To (Rotation Interpolate To) 노드는 현재 회전값(Current)에서 목표 회전값(Target)으로 **프레임 속도에 독립적으로(Frame-rate Independent)** 부드럽게 보간해주는 함수입니다.

## **1\. 주요 입력 항목 (Inputs)**

| 입력 이름 | 타입 | 설명 |
| :---- | :---- | :---- |
| **Current** | Rotator | 현재 오브젝트의 회전 상태입니다. 보통 Get Actor Rotation 등을 연결합니다. |
| **Target** | Rotator | 도달하고자 하는 최종 목표 회전값입니다. |
| **Delta Time** | Float | 지난 프레임 이후 경과된 시간입니다. 보통 Get World Delta Seconds를 연결합니다. |
| **Interp Speed** | Float | 보간 속도입니다. 값이 클수록 목표값에 빠르게 도달합니다. |

## **2\. 핵심 특징: 최단 경로 (Shortest Path)**

RInterp To의 가장 큰 장점은 **최단 경로 자동 계산**입니다.

* 예를 들어, 현재 각도가 350도이고 목표가 10도라면, 일반적인 산술 계산은 역방향으로 340도를 돌려고 할 수 있습니다.  
* 하지만 RInterp To는 내부적으로 쿼터니언(Quaternion) 로직을 사용하여 350 \-\> 0 \-\> 10 방향으로 단 20도만 회전하도록 계산합니다.

## **3\. 작동 원리 (수식적 이해)**

이 노드는 매 프레임마다 호출되어야 합니다. (예: Event Tick)

작동 방식은 다음과 유사합니다:

새로운 회전값 \= 현재값 \+ (목표값 \- 현재값) \* 가속도(Speed) \* 시간(DeltaTime)

결과적으로 목표값에 가까워질수록 회전 속도가 서서히 줄어드는 **지수적 감쇠(Exponential Decay)** 효과가 있어 매우 부드러운 연출이 가능합니다.

## **4\. RInterp To vs RInterp To Constant**

| 노드명 | 특징 | 용도 |
| :---- | :---- | :---- |
| **RInterp To** | 목표에 가까울수록 느려짐 (부드러움) | 캐릭터가 적을 바라볼 때, 카메라 부드러운 회전 |
| **RInterp To Constant** | 일정한 속도로 회전함 | 기계적인 회전, 문이 일정한 속도로 열릴 때 |

## **5\. 실전 사용 예시 (Blueprint Logic)**

1. **이벤트 연결**: Event Tick에 노드를 실행 핀으로 연결합니다.  
2. **현재값 가져오기**: Get Actor Rotation을 Current에 연결합니다.  
3. **목표값 설정**: 캐릭터가 바라볼 타겟 위치를 Find Look at Rotation으로 계산하여 Target에 연결합니다.  
4. **델타 타임**: Get World Delta Seconds를 연결합니다.  
5. **적용**: 노드의 출력값(Return Value)을 Set Actor Rotation에 연결합니다.

## **6\. 주의사항**

* **반복 실행 필수**: 이 노드는 한 번 실행해서 목표값으로 순간이동 시키는 노드가 아닙니다. 반드시 Tick이나 Timeline처럼 매 프레임 반복 호출되는 흐름 속에 있어야 합니다.  
* **Interp Speed 값**: 보통 1.0에서 10.0 사이의 값을 사용하며, 값이 0이면 전혀 회전하지 않습니다.