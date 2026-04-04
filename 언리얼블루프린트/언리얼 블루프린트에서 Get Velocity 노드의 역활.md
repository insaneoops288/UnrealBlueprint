# **언리얼 블루프린트: Get Velocity 노드 이해하기**

Get Velocity 노드는 게임 월드 내에서 오브젝트의 동적인 움직임을 파악하는 데 가장 기본이 되는 노드 중 하나입니다.

## **1\. 기본 역할**

이 노드는 대상 액터나 컴포넌트의 \*\*현재 속도 벡터(Velocity Vector)\*\*를 반환합니다.

* **벡터 값:** X, Y, Z 세 축의 수치로 구성됩니다.  
* **방향과 크기:** 이 값에는 이동 중인 **방향** 정보와 초당 이동 거리(센티미터/초, cm/s)인 **속력** 정보가 모두 포함되어 있습니다.

## **2\. 주요 출력 값 활용**

Get Velocity의 출력은 Vector 타입이므로, 이를 활용하기 위해 주로 다음 과정을 거칩니다.

### **A. 이동 속력(Speed) 구하기**

단순히 "얼마나 빠른가"라는 스칼라 값이 필요할 때는 Vector Length 노드를 연결합니다.

* **공식:** Get Velocity \-\> Vector Length \= **현재 속력(Speed)**  
* **활용:** 캐릭터가 일정 속도 이상일 때 달리기 애니메이션으로 전환하거나, UI에 속도계를 표시할 때 사용합니다.

### **B. 이동 방향 확인**

오브젝트가 향하는 물리적인 방향을 알고 싶을 때 사용합니다.

* **활용:** 낙하 중인지(![][image1]), 상승 중인지(![][image2]) 판별하거나, 피격 시 밀려나는 방향을 계산할 때 유용합니다.

## **3\. 대표적인 활용 사례**

1. **애니메이션 블루프린트 (Animation Blueprint):**  
   * 캐릭터의 현재 속도를 체크하여 Idle, Walk, Run 상태를 결정하는 블렌드 스페이스(Blend Space)의 입력 파라미터로 가장 많이 사용됩니다.  
2. **낙하 데미지 시스템:**  
   * 캐릭터가 지면에 닿았을 때, Get Velocity의 ![][image3]축 값을 확인하여 충격 속도가 일정 수준 이상이면 데미지를 입히는 로직을 만듭니다.  
3. **발사체 및 물리 상호작용:**  
   * 발사체가 벽에 튕길 때의 반사각을 계산하거나, 물체가 부딪혔을 때의 충격량을 속도에 비례하여 계산할 때 사용합니다.  
4. **사운드 및 이펙트 제어:**  
   * 자동차의 엔진 소리 피치를 속도에 따라 조절하거나, 빠른 이동 시 발생하는 바람 이펙트(VFX)의 강도를 조절하는 데 쓰입니다.

## **4\. 주의사항**

* **Physics 기반:** Get Velocity는 주로 Character Movement Component를 사용하는 캐릭터나, Simulate Physics가 활성화된 스태틱 메쉬에서 정확한 값을 반환합니다.  
* **좌표계:** 반환되는 값은 일반적으로 **월드 좌표계(World Space)** 기준입니다. 로컬 기준의 속도가 필요하다면 Unrotate Vector 노드를 사용해 변환해야 합니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAZCAYAAABzVH1EAAACc0lEQVR4Xu2WTUhUURiGR6ag7D8bBufv3pkGBjGkGLClP0SbEAKXmgQtWgotCsRFEK6ksJEIRAgRNwXtpI2I4ELIoE22CFoUQbQocdFGkHo+5tw7Z75M546TEd0XXu7c7++c93znnjORSIgQIUL803Bd95DjOFM7kZiH0NW5+wGGPZlMJlP8jGpfFZhoG9yEJZK6ebYKc7lchudz+EPEFIvFgzr3TyIejx/JZDJPGPsVnIRrzK9Lx/kgYCCVSuVtm0yaIo//lgjQxPgTTPwl3WgRA++DzOULz3YdLDiAc07ZpMhtI+IZnTmh/HUjkUikqXkf9mifDae8S77CEc/GnHK8f0bcXSu0DBxZOGuZbBELDRLRxOAXqfcCzmez2Q6x6SAbxA2YOdyybLLlP8DlQqFwzI4XRFmlZvNbBrxO4BZcZLudtgODQrYj9a5S6zV8JN3QMb8D8WMihEXt82wsQBzbeyOm1Y6vAs5+EUHyO5Ic7Q8CagxT6610N5/PH9f+3cACzGghsVjsKLYl+B170Y73gfOKCdiTCFkp+IA6Q3T0sPbXir0IWcf5kQLnLbN8L9dquT/kqDaDr8Be7Q+KuoTQgU4RIk/bzoomKfiUpFO2fTuk0+mzjRRCjZIWIvPAtgo/mQuyArk/ZDvBS1WOiH9u39P2nSBnPjnjDdhaN42QO57NqZxaq1WLK9+CiMCx5RstH3izncBaQN6wqT1az8dOh8+Ruw7HPJtTuVtKfqC0RpSJCNjv2eXe4P0G3IBrHJln/KSAMP8O+pzydgt0/EbKF/W0++vN/g1e0MH7jlpvdgtR6QQduiz/vbQzRIgQ/wF+AjxIsALfCU9BAAAAAElFTkSuQmCC>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAZCAYAAABzVH1EAAACb0lEQVR4Xu2VO2gUURSGZ1kFn/iIy5p93dlFWIISlAVLFREbsUoZFSGFpWChIBZCSCFp4loIEhERGwUrSxHBQjBW4jaChSKIRRIsbISg32HvzJ490WRnsyrK/PBzZ85rzn8fc4MgRYoUKf5phGG4wTl3ayUScx2GNvdPgM9uLxaLJR6z1tcFGh2B32CTpCOMw8JarVZhfAS/i5hGo7He5v5O5PP5zZVK5Q7ffgVvwBb9HbZxMQgYL5VKe7RNmqbIzb8lAmT4/gyNv2Q1hsTA+yl6+cy41wYL1uG8b2xS5KIX8ZCV2Wb8y1CtVg8RO10oFMrW1w9ce5fMw8uRjZ5qvH9C3FUV2gaOKrynTFrEk15EeGQQM0rOY6G824AkoMa47+GCssmWfw+f1+v1rTpekGUWN/nnDGrPErgEn7LddurAXuEn5wU8Eax2QH8BcqdECJN6MrIxUXls77yYYR3fBZxjIoLktyQ5608C2dfUuiK1GCfk4NqYlcCE3rVCcrncFmzP4FfsDR0fQ2bPB6xZhAarupGaZ2jsNbwWHdzVsBYhizg/UGC/Mst5OT2g+yPLN4669pa7TcndNkCjLyGswEERIqO2M5tFCj4gaYe294lYiDQpd5QN0CCuaYVIH9jm4Ed/QXYg94dsJ3isyxHE/+1Ja0+CaGtRpwWnE2ytc17IpcjmOn+tua7JlbMgInAsxUblA29+JrAX6MMOzyc97OVyeR/5i3AqsrnO3dKMA2VpRJmIgGORXe4N3ifgF9ji17wrTuoRbgC/36B9Uc+Gy2/2BXjABg8M0rTM1KBudgU5WyOs0PGkq5oiRYr/BD8AMiCvNlHJLwgAAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAZCAYAAADuWXTMAAABFUlEQVR4XmNgGAUDCOTl5SWBuBeIZ+HCcnJy1uj6wEBWVtYPqOA/ED8EKsqDGgbCKUD8F4hPS0tLy6DrAwFGoOQUIF4sLi7ODRNUVFQ0A4q9Bxp2C8iWR9YAB1JSUiJABTtkZGRUYGIgxSBNIM0gQ5DVowCgIhugom4gkxEmBnImEP8C4iAkpZjA2NiYVUtLiw3GV1JS4of6MxhZHUEAMkgeErJlDEguIQigGnuA+B+IjS6PDzCCbJOHRNcsmCCQrQmMRjdkhRgA5D+oP/eA/AwTV1BQaAAa6oKsFgWAJIGa3gPxFeT4BLLFgWJbgVEojaweDvAkBEagrZVA8fkgNpI4BOBJCMxAsUWggAMaEIEkPgroDgDCC0YGIF/cVgAAAABJRU5ErkJggg==>