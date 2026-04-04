# **언리얼 엔진 블루프린트: Sin 노드의 역할과 활용**

Sin 노드는 입력값(각도)을 받아 ![][image1]에서 ![][image2] 사이의 값을 반환하는 삼각함수 노드입니다. 게임 개발에서 이 "부드럽게 반복되는 수치"는 시각 효과와 움직임을 구현하는 데 핵심적인 역할을 합니다.

## **1\. 핵심 원리**

Sin 노드의 가장 중요한 특징은 입력값이 아무리 커지거나 작아져도 출력값은 항상 일정 범위 내에서 **곡선을 그리며 반복**된다는 점입니다.

* **입력 (Value):** 일반적으로 '시간(Time)'이나 '도(Degrees)' 단위를 넣습니다.  
* **출력 (Return Value):** 항상 ![][image1] \~ ![][image2] 사이의 부드러운 부동 소수점(Float) 값을 반환합니다.

## **2\. 대표적인 활용 사례**

### **① 공중에 떠 있는 물체 (Hovering)**

가장 흔한 사용법입니다. 아이템이나 보석이 위아래로 부드럽게 넘실거리는 효과를 줄 때 사용합니다.

* **공식:** Get Game Time in Seconds → Sin → Multiply (이동 범위) → Add (현재 위치)  
* **결과:** 물체가 특정 지점을 기준으로 위아래로 왕복 운동을 합니다.

### **② 깜빡이는 불빛 (Flickering/Pulsing Light)**

조명의 강도(Intensity)를 조절하여 숨 쉬는 듯한 효과나 경고등 효과를 만듭니다.

* **공식:** (Sin(Time) \+ 1.0) / 2.0  
* **팁:** Sin은 ![][image3]까지 내려가므로, 값을 ![][image4] \~ ![][image5] 사이로 보정(Remap)하여 불이 완전히 꺼지지 않게 조절합니다.

### **③ 크기 변화 (Pulsing Scale)**

UI 요소나 마법 진이 커졌다가 작아졌다가 하는 연출에 사용합니다.

* **결과:** Set Actor Scale 3D에 Sin 결과값을 연결하면 물체가 부드럽게 팽창과 수축을 반복합니다.

## **3\. 핵심 파라미터 제어 방법**

단순히 Sin 노드만 사용하기보다, 아래와 같이 수치를 곱해 조절하는 것이 일반적입니다.

| 조절 목표 | 방법 | 예시 (블루프린트 로직) |
| :---- | :---- | :---- |
| **속도 (Frequency)** | 입력값(Time)에 곱하기 | Time \* 2.0 → Sin (2배 빨라짐) |
| **폭/거리 (Amplitude)** | 출력값(Return)에 곱하기 | Sin \* ![][image6] (왕복 범위가 50유닛으로 증가) |
| **최소/최대값 조정** | 출력값에 더하기 | Sin \+ 1.0 (범위가 ![][image7] \~ ![][image8]으로 이동) |

## **4\. 주의사항: Degrees vs Radians**

언리얼 블루프린트에는 두 가지 타입의 Sin 노드가 존재할 수 있습니다.

1. **Sin (Degrees):** 입력값을 '도(![][image9])' 단위로 계산합니다. (![][image4] \~ ![][image10] 사이클)  
2. **Sin (Radians):** 입력값을 '라디안' 단위로 계산합니다. (![][image4] \~ ![][image11] 사이클)

일반적인 Get Game Time in Seconds를 입력으로 쓸 때는 **Sin (Radians)** 혹은 일반 **Sin** 노드를 사용하며, 특정 각도 값을 다룰 때는 **Degrees** 버전을 확인하고 사용해야 합니다.

## **요약**

Sin 노드는 \*\*"시간에 따라 부드럽게 반복되는 모든 것"\*\*을 만들기 위한 마법의 도구입니다. 움직임, 빛, 크기, 투명도 등 매끄러운 왕복이 필요한 곳에 적용해 보세요\!

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACgAAAAXCAYAAAB50g0VAAABoUlEQVR4Xu2WvUvDUBTFW1pB0UGHWgrNt4tzdFDExclBB3HQv8DFWV0dOrnoJBSXDoKIi6s4dBQ6KYKrgtDJ0SI61N+1efqIVJumRYQcOOS9m3PuO3lpkqZSCRIkSPCBYrE4ZNv2jmVZ5fC5H5DBs47nHtbhpu/7A2FR16D5KE33ON5wfIVNxpWwrg3Spmlu4bnj4iakwPiK2iHDbEjbHWTXaLjguq5J8xn43GlAtA58xL+taoZhLFNrSC9d2xOwkB8lILoN2XF8S6qmesCSru0JughYQf+Gb07VtICX+Xx+WNfHRpSAsriEEL34VJ2xa7UelhrjMd0TG1EC5nK5EbTVcEDmBfgQsKB7YqOvAe0WDiiWO+TipzlAXwOCjOd544HgVxJiUDcLogQEWbRnsMGrZVoVmU/CJ1iVi9ANsRExoIQpWe1fM8dM05o8Pmg6KzvCIidMM6Fz8sQ24ZGqyQ5xMRfwVH3e8O6juZW7+eWOAe23JIt/o9odQuwyf4Erul8+cdSu0Z2jWWNc55ZP6Zo/h+ye4zjzhFzt6R+FBP8F72wqlqbBS1I7AAAAAElFTkSuQmCC>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABkAAAAXCAYAAAD+4+QTAAABQElEQVR4Xu2Uv0rDUBjFG7oIOgg1RvPvJmTqVhDc3BxcfBMnHaTgqzi46QOoi4ODL+DQxUULFlFwEbpWf19syvWzEa6IUw4cmu/cc+65Sdu0Wg0a/Be8LMt6xphLvfATyKzBM3IjeCyz9oipNzU9wQkcak8diqJYxX9Lvs/Y5voI3kRR1PlijOM4yvN8E+MyPHEpwbsHH2QPmdk8Zn4UXXtncCnxfX8J77VQrpV2zl4LOlPCpcQ69bySIVzXmRIuJWmabuAdw6sgCBZFs0rGsq4zJX5TIplKa0pqga8LX2XTOV+86F2dKeFSEobhCt5BTclA1nVG0OYWTzGM9IJ16gme7UpPkmQL7UU+ZeaQO8xv1TyD9VN8t4l+WHmmJ7xAu+PtYKy4h34A7+E+fDaf/3bP8vwNeDQJB9j99s5q4IoP+ox4w0TKm50AAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABkAAAAXCAYAAAD+4+QTAAAAjklEQVR4XmNgGAWjYCgARnl5+e1ALIkuQTFQUFAwAOKVQMOfA/FDmlgiIyMjraioaAa0SIBmliCDUUtIAgNvCVBCE4inAfEsInApMCVxoJsBAngtMTY2ZgUmQ3GQAkIYlFTR9cMAXkuoBehhCTPQgqdKSkpy6BIUAzk5OWOg4V+B+D8yBoqXo6sdBYMHAAD1aDHi7/u9ZwAAAABJRU5ErkJggg==>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAWCAYAAAD5Jg1dAAAA3klEQVR4XmNgGNFARUWFT05OboK8vPw7IH0SiK3R1TAoKSnxAxXsAeI5MjIynLKysm5A9msg7YeiUEFBIQIo8QloijFUiBHIng8UOwEyBKaIAyiwFYgfArEkTDNQYTmQ/xtI24AFQJJQRaeBgoJoCv8DcRFMwBjI+QrEB0RFRXmQFPqCFAJtXEg7hUpAgedAfFhdXZ0XphCoIB2kEORWmEJBkEfksfv6PxAHwcRAPp8EMhVkOj4xkKChPCTqYkB8ZWVlMSD/CpA/AchlhCuEAWlpaWGQJ4CxoYZVwcAAAGi+RqhBZojtAAAAAElFTkSuQmCC>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAWCAYAAAD5Jg1dAAAATUlEQVR4XmNgGAVAIC0tLSMnJ+eLLg4GCgoKBkC8Ul5e/ikQ/8WpUEZGRhqo0AFomjCQrsCpEBkAFZWPKsQJiFXICIyZWqDCMHSJoQAAhxkWLjlcysgAAAAASUVORK5CYII=>

[image6]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAUCAYAAACaq43EAAACDUlEQVR4Xu2VvUscURTFd4lWRqIxy+J+ze4KLhKsNlgEK1nsJKAWwvaJVQotjJ0gQvwHtBHFzkDKNIYUURuJlaBYBbQSCy0ksTPxd3fem9y5TGFhFTxwmXnnnnPv+5rdVOoRj/gvEQRBb7lc3iWWS6XSJOMm75+IBSNNkxshDolztB+z2WyH0SQh5iMuWqw0Js6Ivz4oulWtVp9pN/yEM76WsWiY3NdMJvNU6yysL5qsa3zsVrlJwTp0WptlEmj2yW34nOjgrvFMaa2F9emENP4uz1hCAWOD/B+ec55zPtmpzwzblDwG69OJ+zSeC8JjaHpONf5ZqVSyWq9hfTrRasyWveH5RbalWCz2Gc2aFCA35jneu+EOgvCiVbVew/p0QhqfEiv5fL6HCczzfsUqhrxGzt4WkEslEyZ+u3uRCOuLQNEuEu/97ZRtQ3wC/0Mm4jQP39hCFZRL0RDuwRvncrkXJI4lSfF3wqmCkYH3phsn3eq9Wq3W6XkL62tBDJA7JI8KhcKgcKV/l+aSGHDmATde8t4kTn4c8L/lcr5SupgmAuQsq/2Qch94EP7S3BKLngNtFFw15z6N5gL+pdOIdyYIP5/oE7O+CDJLhOvEN9lunjc8l+v1ervWsSPPKbItRYJw62Ul41pDfhjulzTz/gRfbPVpfhb7EUxy7kWdMHgiF0l09/yD8Ih8HMPoHbdKuJ08VRZ0AAAAAElFTkSuQmCC>

[image7]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABYAAAAUCAYAAACJfM0wAAABbklEQVR4Xu2TMUvDUBSF001RlCI1lKR5SRpcRdpNZzcdREHQ2R/g4i/oXpwF0f8gLg6OaldXBycHaYeCk0M9p8kLJzF1F3rg0nvPfd9N097nOHPNNVNJkqwEQdA3xowQH8i3y2dmqcQ952wcx6swHxBXvu8v0kP+2Wq19gsTKpSxOQdml+y0GYbhMYoxntSxAPJreE8ErVclsspBNbJsLGDAHeId0bRdNC9Qf+NzR6CChM05iixfuZkNHcCoaxPeBHGukMqyylHTwVEUbaI5Rjw2Go1lgU6ywT2FVJZVjiLL6R0kX+XB8Pc4GK97o5DKsuXBZP/n4DqaA1O9FRPEgUIqYX9vBYXGpUlvTWybVR6Wv4v6zHXdJT2nZ6xnky3ECAdOpfmKuo+0xhob4MJ7y94iX0GyyrXb7XWytuZtOYIxRPRMumr3eut4ZeHdmvSqd61PVjn8Jy9kpe84nuet8YdHHBIoNP+QcvgyG7BqP/pmfzaefOArAAAAAElFTkSuQmCC>

[image8]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABYAAAAUCAYAAACJfM0wAAABpklEQVR4Xu2TO0sDQRSFd1FBURAfcTGvzW7AwsIHq4IIlnY2aiHE2loCYmdnZSNiJaKQP2AlNilEK01rKygIFkELIaCF6HfjjplMHqh1Dhx27uPcvTNzx7JaaKEhksnkaCqVunRd91Poed48btvMqwe0e2he4BPrazhXDlBkRhwUnsBswx6T4tgb1SVq4ft+L7lH8Xi8S+xEIrGAXbQQd7I4hbloNDqoBNgl+Ai9SplaoF+lqUBz2dgn0q2D+C48gqyKsr4SH0mLmqgKYVNncFj3o9mSbzuBA/imF8G+CH+WqUiqIQXhA7o+3a8K1wWCe/gOZ82YArsdJ/4aiUR6dH+zZiQo3R4GQdBhxhTkbMkpmYUbHh+dkO/m5cbNmI4/FQ7HJ8/49JsxE78qLFvGsU3irto+6yxc+kkyIJdGvOA2mAqBTXBTCmtnKtNyzMBPqyTWU+SsO47TrXzk7OPzla18arEMP9zwOWuUJ1oWNZn3SXLWlJ1Op4fw3ZoCkwU1o/JksXOwKJ2rQtb3bp/hDszwaG74nmvx/yMWiw3IhcEVLn8El/0Fgbx3MmPKJyQAAAAASUVORK5CYII=>

[image9]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAgAAAAYCAYAAADH2bwQAAAAZ0lEQVR4XmNgGFZAVFSUR1lZWQzIZEaXY1RQUMiUl5dvAuJoIJ4jLS0tA5cFCmgCFUTA+FJSUrJAsWy4Ajk5OV8QhvFBVgEV1AIVcoEF0E1QVFQUB/IzYHwQQHED0LTpKG4YBYMGAACA/RAQvk2jjwAAAABJRU5ErkJggg==>

[image10]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB0AAAAXCAYAAAD3CERpAAACWklEQVR4Xu2UPWhUQRSFdzEBRUGIrGv27+0fgj9gZMVGQQgiNhERC8F0FjYBEYuAWAgSrIQgdqaxEAtrQdBCK0XBThQrI4EgISmEBX+I8Tuzczd317xGsRD2wuG9c+bMvXfem5lMZhCD+O+jUqnsBc+TJFkES+Buv0dRrVZ3aQysyMec2Xw+v9V7Wq3WMPonxhfAC96P+PEQDBwET2q1WiLebDZz8Ndgj/dRcIwEH8Al6KZCoVDG8xZMOVtWjZRKpRERFVQD4KjzhKIzYA3cctptcNV4sVjcQdFXaHPQIWkkmo7zpIUol8v74cvGiSz8Pt7HNLKlq5LsekrRGec5B19l8nHTaKQkT71e321abKRt3GnLif9y+geNRmOnnuIU2IzhEc+TnoNFEtS7E3+PsKqUoms8J7zeExjGwTdrgvdRMB9xATwFzyIft3m5XG5b1L+sZ+spOu31EKzockz0HVwxHXML3tZEMGfNoE/CV9iAh8Vd0XmbG33pRRWaSPFj2nH2413RHxW3C+M/1bF4AB3646IWmKYwzWY6R8CK6iuMOo999gU18NdFtSKMH1lt0SVPK9pWY27DbbiRGL8YBO1ahDdgiX9zwBnD6vT0K0gp2j0OSeeobVT0Z8WOG2dsO8JLurjnrjNt/TtJ72baB//Mc9I0HSm0r1wIp0yzS4TXrDgLyeN5B26YFgLTGOJ78BCcTzp366rtUgu0M0lnVbrBrsV3NbaeLBNupUM0d1O51IDyanHeE0IFovksmFCH/R6FVqJxkp3W3ds/bkGuE8oVb6uepgbxT+MXMSLKc0k6LSUAAAAASUVORK5CYII=>

[image11]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABUAAAAZCAYAAADe1WXtAAABnElEQVR4Xu2UvUsDQRDFE1BQFD+JgVzuI5c0gqAQEATtFLXQQmsbGxsrBQu19R+wCGIjFkELO8FCLMTWSoiNWAQJFjapk0L9TdzFdSFcUgp58Nidt7PvZneHi8U66KCBRCLR77ruhIz2WtvI5/Pdvu8fwFIQBOeMX7AYhuGgndsyMNj1PO9EzFV8qIyv0+l0r50fCcyG2fwI68xXRMPIIS7DT7R5e08kHMcZZfOTqmxHNLlT5vdK27T3tIRMJuNT0YI+qlG9mC7rvFwuN0C8D0+bsADHf50N0AFzLNZgKZvNjokmyfAVfsAKrMN3NVYo5I3xmQInbb8G6IBbEqokTEusrugGfZEwLh/A5DKZTPYxnqH3WBZ/odqrTOKU1uTYqVTK1THr2/BI5tKCWm+GOF/eIzGwFzSkd1l+0F0RZSrHkgqKWmDjBve7aiZhsoT+Im2n4uammK3BY6PZ5SMFDGaNtC60C3glcxGY38ndGjk/YGHdV68pr6heUl5Z4tDIm4E1qtsytKr8L3TcNuSV4ZCpcbIRhripdfBP8A2KcWihFqak2wAAAABJRU5ErkJggg==>