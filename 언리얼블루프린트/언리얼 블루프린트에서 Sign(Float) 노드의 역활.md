# **Unreal Engine Blueprint: Sign (Float) 노드 가이드**

Sign (Float) 노드는 입력값의 부호에 따라 특정 값을 반환하는 수학 함수 노드입니다.

## **1\. 동작 원리 (Return Values)**

입력값(![][image1])에 따른 출력값(![][image2])은 다음과 같습니다.

| 입력값 (Float) | 출력값 (Float) | 의미 |
| :---- | :---- | :---- |
| **양수 ( \> 0 )** | **1.0** | 입력이 0보다 큰 경우 |
| **음수 ( \< 0 )** | **\-1.0** | 입력이 0보다 작은 경우 |
| **영 ( \== 0 )** | **0.0** | 입력이 정확히 0인 경우 |

## **2\. 주요 활용 사례**

### **① 이동 방향 결정**

캐릭터의 속도(Velocity)나 입력 축(Input Axis) 값을 Sign 노드에 연결하면, 캐릭터가 어느 방향으로 움직이고 있는지를 단순하게 \-1 또는 1로 얻을 수 있습니다.

* **예시:** 이동 속도가 500이든 10이든 상관없이 "앞으로 가고 있음(1.0)"을 판별할 때 사용.

### **② 조건문(Branch) 대체**

특정 값이 양수일 때와 음수일 때 서로 다른 계산을 해야 할 경우, 복잡한 Branch나 If 문 대신 수학 식에 직접 Sign 결과를 곱해 로직을 단순화할 수 있습니다.

* **예시:** 데미지 \* Sign(공격력) (공격력이 음수면 힐이 들어가는 로직 등)

### **③ 캐릭터 회전 및 뒤집기 (Sprite Flipping)**

2D 게임에서 캐릭터의 이동 입력값이 \-0.5이든 \-1.0이든 상관없이 왼쪽을 보게 하려면 Sign 노드를 사용해 Scale 값을 \-1로 고정할 수 있습니다.

### **④ 벡터 비교 (Dot Product와 병행)**

두 벡터의 내적(Dot Product) 결과값에 Sign을 취하면, 대상이 내 앞에 있는지(1.0) 뒤에 있는지(-1.0)를 아주 빠르게 판별할 수 있습니다.

## **3\. 주의사항**

* **정밀도 문제:** 입력값이 아주 작은 소수(예: 0.000001)라도 0보다 크면 **1.0**을 반환합니다. 완전히 정지한 상태가 아닌 미세한 움직임도 양수로 처리될 수 있으니 주의가 필요합니다.  
* **0의 처리:** 입력이 정확히 0일 때만 0을 반환합니다. 만약 0일 때도 1이나 \-1 중 하나를 반환하게 하고 싶다면 Select 노드나 Compare Float을 병행해야 합니다.

## **4\. 블루프린트 예시 구조**

\[Input Axis MoveForward\] \--\> \[Sign (Float)\] \--\> \[Set Actor Relative Scale 3D (X값에 연결)\]

*설명: 위와 같이 구성하면 전진 키를 누르면 Scale이 1이 되고, 후진 키를 누르면 \-1이 되어 모델이 반전됩니다.*

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADEAAAAZCAYAAACYY8ZHAAADFElEQVR4Xu2WO2hUQRSGd0kERcGIxnWzj9mHhRHBYlGJiIVYGBALRRACtqJsY2IQgoVNQLEQBRsJKsISESGNAYuAgo2gYCUKEtAQsBJRNCCC+v25MzeT2V2iIGQj+8PPznnM3HPOnDlsItFGG2200fLI5XJlY8x1eNMxn89fKhQKXaFvy4JgVxN4Gp6Dz+Hecrm8GVNH6Ltc6Onp2URhT6ZSqbWhbREIfgTH86G+FaC4iO9LsVjcGdpi2NuYxHlfaGsBdBLbA3UJ8W0IjTEymUwWp1ldW2hbbmSz2QyxvSOB24jJ0B4Dh4M4/mTZGZiS3NJpbK/xuaqe5Hc78kN+Z7AVfGf0B+BLOEFBclz/ftZP4RtYrVQqq0TWQ3Aajkp2+zluC7oRFdWe16+zFBt8YaKhc3zhix60Ef4K9WyocPA1bH3wG3yMfFnJUKE1yJNqRflqGCDfQr/VRAPiA7wiPyXL+i0cYnkWnoB7kL/Cfvc91oPwI+yVjE8X67v6ti10uru7e53zj6EgFAz8EdrQXdA7EWXXlXqVS6J74g5lVB9BPuNak3PvO1/5yBdOwTFbgCr8DvvseQ17v5GuDhhLJqraq9CmaaCqE9Ap7HMEusvZdKg+kLAtyLpX/etaEw4Evrqdac7apiTQPUJ+ViqV1suHb6Vkh2Nun2D+5j3YgBpBFa8pSf/h63aUnO9o9XXjUAmaqE2mrE/FRC0Sj3R328ZLXmikq4Ox7wEOhjZBgSsB41VdIIGL3MwOz1Vo1hID+ob2SLY3q/aMR7q+b6LEKk5n9XW6RfDfg3+gD7UQ9jm/6q497P5hvQfpm4xDJTZuolYqSMF6DL6Hac9Hyc/ftpJVh1h9XJBCNBQO2T0R8gvvYVo9uchoYasWvof5drATadz+TfFbc8I9atbH4Gd41O1nPQpn3Sjl27uRP8GaAlYRFI8dCDVckvJBf0fvyZ3zx9BBDcca/610E07wbnW+cshdBLkx0fxBdih5+TmF1r7sdEuc8+/QpJVWFgj+sG2lamhbCegg8BskMePx3pJ/m9v4T/AbkgzwYnNFYOkAAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,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>