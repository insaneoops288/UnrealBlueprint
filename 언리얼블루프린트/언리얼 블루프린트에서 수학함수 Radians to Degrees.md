# **언리얼 블루프린트: Radians to Degrees 변환 가이드**

언리얼 엔진에서 수학 계산(삼각함수 등)은 주로 **라디안(Radians)** 단위를 사용하지만, 액터의 회전값(Rotator)이나 사용자 인터페이스에서는 **도(Degrees)** 단위를 주로 사용합니다. 이를 위해 블루프린트에서는 전용 변환 노드를 제공합니다.

## **1\. 주요 노드**

### **Radians To Degrees**

* **기능**: 라디안 값을 도(Degree) 값으로 변환합니다.  
* **검색어**: Radians To Degrees 또는 RadToDeg  
* **수식**: ![][image1]  
* **특징**: 입력값(float)에 약 57.2958을 곱한 결과와 같습니다.

### **Degrees To Radians**

* **기능**: 도(Degree) 값을 라디안 값으로 변환합니다.  
* **검색어**: Degrees To Radians 또는 DegToRad  
* **수식**: ![][image2]

## **2\. 사용 예시**

### **회전값 계산 시**

액터의 Get Actor Rotation 노드에서 나오는 값은 Degrees 단위입니다. 만약 이 값을 가지고 Sin이나 Cos 같은 삼각함수 노드에 연결하려면, 반드시 Degrees To Radians 노드를 거쳐야 정확한 값이 나옵니다.

*(참고: 블루프린트의 Sin 노드는 기본적으로 라디안을 입력으로 받습니다.)*

### **삼각함수 결과값 변환**

Atan2 노드 등을 사용하여 두 벡터 사이의 각도를 구하면 결과는 라디안으로 나옵니다. 이 값을 액터의 회전값으로 설정하려면 Radians To Degrees 노드로 변환한 뒤 Make Rotator에 연결해야 합니다.

## **3\. 요약 팁**

* **라디안(Radians)**: 수학적 계산, 삼각함수(Sin, Cos, Tan) 입력용.  
* **도(Degrees)**: 화면 표시, 액터 회전(Rotation), 에디터 수치 입력용.  
* **PI 노드**: 직접 수식을 만들고 싶다면 Get PI 노드를 사용해 ![][image3] (3.141592...) 값을 가져올 수 있습니다.
