# **Unreal Engine Blueprint: Vector Length XY 노드 가이드**

Vector Length XY 노드는 입력받은 벡터의 **평면적(2D) 크기**를 반환하는 함수입니다.

## **1\. 수학적 원리**

일반적인 3D 벡터의 길이(Vector Length) 공식은 다음과 같습니다:

![][image1]반면, Vector Length XY는 Z축을 ![][image2]으로 간주하여 계산합니다:

## **![][image3]2\. 주요 역할**

* **Z축 무시**: 높이 변화(점프, 추락, 경사면 이동)에 영향을 받지 않는 순수한 수평 이동 거리나 속도를 측정합니다.  
* **2D 거리 계산**: 3D 공간 상에 있더라도 두 대상 사이의 '지도상의 거리'를 구할 때 유용합니다.

## **3\. 주요 활용 사례**

### **① 캐릭터의 이동 속도(Ground Speed) 확인**

캐릭터가 점프 중이거나 낙하 중일 때, 캐릭터의 Velocity(속도) 벡터에는 Z값(중력이나 점프 힘)이 포함됩니다. 이때 그냥 Vector Length를 쓰면 떨어지는 속도 때문에 값이 커지게 됩니다.

* **활용**: 애니메이션 블루프린트에서 '걷기/달리기' 애니메이션을 전환할 때, 오직 바닥에서 움직이는 속도만 체크하기 위해 Get Velocity \-\> Vector Length XY 순서로 연결하여 사용합니다.

### **② 수평 거리 판정**

플레이어와 적 NPC 사이의 거리를 잴 때, 적이 플레이어보다 훨씬 높은 성벽 위에 있더라도 "평면상으로 얼마나 가까이 있는가"를 판단해야 할 때가 있습니다.

* **활용**: 스킬의 사거리 체크, 미니맵 상의 거리 표시 등.

### **③ 발사체/이동 제어**

특정 발사체가 수평으로는 일정 속도를 유지해야 하지만 수직으로는 중력의 영향을 받아야 할 때, 수평 성분의 힘만 추출하여 보정하는 용도로 사용합니다.

## **4\. 유사 노드와 비교**

| 노드명 | 계산 범위 | 용도 |
| :---- | :---- | :---- |
| **Vector Length** | X, Y, Z 모두 포함 | 3D 공간 전체에서의 실제 속도나 거리 |
| **Vector Length XY** | X, Y만 포함 | 지면 기준 속도, 수평 거리 |
| **Vector Length Squared** | X, Y, Z (제곱값) | 성능 최적화용 (루트 연산을 생략하여 거리 비교 시 사용) |
| **Vector Length Squared XY** | X, Y (제곱값) | 수평 거리 비교 최적화용 |

## **5\. 블루프린트 구성 예시**

1. Get Velocity (변수 또는 함수) 호출  
2. Vector Length XY 노드 연결  
3. 반환된 Float 값을 Speed 변수에 저장하거나 애니메이션 블렌드 스페이스의 입력값으로 사용

**팁**: 성능이 매우 중요한 루프 문 안에서 단순히 거리의 "크고 작음"만 비교한다면, 루트(![][image4]) 연산이 없는 Vector Length Squared XY를 사용하는 것이 더 효율적입니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAmwAAAAyCAYAAADhjoeLAAAECklEQVR4Xu3dPYgcZRgH8L0YK0XlJJzhdm72ThtBqxNLFfyAgCiSRvADsfSjSWEjalBsREHwBA2KWliKhkRUOPADOys/gha2YiFylWWiz+PNJOPL7jm77l0i+f3gYWae991957b6MzO7NxgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAwEVjOBw+V1XVXWq2is/v5vIzBQCYm9Fo9GzZAwDgArG0tHRZXdffln0AAC4QKysr38fmkrLfx9ra2pWjELsL5dgeWIhzvz3PoRzYAwv5d5+ntQGAi01d10+Xvb7itRsRmr6M7Sfl2G6LNV+Keizq18EeB8Zm7SPnY20A4CITYevJwZjAEUHk7aifo/6squqe7MXcz/I46qeog3l1KbavHDhw4PLYflG8xUxGo9FV8V4nm3V+yON2LNa/O/uxfTfO6drY/63pr8f+oXPvMhcLzTlk5WdxLOpE1OlY785cO87ttl1aGwBgW4SNhyJsPF/2O/ZnYGkPYv/r7mAr+i9EeHmv7I8Tc18re+PEvAeiTrXH6+vrl8bx64MiXC4vL18da3/V7U3SBMuDZX+c+GzuyPduj5twmp/FP9bvuzYAwL/ZVzZSBJBvFhcXryj7XRlSVldXl6LqqLE/WxGh5c384kLZH2eKwLYadaY9jgD1VNQb3TlN/9Woh8v+ONMEtgyh7X4TFvMK27HunOz3XRsAYJKFqqruba4MlfYNh8Mby2YpwtjReP3pqMPlWIr+MxnmmlurO2q+jfpB2Z8kzzvOcTmDYq4xZvzxDGDxN94Q2+vL8VIGz76BraO9NbrZbTZr39d3bQCASfL2XX6T8ru8xdcdiJDxefd4kryqlYElg1s5Nq28whXv91HZnyTW3ah3CIvTyrA2TWDLoBjztyI0XleOAQDMVb39TcoPB51boxGcjnemTJJh7+/AFvVjOdhH3dxKzIr3eie2v3R7EQTvL1/TivEnojYGxXNjfcVrD3XXinq/qbO9CGW3lK9rxfhWVnu807kCAPxn9fazaC/nflVVNw0mPNfWivmHoz7O/ea2aN5W3V9Mm8oMV9hO1XO83VhPcYUt5m3G3/3IoBMW49w/PTcDAGDOMnBF/T7Yvmp2ohwvxdyTEViuyf3mOa2t3JbzpjFDYPsjX1P2Z9U3sI37RmjTe7EzDQBgvla2fzssf8Ps+HA4vLUcT/lgfxPsss60z711ellvla/rq29gizU2izWPlHNmUfcIbHnbs1j7bPmvBgDArovQ8WgEpgfL/h7K33WbS/iaRQbGeV6xAwDYDfnc2o7PrgEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADw//AXcwgC8w4TwSwAAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAWCAYAAAD5Jg1dAAAA3klEQVR4XmNgGNFARUWFT05OboK8vPw7IH0SiK3R1TAoKSnxAxXsAeI5MjIynLKysm5A9msg7YeiUEFBIQIo8QloijFUiBHIng8UOwEyBKaIAyiwFYgfArEkTDNQYTmQ/xtI24AFQJJQRaeBgoJoCv8DcRFMwBjI+QrEB0RFRXmQFPqCFAJtXEg7hUpAgedAfFhdXZ0XphCoIB2kEORWmEJBkEfksfv6PxAHwcRAPp8EMhVkOj4xkKChPCTqYkB8ZWVlMSD/CpA/AchlhCuEAWlpaWGQJ4CxoYZVwcAAAGi+RqhBZojtAAAAAElFTkSuQmCC>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAmwAAAAyCAYAAADhjoeLAAADJUlEQVR4Xu3cv4scZRgH8N1wRUBBm+Pgbnd277hCQVBQAopgQPFHGxsLawXRNFEsRLBQsDBdICCkiClEKxvRwsLiikCENAqW/gMhIFioYHwefVeGh00ynGhyw+cDDzP7vO/OzHZf3tmZyQQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGGA2m52Yz+fPRT17lKv+LgCAUYig81jXdRdrHwCAu0SEtS8Wi8WDtQ8AwN1hGoHtydocarlcnrxTYW9vb+++uPanY3daxwAARmN7e/uB2Byr/SHyVmoEtpeiPtvd3V3U8f9aBMVzef7YflXHAABGo+u6r2svQtAjEYKuRt2I+jHqtejd3z5nXYhpG7H9JPrH8zux/005zKHEcd6K+q2dJwPZMvvtmr7PsbyFm6trsX+2fefb/jEAAEZjZ2dnNrnF6loEoQ+iPo3djVxBC0/UOSkC1Mv7+/ubtb9OzH279qq8RRvn+n3Ru9Ua+2eif74/r5lGmPPABABw5K37j9c0QthHtdkXIenxqOvz+fyh2H5Zx1Mc40QEps9r/2aGBLa2enY56vVVL8+f/f681j+1tbV1T+0DABwZEWiuRF2alNAWIeu9CFtv9nvrLNpt0Mma0JerbhHA9nKlLua8U8fXGRLYUsx7NI75S9se1PEU/Xfz3DHnjToGAHCkZOCKgPZh6Q0JWNOY92sLbBt18DCGBrZ0q7AIADAqEXr+iLq2+ry5uXnvkNuI8Z0Xo17I0Ja3Rev47bQHFT4u9V3/c15L/d5KXve6hyIAAEZnNps91VaqjuVTnbH/Q51TxZyD1as6ln+/OuNynXMYQ1fY2nVmYHumjgEAjFG+guNKhp8IQqdv99+19gqNf57QjPlbGZ76cw5raGCLkLkT5/wpt3UMAGCU8hZorrJFvT+5yas8chWtzcn/vL2avQhrD8fnn1f9xb9839qQwNY7119VxwEARquFn7Vh7f8SQfD52gMAoOm67pXaAwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAODO+BPiTKztIOBKNAAAAABJRU5ErkJggg==>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABEAAAAXCAYAAADtNKTnAAABHUlEQVR4XmNgGAXUBkxAzIYuSBKQlZWtkZOTu4QuTjSQl5dPUlBQKEMXJwUwAQ05IioqyoMuQTQAesNfUVGxCchkRJcjFjABw2E30CsS6BJEA6ArTIFemc1AgSsYgQasB7pECV2CaCAlJaUBNGQeA4WuWCotLa2GLoECgM50BeJKBiw2AQ1QBOK1DJBUihWAbCkB4v9A/FFCQkIUi/wUGRkZezRxBADangbE/UA8EWQQMA3UIsuLiYmJA+U2MeBxBTIA2bgGZBBQUwxMEMjeyECkAWAATAe2UG8dB3KZQWJAQ1zQlBEEoHxxAmQQKGGJi4tzg8TQFREEQAOCQIYAk/YqoCvK0eWJBSxAQ+5AvXUYXZJoAHRBLtQ1BMsMAOQxMolBrtcwAAAAAElFTkSuQmCC>