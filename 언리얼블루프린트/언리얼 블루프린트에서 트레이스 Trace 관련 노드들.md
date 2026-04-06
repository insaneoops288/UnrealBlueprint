# **언리얼 엔진 블루프린트: Trace 노드 완전 정복**

언리얼 엔진에서 \*\*트레이스(Trace)\*\*는 흔히 '레이캐스트(Raycast)'라고 불리는 기술로, 가상의 선이나 도형을 쏘아 물체와의 충돌 여부를 판단하는 데 사용됩니다.

## **1\. 트레이스의 주요 분류**

트레이스 노드는 크게 세 가지 기준(모양, 대상 필터링, 결과 개수)에 따라 나뉩니다.

### **① 모양에 따른 분류 (Shape)**

* **Line Trace**: 가장 기본적인 직선 형태의 추적입니다.  
* **Sphere Trace**: 구체(Sphere)를 굴려 추적합니다. 캐릭터의 발밑 체크 등에 유용합니다.  
* **Box Trace**: 상자(Box) 모양으로 추적합니다.  
* **Capsule Trace**: 캡슐 모양으로 추적하며, 주로 캐릭터(Pawn)의 충돌 범위와 유사한 체크를 할 때 사용합니다.

### **② 필터링 방식에 따른 분류 (Filtering)**

* **By Channel**: 콜리전 설정의 '채널(Visibility, Camera 등)'을 기준으로 검출합니다.  
* **By Object**: 특정 '오브젝트 타입(WorldStatic, PhysicsBody, Pawn 등)'을 기준으로 검출합니다.

### **③ 결과 개수에 따른 분류 (Multiplicity)**

* **Single Trace**: 가장 먼저 부딪힌 대상 **하나**만 반환합니다.  
* **Multi Trace**: 경로 상에 있는 **모든** 대상(또는 특정 조건에 맞는 모든 대상)을 배열 형태로 반환합니다.

## **2\. 핵심 트레이스 노드 리스트**

| 노드 이름 | 설명 |
| :---- | :---- |
| **LineTraceByChannel** | 특정 채널을 기준으로 선을 쏘아 첫 번째 충돌체를 찾습니다. (가장 많이 사용) |
| **MultiLineTraceByChannel** | 선 경로에 걸리는 모든 충돌체를 배열로 반환합니다. (관통 총알 등에 사용) |
| **SphereTraceByObject** | 특정 오브젝트 타입들을 대상으로 구체 형태의 범위를 체크합니다. |
| **BoxTraceByChannel** | 박스 형태의 영역 내에 특정 채널의 물체가 있는지 확인합니다. |

## **3\. 노드의 주요 입력 핀 (Inputs)**

* **Start**: 트레이스가 시작되는 월드 좌표 (![][image1]).  
* **End**: 트레이스가 끝나는 월드 좌표 (![][image1]).  
* **Trace Channel**: 검사할 콜리전 채널 (기본값: Visibility).  
* **Actors to Ignore**: 트레이스에서 제외할 액터들의 배열 (자기 자신 등을 넣을 때 사용).  
* **Draw Debug Type**: 트레이스 경로를 시각적으로 보여줍니다. (개발 단계에서 필수)  
  * *None*: 표시 안 함.  
  * *For One Frame*: 매 프레임 선을 그림.  
  * *For Duration*: 지정된 시간 동안 유지.  
  * *Persistent*: 영구적으로 남김.

## **4\. 트레이스 결과 처리 (Hit Result)**

트레이스가 성공하면 Out Hit 핀을 통해 **Hit Result** 구조체를 반환합니다. 이를 **Break Hit Result** 노드로 분해하면 다음 정보를 얻을 수 있습니다.

* **Blocking Hit**: 무언가에 부딪혔는지 여부 (![][image2]).  
* **Hit Actor**: 부딪힌 액터가 누구인지.  
* **Hit Component**: 액터의 어느 컴포넌트에 부딪혔는지.  
* **Location**: 실제 충돌이 일어난 월드 위치 좌표.  
* **Impact Normal**: 부딪힌 표면의 법선 벡터 (반사각 계산이나 이펙트 배치에 유용).  
* **Phys Mat**: 부딪힌 표면의 물리 재질 (발소리 구분 등에 활용).

## **5\. 실전 활용 예시**

1. **아이템 상호작용**: 카메라 중심에서 정면으로 LineTraceByChannel을 쏘아 앞에 있는 아이템 액터를 감지합니다.  
2. **바닥 감지**: 캐릭터 발밑으로 짧은 Sphere Trace를 쏘아 현재 딛고 있는 지형의 종류를 파악합니다.  
3. **범위 공격**: 캐릭터 주변에 MultiSphereTraceByObject를 실행하여 일정 반경 내의 모든 적(Pawn)에게 대미지를 줍니다.  
4. **탄착 지점 계산**: 총구에서 총알이 날아갈 방향으로 Line Trace를 쏘아 벽에 박힐 위치와 각도를 계산하여 데칼(Decal)을 생성합니다.

## **6\. 주의 사항 및 팁**

* **복잡도(Complexity)**: Trace는 매 프레임 수십 개를 실행해도 큰 무리는 없으나, Multi Trace나 복잡한 Shape Trace를 수백 개씩 동시에 실행하면 성능 저하의 원인이 될 수 있습니다.  
* **Collision 설정**: Trace가 원하는 물체를 무시한다면, 해당 액터의 **Collision Presets**에서 해당 채널이 Block으로 설정되어 있는지 반드시 확인하세요.  
* **Async Trace**: 성능 최적화가 극도로 필요한 경우 C++에서는 비동기(Async) 트레이스를 사용할 수 있지만, 블루프린트에서는 기본적으로 동기 방식으로 작동합니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADsAAAAZCAYAAACPQVaOAAADaElEQVR4Xu2WTUhVQRiGj1hQFEQ/Znr1nqtG0M8iuEEQBS0qMmljQQtdBC3aRLsMaiO0ijYh1EKEaCFi1MqioBbSpqCICqIohAxLWoggGVKoPa/nm9s43XspuAuD88LLzHx/83O++eZEUYoUKVKkWCLIZrOf4jh+BHtzudwg7Qi87vTNzc1rGA/JzpHxQz/GfwUW3wrn2cjRUOfAQXQ3NDSsC+WVAvFXaA20h0NdRcEm80w0TXs+1AnIt8P3obySIH4za5iAW0NdRZHJZBqYZAz2hLp8Pr+c076F7lKoqyQsu4ZrampWh7qKQhNoomKTIXvBZk/4smLQofB1jovc8y2hXmhqaqrVVZGN7BFVcdDrkceqA/AUrGtpadkY+irNS8VXzMbGxkPq19bWrlKfK7fZt/FRxST9cEQL8uX6qrawoiDoSvzOwW9M2oX9ZfoTqKp8Oy1QNlYEe+Er+jvVwvmAI76vxZ8h/lnznVHhlE4Hw7gPftF9p/1gc0xhf9CPUwDKHjimlHYynHbrvvp2IfC5D2exPenJOvxDY9wGp7HZpLFdGxWj0xozxwHGc2FWCfYaKH4hu7DvhFej5COdUYv+Jv1JuMfFz5aoQQupIAPYauNO+sOB2SKgb9cEpM0OjS2V5acv62ye2sa6nUybRnaMbrXZ6Ombd3oHiz8Hm3x5Nilm02TVEdp+G4/HVtws29oii/8HcNirCWGHxrRvSqZBgmXY3IazcCybvL/q32Pzu5yRLVaVPu87+7CFFg5I8OqIDmHRlbC1FmLG9nQWy4yiwLhODkoHbZI2F9r4MPtR2B7qHHL2dsK+UOfDbBa9BG4DcMqXR78PeUB9sy2aGSXBBtda8Ocs8nWoD1FfX78hTr5+yR+RKFlYqbujFNNdcweycGhkxT5kg1m7x3DUd9KVQTbpZ0qcZMa4b1cWKtk2qTgU6otAGxlwi/ShNOYwGtW3eBdCG2R35RsnGbLwM2F37Y7k9DO0H+GY87GacEMxIy+1bY5hN/4ryIlgXVFwR8oBn8/wK34PaL/Da7DO6XPJ8/IW/oAvZYPscbR4sT/hM3hFG3ZybQ7ZRfguTg7nCfNsc3qHOKnW+0N5WahiuvfrH1Btb502WLz62c+DbJS2odKKUeGAQkhXTq/Y0T98oBQpUqRYUvgFmrMEzClgJZMAAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEcAAAAZCAYAAABjNDOYAAAEI0lEQVR4Xu1XTUhUURR+gwVFRUXZ+DdzZ8ZIjKhgygiqhdQiyAgRikQXbfqTNkGhtApatAhCdxJEhQUhEZRUFDXRwsigFlmRRBr+QGBSoGg/Tt/nO1fv3PeGNgNT8D44vHfP+e455557z30zjhMgQIAAAQL8VygrK1sYjUY/ZxOl1Hs814IasufmAvBfK3HSkGcVFRVLbE6+EUJiHUwwFost8LEx8WkuxLLlBIwpMeptW96BnVuOxHqYoG0joB+X5C/ZtlwA8RPw/T0ej2+wbXkHk2JyPB22jYB+QorTattyARRnJzeHm2Tb8g6eCFl8l21D4apoQ+KnHJ97J5lMzoetTiRp2zVgqyGntLR0hamXlupS2VsqFIlESjgXuexgPJtg5MD4BbYd9+pq2LZpWyKRWEM+Yu+zqF5w16QApy39Qcg4nOx3fApDPezfWDi8n+c7Fl9mcYqgvwW5CjkOmYQcc8Qf+RgP+rUU9NWQd5AviHECzzeQeyYH+j3QjUFOQl5CrkN69CZE3VP5FvIbcqGkpCSCZwrSDunmPMdnbbNQbkulIV+j3i9Vo80nuFuwT2PxzVqHHd4O3R1+ATmGbaP4+qA50N1kotDt4liSn456W0p/CK6Fw+FFwuXdNOzDOcMBCwL/L5Tx1cN7p+Q1wLiQT3qy+PuIjQlrnQcSoItH3NQbwdrN4wynSeVe0ltNPhdIX3jWYDhP/N7gu+ZgXC/6FOMxLsdzXhxHdrcP0qR1WMB6jF8xF46N3GY5hr/ZFsX7WXkyr/t64wgUbTP0vYi3UusyoBcEabFtBPTnVOaXJIQ5lzmnsLBwscmVnZgpjhQwjQT2mhzxx3gp3VKQUYujC8h24gkeVu4pbtCbRL/CqdTzsPBSjPuztKjn2kAxDytr8zIgiQzadwVh7MQIF06d8WXzfPaVu/DnuPCWKveSf6RbwuCMcC58H8Czhe/K+grCdkX8+94F9Enfdg5cvK0joKuE9LN4WofirlPuXRU3uSb0KfC0FCG9OgXORXKpk0CjkHGTy75V7uU580MRz1Yu0uSIPg1J8dQxroy5QW2IU6fnUm/PFRRkKQ7buFPrxMcheaf/jDVi3CRczotDnmjbDKSl+KXybSk50mmeBK2TRaUgEwY1hMDN5FrHvoM2TQInBt0YClnFsXIvyZmWRayn3E3h8VR5To6cyLsyN6OA9EnfzEvW9ZCtrblmS9nFRbwjeG/TjhQNfxF+ubL9p+KJawBnCM/byj0x1dSbJNgeQP8L8hjyQ3nb5yh0PyG9KMwmy1YE/ZRyN+I1pBu6Lc5cDObQqNxLukcu11qJ06f94b0YMmD/vhLuJGQIstu05QTSSsW23gQWtIwcvx9vhJxEXx96bjY7QVt5efkqPaa/WOYVEbILo+HDDRAgQIAA/wr+AImHdPg0h6iqAAAAAElFTkSuQmCC>