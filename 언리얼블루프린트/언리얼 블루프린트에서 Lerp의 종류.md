# **언리얼 블루프린트 Lerp(Linear Interpolation) 가이드**

Lerp는 **선형 보간**의 약자로, 두 값(A와 B) 사이에서 특정 비율(Alpha, 0\~1)에 위치한 값을 계산하는 수학적 기능입니다. 블루프린트에서는 데이터 타입에 따라 여러 종류의 Lerp 노드를 제공합니다.

## **1\. 데이터 타입별 기본 Lerp 노드**

가장 흔히 사용되는 노드들로, 입력 데이터의 타입에 따라 결과값이 결정됩니다.

| 노드 이름 | 입력 타입 (A, B) | 설명 |
| :---- | :---- | :---- |
| **Lerp (Float)** | Float (실수) | 가장 기본적인 수치 보간입니다. |
| **Lerp (Vector)** | Vector (X, Y, Z) | 두 위치나 크기 사이를 보간합니다. |
| **Lerp (Rotator)** | Rotator (회전) | 두 회전값 사이를 보간합니다. (가장 짧은 경로 옵션 제공) |
| **Lerp (Transform)** | Transform | 위치, 회전, 스케일을 한꺼번에 보간합니다. |
| **Lerp (Linear Color)** | Linear Color (RGBA) | 색상 값을 보간하여 그라데이션 효과를 냅니다. |

## **2\. Lerp (Rotator)의 특수성: Shortest Path**

회전값을 보간할 때는 단순히 수치를 더하고 빼는 것 이상의 계산이 필요합니다. Lerp (Rotator) 노드에는 중요한 체크박스가 있습니다.

* **Shortest Path (최단 경로)**: 이 옵션이 켜져 있으면, 엔진은 회전 방향이 ![][image1]에서 ![][image2]로 변할 때 ![][image3]를 돌아가는 대신, 가장 가까운 방향인 ![][image4]만 회전하도록 계산합니다. 캐릭터의 회전을 부드럽게 처리할 때 필수적입니다.

## **3\. 고급 보간 및 유사 노드**

단순한 선형 보간(Linear) 외에도 더 부드러운 움직임을 위해 사용되는 변형 노드들입니다.

### **Ease (이즈)**

Lerp와 비슷하지만 보간 방식(Function)을 선택할 수 있습니다.

* **인터페이스**: A, B 값과 Alpha를 받지만, 추가로 Easing Func를 설정할 수 있습니다.  
* **주요 기능**: Sinusoidal, Exponential, Circular 등 다양한 곡선을 사용하여 시작은 느리고 끝은 빠르게(Ease In) 혹은 그 반대(Ease Out)의 효과를 낼 수 있습니다.

### **Interp To (실시간 추적 보간)**

Lerp는 보통 **Timeline**과 함께 쓰여 특정 시간 동안 0에서 1로 변하는 과정을 그리지만, Interp To는 매 프레임(Tick)마다 현재 값에서 목표 값으로 다가갈 때 사용합니다.

* **종류**: FInterp To, VInterp To, RInterp To, CInterp To (Color) 등.  
* **특징**: Delta Time과 Interp Speed를 입력받아 실시간으로 목표를 추적하는 부드러운 움직임을 만듭니다.

## **4\. Alpha 활용 팁**

Lerp의 핵심은 **Alpha (알파)** 값입니다. 이 값을 조절함으로써 다양한 연출이 가능합니다.

1. **Timeline 노드와 결합**: 타임라인에서 0\~1 사이의 커브를 만들어 Lerp의 Alpha에 연결하면 시간에 따른 정확한 애니메이션을 제어할 수 있습니다.  
2. **Clamping**: Alpha 값은 보통 0.0(A)에서 1.0(B) 사이를 기대합니다. 1.0을 넘어가면 B를 넘어서는 외삽(Extrapolation) 결과가 나오므로 주의해야 합니다.  
3. **Map Range Clamped**: 특정 범위의 값(예: 체력 0\~100)을 Lerp의 Alpha 범위(0\~1)로 변환할 때 유용하게 함께 사용됩니다.

## **5\. 요약: 언제 무엇을 쓸까?**

* **정해진 시간 동안 A에서 B로 이동**: Timeline \+ Lerp.  
* **매 프레임 타겟을 부드럽게 추적**: Interp To 시리즈.  
* **선형적이지 않은(부드러운 가감속) 연출**: Ease 노드.  
* **회전값이 튀지 않게 처리**: Lerp (Rotator)의 Shortest Path 체크.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACUAAAAXCAYAAACMLIalAAACy0lEQVR4Xu2VXYiMYRTH38mqla98jGlnZ+adL4ZSaKIociGSSCFib7QXPm6QIpTS5mKV1NpSUkgb4ZYbF7ZcEOXKxwUuSFytjWxpi/X7v+/zzDze9e6sCynNqX/Pc/7POec558x5n/G8pjSlKf9YyuXytFwud8D3/Yv5fP5UsVicB51wbTjfxvmKZDI5RWelUmkOXAf2i127TCYzCbudigXOFAqFym/OD4O7+J/U3e55IDgtwqCf4KsxmsG6B31Yjl49sRb4m3AjEdyigOk2lvZw90CXklfC7F+ALcYkAXeQxOZKYZ3J2QklamMEAtkDvmez2U3SlRj6EzAAFjh2l8Azzt+x3gYboCfUAnmB71H5Kobl0HeBlxSfSqfTs9kfd33QO917LHnWD6vulF6pVKayfwC+qIuO3Xkuq9Y9fxVbDJ246vIUuxT+q4pW97A7Uq1WJ5pjdW6fEnZ9PBmoAs9UjfNCggyCfjM/gTRKStWCgWhS8oEfAqelE38lNt26x3RWHY8XDR1GffqJogMM3wt/jvUpeA8egiX23F4el5TLs2+Fa3OLHiWpVGoyzjfMvLzBaZ03el4uwx+zPHoHtp9o/TKjb0QfGU9Sfyw4zyfIB3BNyVpes+Y5iba3t2dMx66jtrCu/2tJIQmC9JkL9kYPraj94K06q0GNuzyOjxUNOcaHBOeLsJ92rWrW3eg/3CSdpIQ2fIqsH6OX26T8yFMQK47DkPaWV2AlBXqMnU2ylpTz8wVfqaA9uINdq7XDdw3csFbLjSk8BVkcXuFwxb7MXDaLoI/98FkIvi7W5aDX7SY22+G++fXXOhj+XPixFAylUegCj9yXv6H44YC+5pJusEOVgs/iHTMF1//VfT98gfUvoBd/v86skZImqQuyI9Zmk9DzfOR5GZeo3fnwv28rQ7vK7Ygr6qwu4+FbO0blCf2hN4rVlP9SfgK4XtszO1TyrAAAAABJRU5ErkJggg==>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABsAAAAXCAYAAAD6FjQuAAABo0lEQVR4Xu2UO0sDQRSFE1SIKKhIDCa72TwESSEIa2UtYiUBLURLOxG0ErESxELtBBu7FPkHWimS2nQWNhYiBPwDWgb9rpmJk3UfggoWOXBg5pw798zuzk4s1kUX/xa5Fla8ukY+n590HOcYnsNVy7L6TV8Wo5/BC7iE1GP6McQS3IA3sEl9paNAQRbDe/zpZDI5yPgAXhUKhSHxi8XiGN6u3gDerGzI26REUVmZDb+wdDpt4z3ANa1ls9kR5nW4qeZzrF3QvtrQHlpCa21gjMMnvzAJga80dA05jlaFNWksntN6dR/IZDKjaFtG/Sciwk59wuQbVdCf0QtMe5lvM19XwSd8Y8esbyMsTDUNCuvQ0YYJSbmu22fWdiAoTL37mrepwC/sWwgKS6VSA+jXfk1/PUwQ1DRIj0RYGPqhX1MV1uDkWaYeibAw27YX8ZryL2mNugTapVDGZn0kdBisMo2bnvwzNLyF+1rjppiQp8qFXG9fILuVRbJz+Kb4QpM7Gk7pOp5uBv2R+h24zLhOzVHoEf8J5GQSOk9IWa4wr9/Fn+MdmT+DfoSH4W8AAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACUAAAAXCAYAAACMLIalAAACpklEQVR4Xu2Vz4uNURjH3xtq5Fd+XDf313nvneFSCt1kRRZTkkgNJewszKz8KopSmpQRm9koLEi3/ANsLNyajVhjgQVNrIbIbKY0Pt/7npNzz72vhkjqPvX0nud7nh/f55znfd8o6klPevKPZWBgYGm5XD5hjLkZx/GlarW6DjgT+vmC3378r4Z4sVhcCH5YubRfqVRqXfbPoA+peVG1/f2WELQJhyZFduK0nOdx7BkFRinE8InZf8Pjro/TzDLwR+hoNptdzP5m1i/QIeuSATsJsbUyeK5g74KI/siCAI6j30ql0j7ZIob9DJ1CN7Q5I/V6fQH4bXQ2JEXsOcUqh8Owj6AvaT6Xz+dXsT7vx2Af66gDcF0FtCm7VqstYT2BftEptjlHrcIHbcykT8o1ExKl2a3gX9W0Tg+/s2rMbuvkRkTYj2l1rg5YzpNN8EaSfEKbSuL72mvT3K3n+dYnoG7RqZAUJOrg0+hl2eTfjs+Y6tiT3eP7d4iGDqcGzu80D/6evbZr4Nt4rglJueJppHycdZ9yhE23SS6XW0TwfZExyQDviuzJOQEfQk+zzKSQ2mu6z1kHqV8WezXv0XsiK4w7xzS39HbJ7kYKe/dfIxUlJ9GwBYax55P4iq7NOXQjlVY8DU8VzQnOp6TeG+Fe7VbX/f39q1lP6GqdmuQkZ9EZO3/DPKvYH8LijpQJPgWp4gVMa+1wJbZFx31/J6bLSWlowZroA/A+h5N30JIfdNhPhU9BiYBXBNxx81IoFFaS9KlJPgtbwhgJPkX2JtFG5H31yXPUnmTFQhqFUfSJyz8nMcmAvobIGHpInaKfhYe+7i1V5yY5SelHXZ/2NQLs3wB7HCf/RhF6HgeflzmJjjtO/n0HeNN2+PP1G5LRD/0P5erJ/yXfAeWM2BS7y3pxAAAAAElFTkSuQmCC>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABsAAAAXCAYAAAD6FjQuAAACDklEQVR4Xu2UO0hcQRiF7+IGlAhGwrq4r9lHQFIpbFAQBQsRK5GYQtwmmCJNIBohD0EwhBRREBQEsROxs7DQQrSwdvs0FiFBSJdCMJ3od3Zn9LpeFxECKfbA4f7/+V8z985cz6uhhv8OxphWOANX0+n050Qi8aQyR8hkMm3kzCkPjpHX4I9Tm0ZfhttwBKnOH1eDTgJ7yWSyF7sdeweewynCIZenYvidfh2RSKQR+4vqstlsk+K5XK6F2Ee3AGLdWpCr9xRIpVJbiOOeXUU8Hn9M0SHaKbG8tFgslsQ/ggVXS6wZvwjfWL+fukEXtwuaRqsvCab8+n7CE+3KJSrJlHf3zvoF/3CLENoGPFBjxUz51ZWgRaO9vczO5/MPmLyIuKvBTsf/oGF6ysdeChimb7SG/hs9ixvGn8B/ZQfPswHjzw9CmMRNeEZxnwTb9LZh13S0RwyJaiP+3ECQ3KUGcFUF9t0fVDa1uTeG3Rk6WRTvw/VoNPpQmp5Wu9H03sO0C4pWKF4IuD+BTW/Tq8INoviTZ68ATZ5y9was/TWoqR12zMlL+PVq0BGeonBSthPxX6M/l83QIewzhvX74vWm/APYke30agiR+JKCv1ohzX454v/h2aMkd9HhrCvUL001aKNOqwpzdanPA+juTwns7hnaD7T38AV2kUHf7nTE7wOdTH1HhgzrF1YZr+Gf4wKjEZomr81ahAAAAABJRU5ErkJggg==>