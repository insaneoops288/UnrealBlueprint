# **언리얼 엔진 블루프린트: Acos 노드 활용 가이드**

Acos 노드는 \*\*Arccosine(아크코사인)\*\*의 약자로, 수학적으로는 코사인(![][image1]) 함수의 역함수입니다. 블루프린트에서는 주로 입력된 수치(코사인 값)를 바탕으로 해당 \*\*각도(Degrees)\*\*를 찾아낼 때 사용합니다.

### **1\. 주요 역할 및 특징**

* **역할**: 코사인 값(비율)을 입력받아, 그 결과값에 해당하는 **각도**를 반환합니다.  
* **입력 범위 (Value)**: **\-1.0에서 1.0 사이**의 실수(Float)여야 합니다.  
  * 이 범위를 벗어나는 값을 입력하면 NaN(Not a Number, 숫자가 아님) 오류가 발생하여 게임 로직이 깨질 수 있으므로 주의해야 합니다.  
* **출력값 (Return Value)**: 기본적으로 **도(Degrees)** 단위의 각도를 반환합니다. (0도 \~ 180도 사이)  
  * *참고: 수학 라이브러리에 따라 라디안(Radians)을 반환하는 노드가 별도로 있을 수 있습니다.*

### **2\. 가장 대표적인 사용 사례: 두 벡터 사이의 각도 구하기**

게임 개발 중 "내 캐릭터가 적을 정면으로 바라보고 있는가?" 혹은 "두 화살표 사이의 각도는 몇 도인가?"를 계산할 때 가장 많이 사용됩니다.

#### **계산 공식**

두 벡터가 **정규화(Normalize)** 되어 있을 때:

![][image2]즉, **내적(Dot Product)의 결과값을 Acos에 넣으면 두 벡터 사이의 각도**가 나옵니다.

#### **블루프린트 구성 순서**

1. **Vector A**와 **Vector B**를 준비합니다.  
2. 각 벡터에 Normalize 노드를 연결하여 길이를 1로 만듭니다. (필수)  
3. 두 벡터를 Dot (Vector) 노드에 연결하여 내적값을 구합니다.  
4. 내적 결과값을 Acos (Degrees) 노드에 연결합니다.  
5. 출력된 값이 바로 두 벡터 사이의 **사이각**입니다.

### **3\. 노드 옵션 주의사항**

* **Acos (Degrees)**: 결과값을 우리가 흔히 쓰는 '도(0\~180)' 단위로 출력합니다. (가장 많이 쓰임)  
* **Acos (Radians)**: 결과값을 '라디안(0\~π)' 단위로 출력합니다. 복잡한 수학 공식에 바로 대입할 때 사용합니다.

### **4\. 실전 팁: 안전한 프로그래밍**

Acos 노드에 들어가는 값이 부동 소수점 오차로 인해 1.000001 같이 미세하게 1을 초과하는 경우가 생길 수 있습니다. 이를 방지하기 위해 **Clamp** 노드를 사용하는 것이 권장됩니다.

* **추천 구조**: Dot Product \-\> Clamp (Min: \-1.0, Max: 1.0) \-\> Acos

이 방식을 사용하면 입력값이 범위를 벗어나 NaN이 발생하는 현상을 완벽히 차단할 수 있습니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABoAAAAXCAYAAAAV1F8QAAABvklEQVR4Xu1SPUsDQRS8Iwp+oRGJl+/c5YJ/IKCNhY1iCkG0UNBGwY9eMCgiWFj4CwSxsbAVLGxFsBEUbQQFLVS0EAtJHURnzreX5YgprO/BsPtm5r23e3uGEUYYYYTRMFKpVE82mx2zbXs8k8m4QT2fz3eBH6GH3qDOQO2Q9IjD35dOp1t9sVgsNudyuQ2gAtMmTHPYvymTpt9DXyS4J0dN9UE+AcygfhrrCfAMJPxBEJZBVNFgWAoSmskEv4r9A3y2VmOTA1aYw9ON/aXSeWPkR/4gx3EsJHfAWSwW6xCfyZPJ6gCvwL5qogKzDkSjxzucZVntIjdh+Cw8US9DMghDlUW1FrXg94b+jbUc1MCvUwNKxm/jXckf0W+rUCh0+mbVqMGg8l+DlAbMM+dtsP8Ujrh2XbdXmb0bAYdITb0RA3xJitbraNuilfjj4LAD5Pk+2K+B/8K6pBdMchhFpBFy2MfVaXCYBegfeM9+VcOm5FgrPfhGT4YcVn6GC/hGVQ3DBDEFoQLcyiOf19HfMfRY8EKOGg0y6Ao4BfaAG+g7+u+vR4S3kJt4NwtEIz2STCbb0DzKoVhbAnoY/4sfRl5+bn+PW+AAAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,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>