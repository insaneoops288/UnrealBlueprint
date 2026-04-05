# **Get Unit Direction Vector 노드 가이드**

언리얼 엔진에서 Get Unit Direction Vector 노드는 두 위치(Vector) 사이의 **순수한 방향 정보**를 추출할 때 사용됩니다.

## **1\. 기본 개념**

이 노드는 From 지점에서 To 지점을 향하는 방향을 계산합니다. 결과값은 항상 \*\*정규화(Normalized)\*\*된 상태, 즉 벡터의 길이를 1로 만든 상태로 출력됩니다.

* **Input (From):** 시작 위치 (예: 내 캐릭터의 위치)  
* **Input (To):** 목표 위치 (예: 적의 위치)  
* **ReturnValue:** From에서 To를 향하는 방향 벡터 (길이 \= 1.0)

## **2\. 수학적 원리**

내부적으로는 다음과 같은 단계로 계산됩니다.

1. **벡터 뺄셈:** To \- From을 통해 두 지점 사이의 거리와 방향이 포함된 벡터를 구합니다.  
2. **정규화(Normalization):** 위에서 구한 벡터를 그 벡터의 전체 길이로 나눕니다.  
   * 공식: ![][image1]  
   * 결과적으로 방향은 유지하되, 크기(Length)는 정확히 **1.0**이 됩니다.

## **3\. 왜 'Unit(단위)' 벡터를 사용하는가?**

방향 벡터의 크기가 1이면 다음과 같은 수학적 이점이 있습니다.

* **속도 제어 용이:** 방향 벡터에 특정 숫자(예: 500)를 곱하면, 해당 방향으로 정확히 초속 500의 속도를 부여할 수 있습니다.  
* **회전 계산:** Rotation From XVector 같은 노드와 함께 사용하여 특정 대상을 바라보게 하는 회전값을 만들 때 필수적입니다.  
* **예측 가능성:** 벡터의 크기가 1로 고정되어 있으므로, 거리에 상관없이 일관된 '방향성'만을 로직에 반영할 수 있습니다.

## **4\. 주요 활용 사례**

### **A. 투사체 발사 (Bullet/Projectile)**

총구 위치(From)에서 조준점(To)으로 총알을 날릴 때 사용합니다.

* Get Unit Direction Vector로 방향을 구한 뒤, 여기에 '탄속(Speed)'을 곱해서 Velocity에 입력합니다.

### **B. 적을 향해 밀쳐내기 (Knockback)**

내 캐릭터(From)가 적(To)을 공격했을 때, 적을 내 반대 방향으로 밀어내고 싶다면:

* 이 노드로 방향을 구하고, '밀려나는 힘(Force)'을 곱해 적의 Launch Character 노드에 연결합니다.

### **C. AI 시야 및 추적**

AI가 플레이어를 바라보게 하거나 플레이어 쪽으로 이동 명령을 내릴 때 방향을 결정하는 기초 데이터로 활용됩니다.

## **5\. 주의사항**

* **동일 위치:** From과 To의 위치가 완전히 같다면(거리가 0), 수학적으로 0으로 나누는 오류가 발생할 수 있습니다. 언리얼은 이를 안전하게 처리하여 (0,0,0)을 반환하지만, 로직상 두 위치가 겹칠 수 있는 상황인지 체크하는 것이 좋습니다.  
* **순서 주의:** 반드시 From이 시작점, To가 목적지여야 합니다. 순서가 바뀌면 정반대 방향이 나옵니다.

**요약:** 이 노드는 "A에서 B로 가려면 어느 방향으로 가야 해?"라는 질문에 대해 "정확히 이 방향이고, 길이는 딱 1이야"라고 답해주는 도구입니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAG8AAAAiCAYAAABRA4fSAAAEkklEQVR4Xu1ZW4hOURQ+YxC5X8Ywl3+fuTCZiJq88SThARmU8iRlqHkQ4YEybilJmjxpClNTkpIyJSkTL8qzFGmGRENeFKkp4/ucteff1v+f8Y8Z/+Wc89Vq77XOXnvvs9Ze6+x9tudFEKlU6oaWFRBlxpg+LUwA1NbWNpBqamqW2lKMVa7b5gMYu87OiYSFtAGyfpTzdNsEmSgTKgrAafu9Ai2kBAkmFkhD10KoE7RHtx8PKioqZiJ6TmUZ6zf5vj9X6yQIAb5p02HMezDcdhhuI8rvKB/W1dUZlHvBf9A640R5Q0PDIvQ7DDoKWkLCeKtQ9jY1Nc3SCglCgE3AipaWlilS30qjwmlt9jn4q+nWEwbuGL9h0bS4QvIYe5orKzpg4gOY6Hotzzeam5unwli7LY95PSfZnRwNifqutMZvo+8z40yldBL66LQ86mdYYjyfJbMB58US1IhFtc5Lb5rKxcltjFgKELVNqB9hWxKeH+M8qVNfX78M9cONjY2zRf/vYCdQOhtC/aABrVNoYE7fQF1aTuB95sMot5hOUV8J4x3SbXKFGJ6LgClzOSPefQ5ZO6gV471CeRL0mmlVdJ/wm1ldXV1DO6K+BWUnnY1yEOVBeXYRdM7hn8GRc9xxQsHzETruhvJNkq2jk24T5PsBrVNoyLwyooovDfkjzP+2lYF/UVVVtdBtlyMmQ/cOqMcEm5SnsE29fcg+wV+B/ao5HlM62gwyurhwgLOeRKH00Yt2m1C2gn7afvgeoB0O/9ZIpP4zeNBEMUnLCw0aDS/Xx92gfgb5I+OkU7Zx+bEAOmuhO2R5GN53Ho9AjL/c8nBcJfg3qE522gyA7nAenI+RVGx5VxfDdFg+chCjntdygk4F9Xiy4rnJAX9ENcsJGOe4cSIkDGjT5S4kE6TYt6rND6Zc2al+BbVSngq+qV9tO6ZUylCuTmuHo4wdQmEn6+4D2dkVzR8LCzHqZi0nIL8Kusu5y9HiRs7fDwX00wt6r+UudOQQMu4DT2zHbzDaXJK0yij9YiRS/eCb+tHqot7FIwj0L1hZKDhBvhxWxRrjpCJ2zkGYz7WOC0lLGQfYUegyP8q6n4mGHxyg8/FrijvKrCkZ77mA5DkBQHv5zhFD81bm8lkh29xNrMOo7ZjEdS+dbuw5atSzDPvI4iD+fTjNaNYE+TauRN1PgjECH/5argyJnj7mW/vMBL+bht32+QTGvp9QJqX0mRtO6zCBo0bC2wR5eSQXJyhCVFZWzjDB9vqPKCNPuSvLhrC0OQqdx2JZrPuZKHCDomUFxP+9jDWyrTVOlDGVgh/mdw+Gvq3/KhQL7KUnfzLYUoyVj41KBky+L2O5IcEAd42cZ5xIGkoF56nH/LOu9YoU8buMNcGx4CUGe4fyM+iElJ+MHCYTJBgzkBkOuLxkhWwU2ctYzGGLlpUEXOfJn4vYXcZGwnn8TxnHy9iSd16cL2NL3nkajAYTk8vYKDpv2GSJqihexkbKeXG7jI2U88SosbmMjZrzYnUZGynn5QI/QpexsXNelJA4r4RRss7jfaOWxQ06rRK/AOrKLdBp8vSCAAAAAElFTkSuQmCC>