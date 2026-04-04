# **Unreal Engine FInterp To 노드 완벽 가이드**

FInterp To는 **Float Interpolation To**의 약자로, 현재 값(Current)에서 목표 값(Target)으로 시간에 따라 부드럽게 수치를 변화시키는 노드입니다.

## **1\. 주요 입력 핀 (Inputs)**

| 이름 | 타입 | 설명 |
| :---- | :---- | :---- |
| **Current** | Float | 현재 프레임에서의 수치입니다. |
| **Target** | Float | 도달하고자 하는 최종 목표 수치입니다. |
| **Delta Time** | Float | 프레임 간의 시간 간격입니다. 보통 Get World Delta Seconds 노드를 연결합니다. |
| **Interp Speed** | Float | 보간 속도입니다. 값이 클수록 목표치에 빠르게 도달하며, 0이면 변화하지 않습니다. |

## **2\. 동작 원리**

FInterp To는 단순히 선형적으로 움직이는 Lerp와는 다릅니다. 이 노드는 **지수적 감쇠(Exponential Decay)** 방식을 사용합니다.

* **특징**: 목표 값에 가까워질수록 변화 속도가 느려집니다.  
* **부드러움**: 갑작스러운 수치 변화에도 튀지 않고 아주 매끄러운 곡선을 그리며 수치가 변합니다.  
* **프레임 독립성**: Delta Time을 사용하기 때문에 컴퓨터 사양(프레임 드랍 등)에 상관없이 일정한 시간 동안 일정한 속도로 움직입니다.

## **3\. 수식 (근사치)**

내부적으로는 대략 다음과 같은 로직으로 계산됩니다:

![][image1]*(실제 엔진 코드는 프레임에 더 안정적인 지수 함수를 사용합니다.)*

## **4\. 실전 활용 사례**

### **A. 카메라 FOV 변경**

저격 총의 줌(Zoom) 기능을 구현할 때, 버튼을 누르는 순간 FOV가 바로 바뀌면 어지러울 수 있습니다. 이때 FInterp To를 사용하면 휙 바뀌는 것이 아니라 슥\~ 하고 빨려 들어가는 연출이 가능합니다.

### **B. 사운드 볼륨 조절**

특정 구역에 들어갔을 때 배경음악이 서서히 커지거나 작아지게(Fade in/out) 할 때 유용합니다.

### **C. 동적 UI 게이지**

HP 바가 깎일 때 뚝뚝 끊기지 않고 부드럽게 줄어들게 하여 타격감을 높일 수 있습니다.

## **5\. Lerp(Linear Interpolate) 노드와의 차이점**

| 구분 | Lerp (선형 보간) | FInterp To (지수 보간) |
| :---- | :---- | :---- |
| **제어 방식** | 0에서 1 사이의 '알파(Alpha)' 값으로 제어 | '속도(Speed)'와 '시간(Delta Time)'으로 제어 |
| **용도** | 타임라인이나 일정한 비율로 움직일 때 | 매 프레임 업데이트(Tick)에서 부드럽게 추적할 때 |
| **움직임** | 처음부터 끝까지 일정한 속도 | 처음엔 빠르고 목표에 가까울수록 느려짐 |

## **6\. 주의사항**

1. **Tick 연결**: 이 노드는 지속적으로 값을 갱신해야 하므로 보통 Event Tick이나 Timer 이벤트에 연결해서 사용해야 합니다.  
2. **Current 값 갱신**: Current 핀에는 반드시 **현재 변수에 저장된 값**을 연결하고, 노드의 결과값(Return Value)을 **다시 그 변수에 저장(Set)** 해야 합니다. (그렇지 않으면 값이 업데이트되지 않고 제자리에 머뭅니다.)

### **관련 노드 가족들**

* **VInterp To**: 벡터(Vector) 값을 부드럽게 변경 (위치 이동)  
* **RInterp To**: 로테이터(Rotator) 값을 부드럽게 변경 (회전)  
* **CInterp To**: 컬러(Linear Color) 값을 부드럽게 변경 (색상 변화)

[image1]: <data:image/png;base64,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>