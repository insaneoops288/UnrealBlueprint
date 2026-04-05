# **언리얼 블루프린트: Asin (Arc Sine) 노드 상세 설명**

Asin 노드는 수학의 **역사인(Inverse Sine)** 함수를 계산하는 노드입니다. 사인(Sin) 함수가 각도를 입력받아 비율을 반환한다면, Asin은 그 반대로 비율을 입력받아 해당되는 **각도**를 찾아냅니다.

## **1\. 노드의 역할**

Asin 노드는 입력값 ![][image1]에 대하여 ![][image2]를 만족하는 각도 ![][image3]를 반환합니다.

* **입력 (Value):** \-1.0에서 1.0 사이의 실수 (Float).  
* **출력 (Return Value):** 해당 비율에 대응하는 각도.  
  * 언리얼 블루프린트의 기본 Asin 노드는 **도(Degrees)** 단위로 결과를 반환합니다.  
  * 라디안 값이 필요하다면 Asin (Radians) 노드를 별도로 사용해야 합니다.

## **2\. 수학적 특징 (중요)**

Asin 노드를 사용할 때 반드시 알아야 할 두 가지 제약 조건이 있습니다.

1. **입력 범위 제한:** 입력값은 반드시 **\-1.0 \~ 1.0** 사이여야 합니다.  
   * 만약 1.1이나 \-2.0 같은 값이 들어가면 수학적으로 정의되지 않으므로 NaN (Not a Number) 오류가 발생하거나 예기치 않은 결과가 나옵니다.  
2. **출력 범위 제한:** 결과값은 항상 **\-90도 \~ \+90도** 사이로 나옵니다.  
   * 180도 이상의 회전이나 반대 방향의 각도를 계산할 때는 추가적인 로직이 필요할 수 있습니다.

## **3\. 주요 사용 사례**

### **① 두 벡터 사이의 각도 계산**

두 지점 사이의 높이 차이(Z)와 거리(Hypotenuse)를 알고 있을 때, 경사각을 구하기 위해 사용합니다.

* **예시:** 캐릭터가 경사면을 오를 때, 발의 각도를 지면과 맞추기 위해 사용(IK \- Inverse Kinematics).

### **② 절차적 애니메이션 (Procedural Animation)**

특정 오브젝트가 위아래로 흔들리는 운동을 할 때, 현재의 위치(비율)로부터 애니메이션의 진행 단계(각도)를 역추적하여 로직을 짤 때 유용합니다.

### **③ 조준 및 회전 제어**

대상을 조준할 때 상하 각도(Pitch)를 계산하는 상황에서, 대상과의 거리 대비 높이 비율을 Asin에 넣어 필요한 회전값을 얻을 수 있습니다.

## **4\. 블루프린트 활용 팁**

* **안전장치 (Clamping):** 입력값이 \-1.0 \~ 1.0 범위를 벗어나지 않도록 Clamp (Float) 노드를 Asin 앞에 연결해 주는 것이 좋습니다. (예: 계산 오차로 1.00001이 들어가는 경우 방지)  
* **삼각형 계산:** 직각 삼각형에서 높이 / 빗변의 값을 Asin에 넣으면 빗변과 밑변 사이의 사잇각이 나옵니다.  
* **노드 종류:**  
  * Asin: 결과가 **도(Degrees)** 단위.  
  * Asin (Radians): 결과가 **호도(Radians)** 단위.

## **5\. 요약 비교**

| 함수 | 입력 | 출력 | 용도 |
| :---- | :---- | :---- | :---- |
| **Sin** | 각도 | 비율 (-1 \~ 1\) | 각도에 따른 위치 계산 |
| **Asin** | 비율 (-1 \~ 1\) | 각도 (-90 \~ 90\) | 비율에 따른 각도 역산 |

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAsAAAAZCAYAAADnstS2AAAA3UlEQVR4XmNgGAUjESgoKEjIy8tPBeK7QLwFyLcH0qeB+IKsrKwbXKGioiJQTH4rkNYDchmB7GY5ObmNSkpKckD2aiC+KiUlJQJSC5LsAuq2hWkG8ouAJnsAsQOQ/ReIN8vIyHCC5FiAguYwDogPlFwD5EuDDALSQsbGxqwwg1AASBFQ8QMgkwVdDgMA3WoDVPwbXRwMgJ7gBzpjIcgjoqKiPEB2A7JiID8CaEAqjAP2BJDeBQoVIH0KyP8KkoOG0nppaWkZmGYGkAeAEuJAhRxQIUagAmEQDVc0EgAAivcsVCnyKzwAAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFUAAAAZCAYAAABAb2JNAAAD3ElEQVR4Xu2YT0hUURTGZ9CgqMgoE/+9N6OQlEQLqQiKAo0KqkUEBbUrsKC1QYsIokW0CXIR0R8KQpKIXBRBEkEbSRcVlRG0MIKICEEqbCHT7/PdO9y5zahDjWm+Dw7v3nPOffedb847575JJGLEiBEjRowYMXwkGxoaVqbT6bUa+8ZiEIbhrVQqtd/XGySDINiFzxukj/1Ca2D/JejuolvvLpi1EJkENIKMQchW314Ekqy/3dLSMs83gCT3P468EJmgE+lCX24d6uvrN6MbcMmetaitra1TsMgnZJVvnyrIwmbWb/T1ArZt2EaRvWZ+QvtxbXDcykU0uguJP3xj/huQpacrKysX+frGxsYVkPUKsh7W1dUtkM6Q+p1ri+tLtu5B9wFb2tXPSUDEUojo9/VCGL32GUhvtzrGN/KRqsw1b8xBVz8jwcOu5kHvIG8Zv+P6CKkmuAqu55HnjF+SSWvkz3wnfveUNWRPDbZz6N5rvUjy66bIQT/i6gTTgPqQr6FTWhg/9XVCVVXVQnS9yBVXP51QbMR7TJxIFD/XLsVPnKfGY2eizjCoRmAXmgXVDMtw3IT0hE7mUGeXMT+CDLNBt+qu9OrsYdTQjtp7CazbrfWuTkC3EfkZRj/mIWSfRL7I65qamuX+GpPFT/KVEgsFxn1a7f2mKK1+MviQnb0vIoeZlpnm+ZZn2hBGb9wo92nLBiyDXcz8WhiROo4gT41TUApOxFid1iBDCtzqBLN+yNUJ+LWjzyADyGVHMqy5nsjTkLCdRd5zCqjybRalIpXn3YHfqYR5LnMy6tSPH0bN/Av2ZhmamHxGxpDHKDvsIotgYlKz5GscFkeqsi6jBuSodbxSjT3g6LKw93L3nS6w5ypKYK0zP2ifXXzkvD0EsAWHQQVj5JrtxEIpSMVnPrr7oVc7lYHMX+lU4Ppb/EtSPehj5XqQe+yLoOAsw4wrcGoTsW6mFEsq0qumYvWTkJpTO7VvyqvJLsJ/+Pq7CMyJxo3TNaqm5ny9yFlEOD7FkprTSMwe+RrVFdfX3nOiJqS3YDKfEpGaTJmub2rpeJO1RsZp7nFJyVKoUfW7dY75SeQHunVW19TUtDiMjj5ZUs3X10c/aK1D983OLZSV6IfM6UGvUwfzQd/Pwvkhp/1IZU48akbDQXREPINkZLOngixnYnacXYIyC0tSq1IFvqgSZl/sFb7Bxwz4ovrtWVWGJsnw0iGY4Nt/KjCH7m5lSMI7ncxlqCYV+pdqUuigzfpn9kMjhgHZ2pMq/H9qQZiy9CD9v/yf+jdhvvWvQux231YIpjndDPT5F2Pu4he8UFbl90Ag9AAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAYCAYAAADDLGwtAAABCElEQVR4XmNgGAVAICMjIyQvL38AiM8DcTFQiBFdDYO0tLSMgoLCKZCkrKysDlDhQyC2RFEENIkTKLgZZBqID6QloQpbURQCTcoACv4F0h7ICoH8hXBFioqK4kDB60B8VUpKSgQkJicnZwzkf0VRCOREAAX/A/EkmBiQHYQiBjIBZBIQ/wTipUA8C0qD+F9BJoMVwqwA4gOioqI8aGInlJSU+GEKXYAC/+BWMICtzQHi/yAPwsTgCoGC6UgKTwDxFWVlZTG4QqCP9YGCn4AafJEU/gAGuB9cEQiA3AA1oQrEB2qUB2oqY8AWdUBJM6DC+0C8HIjPYVUEA0A3coAC3tjYmBVdbqgAAL2DRz2w7t/QAAAAAElFTkSuQmCC>