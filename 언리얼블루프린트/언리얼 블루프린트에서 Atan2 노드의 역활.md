# **언리얼 엔진 블루프린트: Atan2 노드의 역할**

언리얼 엔진 블루프린트에서 **Atan2 (Arctangent 2, 아크탄젠트 2\)** 노드는 주로 **2D 좌표계에서 방향(각도)을 계산**할 때 사용되는 매우 중요한 수학 노드입니다. 주어진 X와 Y 좌표값을 바탕으로 원점(0, 0)에서 해당 좌표(X, Y)로 향하는 선이 X축과 이루는 각도를 반환합니다.

## **1\. Atan2 노드의 핵심 역할**

Atan2 노드의 가장 큰 존재 이유는 \*\*두 점 사이의 상대적인 각도(방향)\*\*를 정확하게 알아내는 것입니다.

* **입력(Input):** Y 값과 X 값 (주의: 수학적 관례상 블루프린트 노드에서 Y가 위, X가 아래에 있는 경우가 많습니다.)  
* **출력(Output):** 입력된 (X, Y) 좌표가 향하는 각도. (라디안 값으로 반환하는 기본 Atan2와, 도(Degree) 값으로 반환하는 Atan2 (Degrees)가 있습니다.)

## **2\. 왜 그냥 Atan이 아니라 Atan2를 쓸까요?**

일반적인 수학의 Atan(아크탄젠트)는 ![][image1] 값을 입력으로 받습니다. 하지만 게임 개발에서는 일반 Atan 대신 Atan2를 압도적으로 많이 사용합니다. 그 이유는 다음과 같습니다.

* **0으로 나누기 오류 방지:** 일반 Atan은 X가 0일 때 분모가 0이 되어 수학적 오류(Division by Zero)가 발생하지만, Atan2는 엔진 내부에서 이를 안전하게 처리하여 X가 0일 때도 90도 또는 \-90도로 정확히 계산합니다.  
* **사분면(Quadrant)의 정확한 구분:** 일반 Atan은 1사분면과 3사분면, 2사분면과 4사분면의 각도를 구분하지 못합니다(예: \-1/-1 과 1/1 은 같은 값을 냄). 하지만 Atan2는 부호를 개별적으로 입력받기 때문에 **360도(-180도 \~ 180도) 전 방향의 각도를 정확하게 판별**해 냅니다.

## **3\. 실무(게임 개발)에서의 주요 활용 사례**

### **① 캐릭터나 포탑이 특정 대상을 바라보게 할 때 (Look At)**

플레이어 위치에서 적의 위치를 뺀 방향 벡터 (Delta X, Delta Y)를 구한 뒤, 이 값을 Atan2에 넣으면 적을 향하는 각도를 구할 수 있습니다. 이 각도를 캐릭터의 **Yaw(Z축 회전)** 값으로 설정하면 대상 쪽으로 자연스럽게 회전합니다.

* Y 입력 \= TargetLocation.Y \- MyLocation.Y  
* X 입력 \= TargetLocation.X \- MyLocation.X

### **② 조이스틱(게임패드) 입력 방향 계산**

게임패드의 아날로그 스틱은 좌우(X축)와 상하(Y축)의 입력값(-1.0 \~ 1.0)을 제공합니다. 이 두 값을 Atan2 노드에 연결하면, 현재 플레이어가 스틱을 어느 각도로 밀고 있는지 정확히 계산하여 캐릭터를 해당 방향으로 걷게 만들 수 있습니다.

### **③ 마우스 커서 방향으로 투사체(총알) 발사**

탑다운 슈팅 게임 등에서 내 캐릭터의 위치와 화면상 마우스 커서의 위치 간의 X, Y 차이를 Atan2로 계산하면, 총알이 날아가야 할 궤적(각도)을 쉽게 얻을 수 있습니다.

## **4\. 요약**

* **역할:** X, Y 좌표(길이)를 입력받아 \*\*방향(각도)\*\*을 알려주는 만능 나침반.  
* **장점:** 360도 전 방향을 완벽하게 계산하며 계산 오류가 없음.  
* **팁:** 언리얼 엔진에서는 결과를 각도로 바로 쓰기 편한 **Atan2 (Degrees)** 노드를 사용하는 것이 직관적입니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABEAAAAYCAYAAAAcYhYyAAABN0lEQVR4XmNgGJRAQUFBQE5OrkNeXn4WCIPYIDFpaWlhIN0JEgPSE4FYAV0vCjA2NmYFqpkJ1HAHqFkGJg4UqwRhJSUlfmT1OAHQBS5AQ34BNXmA+EC2E1AsFchkRFOKGwA1CAI1ngbiKUAuI8grIBeiqyMIgAZYAvE3IL6ALkc0ALpGCWjAQ5iXyAJAAzyB+BnIMHQ5ogHQgFYg3gp0CQe6HEEA1KgJxHOB+CMQPwelFXQ1IwUA/X+FVIxuBihvOJCK0c2gPpCHRPM0eWiRABRiAUZ1HowPxNkgMXR9WAFQcQ4QP1FUVJQHGjKd6GIAGUBd9BaIm4BhIIEuTyxgARqwXEZGRhVdgmgAKkOAhswBYkt0OWIBM9ALGcCwcJWHFk7oCggBUGlWANQcBOKAAheIFdHUwAEAu11TzfUlsXsAAAAASUVORK5CYII=>