# **언리얼 블루프린트 Atan2 노드 완벽 가이드**

언리얼 엔진에서 Atan2 (Arctangent 2\) 노드는 주로 \*\*2D 좌표(X, Y)를 기반으로 각도(Degree 또는 Radian)\*\*를 구할 때 사용됩니다. 게임 개발에서 캐릭터가 특정 방향을 바라보게 하거나, 조이스틱의 입력 방향을 계산할 때 필수적인 노드입니다.

## **1\. Atan2 노드의 핵심 역할**

Atan2 노드는 원점 $(0, 0)$에서 점 $(X, Y)$까지의 선이 **X축과 이루는 각도**를 반환합니다.

* **입력값**: Y (수직값), X (수평값)  
* **출력값**: 각도 (언리얼 블루프린트의 Atan2 (Degrees) 노드는 \-180도에서 180도 사이의 값을 반환)

### **⚠️ 주의사항: 입력 순서**

수학적으로나 언리얼 블루프린트에서나 보통 **Y가 먼저 입력되고 X가 나중에 입력**됩니다. (Node 구조상 위가 Y, 아래가 X인 경우가 많습니다.)

## **2\. 왜 Atan 대신 Atan2를 사용하나요?**

일반적인 Atan(y/x) 노드는 다음과 같은 한계가 있습니다:

1. **사분면 인식 불가**: ![][image1] 값만 전달받기 때문에, 점이 (+, \+)에 있는지 (-, \-)에 있는지 구분하지 못합니다. 결과적으로 180도 범위를 넘어서는 각도를 계산하기 어렵습니다.  
2. **Zero Division 에러**: ![][image2]가 0일 경우(수직 방향), 분모가 0이 되어 계산 오류가 발생합니다.

**Atan2는 이 문제를 해결합니다:**

* **![][image2]**와 ![][image3]를 따로 입력받으므로 점이 속한 **사분면(1, 2, 3, 4사분면)을 정확히 판별**합니다.  
* ![][image2]가 0인 경우에도 오류 없이 90도 또는 \-90도를 정확히 반환합니다.

## **3\. 주요 활용 사례**

### **① 마우스 위치나 타겟 바라보기**

캐릭터가 마우스 커서 방향이나 특정 타겟을 향해 회전해야 할 때 사용합니다.

* Target 위치 \- 캐릭터 위치를 통해 상대적인 ![][image4] 값을 구합니다.  
* 이 값을 Atan2에 넣으면 캐릭터가 회전해야 할 정확한 Yaw 값을 얻을 수 있습니다.

### **② 조이스틱/패드 입력 계산**

게임패드의 스틱 입력은 보통 X축과 Y축의 \-1.0 \~ 1.0 사이 값으로 들어옵니다.

* Atan2(StickY, StickX)를 사용하면 플레이어가 스틱을 어느 방향(각도)으로 밀고 있는지 즉시 알 수 있습니다.

### **③ UI 화살표 및 인디케이터**

화면 밖의 적 위치를 가리키는 화살표 UI를 만들 때, 적의 상대 위치를 ![][image4]로 계산하여 Atan2로 UI의 회전값을 정합니다.

## **4\. 수학적 배경**

Atan2는 탄젠트(![][image5])의 역함수인 아크탄젠트(![][image6])를 확장한 것입니다.

![][image7]반환되는 결과값(![][image8])의 범위는 다음과 같습니다:

* **Radians**: ![][image9]  
* **Degrees**: ![][image10]

## **5\. 블루프린트 구성 팁**

1. **방향 벡터 분해**: Break Vector 노드를 사용하여 벡터의 X와 Y 값을 분리한 뒤 Atan2에 연결하세요.  
2. **회전값 적용**: Atan2에서 나온 결과값을 Make Rotator의 Yaw 핀에 연결하여 액터의 회전을 제어하세요.  
3. **단위 확인**: Atan2 (Degrees)는 도 단위를, Atan2 (Radians)는 호도법 단위를 사용하므로 노드 이름을 잘 확인해야 합니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACsAAAAZCAYAAACo79dmAAACjUlEQVR4Xu2WvWsUYRCH7/AUxcJvDy53t/fVeIgiJ4GIWlkogoVpLCzsbIKIELWSiAQEiwhKkJAUClYRLSQQULARSaXl/QOKhYWNCDbG53f77vFmbm9vA4IH3gPD7s7MOzM7++7sZjIjRowY0SEIgkPIPLLgyY1ms7kt8qlUKtvRTfs+5XL5PvrdfiyfUql0GPtTrfXUOdZdM7kU60m1Wj0iB46njX26G0MnY2NjRY7XMfxGfsYUkSX5cWxfkE8Ev8x1QXrj1wWfO8S5ZNRZcu0jxgnkA7KueLputVpb5eDsS8gz5Fy9Xj+odRuicEd5jG0F2GAIyZL8pjqVz+d3WqMF3z3EeYlUrS2CWGeDsDmffT81juu3vm8sOM2q2GKxuMPoJ5GVWq22y9f3Q4VQ8FzGdsRDsYi5pnzIlHTk3cv5a+Si9e8Bpwnkl/ZbpKPj4+jecQx83wRy+C9R7ElrsKhIV+ya4qshyKT1i0UdJckqnZnRtSv0/SYKVQHKupzmKThfbQNthzZ5r2QSnkYP7m51p0ddoePWJwm3vvNYU5DF93EQdvdV9JKlhs7W3OJvyHlrT0JPhjXP/W2UhIoLwvGkfN/TruviAqxrPFnbIIJwzy9ymrM2i8vzgEd/l+Mb5Yy2X2oKhcJ+Fn5Vh61tEKx5SHcuWH0MWyjstmaxitY8dt1ta4Ra577oLWbRij4W1paEm9MvUiTrzGx9taI96s/4mA9Jfwhyi0WzVj8I1517Vm/oKTRCW8Btv1U752OJRhdyxtoGoNm6iExYgw/2R8jHRqNxIMZ2DPkRhHP+lLX/NUgwRWeuWv3Q4T6by0HCf8DQ4F7I+cxmvjz/Coqd04+L1Q8ldHUhzX/Af8cfQeKpafl9rMAAAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABIAAAAZCAYAAAA8CX6UAAABLUlEQVR4XmNgGAVDHIiKivLIycnVy8vLz0LGCgoKHNLS0sJAuhNZHKi2AygmgG4OCDArKyuLASX9gQpfAfF/ILsRJGFsbMwK1BgGFPsNxOeB7BhZWVkpoBQjmhkogBGoeArIICCehCRWCjSgDmQoimp8AKjJEoh/AvF1IJcRaEAZEM8gyRAQkJGR4QRq3AF1VTAQL1VSUuJHV0cUAGrOgRq0jmxDQABogCIQPwF5E12OJAD0nirQkLvAmGtAlyMaKCqCHCN/BIiLQAEO5IujqyEIQOEBdMUqoGYzUCIFhROQH4GuDi8AGQLUuB4UUzAxkEGgGATFJLJanACURoCaJgNtT2BASrHQAH8PTMk6CNU4ACiGgLZuBOIVQC4zmtwckKuAuJmBQJYYBUMJAAAjzEXYfNhWZQAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAZCAYAAAA4/K6pAAABDUlEQVR4XmNgGAWDACgoKBjIy8vPQsdycnKuMDWysrKmyHJAPROBWAFmAIeioiJQXL4PiP8D8R0g3wwkDjNASUmJHyg+DYh/AXEr0HBtY2NjVpg8GAAlNIH4LRB/BSowRpaDWnAEZDCyODpgASpaA3IF0IBymCDU9q1AHIysGCsAOjsC6o0TII0gMSB7MhDnAJmMaMoxAVAhyK1PgPgnEFsChRiBdCmIRleLCzACnT8f5AqgaxpAzsYILEIAqNEDqPEfFG9HlycIpKWlZaDeuAuPa1IANNF8A+I1QC4LujxBALQ0HRQGQFyELkcMAIX6UiD+DQxMG3RJggCY0sSBmq8D8UMglkSXHwWDFQAAdrJC5sHSvLgAAAAASUVORK5CYII=>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACoAAAAYCAYAAACMcW/9AAACRElEQVR4Xu2WvWtUQRTFd8EExSJBhYX9eruL1eIHsiIoahVBSROihRAL/wARWTBaSSwCARELQYJg4z9gIQHBQBqRVFrJthZCioBpREiExN/ROzpcH9m3UWEhe+Cw8+65c+bOzJt5m8sNMMAAuxt7qtXqzSRJngbyPF+v149J5Pd8rIm1Wm2vN0kDuU98X9huNpvDIUdexG5HY88RG419AvKlUukgSW/hFnwPz7RarSGJpj0z7Tm8pD7OIxX0LTPoLfpswq+0J1KKyFcqlZMalyKv0S4q5nJ+A4OLZvgJ1kNcg/G8iMlYnJ8V7EiB/h2b6JTXQR7v6UKhsN8LqWg0GiMYLZvhDcXK5fIB2i/hpM/vBfSflS8FvcJzn9Muw4U41hUq0ApdZiUSGcjI5/UKPE7DdbjG1h4JccY4RWxJY8X5XUEnVaet1yvQ4XW4ntvufckIraJWU4uA54xiVuSbnosUdICSn6dvy8/+b0GhDTxXzHsVjvucTLAiHzDj+/y+jmf/L2D+L+Sr0+31zKCouxjckyHtqzbzjk6tz90JisXiIfw+aFW1ul7PCl0R8+HujK8UFe2TdwL8z+L3DS7okvd6Fvy4x0KRAdp226Y/rpQAOyQX+D3sNQ/y7tguzXqtK+yr8xi+8xqxE/ALXOdQnfO6gNa2wT9SbMnrAdGp3+zpw0HyFRvgF1V00Hl+6HVRXy/nM0Z8A36m3Yo1gVfouDTvA9co/qjP/+/QtqYV2ldgdaZg28f7CjpErOQj/U/w2gD9iO/ESK/g24/f8AAAAABJRU5ErkJggg==>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAXCAYAAAAYyi9XAAABs0lEQVR4Xu2UsUvDUBDGW0RQEBS0Sts0SVsRHNQho7voIEh1c3BU3HRxcXDxL1DUTXQSdRC6OEnBTcGpLi5aEIqTICqCQ/2dydkYbNChuOTgI5fv7t737uVeYrHIIouskZmm6ViWtRHkm2aITSC6G+SbZXEEN23b3gsGmmKZTGYMwZdGgrlcrlNyOIEZ8gahWoI51HYRS8ozkUh0qB/Mk2+3Q7AWBPykxPGnwSNYBrPgDJQNw+j3rbHiqzsAJ/j74Bl/23Gc1rpi7HPRJKj81KFw/g2k0+lu3q9AEdF2zYM34O7Bu3L46+Apm82OKKeBhoIkDxNblWNVzttEReqU0zXApXLauW72y8IEPYvbrs2RtwVuQwRLPq7wZ0E6G4C/8OLzdIxrHYUIHisnQr8S5NsM4a/l8/le+DIFNyKk+WFH6t90Q8FUKtVD4JrAqQwCReNgyXT/PnJdDnXSZDrh7kCV+KgIyDXwDc25rmu5U10DhbpaPSjn/SqiAq8j+SEsgjdZyOus6N3bKniAm9LhUMAtWO4JfHGgJBv7JkpiG0J9wXsjPAVJuRK8xpUT+PMi+xf7AEeAnJygBJS7AAAAAElFTkSuQmCC>

[image6]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADYAAAAXCAYAAABAtbxOAAAC+klEQVR4Xu2VTUhUURiG76CFUdFA2eg4esYRsqRFMNWiTS5cFFSYBBGUm+iXiDYhBi1dB+JKimohFZUQ9E8LqUXgokUYrVoMCFFBbVIiSHte7znXMzdnMojAuB+8nHPe7/879ycIEkkkkUQSSeTvSktLSx/YG+eXvBhjRv7Xxt7+88by+Xwd6CTxAe3j+vr6+lUU1tjW1raeY21TU9Nau/cllQ+lWw00NzdnxUmRy+VW4D9bpbHIV9A5bgCfVg3KLX2xWFzW2tqa0Rq3nRMlw2EKDKgx1mnW3sAGZ1+0+lkwyfkSeCw7FSwbkjZwfgZK6M5yPsf+KxiikC2sH62/w5TiytcO7SE+r8Eh+OOKpZhejX3OF/45OMX+ARgGEzRonG0kGF2Xg5x1Zj/kJ7ZSw/mmDS79JvBB0ysUCmtsIeN2morZCfcDvNNUbdwFb8wbXAk0Wtsv4DLbWmen2HCvwAwYgKqxtvIddHaRUNgGFBfdhOwNajInfDs7gBn0Xfac1gp31BY9NxhJJpNZib4/7z1WlRrTowR/LGY7Bt5ks9l1zs7erPj32Bccz7mk2tz5F9FkMdqP0dN4oRLbWPwma+HuVCral9/ZqEHey63YnQafVLCxNyjxGivjKzamm0I5asLrv0Dy3sU25iWrWrSkio0+HAeVX0MFu9i/jDfwR43puYUcV1BubLs49ygupjEkBTdSpehIfBvWLuKdtPvD0oFh94VbqIFqjYmX3nEBV7/ZhDcVKUz4tZlrjOT97HcH8w1M47MtChCECTVpG2eH4+E2EuOaF3cWnLH788TZF4Rx9THSB6HH+Zrww1PSsIlxVe9se3v7argXYJILyXm2k+Kld5wkheMRFQwmwBXOe1jvKhn7exS408x/7h3Kbk6J4QZNWNAYPrdYH8UKuGHCX8B9FesaxraB8xP4b+A2GIXrZv0OPoMeDdnmjWAHr6fI58qfGvez8366KRVbZrQIIVGd8X6iC+jT1XTydQ13dHQsr/jzTSSRpSE/AeznHK6oXW2zAAAAAElFTkSuQmCC>

[image7]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAmwAAAAyCAYAAADhjoeLAAAE20lEQVR4Xu3dXeieYxwH8C3UhLzObHte/ttkKeXgT1IrDqY4IG8HajhQXpIcEAuRY3KilcW0EDJvSYxaWpQVJ0ukvBwoL0Ws1MRkfH977vu/u3uyzZ4d0OdTv+7rvq7rvq/n+R99u+7nef7z5gEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEBjdnb2qNFodN14PH61P/Z/0Ly/u9Oc3x+bgvm59/paoz8AADA1CWovpjYkeFy7fPny4/vj/wUzMzPv5j38mdo2rxfMMnbLYDA4uts3Tfm7vVZr9PsBAKZiOBxelrBxTbUTPGZTa/tzDpesfe7ChQuP7fcfrNoZXLJkySnVrveQ869Ty9rx9L21d/b01XvIelv7/QAAhyxB7eIEjd3tedprDkNgmz8zcXm1uwNZb0c/sGXegryGq3O8sNufuYuXLVu2KM351e5el/N1uebSajfhqXba1tR5+ldVtXMbRyxduvTkXt9+1bpNs71+7v1kjdUVQNtzAICpSMjY2Atsj6au7M45VLnfmwlfH2Wtm9LekvZpTf+6Jljtqeqr0JX2zgpsOT6R4/VNf+2a1bwaey3Hzalfu+u0BoPB0oz9ljq/zrPezblmeTtej3zT91TGv2/7Mv5Yzt9vz/vq82kZvy/1dOqG1Eup7ak72zm1xrgJiQAAU5OA8UvqvdTipn5O8DixPy8B54TOnH2qv9vUlfvd2Oyu1XpbU5+0Y2l/1d0pS5g6I333d8b3BLmS+6ztnjehq93xmpN5n3U/h1fzumvkfFM9Ps21v7d9ae+u+7fnfRl/sdndOzLtr/N+Bzn+WGGwnVNr1FqdywAADl0FoNSj3fPu+LQ0O1S3pn6okNb2V7v/SLTpvyL1UPf1HEhgq6CWeW93+/qBbeXKlcfVMdfubPvS/n2072PTOe01TdB7I/dc0J8jsAEAh8V4srO0utrD4fCsnG/ozymZc3vGHv+HeqQfnloVsmpO064dtn0C26JFi45ZsWLFqQk8H6RvR/fatv13gS19s53xBzL+cLVzvGPcPNqteW3g6sxdlfF7q908Qv3bENZXO2rt36vmd69pwtxc+AUAmIoEjE8r2CR4nJfjl/3xKagvCOxuw1OOf1RIS+jZWCEt7S31qLFCU/NlgR3j5tuW7ZcH8truyfGSClh13rnvs+2H/NO+qsY69d2o+dxaXZs1zm6u26NCV/rX1I5cjlvaEFavpVnzwe786htNPuf27bgJpjk+0/2pkAqP7X0AAKYmoePK1Oep7YfrG44JMW/n/r+mXkkQujzHXbVuM3ZR2ttS7zXn9eO99bm6J1PvpF4eTX7j7ILx3jBWu3Rtu8JVPRr9stuX+nDUfBYv7TPHvS9SNI9ov0l9nNpZgbH6K4CNJ7uOG7vzx5O/0fPjyaPazVnzhcw9qTdnzbjzUyIAAByEcWf3cDT5Juqupv+21B97Z05+ly59d3X79qeCWuqLfj8AAAcoYer1tp3Atn7cPFrN8afR5F9WzamdtMFgcHq3b39yj7UzMzOb+v0AAByEBLHn2p/7qM/H1U9z9Of8Gwlqpw2Hw3P6/QAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAALCPvwCrBCd3CAHgAwAAAABJRU5ErkJggg==>

[image8]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAYCAYAAADDLGwtAAABCElEQVR4XmNgGAVAICMjIyQvL38AiM8DcTFQiBFdDYO0tLSMgoLCKZCkrKysDlDhQyC2RFEENIkTKLgZZBqID6QloQpbURQCTcoACv4F0h7ICoH8hXBFioqK4kDB60B8VUpKSgQkJicnZwzkf0VRCOREAAX/A/EkmBiQHYQiBjIBZBIQ/wTipUA8C0qD+F9BJoMVwqwA4gOioqI8aGInlJSU+GEKXYAC/+BWMICtzQHi/yAPwsTgCoGC6UgKTwDxFWVlZTG4QqCP9YGCn4AafJEU/gAGuB9cEQiA3AA1oQrEB2qUB2oqY8AWdUBJM6DC+0C8HIjPYVUEA0A3coAC3tjYmBVdbqgAAL2DRz2w7t/QAAAAAElFTkSuQmCC>

[image9]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADoAAAAZCAYAAABggz2wAAACEUlEQVR4Xu1WO0jDUBRtaAUFFX9FaNOmKYEubsVBECcFRdwcBN0EBQdXV1dBQVQc3MRBcBMdBBdxddatSxEFQQepQx3Uc+oNpM/G/iCmkgOXd+999/NOkndJKBQgQIAAAf4AyWRyDTKj+lsRhmHsgktW9ZcQEG1BBESJgGhj0EzTHEPDfchBJUGvVcRF1MRK0HW9A/FLag1V2JPxnhDNZrNtqVRqHc2KqJfH+gIpiF6gLfpWqEaiqHftyCtCniD3kHfIg/hzkGnGe0IUNRbQ6MiyrG6xt2FPUce6CXu8PON38MEhbxFqOBaLDSD/FG9ukDoewAlXNacuovhc4rBnaxXGI02Dnuanxho8EJpe0pfJZLqgn7sewAVSS6MOYpOocQw1kkgkhqGfRaPRzvIMb4iWAQeb42GwtpMs9DvXA1RHhCRRa5kG9HnIVdNEm4UMkAvWpc3GkLzrAaoAb3AIBHJ2Puv6giiajUCeUXOUNhvDfjXkvjqBh2Jhf8L+5CtBBtwN4nppC9Ecr4ca6yXR0mcGubWHhRB9g+w5A+Uec2J+8gzOPRvYM43vKbtj+4ToB++tM5bwkqiGt9PHiel04lA9vK9Onw30TmNvQ/UTrMN6IRlKgnA8Hu/n6vCV4CXRuiGDa0X1NwLfEuWbAdFDrLq61wh8S1Tu248h1SiqEjXkNytZ59+Ln8Dzg8ejK9H/ji/DR74BMxcPeAAAAABJRU5ErkJggg==>

[image10]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGwAAAAXCAYAAADug6rPAAAFGElEQVR4Xu1YXYhVVRQ+l5mg6NdqnGl+zr53ZmIY8yG6GRjVg1jiQz5URDngS5CjBj6EBj1FMVBvYqRQgVREVkKF9KAECgMW9VAD1kAomKRDiQiC8xI4fd/da93Zd3nO9o5zG29yPljsvddae52913f2zzlJUqBAgQIFChQosJQol8vjaZqeplhbgaVFwMVaa6sDxtcgT1v9YoGYK5xzR1BWrQ3owOBehP0UZIYCv7e7u7tvDXxK0K+BTInPX9YHMbZA/ybKF1B+AHko6L9QdKD/xq6urtusgejr6+vHcz6HzznI35DvrU9i5sXko3ylWq3eROPg4OCdaL8H/csU1qmzQaJ8tJIwDGA9Yn0tk7oMuZRFGPSvQj4ZHh6+g22WaH+KyR4OfJ7lpCGPsk2iEGs/fTSpaD+v/v39/bfA962sBOQBsW5Gnx2QHyCzkLkswkgWbJPwX4dmicL6wMDAhsCthPHshN80xjJMBXx6GBv6vWyjvg22+7UD+q+Ebbu2FVE+WkzYaKVSeQQDvQvykcsgDLqKTGIw1GMifdD9zrq8ifTZl/gE1cBY0F/kitK22gjYNkLuC3VXQSeS9jiTKOP6I4swPO8N2CasHrpvSbrU2f9P5jP0IanQz7IO3/EwPuvUzXt7RPloJWEh8giTpJ+FrA71IyMjt0M3KT5rUb9sJ08ymFTIATQ700WusBAaO4cwzuUzVDtDPXRH1R8+m9Ges7mU+V4S//ZaYSHyCINuFHI+9fv7mkRWEN90tKdZ55g4eciY6auEncRK7i638AzT2FmEQb9bxrNHt3GeS1x56iPz/Qdjf6zeMZknjFt6251hIfII40Shf18SMEc/+DyJ+m+QZ+iD8kPa7LjQXgb9T84f6LUtlQkmeXqwXytchDBu87BdkDGf4zaH5+/SZJMM6L9zGfPlOGW8y0RVwpl4D4X10Fdh592ApSaMkAvEPkkAhReUbYlMQPpeQRiTCf3RvLiLgYsQRjh/Y+VlSsf8s9pi49K4LEN9DHbeDbgehMHWA/0h2L9K/dZYS0Iqt6k2JIw3wE2w/wI5qOPFSnuYxti4NC7LUB+DnXcDLGFI1oPOb1lNCf3DeIo8wrC9QO2mod+JZkmu9O86SULQt10Iq1/XOXa2nV9tJyDHh4aGlsfGpXFZhvoY7LwbYAnjWcAzQR4UldjZkUdY2V+R67crQQn6ddBfkPNgwnkCa2eaore3917ofnULTEAzkDldQRh0FczhtPnm0m9HjnE9mp0oD0Bm4bcq9HNyybJxY1gQYa1CFmF6ONMW+ipgmxBSxpgMjs3Y9W2d5GdAaFssNLZNLAmBXLQvnthmMJfNUq+9ZDaX7Mc8JDkXjCzYGA34jwjrQMz9zv89qP2pUKT+Rsib1lOhXrbKKWnyG2svkvGj3KZqQHvc+V9UD6guBtkpTnJVW5sFbnwpfM+k87e5GuT77iDnY3+dQXdICWaJ5xyGfBHsOvTZhf7Hg35XRZSPVhLGOHzLMiTcAnkGbHWezC8hY5jkO87/JdiksZCou5kQkkYf59/g885skzFIsj9O/R+TTMhOoOOsC/OiPvJr6pjzq/t1yEuQI+jbE8biLynop9D3m/L8t+GMXk6aRZSPVhK2EPAbhqsMz36OZdYHZOJXalV9zBveNPhCWN01gD+HRzkWCuvWgZA7wBP0YZl3xscQ5eN6EbaUQHJ3WF07I8rHjU4YV67+Pf+/IMrHjU5YMxeOdkMWH/8CEN7b4052HC4AAAAASUVORK5CYII=>