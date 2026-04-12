# **언리얼 블루프린트: Lerp (Linear Interpolation) 노드 완벽 가이드**

**Lerp**는 **Linear Interpolation**의 약자로, 한국어로는 \*\*'선형 보간'\*\*이라고 부릅니다. 쉽게 말해 \*\*"두 값 사이의 중간 값을 계산하는 노드"\*\*입니다.

## **1\. 노드의 기본 구조**

Lerp 노드는 보통 세 개의 입력 핀을 가집니다:

1. **A (Start)**: 시작 값 (Alpha가 0일 때의 결과값).  
2. **B (End)**: 목표 값 (Alpha가 1일 때의 결과값).  
3. **Alpha**: 보간 비율 (보통 0.0에서 1.0 사이의 값).

### **수학적 원리**

결과값 \= ![][image1]

## **2\. Alpha 값에 따른 결과**

Alpha 값은 '진행률'이라고 생각하면 이해하기 쉽습니다.

* **Alpha \= 0.0**: 결과는 **A**가 됩니다.  
* **Alpha \= 1.0**: 결과는 **B**가 됩니다.  
* **Alpha \= 0.5**: 결과는 A와 B의 정확히 **중간 값**이 됩니다.  
* **Alpha \= 0.2**: 결과는 A에 가깝고 B로 20%만큼 이동한 지점의 값이 됩니다.

## **3\. 주요 활용 사례**

### **① 부드러운 이동 (Movement)**

캐릭터나 오브젝트를 현재 위치(A)에서 목표 위치(B)로 부드럽게 이동시킬 때 사용합니다.

* Timeline 노드와 함께 사용하여 Alpha를 0에서 1로 시간에 따라 변화시키면 매끄러운 이동 애니메이션이 만들어집니다.

### **② 부드러운 회전 (Rotation)**

Lerp (Rotator) 노드를 사용하면 두 회전값 사이를 자연스럽게 연결합니다. (예: 카메라 뷰 전환, 문이 열리는 회전 등)

### **③ 색상 블렌딩 (Color/Material)**

두 가지 색상을 섞을 때 사용합니다. 데미지를 입었을 때 캐릭터의 색상을 흰색에서 빨간색으로 잠시 변하게 하는 등의 효과에 유용합니다.

### **④ UI 게이지 (Progress Bar)**

HP 바가 깎일 때 뚝 끊기지 않고 스르륵 줄어들게 하려면, 현재 표시되는 값(A)과 실제 HP 값(B) 사이를 Lerp로 연결합니다.

## **4\. 유용한 팁**

1. **다양한 타입 지원**: Lerp는 Float 뿐만 아니라 Vector, Rotator, Color, Transform 등 거의 모든 데이터 타입용 버전이 존재합니다.  
2. **Clamp Alpha**: Alpha 값이 0보다 작거나 1보다 커지는 것을 방지하려면 노드 디테일 패널에서 **'Clamp Alpha'** 옵션을 체크하는 것이 안전합니다.  
3. **Lerp vs VInterpTo**:  
   * **Lerp**: 타임라인 등으로 제어되는 '특정 진행률'에 따른 값을 원할 때 사용합니다.  
   * **InterpTo**: 매 프레임(Tick)마다 현재 값에서 목표 값을 향해 '일정한 속도'로 추적하게 하고 싶을 때 사용합니다.

## **5\. 블루프린트 예시 흐름**

1. **이벤트 발생** (예: F 키 누름)  
2. **Timeline 실행** (0초부터 1초까지 0\~1로 변하는 Float 트랙)  
3. **Update 실행** \-\> **Lerp 노드**에 타임라인 값을 Alpha로 연결  
4. **A**에는 시작 위치, **B**에는 목표 위치 입력  
5. **SetActorLocation**에 Lerp 결과값 연결

[image1]: <data:image/png;base64,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>