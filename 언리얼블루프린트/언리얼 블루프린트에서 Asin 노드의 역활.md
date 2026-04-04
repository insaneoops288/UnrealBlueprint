# **언리얼 엔진 블루프린트: Asin 노드 가이드**

Asin 노드는 수학의 **역사인(Inverse Sine)** 함수를 수행합니다. 특정 사인 값을 입력받아 그에 해당하는 **각도**를 결과로 돌려주는 역할을 합니다.

## **1\. 핵심 역할**

* **사인 값 \-\> 각도 변환:** ![][image1] 일 때, ![][image2] 값을 입력하면 각도 ![][image3]를 찾습니다.  
* **삼각형의 각도 계산:** 직각삼각형에서 '대변(Opposite)'의 길이를 '빗변(Hypotenuse)'의 길이로 나눈 값(![][image4])을 알고 있을 때, 그 사이의 각도를 구할 때 사용합니다.

## **2\. 노드의 입출력**

언리얼 엔진에는 보통 두 가지 버전의 Asin 노드가 있습니다.

* **Asin (Degrees):** 결과값을 '도(Degree, 0\~360)' 단위로 반환합니다. (가장 많이 사용됨)  
* **Asin (Radians):** 결과값을 '라디안(Radian, 0\~2π)' 단위로 반환합니다.

### **입력값의 범위 (중요)**

* **범위:** \-1.0 \~ 1.0  
* 사인 함수의 결과값은 항상 \-1과 1 사이에 존재하기 때문에, 이 범위를 벗어나는 값을 입력하면 NaN (Not a Number, 오류값)이 발생하거나 결과가 정의되지 않습니다.

## **3\. 주요 활용 사례**

* **회전 및 각도 계산:** 캐릭터가 특정 경사면에 있을 때, 경사면의 높이차를 이용해 캐릭터의 기울기(Pitch/Roll)를 계산할 때 유용합니다.  
* **절차적 애니메이션:** 특정 수치를 부드러운 각도 변화로 변환해야 할 때 사용합니다.  
* **벡터 연산:** 두 벡터 사이의 관계를 각도로 표현해야 할 때, 내적(Dot Product)과 함께 사용하여 삼각법 기반의 로직을 짤 때 활용됩니다.

## **4\. 블루프린트 구성 예시**

만약 빗변의 길이가 100이고 높이가 50인 삼각형의 각도를 구하고 싶다면:

1. 50을 100으로 나눕니다 (결과: 0.5).  
2. 이 0.5를 **Asin (Degrees)** 노드에 연결합니다.  
3. 출력값으로 약 30.0(도)를 얻게 됩니다.

### **요약**

Asin 노드는 **"이 사인 값이 나오려면 각도가 몇 도여야 하지?"** 라는 질문에 답을 주는 노드라고 이해하면 쉽습니다. 주로 거리나 높이 비율을 가지고 회전값을 만들어낼 때 필수적으로 사용됩니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFUAAAAZCAYAAABAb2JNAAAD3ElEQVR4Xu2YT0hUURTGZ9CgqMgoE/+9N6OQlEQLqQiKAo0KqkUEBbUrsKC1QYsIokW0CXIR0R8KQpKIXBRBEkEbSRcVlRG0MIKICEEqbCHT7/PdO9y5zahDjWm+Dw7v3nPOffedb847575JJGLEiBEjRowYMXwkGxoaVqbT6bUa+8ZiEIbhrVQqtd/XGySDINiFzxukj/1Ca2D/JejuolvvLpi1EJkENIKMQchW314Ekqy/3dLSMs83gCT3P468EJmgE+lCX24d6uvrN6MbcMmetaitra1TsMgnZJVvnyrIwmbWb/T1ArZt2EaRvWZ+QvtxbXDcykU0uguJP3xj/huQpacrKysX+frGxsYVkPUKsh7W1dUtkM6Q+p1ri+tLtu5B9wFb2tXPSUDEUojo9/VCGL32GUhvtzrGN/KRqsw1b8xBVz8jwcOu5kHvIG8Zv+P6CKkmuAqu55HnjF+SSWvkz3wnfveUNWRPDbZz6N5rvUjy66bIQT/i6gTTgPqQr6FTWhg/9XVCVVXVQnS9yBVXP51QbMR7TJxIFD/XLsVPnKfGY2eizjCoRmAXmgXVDMtw3IT0hE7mUGeXMT+CDLNBt+qu9OrsYdTQjtp7CazbrfWuTkC3EfkZRj/mIWSfRL7I65qamuX+GpPFT/KVEgsFxn1a7f2mKK1+MviQnb0vIoeZlpnm+ZZn2hBGb9wo92nLBiyDXcz8WhiROo4gT41TUApOxFid1iBDCtzqBLN+yNUJ+LWjzyADyGVHMqy5nsjTkLCdRd5zCqjybRalIpXn3YHfqYR5LnMy6tSPH0bN/Av2ZhmamHxGxpDHKDvsIotgYlKz5GscFkeqsi6jBuSodbxSjT3g6LKw93L3nS6w5ypKYK0zP2ifXXzkvD0EsAWHQQVj5JrtxEIpSMVnPrr7oVc7lYHMX+lU4Ppb/EtSPehj5XqQe+yLoOAsw4wrcGoTsW6mFEsq0qumYvWTkJpTO7VvyqvJLsJ/+Pq7CMyJxo3TNaqm5ny9yFlEOD7FkprTSMwe+RrVFdfX3nOiJqS3YDKfEpGaTJmub2rpeJO1RsZp7nFJyVKoUfW7dY75SeQHunVW19TUtDiMjj5ZUs3X10c/aK1D983OLZSV6IfM6UGvUwfzQd/Pwvkhp/1IZU48akbDQXREPINkZLOngixnYnacXYIyC0tSq1IFvqgSZl/sFb7Bxwz4ovrtWVWGJsnw0iGY4Nt/KjCH7m5lSMI7ncxlqCYV+pdqUuigzfpn9kMjhgHZ2pMq/H9qQZiy9CD9v/yf+jdhvvWvQux231YIpjndDPT5F2Pu4he8UFbl90Ag9AAAAABJRU5ErkJggg==>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAsAAAAZCAYAAADnstS2AAAA3UlEQVR4XmNgGAUjESgoKEjIy8tPBeK7QLwFyLcH0qeB+IKsrKwbXKGioiJQTH4rkNYDchmB7GY5ObmNSkpKckD2aiC+KiUlJQJSC5LsAuq2hWkG8ouAJnsAsQOQ/ReIN8vIyHCC5FiAguYwDogPlFwD5EuDDALSQsbGxqwwg1AASBFQ8QMgkwVdDgMA3WoDVPwbXRwMgJ7gBzpjIcgjoqKiPEB2A7JiID8CaEAqjAP2BJDeBQoVIH0KyP8KkoOG0nppaWkZmGYGkAeAEuJAhRxQIUagAmEQDVc0EgAAivcsVCnyKzwAAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAYCAYAAADDLGwtAAABCElEQVR4XmNgGAVAICMjIyQvL38AiM8DcTFQiBFdDYO0tLSMgoLCKZCkrKysDlDhQyC2RFEENIkTKLgZZBqID6QloQpbURQCTcoACv4F0h7ICoH8hXBFioqK4kDB60B8VUpKSgQkJicnZwzkf0VRCOREAAX/A/EkmBiQHYQiBjIBZBIQ/wTipUA8C0qD+F9BJoMVwqwA4gOioqI8aGInlJSU+GEKXYAC/+BWMICtzQHi/yAPwsTgCoGC6UgKTwDxFWVlZTG4QqCP9YGCn4AafJEU/gAGuB9cEQiA3AA1oQrEB2qUB2oqY8AWdUBJM6DC+0C8HIjPYVUEA0A3coAC3tjYmBVdbqgAAL2DRz2w7t/QAAAAAElFTkSuQmCC>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAXCAYAAAARIY8tAAABiklEQVR4Xu1TMS8EURDeyx6VCJElZ3dv9+42Ettu0ChcqVDR+AH8AYVEL6GV6CgUColG4T+oVEJDQdRUp5Cc9X3MvDy7xRVcorhJvryZb2bfN+/NW8cZ2MD6bvV6fSOKoo84jveLuT8xbH4A5MA5wmox/2tLkmQUmy8DtWLuX1klDMNp3PcK7noJ67gmEI+xe652ve/7E+Q9zxshYfs/LMuyISQPgSNsvAZsAk/MUVDuP4fAiX5DX3nU7GE95oyAd2ALJRUjgIJFkLfsQDn4bfXRaYD42Rag4cRz4N+AF+QWQFVF5KHRaEyZQumyi6J1hC65glgNeCwK4LsMfIebOvK65GQd5kyhdHgXfR+ZR7wCVjXfS8DmEe+UBGjNZnMGiUugK0K5I/fYSwDYtbjtkkAQBD7IWQldiqHgQrleAtzU4soCMoNXDGZeOcRtHZQOGTh1rNdhDdnwPA055rROBW6Aa3R5JoO6lxw7yi18dWc/U+UFhjMnTtN0mP8CXLfVak0S9E0HA+u3fQJt6obZvZADFwAAAABJRU5ErkJggg==>