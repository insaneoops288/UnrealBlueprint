# **언리얼 엔진: Get Accumulated Time 노드 및 시간 시스템 이해**

언리얼 엔진에서 시간은 다양한 방식으로 측정됩니다. Get Accumulated Time은 주로 특정 시스템(예: 오디오, 미디어, 머티리얼)이나 월드 엔진 레벨에서 **누적된 시간**을 추적할 때 사용됩니다.

## **1\. Get Accumulated Time의 주요 역할**

이 노드(또는 함수)의 핵심은 \*\*"중단되지 않고 계속해서 쌓이는 시간 값"\*\*을 제공하는 것입니다.

* **연속성:** 게임이 시작된 시점부터 현재까지 흐른 총 시간을 초(Seconds) 단위로 반환합니다.  
* **머티리얼(Material)에서의 활용:** 머티리얼 에디터의 Time 노드가 대표적인 누적 시간 노드입니다. UV를 지속적으로 스크롤하거나, 물결 효과(Sine wave)를 줄 때 시간에 따라 변하는 값을 만들기 위해 사용됩니다.  
* **시스템 동기화:** 오디오 재생이나 미디어 프레임 계산 시, 절대적인 시간 기준점이 필요할 때 사용됩니다.

## **2\. 블루프린트의 주요 시간 노드 비교**

블루프린트에는 상황에 따라 선택해야 할 여러 가지 시간 노드가 있습니다.

| 노드 이름 | 특징 | 일시정지(Pause) 영향 | 시간 배율(Dilation) 영향 |
| :---- | :---- | :---- | :---- |
| **Get Game Time in Seconds** | 실제 게임 플레이가 진행된 시간. | **예** | **예** |
| **Get Real Time in Seconds** | 실제 현실의 시계 시간. (디버깅 등에 유용) | **아니오** | **아니오** |
| **Get World Delta Seconds** | 지난 프레임과 현재 프레임 사이의 시간 간격. | **예** | **예** |
| **Get Audio Time** | 오디오 믹서 엔진 기준 누적 시간. | **아니오** | **아니오** |

## **3\. 주요 활용 사례**

### **① 머티리얼 애니메이션 (Material Graph)**

머티리얼 내부에서 Time 노드는 Accumulated Time과 같습니다.

* **예시:** Time → Sine → Base Color를 연결하면 시간에 따라 색상이 깜빡이는 효과를 낼 수 있습니다.

### **② 일정 주기마다 반복되는 로직**

특정 로직을 5초마다 실행하고 싶을 때, 누적 시간을 나머지 연산(![][image1] Operator)과 함께 사용합니다.

* **Logic:** (Accumulated Time % 5.0) \< DeltaSeconds  
* 이 방식은 Delay 노드 없이도 시간에 기반한 주기적 체크를 가능하게 합니다.

### **③ 타임스탬프 기록**

게임 세션 중 특정 이벤트가 발생한 시점을 기록하고, 나중에 그 시점으로부터 얼마나 시간이 지났는지 계산할 때 기준점으로 사용합니다.

## **4\. 주의사항**

1. **정밀도 문제 (Floating Point Precision):**  
   게임이 매우 오랫동안(예: 수십 일) 켜져 있는 경우, 누적 시간 값이 너무 커져서 소수점 아래 정밀도가 떨어질 수 있습니다. 이로 인해 미세한 애니메이션이 끊겨 보일 수 있으나, 일반적인 게임 세션에서는 거의 발생하지 않습니다.  
2. **Pause(일시정지) 대응:**  
   인벤토리 창을 열어 게임을 멈췄을 때도 시간이 흘러야 하는지(예: UI 애니메이션), 아니면 멈춰야 하는지(예: 캐릭터 스킬 쿨타임)에 따라 Game Time과 Real Time 중 적절한 것을 선택해야 합니다.

## **요약**

**Get Accumulated Time**은 "시작 버튼을 누른 후 지금까지 몇 초가 지났는가?"에 대한 답을 주는 노드입니다. 주로 **변하지 않는 시간 흐름의 기준**이 필요할 때 사용합니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAXCAYAAAAC9s/ZAAABnElEQVR4Xu2TP0vDUBTFE6yDqDhILW2Spk2U4uRQHFxEwYKLDk6i4FcQHRS1IAgO/gNxEnF1EcXVwUkHERd3p45+C3+nJvYl6RcQPHC49917z3335b1Y1j9M9IRhOJLP5wfSCROFQqG/Xq/3JoJBEAz5vv8ATyqVym25XF4jbCeKgOu6o6pzHMdNJBBsk7jDzZEcxn+l0TOcxy8iHMPuwi/P8xYTYo1E4kkFcQzhTrVancbOEF/FLrPJOv4F6ZwhtyztAFuaIo7JhwvxWt9GR8uMLtRqtUEavKQabMG5aGkjPsqMboIGZ/AG1+a8fYgvsY5y+A14bqVHN8F50fvvcE/F2E3FNTL+fdfR04h2btBswvq5wpyaxaMrT7MVeEwsTKq7QMJ4dD0chFes97Hj8FpTpzW/SI+OPwU/aRBorfcBN5KqDhKjC/omsAWLWkcbHHYkBszR45iu2GwgC09xe+KaNtRZD0YPx4xTvGQ2oGEd/8CsEdqj6ybSiVKp5CH4gLMsbWwzUxe9+6YKEokIHG0S0Rt81Pkzv/TfxTcjBVvVTglwqwAAAABJRU5ErkJggg==>