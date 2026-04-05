# **언리얼 엔진: Degrees to Radians 변환 가이드**

언리얼 엔진 블루프린트에서 회전 값이나 각도를 다룰 때, 수학적 계산(삼각함수 등)을 위해 **Degrees(도)** 단위를 **Radians(라디안)** 단위로 변환해야 하는 경우가 많습니다.

## **1\. 블루프린트 노드: Degrees To Radians**

블루프린트에서 가장 간단하게 사용할 수 있는 전용 노드입니다.

* **노드 이름:** Degrees To Radians  
* **입력 (Float):** 변환하고자 하는 도(Degree) 단위의 값.  
* **출력 (Float):** 변환된 라디안(Radian) 단위의 값.

### **사용 예시**

주로 Sin, Cos, Tan 같은 삼각함수 노드를 사용하기 전에 이 노드를 거치게 됩니다. (언리얼의 삼각함수 노드는 기본적으로 라디안 값을 입력으로 받기 때문입니다.)

## **2\. 수학적 원리**

블루프린트에서 직접 계산하고 싶다면 Degrees \* (Pi / 180\) 연산을 수행하면 됩니다.

## **3\. C++에서의 대응 함수**

만약 블루프린트 로직을 C++로 옮긴다면 FMath 라이브러리를 사용합니다.

\#include "Math/UnrealMathUtility.h"

// 도(Degrees)를 라디안(Radians)으로 변환  
float DegreeValue \= 90.0f;  
float RadianValue \= FMath::DegreesToRadians(DegreeValue);

// 반대로 라디안을 도로 변환할 때  
float BackToDegree \= FMath::RadiansToDegrees(RadianValue);

## **4\. 주의사항**

1. **삼각함수 노드:** Sin, Cos, Tan 노드는 입력을 **라디안**으로 받습니다.  
2. **역삼각함수 노드:** ASin, ACos, ATan 노드는 결과값을 **라디안**으로 출력합니다. 이 결과를 각도 단위로 화면에 표시하거나 회전값으로 쓰려면 Radians To Degrees 노드가 필요합니다.  
3. **Rotation 변수:** FRotator나 SetActorRotation 등에서 사용하는 값은 기본적으로 **Degrees(도)** 단위입니다. 따라서 엔진의 표준 회전 함수를 쓸 때는 굳이 변환할 필요가 없습니다.
