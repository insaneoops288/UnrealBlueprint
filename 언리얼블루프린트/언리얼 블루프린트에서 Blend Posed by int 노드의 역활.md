# **Blend Poses by int 노드 분석**

언리얼 엔진의 애니메이션 시스템에서 Blend Poses by int 노드는 여러 개의 애니메이션 데이터 흐름 중 하나를 조건에 따라 선택하는 **스위치(Switch)** 역할을 합니다.

## **1\. 핵심 역할 (Core Role)**

이 노드의 가장 큰 역할은 \*\*정수형 변수(Active Child Index)\*\*를 기준으로 입력된 여러 포즈 중 하나를 골라 결과값으로 내보내는 것입니다.

* **상태 선택**: 예를 들어 '무기 타입'이 0일 때는 '맨손', 1일 때는 '검', 2일 때는 '총' 포즈를 출력하도록 설정할 수 있습니다.  
* **부드러운 전환**: 단순히 포즈를 '탁' 하고 바꾸는 것이 아니라, 설정된 **Blend Time**을 통해 이전 포즈에서 다음 포즈로 자연스럽게 섞이면서 전환됩니다.

## **2\. 주요 설정 및 핀 (Properties & Pins)**

### **입력 핀**

* **Active Child Index**: 현재 어떤 포즈를 사용할지 결정하는 정수형 입력입니다.  
* **Pose 0, 1, 2...**: 실제 애니메이션 소스(시퀀스, 블렌드 스페이스 등)를 연결하는 핀입니다. 노드를 우클릭하여 \*\*'Add Blend Pin'\*\*을 선택하면 입력 핀을 무한히 늘릴 수 있습니다.

### **디테일 패널 설정**

* **Blend Time**: 포즈가 전환될 때 걸리는 시간(초)입니다. 값이 ![][image1]라면 새로운 인덱스로 변경될 때 0.2초 동안 서서히 블렌딩됩니다.  
* **Transition Type**: 전환 시 사용할 보간 방식을 결정합니다 (Linear, Cubic 등).  
* **Reset Child on Activation**: 해당 포즈가 선택될 때 애니메이션의 재생 시간을 처음으로 되돌릴지 여부를 결정합니다.

## **3\. 사용 예시 (Use Cases)**

이 노드는 다음과 같은 상황에서 주로 사용됩니다:

1. **무기 시스템**:  
   * 0: 맨손 (Unarmed)  
   * 1: 한손검 (One-Handed)  
   * 2: 양손검 (Two-Handed)  
   * 3: 활 (Bow)  
2. **캐릭터 상태 이상**:  
   * 0: 정상 상태  
   * 1: 부상당함 (절뚝거림)  
   * 2: 중독됨 (비틀거림)  
3. **특수 포즈**:  
   * 특정 상호작용(예: 버튼 누르기, 레버 당기기) 시 정수 값만 변경하여 애니메이션을 교체할 때 사용합니다.

## **4\. 'State Machine' vs 'Blend Poses by int'**

| 특징 | State Machine (상태 머신) | Blend Poses by int |
| :---- | :---- | :---- |
| **복잡도** | 복잡하고 논리적인 상태 전이에 적합 | 단순하고 수치적인 전환에 적합 |
| **비주얼** | 상태와 전이 조건이 그래프로 보임 | 단일 노드 내부에서 깔끔하게 정리됨 |
| **성능** | 상대적으로 무거울 수 있음 | 매우 가볍고 빠름 |
| **추천 용도** | 달리기-점프-추락 같은 로코모션 | 무기 교체나 단순 포즈 스위칭 |

## **5\. 요약 및 팁**

* **핀 추가**: 노드를 우클릭하여 원하는 만큼 입력 핀을 추가할 수 있어 확장성이 좋습니다.  
* **최적화**: 복잡한 상태 머신을 만들지 않고도 여러 포즈를 빠르게 전환할 수 있어 애니메이션 블루프린트를 최적화하는 데 도움이 됩니다.  
* **주의 사항**: Active Child Index가 입력 핀의 개수를 초과하면 기본값(보통 0번 포즈)이 출력되거나 포즈가 멈출 수 있으므로 값 범위를 잘 관리해야 합니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABkAAAAXCAYAAAD+4+QTAAAB0ElEQVR4Xu2UzytEURTH3yRFKUmTzK838xJZmZosLFjNZhYklML/YKEoKzVsrOyUjSwsZ8nGgrJRFrJgipQksrBjYUrjc173vu67zUsJq/nWt/fO937POfede2ccp4km/hypVKrddd05uAM3c7ncgO2JgnglR+UuwV7b43ie18nCESzH4/GObDab5/0aTtteG+KBFRoNpdPpUd4vYA1OhYyZTGYF8Zxnl9aI52GV5B7Ta0LW8JySVySMicZE+tCe4G0ikUj7RiksDdj9nlmAXQ2jv/GcMHUT5BbwvMM7YzMx4n1YhyVf4WUQvtpNjAIbpm5CjbmCd1fGrHWpJU3Qx31BF4tqYuvfQU8GvgSXR7pJV7vYT5vgnyXvk/xlR52TjKv0W03YOSlulZy1QqHQGixEFYvSo6DO54C8RcKW0CKix+KzXcw4+FVTbwTdQEblqBHxVWPEed8gtwLDiTK16USaFNFq8tSaFONKJxw9ayBjwbPlWj8+4nX5GQQCpgX4wEJOSXLXy/BMCouQTCa7iS/hBxwRTRrI/EWTfE3iR3gvUwqaqN1ss3BM0qRqcBV8rhN88SG+Gzlg0YyR1hsw9A+iEWPX/SzMyDxDt6OJ/8QXZASRfspQ9AYAAAAASUVORK5CYII=>