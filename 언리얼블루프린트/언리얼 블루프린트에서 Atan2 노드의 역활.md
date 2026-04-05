# **언리얼 엔진 블루프린트: Atan2 노드 완벽 이해**

Atan2 (Arctangent 2\) 노드는 수학적으로 두 개의 인자(![][image1])를 받아 탄젠트 값이 ![][image2]인 각도를 라디안(Radians) 단위로 반환하는 함수입니다. 게임 개발, 특히 언리얼 엔진의 블루프린트에서 방향과 회전을 계산할 때 필수적으로 사용됩니다.

## **1\. Atan2 노드의 핵심 역할**

일반적인 Atan 노드와 달리 Atan2는 \*\*사분면(Quadrant)\*\*을 인식합니다.

* **Atan(Y/X):** 단순 나눗셈 결과만 보므로 결과값이 ![][image3]에서 ![][image4] 사이로 제한됩니다. (오른쪽 절반만 인식)  
* **Atan2(Y, X):** ![][image5]와 ![][image6]의 부호를 각각 확인하여 ![][image7] 전체 범위를 계산합니다. 결과값은 보통 ![][image8]에서 ![][image9] (즉, ![][image10] \~ ![][image11]) 사이로 반환됩니다.

## **2\. 노드 입력 및 출력**

블루프린트에서 Atan2 노드는 다음과 같은 구조를 가집니다.

* **입력 (Inputs):**  
  * **Y:** 수직 위치 또는 벡터의 Y 성분.  
  * **X:** 수평 위치 또는 벡터의 X 성분.  
* **출력 (Outputs):**  
  * **Return Value:** 입력된 (X, Y) 좌표가 원점(0,0)으로부터 이루는 각도 (라디안 단위).

**주의:** 수학적 관례에 따라 입력 핀이 **Y가 위, X가 아래**에 배치되어 있습니다. 순서를 헷갈리지 않도록 주의하세요.

## **3\. 왜 Atan2를 사용하는가?**

1. **0으로 나누기 방지:** 일반 Atan은 ![][image5]가 0일 때 에러가 발생할 수 있지만, Atan2는 내부적으로 이를 처리하여 ![][image4] 또는 ![][image3]를 안전하게 반환합니다.  
2. **전방위 감지:** 캐릭터가 타겟을 바라보게 하거나, 마우스 위치에 따른 UI 회전을 계산할 때 ![][image7] 모든 방향을 정확히 찾아낼 수 있습니다.

## **4\. 실전 활용 사례**

### **A. 캐릭터가 타겟을 바라보게 하기 (2D/Top-down)**

타겟의 위치에서 내 위치를 뺀 '상대 벡터'의 Y와 X값을 Atan2에 넣으면, 캐릭터가 회전해야 할 각도를 얻을 수 있습니다.

* Target Position \- My Position \= Relative Vector  
* Atan2(Relative Vector.Y, Relative Vector.X) \-\> Degrees로 변환 \-\> Set Actor Rotation

### **B. 마우스/조이스틱 방향 계산**

화면 중앙으로부터 마우스가 위치한 방향의 각도를 계산하여 화살표 UI를 회전시키거나 스킬 발사 방향을 결정할 때 사용합니다.

### **C. 미니맵 아이콘 회전**

플레이어의 위치와 목적지 위치의 차이를 이용해 미니맵 가장자리에 표시될 화살표의 회전값을 계산합니다.

## **5\. 팁: 라디안을 도(Degrees)로 변환하기**

Atan2 노드의 결과물은 \*\*라디안(Radians)\*\*입니다. 언리얼의 Set Actor Rotation 등은 **도(Degrees)** 단위를 사용하므로 변환이 필요합니다.

* 블루프린트 노드: Radians To Degrees 노드를 연결하여 변환하세요.  
* 또는 언리얼에서 제공하는 Atan2 (Degrees) 노드를 직접 사용하면 변환 과정 없이 바로 도 단위를 얻을 수 있습니다.

### **요약**

Atan2는 \*\*"X, Y 좌표를 주면 0점을 기준으로 해당 좌표가 몇 도 방향에 있는지 알려주는 노드"\*\*입니다. 방향 계산이 필요한 거의 모든 로직에서 사용됩니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACoAAAAYCAYAAACMcW/9AAACRklEQVR4Xu2WO2tUQRiGd4lKxELR6MLezt6aLEYJK4KiVgoRO2MhmCI/IISwELUSUwQEkRRCCEKK+BckEFCwEUmlWG1roVgIphFBC+PzZmfC5MvZ3DxIwH3hZWa++/nmsptKddFFF/8Zoijqh7PwWcBmvV4/5G1KpVIvssnQplgsPkJ+LIy1DQ7gM27yKM5cuVw+IwPGK0Y/qdxr3prkcrk84wSK3/BHTAHpQqFwDt1n+J7gI6yzkhu7rZAmzwn8L8K3cFWxtG40Ggdl4PTz8Dm8Xq1WT8lvQxS+JoOypQAbFG2kKe4uH7CQyWSOWOVuQZwh15RPsOzlahjrV6FtLDCaVqH5fP6wkQ/DxUqlcjSU7xWKQ7xl5YJjkpHzOPMX8Ka13wSMLsCfbOtpL6PT55G9ZoxC27+FCnSFLiu2GgGHrV0s1Em2eImteai1K/JN0kUKxFV12nodgRY5R1P2PG4FCq24L/0Kb1h9UtAFito3W7lWwl3cEVyAVd1sq0sKLsdjujjF+FL5/C7uGNlstg/HL+qs1SWEHoq6T/wHKpj5bdfVll4ea9wRBLiE0+L6Q5ss1p45PfL+7QyfRRVtHTqCIPdwmrbyTnAX8BpjzeoMNhXpoW13x23JPo2x8LceXrW6TiBB023dR/xzVu+B/il8V6vVTsboBuH3qP00Xrb6ddD+sxh9cwk9V0g8YG0t9FHY/pI/80aM/paJ+0E/lV7P+onR6wgMhTEShY5MXKH7CnThDmxa+b6CLhGdnEnqf0AX/wp/AHSGpb3pjDriAAAAAElFTkSuQmCC>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACsAAAAZCAYAAACo79dmAAACjUlEQVR4Xu2WvWsUYRCH7/AUxcJvDy53t/fVeIgiJ4GIWlkogoVpLCzsbIKIELWSiAQEiwhKkJAUClYRLSQQULARSaXl/QOKhYWNCDbG53f77vFmbm9vA4IH3gPD7s7MOzM7++7sZjIjRowY0SEIgkPIPLLgyY1ms7kt8qlUKtvRTfs+5XL5PvrdfiyfUql0GPtTrfXUOdZdM7kU60m1Wj0iB46njX26G0MnY2NjRY7XMfxGfsYUkSX5cWxfkE8Ev8x1QXrj1wWfO8S5ZNRZcu0jxgnkA7KueLputVpb5eDsS8gz5Fy9Xj+odRuicEd5jG0F2GAIyZL8pjqVz+d3WqMF3z3EeYlUrS2CWGeDsDmffT81juu3vm8sOM2q2GKxuMPoJ5GVWq22y9f3Q4VQ8FzGdsRDsYi5pnzIlHTk3cv5a+Si9e8Bpwnkl/ZbpKPj4+jecQx83wRy+C9R7ElrsKhIV+ya4qshyKT1i0UdJckqnZnRtSv0/SYKVQHKupzmKThfbQNthzZ5r2QSnkYP7m51p0ddoePWJwm3vvNYU5DF93EQdvdV9JKlhs7W3OJvyHlrT0JPhjXP/W2UhIoLwvGkfN/TruviAqxrPFnbIIJwzy9ymrM2i8vzgEd/l+Mb5Yy2X2oKhcJ+Fn5Vh61tEKx5SHcuWH0MWyjstmaxitY8dt1ta4Ra577oLWbRij4W1paEm9MvUiTrzGx9taI96s/4mA9Jfwhyi0WzVj8I1517Vm/oKTRCW8Btv1U752OJRhdyxtoGoNm6iExYgw/2R8jHRqNxIMZ2DPkRhHP+lLX/NUgwRWeuWv3Q4T6by0HCf8DQ4F7I+cxmvjz/Coqd04+L1Q8ldHUhzX/Af8cfQeKpafl9rMAAAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACoAAAAXCAYAAAB9J90oAAACJklEQVR4Xu2WP2gUQRTG97gIhvgHMZfFu72b3b2DwxgQXAgYUhwikhRpRFRMaaO1RRorgyCmsgokQrBLkTYQxSIQSCQpxEIsjIUQOFKYgGAlkvxedgbGhTsNm+32g4+Z9+abed/Ozeye4+TIkSPHseC6bl+tVnuhlGrDPbhBPJjUkb8BP2ndrsyRuZak4Pv+Y8aewXl4zRpLDxZ8D5cbjcY5Jy52T8wEQTCc0InBEenrh1tE+65UKp3R49fJ3ZW+53m9xNNhGJ6310iDAgv+psCoSUhhcqvkVqSg5KQg8YLojY44QvcTs/clpm1Jzowz9gBeMnEqsGsui/2yCwgo+sbO096EU7ZGTMDvcImwJ9Md1bvSyegBvK11U/QnbY1l9Js8sJPlGWWxy92Mml2k/5r+hK0hvkB+C7bphyYvRyeKolO2NjV0sb+MYvI0uWXbqBhPGjVnOTk/M1DoD0bGTFytVofI7Wdi1I/xiklz/8lxM5cis8TbzG9x1q4SLxB/UFkYBcV6vT6g4gP+T8rPa80tSCHyL+ETPW4u09ED0T5X+mIZlMvlfnKfVXyhTuY11A3NZvOsY70f5WWu4o/ADgwkRztpdtdAzGmTa3qNbKF37qn05baq+Gh8qVQqniXrYZc3yV00CeJHKv6UXrF02YFiX+EMfAg/wvWEySNg6K2Yld1V8VH4oRLHIVPIT81Nv4WRO/pSFJMajaKMi070iT8kOXJ0wiEudquMKzENAgAAAABJRU5ErkJggg==>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABsAAAAXCAYAAAD6FjQuAAAB7ElEQVR4Xu2UwStEURTG3zQUIcIYZt6bN/OmTEpZDDYsxYKRhYX4A2wkZTFlK8lGsrAhOzZjq6ykFGWppJQFTVmxmLLF74x3x/UyjwVlMadO99xzvnO+c+599xlGRSry7yQej7fbtr2NPqEFdNOLSafT1fgXY7HYPWsePcce0DH4dtEZ6i2wrkiOHjcSiYRN0g3BdQmaplmLvRMKheo1WADMBv5j4s3iECIhRgddTFU0Gm1RCRCOEJtU+yJACqMPBBzltCyrD/CUtu8G88g6rnxIAN8eeUfSoKc5mbIDnS456MR0j+ROgh7gAWaV7CmYZf/Mmi4lf/gf0S7B+k4myVKkDNlVJBJpNdwJfMheWTNuXvk7+4YsL5PL8WCfoAXut6eU/J6vyLKuK5hMJtsga9JxRZHOZQIvmRRVk2hknzAiX5D5C+BF6Vo/IvZzf0LmOE4jCTn0TCYicQx7XxX/VTJXgiSNolvoNMkO62kqlWrg/GuwD22fDwTMrO73k0A4HK7THe6dlf4iYvuQvbAO6f6y4haS7oqPWP4oyDVmQGHk/RC/wL+s/ODCgtN93wpJ/RS6JGmJDudZb9mveXH8PXqJ5cGs2u9vSchzcuderK9I5xTJUGCC52B540rkuCEdBjsJSafx04kq8lvyBlq2nFoxNnVNAAAAAElFTkSuQmCC>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABIAAAAZCAYAAAA8CX6UAAABLUlEQVR4XmNgGAVDHIiKivLIycnVy8vLz0LGCgoKHNLS0sJAuhNZHKi2AygmgG4OCDArKyuLASX9gQpfAfF/ILsRJGFsbMwK1BgGFPsNxOeB7BhZWVkpoBQjmhkogBGoeArIICCehCRWCjSgDmQoimp8AKjJEoh/AvF1IJcRaEAZEM8gyRAQkJGR4QRq3AF1VTAQL1VSUuJHV0cUAGrOgRq0jmxDQABogCIQPwF5E12OJAD0nirQkLvAmGtAlyMaKCqCHCN/BIiLQAEO5IujqyEIQOEBdMUqoGYzUCIFhROQH4GuDi8AGQLUuB4UUzAxkEGgGATFJLJanACURoCaJgNtT2BASrHQAH8PTMk6CNU4ACiGgLZuBOIVQC4zmtwckKuAuJmBQJYYBUMJAAAjzEXYfNhWZQAAAABJRU5ErkJggg==>

[image6]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAZCAYAAAA4/K6pAAABDUlEQVR4XmNgGAWDACgoKBjIy8vPQsdycnKuMDWysrKmyHJAPROBWAFmAIeioiJQXL4PiP8D8R0g3wwkDjNASUmJHyg+DYh/AXEr0HBtY2NjVpg8GAAlNIH4LRB/BSowRpaDWnAEZDCyODpgASpaA3IF0IBymCDU9q1AHIysGCsAOjsC6o0TII0gMSB7MhDnAJmMaMoxAVAhyK1PgPgnEFsChRiBdCmIRleLCzACnT8f5AqgaxpAzsYILEIAqNEDqPEfFG9HlycIpKWlZaDeuAuPa1IANNF8A+I1QC4LujxBALQ0HRQGQFyELkcMAIX6UiD+DQxMG3RJggCY0sSBmq8D8UMglkSXHwWDFQAAdrJC5sHSvLgAAAAASUVORK5CYII=>

[image7]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACUAAAAXCAYAAACMLIalAAACgElEQVR4Xu2Vu2sUURTGZzGCwYAgrIv7mp3dVdDGx9rYqIiFFoqoaEj+gI1oI4JCbARJISJIEAQDQoo0ESXBwoCNgoVYKIIQ8QERIhbBMk0E9fc597rHcUcXQQXZAx93zne/87h37twJgq51rWv/2Mrl8kbwIAzD92ChUCgUkxpZo9FYzvxJMCcdeFitVtcbSaZSqRyDfw3egefgsPiE5jjoZ24MbDVzsUFuAfeiKArl1+v1rGtug9VRfBXcTXCrWCyudg1eZTEz+L3SUGgIbha/Lp9xHfMv4QZ8Hp63wx118734F5Tbz3vRCPgMLhtO/rCRZRQMXuXz+ZIIEjfwF8EbFpQT1BD8WROnXMPiNS+fxncp1swPgLWtiOCr6HxKUyPGj8A8GPWcdorYU+AgbqZUKh1QHAX3e41MPvwnxj3yw052SslrtdoajfIpskJJGPd6Dc9N1+hgK/J7C+MdSWtKvN/BX5+ppCHaDa77JmUkGHdNnQC3KTDF+AGcM4vxmn2tbK2mNG95vU5bo63pVRD8FixZcTab7YO77wrOoquI12HGn6fomSBevZpatOdFltZUx6YGCN5Jkkv+i0o09e2cBfHhn1BjIPpjTXlTEpJd4TFjm0qeF/PKDv2Npj6COXarIN8XT2tKh5hxtJ3GN6V5y6eavjrET8ACB2+T57Viu2pX9IeCvinGptBOY3aqaflUc7f0IyXP5XIrHa2zopWd9jp3n9xBN+k/AnNZXsPtcffWpNUojvkZy3VkBGwm+Ysw/oUMgjG4i8kk+g1hzygy7XblKbhrLz6vATfQ9Dvt47R/6U9NDXAjbyPJkeT2W5OOwjucTq922e9ouvbf2hcBHOUWAtOh4gAAAABJRU5ErkJggg==>

[image8]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABsAAAAZCAYAAADAHFVeAAAA9klEQVR4XmNgGAWjYBSMghEDmOXl5b2BeBoQz8KCJ8nKyiqjayIZGBsbs8rJyU0HGvgTSD8C0UD8BIh/AfEzqNgJJSUlNXS9JAMFBYV0oGE9MjIynKKiojxA9nJFRUVxoPhCdXV1XnT1YCAlJSUCVBAAdEkIIQw00AqohRGEgXoUQL4DmQEMKh2QJUDaFGQpkM2BYgkMkGkZCgDqbwDKtQJxNMhSdHmqAVDQAS25CHSMCxCXA9lrgMIs6OqoAoA+iQBacBcYd9JQy04DaUF0dRQDUOIAGrwD5huoZaBUaYmulhqAWVpaWhhEw/hQX2HE6ygYBXgBAJS4QBS8GSrmAAAAAElFTkSuQmCC>

[image9]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAwAAAAYCAYAAADOMhxqAAAAxUlEQVR4XmNgGAWjAC9gVFBQMJeXl58ExLOwYVlZWVOwSjk5OVegwDIZGRkhkEYgexpQzAaI50tLS8ugGAsCIEFxcXFuEFtRUVEcqGG1lJSUCJDeDqLR1aMAoLMigHgVkMkC1LBNVFSUB10NHACdxAl0xg6ghnQQH6jhNJAviK4ODoAKLIH4GVCRMZT/HMhWQlcHAyDPTkE2Fcj+D3IiukIwAEpKAvFDoIIGJLG3QNyLpAwFMIKC1djYmBUmALWJGUnNsAEAUhkm1D3S+coAAAAASUVORK5CYII=>

[image10]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADQAAAAXCAYAAABEQGxzAAACdUlEQVR4Xu2Wv2sUQRTHN9wJCkIKPX/cr9nbE0EEQQ4LJYJoIRYRixTBlAG1VvAXKUyRSgS1EaIELET9CxQVtLRVFASxPqKoVSy0OD/f7AwMw7k5yG6w2C98mXnf997uezuzsxtFJUqUKFFiBDSbzS1xHF81xiyGPgH9OHwH+/AHvE/8rjAO7Sm+b/ArfJYkyV7f3+l0kM0SnIELvV5vk+9fN7joTYp4z/gbDpg/DGME9BeNRqOpuW1+nviPThNULPo1phU4hn8WLsODNqQqPznbZDA/2W63p1x+LuCCJ3iKbW56GK4Mawj/OHETvlar1bYS/4b4805jfoNmG2EMfIxZlU3MBedH3w3POjtXUHDvXw3pxujTgawVeETeFRn1en27VkxF+0G6HnqfuCTaiBVyyGrIFrtin6a2k7bdHuxPWlnZLp9p1c+1DQ3gKdmFv0MOWQ1F6WqoKPFVq9U6yvgWXpJPASpY/iBP+oLNm/HkSrfb3eHZ+WONhlTYnNeUeMt/uuRPDmtIW1K625obhhEaWiLmOeMHrykdy+Py59pQnOIOiYsjcnU/+8hqSEUrTyuiI5vYy8Ye88xvEzKWa0OR3ZMmPQbXJEVvDi+Q1RDatH8cC7zcB4j/IjLfSf4E8z+Rfacc0O7ahiZ9vXBkNYT+IDyOra6T6jvcR37C2A/jdD30Xxwkh3y9cHDTI7oxhT2J7NHsgLYf32zkPX27DV+a9KRbBUWf1gnobJu3HHsf08Jh0i/5YBj9bYL9E7426e/MdZNut7ngO1Ix6YpdJPcc42fGe4V9a9YDvXfwGAVOqVG9N2GMwEe4RdwZUfPQX6LEf4a/wiXWNZZ0kVMAAAAASUVORK5CYII=>

[image11]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACUAAAAXCAYAAACMLIalAAACQUlEQVR4Xu2Vz0tUURzFHbSFKBTVMDS/3sxQQm4MRwQ30aJNhC4yUvQPqNy6MKKF4j7CbRC08tdKWqgpFAUtXFSCFEQbYUTcBrpwYX2Oc+94ubynb6NIzIHDe++c77v3vPvrNTTUUUcd5wOJQqFwKwiCRd8QyuXyBbxRuA134A94DyvhlKmNAfTfcAtuwP6QmqdwEO817HS8KhQEzprODuCmXwMS6C+pm8hms80SuF5HWzedHgL/Cc8/5ZmaG/l8/hfakK3hvgftkfGbeZ4slUoXrX8IjEyxWOymwUvwbVgotJvwQzqdvurpw3A1lUq10EZKgehwzKt5Ll2+nunjDjVlxx+C147e8BAVikbuolf05b7OO++TyWRrLpfro+YvWq9X04t+oFo9xxopF1Gh+MoO9D/wGw22G1lTOsnzK3OvEYkKJd2O4MlrykVUKPNF79Q43KfuGdcR+DmTyWSdd+Vr8ddgQ8l3dU2nNo+rhSIqlECwy3SwbDoWtzWC1jfv7rrrRYgKFRvHhWLe2zQycAXum2C78L78Mw/FiCAfbX/qCtx/McEqsHjmodCm4DS3TVbTeqB+3ATT0aCayIUu39VjIyyUtjvaR2f31MAiv4L3VR7vPj4ulHxXj4tGGpihgS3fQHsAd3TIeno/nX1SODNyc6LdVdq1tLnkarGgNRBUF6yGuEZ3ZMx/7wXcC6rTpOmah9/tL0Vw1t4bggzSxgLXNXtsnAp0tmg64ENO8K6wr5dG3W3V6IORGv2aOv5b/ANMrc+udVsTPwAAAABJRU5ErkJggg==>