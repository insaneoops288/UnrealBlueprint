# **Unreal Engine Blueprint: Distance (Vector) 노드 이해하기**

Distance (Vector) 노드는 3차원 공간상의 두 지점(Vector)을 입력받아, 그 사이의 \*\*유클리드 거리(Euclidean Distance)\*\*를 단일 부동 소수점(Float) 값으로 반환하는 함수입니다.

### **1\. 기본 구조**

* **입력 (Inputs):**  
  * **V1 (Vector):** 첫 번째 지점의 위치 좌표.  
  * **V2 (Vector):** 두 번째 지점의 위치 좌표.  
* **출력 (Outputs):**  
  * **Return Value (Float):** 두 지점 사이의 절대적인 거리 값 (항상 0 이상의 양수).

### **2\. 수학적 원리**

이 노드는 내부적으로 피타고라스의 정리를 3차원으로 확장한 공식을 사용합니다. 두 점 $P\_1(x\_1, y\_1, z\_1)$과 ![][image1] 사이의 거리 ![][image2]는 다음과 같이 계산됩니다.

### **![][image3]3\. 주요 활용 사례**

게임 제작 시 이 노드는 다음과 같은 상황에서 필수적으로 사용됩니다.

* **근접 상호작용 (Interaction):** 플레이어와 상자(Actor) 사이의 거리를 체크하여 특정 거리(예: 200 units) 이내일 때만 "E 키를 눌러 열기" UI를 표시함.  
* **AI 추적 로직:** 몬스터가 플레이어를 발견했을 때, 플레이어와의 거리를 지속적으로 계산하여 너무 멀어지면 추적을 포기하거나, 사거리 안에 들어오면 공격 애니메이션을 실행함.  
* **사운드 감쇄 (Attenuation):** 소리의 근원지와 리스너 사이의 거리에 따라 볼륨을 조절하는 로직을 직접 구현할 때 사용.  
* **승리/패배 조건:** 레이싱 게임에서 체크포인트나 결승점까지 남은 거리를 계산하여 UI로 표시함.

### **4\. 팁 및 주의사항**

#### **1\) 2D 거리 계산 (Distance 2D)**

만약 높이(![][image4]축)를 무시하고 평면상의 거리만 측정하고 싶다면 Distance (Vector2D) 노드를 사용하거나, 입력 벡터의 ![][image4]값을 0으로 만든 후 Distance 노드에 연결해야 합니다.

#### **2\) 성능 최적화: Vector Length Squared**

단순히 "어떤 거리가 특정 값보다 큰지 작은지"만 비교하는 용도라면, 제곱근(![][image5]) 계산을 생략하는 VectorLengthSquared (두 벡터를 뺀 결과값에 사용) 노드가 성능상 더 유리합니다.

* 예: Distance \< 500 대신 (V1 \- V2).VectorLengthSquared \< 250000 (500의 제곱)을 사용.  
* 수천 개의 액터를 매 프레임 체크해야 하는 복잡한 로직에서 유용합니다.

#### **3\) 좌표계 기준**

입력하는 두 벡터는 **동일한 좌표계**에 있어야 합니다. 예를 들어, 하나는 World Location인데 다른 하나는 Relative Location이라면 엉뚱한 결과가 나옵니다. 반드시 둘 다 Get Actor Location과 같은 월드 좌표계를 사용하는지 확인하세요.

### **5\. 노드 사용 예시 (Blueprint Pseudo Logic)**

1. Get Player Character \-\> Get Actor Location (V1 연결)  
2. Self (Enemy) \-\> Get Actor Location (V2 연결)  
3. Distance (Vector) 노드 연결  
4. 결과값을 Less Than (\<) 노드로 500.0과 비교  
5. Branch 노드에 연결하여 "공격 범위 확인" 로직 완성

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGkAAAAYCAYAAAD5/NNeAAAGMklEQVR4Xu1Za2hdRRA+IRV8a9QYzePszUNDfKAYrUp906IFqxCpFF8/fCI+CKZVEgoqUqRFsVYwpYRGC0mRRlIoBdGglRQpFoSIL9SASonUooJYodUmft/dOTdzJ+eee26C/eP5YDhnd2Zndmdmd+fcGwQZMmTIkCHD/wi1tbWn5nK5E21/Bo+WlpYzOjs7T7D9xw0IzuVhGA5yIpaXwaO5uXmxc24olY8geDPoZ9CMot9Av8j7n3D4xra2ttPt2Dg0NDQ0YsxHGHOx5WUoBnx0H2hz6h0Fxw6A/sag60z/FQwYdsf7PMI0LwZVDChkX7CMDHPR2Nh4Eny7C9RleXPQ3t5+GgTHQZPYhnWax8Cgfw9oGgFYqnkWTU1Nl0DuGz4tL0M8kNCr4LNPyh57EOoA/QoaQXOR5iEwNejfH7fLLMB/DnK7s4IhPeCzFvjs+3K+5Q64w/n75xnLQ9+1oCOgfUnRZmAYIFCf5UWor69vwmRu55Nt3F9nQ3456HwruxBQP/VSv+7nHFMc2RWBNqD31ujOrqurOwWn0WVWrhQoj7mOJfktDwhscjE7hUERBYcQyCs1z4KOBv0IHSssD6jCQrrB6wc9ALmvQBtA29B+Fs/vsLB2O2g+gK4u2HobtB7vE1GgJIlG8XwHzWozbF5gcKDzsPMJPon21aBeJr2VTQLni/FDeK2yvDzUncPdMgTaQpKBU3DiICs2O84Ccp2QP2gDLbxl4K0NZBKi+4Dzxyx332GON8MqRmtr67nQtZWBkaN3CsF35Mmxwio2OWNTQhJ4mIGhf5iceP+cSReUcnYJyFz3lNzl4ijeRx9wQc7viDwx+yI5ViJQ9ij6t/DJttZDJ4N+inM2+p7Goi7ku9re+fsPvCWwc2cg2S0l/DrQm5jPDUEFC+Zux7iHxYH7QNsDuWNhZynaR/lEsxrvd3EtoL60nxcJYFW7krpUOZ3ahgRpP541lpeHuo+SMmwR+XS0BOsttId1fZ8UJI1QMpoTszzJyOdpA3QB3r+F7D1Wrhycv0f/QvBXqT4G/gBtoP+x0O94OnItaK+9vyoAA8QjvCdQx2glNiRIcyrrApy/j6bDhPLayX3jpLCgceePksIFmTZIkhRHZAFFQN8K8P6I9Ipjx7j7rGwSZNFMhBa21e7djaLiHOeP9wGRZdJMgZYXa0mFagSjG/R4oHa8ukJS2ZD5xh936vvoB2Rug+UrVMFxi7lANhhQMdoRCSRMpAp9D4K/WSbPpCjY4z0C3mvQfTKdifcl0Q4V2aKSnjyOSfpKlzuvsGiZG++jdWzD9qXRXcU1cN5ck9FxJkn3GXCH9GDcymA2QCyQXmSiprERgfNypZJRPj5/t44oAx59AzDYz/eoUzL0S25zJcvfqOrQPwmawHs7np+5WQcygGtA9+oxhJTRX9hKCX2vgmZyCb9qcH7aRugryGNxTmIW58yvKTLPg84XN81aXhDp/Af0Id9z/qN0G2g05r6eY0OBPmDBtqmoFwu/3vnja0YRf697qEgwBpDpokI7kcBPfNAaY8ZLZu8C/z3QU85/HI+A3gWv1+4KtiG30WRpHpDvxrhp0FiJRUcFBB08Ah078TzqYk4Lng7oH0b/WbpfipcJ2gljPinoP8zjdflZ5xbQIed9OIb+87RsKRsRoL8GYz61yThvOP/h2RMdOdYws4kGaVj3A9WUV7vVtgugbtjo4+LZZgERmF9BZNe+EXs8QDePceqRXcz7lPdDodIjcv6X+pepg+vgfJSOPMB/hGu2/RyjE4vrkKLAJlRZG84XOV+7+B1bGSQj+hDxeln4avv9JL8g7MXkbtP9FYBb/wlmL23w2IGuXvZrIS6MR4zuE7Ck73eq+JBChd9hyyKhZv+psQHVakg74N0Puatm1eTBY/2V+TovpQ2u9yUS3w2vMojzuf1nFI0zY62sOGVHzHFYFgjITRh7zNgp+jSQ4mNrGPNXiKridvBbiU4JfRn/ZCBOYNajPWps5EtzrYuJBlofjasEaW3AR23oH2NAdf/xQFQMrOG7ZS4U0NsBJ9xo+yOQh8B8DLlxPHeCLrIyKcAdeXfSb5ULhdy7gy7N3xT/BeReWQ2HXWN5GTx43yVdC/8CffXj6PRl5oEAAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAYCAYAAADDLGwtAAABD0lEQVR4XmNgGElAVFSUR1lZWQxdHA7k5eUdgfgnEP8H4j3o8ihAWlpaBqjoCRC3osuhADk5ORuQqbKysn7ocigAqLAcqPAtEGuiyzGDBIEKfBUVFcWB7DVAfADkIbgKkAIgvgBU1A7EaSA2EP8C4klwRUDd8kDJWwoKCpVALiNIDMhOAPkY2X0sQIHlUN8pwgQx3Ae1EiSwBqQJSTOq+0AOB1kBtCodqohBRkZGGij2AMV9MIVA7IkkBgq/30AcBMSWQEMKGYCO1QFZDXO0kpISP5C/B4i/AjUYA+lqIO0CkmMEcnKA+CIQzwXi3UAcCMRXgAp2AuleY2NjVphtyKmEGcQHSUpJSYnA+EMBAAAzJkWeuwxtiQAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,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>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAXCAYAAADUUxW8AAABFUlEQVR4XmNgGAUDCBQUFCLk5eX/I+G/QPwEikHs/3JycjPQ9YEAI1BiPhA/AmJXIJ8ZJqGoqGgG1PgeiPcoKSnxI+mBABkZGWmg5HGgRm1kcaCYJhDfBYrfAhoijywHB0BJF6CiamQxkGKQJqD4a1lZWRNkORQAlHQD2QLjg5wHcibIuSBnI6vFC4Be4ARqmgMNpGB0eVwAFGhlQA3/QDSIDxVnAcaEgbGxMSuyYmTACFQQDrVtFrJCoEFKQLHJQCYLQjkSADkPqhEjOoBiOUCD05HF4AAWj0AFh4BYAk0OKCV/BBigOsjicElQdKDHI8jZQIPsgRqvA+V2gAIRWR+DuLg4N1ByuzxqksSGc1A0jgLSAQCMDE2L987FsgAAAABJRU5ErkJggg==>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABEAAAAWCAYAAAAmaHdCAAABJ0lEQVR4XmNgGAVUBbKyssry8vKt6OKkAEagAetkZGRy0SWIBlJSUhpAQ04AmUzocsQCRgUFhRlycnI26BJEA2lpaRmgK7YxUOAKBqAr+oCucEEXJxoAXSEMdMUZBnJdIS4uzg10wU4GAgYwQjFWAEwXRUAcgS4OByIiIrxAv64C2lSJLgcFTEBv7AXSLOgSYCApKSkPinMg/g/EdxmwOBfogiSgXBK6OBwAFQQAsS0o1KEGoStmArrwIIhGE8cEwCSsCjXkHAOSs4EGhALFspGU4gUgG3eADAIaaI8sBqSZkRXiBUAvWYAMAWrczAAJzGAgLkJXRwgwgvwPMgiYsNSA9HYGUlwBA0BDwqBhcwPosgoGPGkHH2AGGnAdiL8Bs7wIuiQ6AABrbTQ9px/UkAAAAABJRU5ErkJggg==>