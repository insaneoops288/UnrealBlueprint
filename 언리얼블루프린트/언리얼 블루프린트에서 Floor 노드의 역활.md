# **언리얼 블루프린트: Floor 노드 완벽 이해**

Floor 노드는 수학 범주(Math \> Float)에 속하며, 실수(Float)를 정수(Integer)로 변환할 때 가장 자주 사용되는 노드 중 하나입니다.

## **1\. 핵심 기능**

* **정의**: 입력값보다 작거나 같은 정수 중 가장 큰 값을 반환합니다.  
* **수학적 표현**: ![][image1]  
* **입력**: Float (실수)  
* **출력**: Integer (정수)

## **2\. 작동 예시**

소수점의 크기와 상관없이 결과는 항상 '내림' 처리됩니다.

| 입력값 (Float) | 출력값 (Integer) | 비고 |
| :---- | :---- | :---- |
| **4.0** | **4** | 정수는 그대로 유지 |
| **4.2** | **4** | 0.2를 내림 |
| **4.9** | **4** | 0.9가 높지만 반올림하지 않고 내림 |
| **\-4.2** | **\-5** | **주의\!** \-4보다 작은 정수는 \-5이므로 \-5가 됨 |
| **\-4.9** | **\-5** | 음수에서도 바닥 방향으로 이동 |

## **3\. 주요 활용 사례**

### **① 그리드 스냅 (Grid Snapping)**

캐릭터의 위치나 아이템 배치 시, 특정 단위(예: 100단위 격자)에 딱 맞게 정렬하고 싶을 때 사용합니다.

* **공식**: Floor(현재위치 / 그리드크기) \* 그리드크기  
* 이렇게 하면 캐릭터가 145 위치에 있어도 100 위치로 고정됩니다.

### **② 경험치 및 레벨 계산**

플레이어의 경험치가 소수점으로 계산되더라도, UI에 표시하거나 실제 레벨업 로직을 처리할 때는 '완성된 정수'만 필요할 때 사용합니다. 99.9% 경험치는 아직 100이 아니므로 Floor를 통해 99로 취급합니다.

### **③ 타일맵/인벤토리 인덱스 계산**

마우스의 월드 좌표를 인벤토리의 칸(Index) 번호나 타일맵의 좌표로 변환할 때 사용합니다. 화면의 좌표를 칸의 크기로 나눈 뒤 Floor를 적용하면 해당 마우스가 위치한 '몇 번째 칸'인지 정확히 알 수 있습니다.

### **④ 시간 표시 (타이머)**

남은 시간(초)이 59.7초일 때, 플레이어에게는 "59초"라고 표시하고 싶다면 Floor 노드를 사용하여 소수점을 제거합니다.

## **4\. 유사 노드와 비교**

* **Floor**: 무조건 내림 (4.9 \-\> 4 / \-4.2 \-\> \-5)  
* **Ceil**: 무조건 올림 (4.1 \-\> 5 / \-4.8 \-\> \-4)  
* **Round**: 반올림 (4.4 \-\> 4 / 4.5 \-\> 5\)  
* **Truncate (절삭)**: 소수점 이하를 단순히 버림 (4.9 \-\> 4 / \-4.9 \-\> \-4) *음수 처리 시 Floor와 다름*

## **5\. 블루프린트 팁**

* **정수 출력**: 언리얼의 Floor 노드는 기본적으로 Integer 타입을 출력하므로, 바로 정수 변수나 배열 인덱스에 연결할 수 있어 편리합니다.  
* **음수 주의**: 음수 계산이 포함된 로직(예: 월드 좌표계의 서쪽/남쪽)에서는 Truncate와 Floor의 결과가 다르므로 의도에 맞게 선택해야 합니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAZCAYAAAAiwE4nAAABRUlEQVR4Xu2UvUoDQRSFE4hgUFA06+L+/1R2whY2KWwF9QV8AHsl+BRiJxaCKAgW2ifY+BIpbVKk1MpKRL+rG7kMEnAiQmAPHO7M3DnnMLPL1GoVKkwVoig6gjvmug3iOF5NkuTScZx5s/eNKvA3mL7AMAxzehdwAE+CIFjC9BA+wtMsyxb0/okCEa+xfuP7PjlBk3GXtSHsMN6mvsA9Q2Md2GDtDBajOWa3sO95Xot6Dt8w39Qi60A5EcINhg2ZlyF9uV6mdXqzcFFrBNaBJui3MXvFbN/safxZIEYH8s3UFf8I60D5+xDfwWO5Xv39VP86TVNX66wDEW7Bd3iFwTr1GT6URnXGHTm11pQ6u0BBURQzeZ6vSB3N5UTjzCYKtEEV+An5+PAp+novB7yfu+aeceDpW0Z/L1rqkNpzXXfO3Pev+AB6fXQ6Ilz2XgAAAABJRU5ErkJggg==>