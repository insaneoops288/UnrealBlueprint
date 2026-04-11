# **언리얼 엔진 애니메이션: Two Way Blend의 역할과 활용**

애니메이션 블루프린트의 애님 그래프(AnimGraph)에서 **Two Way Blend**는 두 가지 애니메이션 입력을 받아 이를 부드럽게 전환하거나 혼합하는 기능을 수행합니다.

## **1\. 주요 역할 (Role)**

Two Way Blend의 가장 큰 목적은 \*\*"부드러운 상태 전환"\*\*입니다. 캐릭터가 갑자기 다른 동작으로 변할 때 발생하는 시각적 끊김(Popping)을 방지하고, 두 동작 사이의 중간 포즈를 계산하여 자연스러운 움직임을 만들어냅니다.

## **2\. 대표적인 노드 유형**

### **A. Blend Poses by bool**

가장 자주 사용되는 '이진(Binary) 블렌드' 방식입니다.

* **입력:** True Pose, False Pose, Active Value (Boolean)  
* **역할:** 불리언 변수값이 참이면 A 애니메이션을, 거짓이면 B 애니메이션을 출력합니다.  
* **특징:** True Blend Time과 False Blend Time을 설정하여 상태가 바뀔 때 얼마나 빨리 혹은 천천히 섞일지 결정할 수 있습니다.

### **B. Blend (Alpha 기반)**

가중치(![][image1]) 값을 사용하여 두 포즈를 정밀하게 섞습니다.

* **입력:** Pose A, Pose B, Alpha (0.0 \~ 1.0)  
* **역할:**  
  * **![][image2]**: 포즈 A가 100% 출력됩니다.  
  * ![][image3]: 포즈 A와 B가 50%씩 섞인 중간 포즈가 출력됩니다.  
  * ![][image4]: 포즈 B가 100% 출력됩니다.  
* **활용:** 특정 수치(예: 캐릭터의 속도나 조준 각도)에 따라 애니메이션을 미세하게 조정할 때 사용합니다.

## **3\. 주요 설정 파라미터**

1. **Blend Time (블렌드 시간):** 한 포즈에서 다른 포즈로 완전히 넘어가는 데 걸리는 시간(초)입니다. 값이 클수록 전환이 느리고 부드러워집니다.  
2. **Blend Type (블렌드 유형):**  
   * **Linear (선형):** 일정한 속도로 블렌딩합니다.  
   * **Cubic / Hermite:** 시작과 끝을 부드럽게 가감속하며 블렌딩합니다.  
3. **Reset Child on Activation:**  
   새로운 포즈가 활성화될 때 애니메이션의 재생 위치를 처음으로 되돌릴지 여부를 결정합니다.

## **4\. 실무 활용 사례**

* **전투 상태 전환:** 캐릭터가 무기를 들고 있을 때(True)와 평상시(False)의 걷기 애니메이션을 Blend Poses by bool로 교체합니다.  
* **데미지 표현:** 캐릭터가 공격을 받았을 때 잠시 동안 '피격 포즈'를 기존 애니메이션 위에 ![][image5] 값을 조절하여 살짝 섞어줍니다.  
* **상체/하체 분리 블렌딩:** Layered blend per bone 노드와 결합하여, 하체는 달리고 상체는 손을 흔드는 등의 동작을 만들 때 두 포즈의 영향력을 조절합니다.

## **5\. 요약**

Two Way Blend는 단순히 "바꾸는 것"이 아니라, \*\*"어떻게 자연스럽게 섞으며 바꿀 것인가"\*\*를 결정하는 노드입니다. 게임의 시각적 퀄리티를 높이기 위해 적절한 Blend Time과 Alpha 제어가 필수적입니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAwAAAAZCAYAAAAFbs/PAAABBUlEQVR4XmNgGAUjFCgqKorLycn5KigoOAAxB7o8HAAlJeTl5dcA8XogOwJIVwPxE1lZWROQvIyMjDQQq5KnAegMoJz8FSCeZWxszAo1gwXIXw503g6gQk4guwbItoFJzAGZBsSKUMVgAFRQDhT7BLTRA8ier6SkxM8AFNAE4rcg54A0I2sAikUD8VeQYqCmcJgpvkDB/0CBdGTFyHJAvBnkLLAgkOMJEgRJoqmHafgDNMweLiglJSULFLwNxDlIahmBfCcgvgEzTFpaWgaoVgQsC5W8D8SrgXguEJ8A4mIVFRVRIL0NaMMpUGiBQhPJUAZmZWVlMaBJwiAbkMVBJiMF95AHAELKQyZBhU/lAAAAAElFTkSuQmCC>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGYAAAAYCAYAAAAI94jTAAAFXUlEQVR4Xu1Za4hVVRQ+l1HKXk7WODiPs+fOTA2W0MBgIZgUFCWRRvljagR/VdCfolBrfkUiykQKgybIRJRIDwyCkkIFX0FBA0Uw+ifxgSAJEkQJKTp+39lrnVlne87MZaTrBOeDxdln7bVfa6291jr3RlGJEiVKlChRokQI59wC0GBHR0dj2Ef09fXNrlarzei/NeyrBU1NTXd0dXXNR7Mh7CtRjAqMsgF0trW1tc12tLS03BbH8efoGwddRfsJ2z8VMOZx0L8y/kBzc/PtocwMRwXO+Aj2Pgz6EPRMVKNztbW1zYH8S6CdoCE4dk8oMymg7D4M/IfEdthPoG+jyzFcLeAYjuUcYd8MRwX6WAc6jL1XcY578NxNRTOChMIWnZ2dcyG3H7SB0QLG7UX7GOiFUDYXYtVdWPwMnpfxXBrKMHyhby9pOqGMtwxjr7S3t68I+2YyxGH/sDpBuxO809DD01Y2BOTWQ+5nPO9WHt4HQMeZEqxsLiD4POh9LPQ2nuOY6NlQRjZzzk3T42WT5zhP2DeTwfPSCKAFyuvp6bkT70dxlo/xWjHiKWgMGgU6/cTy4ZiLwf97SgdlMobgV8gj7aK8cdBAKCcef4lP5XGzoOW83nznk0bFFb4/Mhs2t+0Q1rkX749RTsflgR7FubmvsK9eMPvOGIZhiWeh4u1tsEDfQtCF0DDxRMqY3MEhsBbCq9mmspy/Metz5Og5qcfToPQY8N4DnQJtAY04f1V/QN+6SIyDUNkqMr+AvhEZJlKGiAftOiwMwN9OORyqnwdDewzPffUuGowBigyT4VuoAYoME/IzoFIgtIsLyXtiGBdY03hOml8g+zIVF0vuQPudSCoV50PjBdBCkaXMVdAI85nwkg1yTV2HyRS8nZjrCBOnyCUhlE4QFYQNAmNehMyZWglzjmIv3eE8Fs5HBCp/OoZJdBkaQM8d8lOIErYh1j2qPAxaCt7lcJAqxxmDoT2IcONiH/5SI0gfb8Q4aLmMp8x5KOI+lZG1WEInMjKOBqWR+5WHNR4C7y/wXlVevSDh9ISbhmF4LuogR5eTGwYCT1IJHJxDmcorzskvglng7wEd0ltHOB+mLjLRmduWkRFjpaHRyGUKBOeNXFjC/5coMkAR36LIAEX8BFJffxZeZS4ii4WKzuQXhfGo9CbFUo2AxpjoTX5JZczB9uB1Fnlm7YxTOJ+3krmUlwcxLOeoibj3qb5DognHKzLMUVZodoBCo0xoADUMaNDyE4D5Zl5okE1zEye0zjaeTIU1YuLNqiQNR7b0w/sS0EXQW1PJYL5+KSK2ykG49ojKGSPvZpty3d3dd2m/hYTVVbUS1l4Jp5kXzhMizgnVPD/ex0DDypOfq5wa2xivpujDr1gq6lfcmth2EHKTfhQFJR5ibwWUuwjP7ZF4uWyacTTJCdwU2l+C951J3td9v1ie86FqgEUB3r+PJ5J88sXN+UV+CZ6bdI56gVEFa5/VMxLMy+Cd556Uh/YHoot3lYf9ro59oVEVVsX5n71+Uv3oZH9ysBAV84BO4ny5e8n0s/0RPPQW5xf9HfQtDPWwDEmuuSw8yivrfDk8bMraBvC/oMK1GiM4h4xjmBhSL4NsL+g38D4VI73ufCgbRftreqTOUU84X5ScxB5ewf7WOP+zymuRd6AE4L/hvM7Sn1t4LozZAd5B9D/nvFFY+veqzI2iwo9Bm3dM7uB1bmBIsldWIb8RXccnTz4wMyVwzi/Yydp5c9QT+vFMmuzDOAcVfnBj3Cqca1kNee3GEOfkjhI3GfDeRhhlyPkE/pS9SSVuEhhSYJC1zv+/kBCvaChXosT/GtcA5S/v1RQB5RwAAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGYAAAAYCAYAAAAI94jTAAAFYUlEQVR4Xu1YW4hWVRQ+PzPRvcZsGpzL2f9caugCCUPKhEZBURIVpA/WBD1EBb0Uhpbz0kVEUUwYNWKYiBTpglFQUYgP5ohSCkk04oNSihAJEQQJKfn3fWev9c86u3Ocw4i/I5wPFnvvtdfal7XWXnufE0UlSpQoUaJEiRIhnHNzQMPVarUl7CMGBgau6O7ubkP/VWFfEbS2tl7X29t7C6pNYV+JfFTglFWgkx0dHZ22o729/Zo4jj9GXw10DvUHbf9UgM4DoH9Ef1dbW9u1ocwMRwXBOB9rHwG9C3o0KhBc0KlC9mmxZxMDGoF9N+z3QuHghvAABvmbxHrYT6BvdZbjioA61OUYYd8MRwX2WAH6Dmvvxj5mo9wOGmUGCYUtIHOvmwxIJdqYjp0anZ2dV0N4GyY/gfIsygWhDD2Mvq9Jhb1twFMG3X+7uroeD/tmMiRgf7c2Qb0HvOOwwyNWNoToHgYdAR2C/Fso54RyuYDwk6D1UHwdZQ0DPhbKyGJ+c9OMeOi/Rn2OE/bNZHC/dII1aH9///Voj2MvH6BZMeIpiGM2hfxC4GUM5c9wj3SJ8WqgoVBOIv4MS+VxsaBFPN5ss6RTe3p6bovMgs1p2415bkb7fsqpXhb4yODYXFfY1yiYdaccw0cM9wI6gH3MsjoWF+QYKC7HAM+wTmPRMXRQhhwjpx7xdCgjBry3Qb+C3gGNgYZAe9G3IhLnIFV2iMyPoC9FhhcpU8Sddh4+DMDfQjkYZinoQ9QnUO5s9KPBOCDPMSl+CHHM56CtoKNonwC9wasjlE2BRoHCNk4k7cQxLkhXWfcLZJ+n4WK5O1BfGclLxfnU+AfodpGlzDnQmC5KFs2HRj1t8jIFbxRj7cGpu1HkkhTKIIjOkzag85RsvBBhzINYS184joXzGYHGn7ZjQN/rPMwQWOcP3GPuw0GMsBmX8ULlYZAF4J1llFpZNY4zDkN9GOnGxT791Z0gfTwRNdAi0afMKSzwVpWRufhiSWREjw6lk5cqj89L8P4C70XlNQqSTo+5aTqGNg5POeSHZd+Dll8HDPMQjeDSTzml1MsrzrhfBM3g7wDt1lNHOJ+mTsPp95jTlpIRZ9VTo5FLPRCcd3LuE/5iIs8BefwikH3XQMvCvohpAh0fhUeZk8hkoaFT94vCRFT9JEFmFtoHQBO86M39UpcxG9uBZjN5Zu5UUDh/byVjKS8L4liOUYi49tx0MgkNvDzHjPOFZhUU8r1DO/wsfzsSqGNYWvkE6FiWlRpk0VzEMS6cPBPJNFgLBlyrRtJ0ZL9N0B4EnQa9OpUMU5Y8IjbS6TL3mMoZJ29nnXJ9fX03aL+FpNUlRQlzP4GguSkcJ0Sckaq5f7QnQCPKk99VTp3tJm2ZcozzqawWfs/xK5aGOoRTE9sOQk7SfhkwiRB7KjDYXSi3RBLlsuia3glcFOqfgveNubz/9/1iec6nqiE+CtD+Np685JMvbo4v8oMo1+gYjQKzCuY+qXskeC+Dd4prUh7qG8QWbwqrGet9D+35KkOboL0HcrvUPjrYn1QWomHuUCXnn7tnTD/r7yNCr3R+0qOgr+CoeaKSHPNYXjhV/6zlc3jEXHj8P/QJDW6fiBxD9Jgm1mmUQXYu6CfwtoqTXnY+lR1E/QtGpI7RSDj/KPkl9v+4nnX+a/6lyLwSwX/FeZstVh7XC4yD1oOec94++6bzSysPFeZMe++Yu4PHuYnHtZrxq4Y6WXzy5AMz9QTO+IOdzJ01RiOhH8+k830Yh5D93Ae9Jc6nwyl/fl4Q4oy7o8QlBqK3BU5Z5/wF/rA9SSUuEZhS4JDloFElHtVQrkSJyxr/AVdT6D0Xe/KPAAAAAElFTkSuQmCC>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGYAAAAYCAYAAAAI94jTAAAFCUlEQVR4Xu2ZW4hcRRCGZ9iIV3S9rEv2cnpvRqKCC6tCIBEFRfKgEQ24mjz4EvRNURJ1n8QgyooKi0EIK16CeCGCoKKoYFAhgguKEH0xoCEQFIIgMWBCMn7/6erZns6Z2ZkImSycH4rTXV1d3af+7q4+M5VKiRIlSpQoUaJECufcSmRmZGSkN20TpqamzhkdHe2n/by0rR309fVdND4+fiXFnrStRHNUIWU7cnBwcHAobhgYGLggy7J3aashJynfFrcvBfrcivxr/b/s7++/MLVZDhgaGjqfRflgs4VbBPXhnR9AdiKzLOyrU5uWINhTdPxHonLaLtD2bBFx7UB91Fc+0razGQT2Moi4j5i8wdz/Qn5HVqZ2RRgbG7sE2y+Q7Tot8DNJ+Wfk3tS2EMbqLgY/wPM4z7WpjY4v2j6RnM5Rpl1G3xPDw8N3pW1nM4yYDcz7Bjs12iYG+yew/Z7npUFHfRPyi1JCbFsIDO9BXmACT/Ks4ejO1AbdGG2H3GmueJvkIflJ25YLiM+b7RIjMkSK+sR6CL4R/ZElF6iSMYYfkEeGLXg1sZra2Yo/pmfQaYLIeo6py1XXU6SyhVdRrQa7aLftYZwrqN8iu9CvCFpR8q15pW3dQifEYLMaOZwSky2mjNYLHIOtGG9WWcESMSKowE75pb7iRSjl19E9g/yGvITMO79Vv6VtW8XI4TgYNJsfkI/MZg75A7tr43F0MUC/Q3a81LQFYx/Pz7t9aeiEmEBAM2JSfQMUFIx2KTFZPSfGJWxGK76eX7DdosBpB6E/Qfmpil2DnT8aDyOrzVY2J5F55TPT5RPUmGEcXcfR7cTX10qcZpcfoVoElWgXpqDP/dgcaFfwucBcJlI/rdAhMXksUwLCe6f6OiwIr3DWrQs6Oq1FdzztFILjIsIoz3DcuMwff3USrE07ooast/6y+ZNAXBVsbCxdoXMb6ydCRfJ00DHG9ej+RvdQ0HULnRCj91IMCmLZmhgMblcQLICpNNy8soL8YliBfjeyJ+w6wflj6qgSnfzIX2pjZNWPxsiu4YLgPMlNr/BnEp0Q04yAZvocdr9+J93KGtAGTgPdkF8CLEHvV3vQZXYbQfYp0Uf5pW4j3xoD2U11hXTR2A2Lwvm8lfsKuiIYsfLRlmjuOjVSP63QITH5KZMSEIhBZmJ9DpSPjRQcDTZpDbw/3LOjlayA9eL4+RCkcBzFVz/qa5CjyONL2eBv2i4RL9uLaOz5YBeR/LbKspuYmLg4tMewY3Vju8LYG/SNkvpphVbE2M9VLpAdLb62Tp9q5gP1I7smixsE20l748HjXUFwr+O5o2KrPLPrtQKsuiZF+X10n0bJ+5Tvl1jn/FG1SZcC6p9li0lec90m/2a/hudzwUc3YMQU/vKB/kWLxdNBx3w3Z/6iMWqqqvM/e30X4qMPm3XO/6RQM1FgrglOnL/uHovaVX6NFXqu84P+inwMUTdZlzy/2MALNmldh+eia20P+vcU8HAbE+TD+ik/zYZVhu0k8hO6t4ykR5w/yhYof6gVGXycKdh33jfIEbcYG+Xmg8z14WBH+VHnY1b/uUXvxbxfRfcV7Xc7T4qu/pPB5v+iqo/BOO9EuWOOao9eIN6yAfYb0Sl66ewDs+EKXPALdj52kY9lgqo+uCFoI+91c6d5rWNkBbmjRJfB6u2FlFnnE/gd8U4q0SXoSIGQrc7/v5CLtmhqV6LEssZ/y5XSvX2FWZAAAAAASUVORK5CYII=>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADMAAAAYCAYAAABXysXfAAADl0lEQVR4Xu1WTUhUURR+g0a/lFE26My864xT0A8UDAWCRUERLmrRSrBttAwStVwWURQVSG7EiJLoB4OgoogWEkGLhCJwl1ARREEEQUKKTt/33rl23vWpg81GmA8O797ze++555wZz6ugggoqmAvGmDpQd0NDQ40rIwqFwpJsNpuEfJkrKwW1tbWrGhsbN2BZ5crKjQQuchb0JZVKpbWgvr5+he/7dyErgqaw3q/l8wE2+0B/xP5FMplc6eqUFThgAYF+k7h25QRk5+IuWwpoQ1v6cGVlRTqdXo4gA7jEZ3wn8G12dVhakD0hLaTM+JqwncxkModdWVmBIEdAl3DIUywFBD7k6oCXg+zrQjML+y7a048rKxvYkAjyAH2RkYBFUJurJ5kd1/1iwoHRghJaxz2/TEQul9uEbcLqqVcdQpz12O+lnrWLAwcNffNcrmxWwKADjo9yzQDyMl0xeuyX6cwyCVjfAO8M6CPoCqifiQC9gqzTkwuhjFOi8xb0SHR6QN+gt1XH4XAAv5d6uHQr6CbWI/g+n3Nw0BEUBzgyZR9cxjilFNcv0D3GYPJik1if9mTkmrBsf4A2iy51pkD97E/hBQOHMW0cjn7w+uDrJV53jegF5c3Eeeq1IxDDa2jI3ZYHg2bwJpgNrWsd6kti3Y1SMHxFfXCRMfNFUIvYU+c7LrLR6kgsjutAR+yYBCam1fIQYzt4v8A7bnkzAGcHaChBXYpMLMlspF8E1eAPgobs6xImLKExJGqn7hetIxecLlulFxkSJkzMrD8XHp8QCneQqbzmm7ChP7mBjdMvFtKko5RbHnTWYv8GNMJmV/0yrUPfjAEaxLaaPBU7kkgT9mHgy/IigPBk3LMph6M8KHlOv9TgsBesY1sq+rcD+ybQGKh9Ph2WkwySq0yUxO63eioxt7mmXj6fX23lCXH+Dq/jW6aFvNhrcVpHns4+DrQN315PsimlUrQ1zj7E+j54T1UDz/h90TwTllEbBwP2z/x/jc6zdtK/6Dfhez5wwEYH4yeFQnS2xQYw4WgdV3KuryMTS/G9DPoAeozL7RKToF/88F/DMIeGCUdvjxqhVeDf4yHtFCPoQ+zYbxeZBPKhuwP0HrxbcrETJiyzYawfcuBYH/+DBH/gdB+pXujBtorlomvdgjZxfPLkRzMybmP+mQex43yUDX5MLyxKcBCwPEzYxAf1iy0q8LlxiQ5QnyWUxR5Xr4IS8Rfakjdn5V5V8gAAAABJRU5ErkJggg==>