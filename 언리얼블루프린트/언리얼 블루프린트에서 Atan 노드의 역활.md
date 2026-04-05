# **언리얼 엔진 블루프린트: Atan 노드 완벽 이해**

언리얼 엔진에서 Atan 노드는 주로 \*\*방향(각도)\*\*을 계산하는 데 사용되는 핵심 수학 노드입니다.

## **1\. Atan vs Atan2 (가장 중요한 차이)**

블루프린트에서 "Atan"을 검색하면 두 가지 주요 노드가 나타납니다.

### **Atan (Degrees/Radians)**

* **입력:** 탄젠트 값 (보통 높이/밑변의 결과인 단일 Float 값).  
* **출력:** 해당 비율에 대한 각도.  
* **한계:** 입력값이 하나뿐이라 결과가 ![][image1]에서 ![][image2] 사이로 제한됩니다. 즉, 점이 내 뒤에 있는지 앞에 있는지(사분면)를 구분하지 못합니다.

### **Atan2 (Degrees/Radians) \- ⭐ 권장 사용**

* **입력:** Y(높이)와 X(밑변) 값을 따로 입력받습니다.  
* **출력:** 두 좌표가 이루는 **정확한 방향 각도**를 반환합니다.  
* **장점:** ![][image3]와 ![][image4]의 부호를 모두 고려하기 때문에, ![][image5]에서 ![][image6]까지의 전체 범위를 계산할 수 있습니다. **게임 로직에서는 거의 항상 Atan2를 사용합니다.**

## **2\. 주요 활용 사례**

### **① 두 지점 사이의 각도 구하기 (Look At 로직)**

캐릭터가 특정 목표물(타겟)을 바라보게 하고 싶을 때 사용합니다.

* **계산법:** Target Location \- Self Location을 통해 상대적인 X와 Y 거리를 구합니다.  
* **노드 연결:** 이 X, Y 값을 Atan2에 넣으면 타겟을 향한 회전값(Yaw)을 얻을 수 있습니다.

### **② 마우스 커서 방향으로 발사체 쏘기**

탑다운(Top-down) 뷰 게임에서 마우스 위치로 총을 쏠 때 필요합니다.

* 캐릭터 위치와 마우스의 월드 위치 차이를 Atan2에 입력하여 발사체의 회전값을 설정합니다.

### **③ 조이스틱/패드 입력 처리**

가상 조이스틱이나 게임패드의 스틱이 기울어진 방향을 각도로 변환할 때 사용합니다.

* 스틱의 Forward 입력값(![][image3])과 Right 입력값(![][image4])을 Atan2에 넣으면 스틱이 가리키는 방향이 몇 도인지 알 수 있습니다.

## **3\. 실전 사용 팁**

1. **결과 단위 확인:** 노드 이름 뒤에 \*\*(Degrees)\*\*가 붙은 것은 도 단위(![][image7])를, \*\*(Radians)\*\*가 붙은 것은 라디안 단위(![][image8])를 반환합니다. 언리얼의 Make Rotator 노드는 보통 **Degrees**를 사용하므로 단위를 맞춰주는 것이 중요합니다.  
2. **좌표계 주의:** \- 일반 수학: Atan2(Y, X)  
   * 언리얼 회전(Yaw) 계산: 보통 X가 전방, Y가 우측입니다. Atan2(Y, X)를 사용하면 정면을 기준으로 좌우 각도를 얻을 수 있습니다.  
3. **Find Look at Rotation 노드:** 사실 두 위치 사이의 각도를 구하는 것은 Find Look at Rotation 노드 하나로 더 쉽게 해결할 수 있습니다. 하지만 순수 수학 계산이나 UI(위젯) 애니메이션 등에서는 Atan2가 훨씬 가볍고 직관적입니다.

## **4\. 요약**

* **Atan:** 단순 비율로 각도 찾기 (제한적임).  
* **Atan2:** 좌표(X, Y)로 360도 방향 찾기 (매우 자주 쓰임).  
* **용도:** 타겟팅, 방향 전환, 커서 추적, 조이스틱 입력 처리 등.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACoAAAAXCAYAAAB9J90oAAACJklEQVR4Xu2WP2gUQRTG97gIhvgHMZfFu72b3b2DwxgQXAgYUhwikhRpRFRMaaO1RRorgyCmsgokQrBLkTYQxSIQSCQpxEIsjIUQOFKYgGAlkvxedgbGhTsNm+32g4+Z9+abed/Ozeye4+TIkSPHseC6bl+tVnuhlGrDPbhBPJjUkb8BP2ndrsyRuZak4Pv+Y8aewXl4zRpLDxZ8D5cbjcY5Jy52T8wEQTCc0InBEenrh1tE+65UKp3R49fJ3ZW+53m9xNNhGJ6310iDAgv+psCoSUhhcqvkVqSg5KQg8YLojY44QvcTs/clpm1Jzowz9gBeMnEqsGsui/2yCwgo+sbO096EU7ZGTMDvcImwJ9Md1bvSyegBvK11U/QnbY1l9Js8sJPlGWWxy92Mml2k/5r+hK0hvkB+C7bphyYvRyeKolO2NjV0sb+MYvI0uWXbqBhPGjVnOTk/M1DoD0bGTFytVofI7Wdi1I/xiklz/8lxM5cis8TbzG9x1q4SLxB/UFkYBcV6vT6g4gP+T8rPa80tSCHyL+ETPW4u09ED0T5X+mIZlMvlfnKfVXyhTuY11A3NZvOsY70f5WWu4o/ADgwkRztpdtdAzGmTa3qNbKF37qn05baq+Gh8qVQqniXrYZc3yV00CeJHKv6UXrF02YFiX+EMfAg/wvWEySNg6K2Yld1V8VH4oRLHIVPIT81Nv4WRO/pSFJMajaKMi070iT8kOXJ0wiEudquMKzENAgAAAABJRU5ErkJggg==>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABsAAAAXCAYAAAD6FjQuAAAB7ElEQVR4Xu2UwStEURTG3zQUIcIYZt6bN/OmTEpZDDYsxYKRhYX4A2wkZTFlK8lGsrAhOzZjq6ykFGWppJQFTVmxmLLF74x3x/UyjwVlMadO99xzvnO+c+599xlGRSry7yQej7fbtr2NPqEFdNOLSafT1fgXY7HYPWsePcce0DH4dtEZ6i2wrkiOHjcSiYRN0g3BdQmaplmLvRMKheo1WADMBv5j4s3iECIhRgddTFU0Gm1RCRCOEJtU+yJACqMPBBzltCyrD/CUtu8G88g6rnxIAN8eeUfSoKc5mbIDnS456MR0j+ROgh7gAWaV7CmYZf/Mmi4lf/gf0S7B+k4myVKkDNlVJBJpNdwJfMheWTNuXvk7+4YsL5PL8WCfoAXut6eU/J6vyLKuK5hMJtsga9JxRZHOZQIvmRRVk2hknzAiX5D5C+BF6Vo/IvZzf0LmOE4jCTn0TCYicQx7XxX/VTJXgiSNolvoNMkO62kqlWrg/GuwD22fDwTMrO73k0A4HK7THe6dlf4iYvuQvbAO6f6y4haS7oqPWP4oyDVmQGHk/RC/wL+s/ODCgtN93wpJ/RS6JGmJDudZb9mveXH8PXqJ5cGs2u9vSchzcuderK9I5xTJUGCC52B540rkuCEdBjsJSafx04kq8lvyBlq2nFoxNnVNAAAAAElFTkSuQmCC>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABIAAAAZCAYAAAA8CX6UAAABLUlEQVR4XmNgGAVDHIiKivLIycnVy8vLz0LGCgoKHNLS0sJAuhNZHKi2AygmgG4OCDArKyuLASX9gQpfAfF/ILsRJGFsbMwK1BgGFPsNxOeB7BhZWVkpoBQjmhkogBGoeArIICCehCRWCjSgDmQoimp8AKjJEoh/AvF1IJcRaEAZEM8gyRAQkJGR4QRq3AF1VTAQL1VSUuJHV0cUAGrOgRq0jmxDQABogCIQPwF5E12OJAD0nirQkLvAmGtAlyMaKCqCHCN/BIiLQAEO5IujqyEIQOEBdMUqoGYzUCIFhROQH4GuDi8AGQLUuB4UUzAxkEGgGATFJLJanACURoCaJgNtT2BASrHQAH8PTMk6CNU4ACiGgLZuBOIVQC4zmtwckKuAuJmBQJYYBUMJAAAjzEXYfNhWZQAAAABJRU5ErkJggg==>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAZCAYAAAA4/K6pAAABDUlEQVR4XmNgGAWDACgoKBjIy8vPQsdycnKuMDWysrKmyHJAPROBWAFmAIeioiJQXL4PiP8D8R0g3wwkDjNASUmJHyg+DYh/AXEr0HBtY2NjVpg8GAAlNIH4LRB/BSowRpaDWnAEZDCyODpgASpaA3IF0IBymCDU9q1AHIysGCsAOjsC6o0TII0gMSB7MhDnAJmMaMoxAVAhyK1PgPgnEFsChRiBdCmIRleLCzACnT8f5AqgaxpAzsYILEIAqNEDqPEfFG9HlycIpKWlZaDeuAuPa1IANNF8A+I1QC4LujxBALQ0HRQGQFyELkcMAIX6UiD+DQxMG3RJggCY0sSBmq8D8UMglkSXHwWDFQAAdrJC5sHSvLgAAAAASUVORK5CYII=>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADQAAAAXCAYAAABEQGxzAAACdUlEQVR4Xu2Wv2sUQRTHN9wJCkIKPX/cr9nbE0EEQQ4LJYJoIRYRixTBlAG1VvAXKUyRSgS1EaIELET9CxQVtLRVFASxPqKoVSy0OD/f7AwMw7k5yG6w2C98mXnf997uezuzsxtFJUqUKFFiBDSbzS1xHF81xiyGPgH9OHwH+/AHvE/8rjAO7Sm+b/ArfJYkyV7f3+l0kM0SnIELvV5vk+9fN7joTYp4z/gbDpg/DGME9BeNRqOpuW1+nviPThNULPo1phU4hn8WLsODNqQqPznbZDA/2W63p1x+LuCCJ3iKbW56GK4Mawj/OHETvlar1bYS/4b4805jfoNmG2EMfIxZlU3MBedH3w3POjtXUHDvXw3pxujTgawVeETeFRn1en27VkxF+0G6HnqfuCTaiBVyyGrIFrtin6a2k7bdHuxPWlnZLp9p1c+1DQ3gKdmFv0MOWQ1F6WqoKPFVq9U6yvgWXpJPASpY/iBP+oLNm/HkSrfb3eHZ+WONhlTYnNeUeMt/uuRPDmtIW1K625obhhEaWiLmOeMHrykdy+Py59pQnOIOiYsjcnU/+8hqSEUrTyuiI5vYy8Ye88xvEzKWa0OR3ZMmPQbXJEVvDi+Q1RDatH8cC7zcB4j/IjLfSf4E8z+Rfacc0O7ahiZ9vXBkNYT+IDyOra6T6jvcR37C2A/jdD30Xxwkh3y9cHDTI7oxhT2J7NHsgLYf32zkPX27DV+a9KRbBUWf1gnobJu3HHsf08Jh0i/5YBj9bYL9E7426e/MdZNut7ngO1Ix6YpdJPcc42fGe4V9a9YDvXfwGAVOqVG9N2GMwEe4RdwZUfPQX6LEf4a/wiXWNZZ0kVMAAAAASUVORK5CYII=>

[image6]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACUAAAAXCAYAAACMLIalAAACQUlEQVR4Xu2Vz0tUURzFHbSFKBTVMDS/3sxQQm4MRwQ30aJNhC4yUvQPqNy6MKKF4j7CbRC08tdKWqgpFAUtXFSCFEQbYUTcBrpwYX2Oc+94ubynb6NIzIHDe++c77v3vPvrNTTUUUcd5wOJQqFwKwiCRd8QyuXyBbxRuA134A94DyvhlKmNAfTfcAtuwP6QmqdwEO817HS8KhQEzprODuCmXwMS6C+pm8hms80SuF5HWzedHgL/Cc8/5ZmaG/l8/hfakK3hvgftkfGbeZ4slUoXrX8IjEyxWOymwUvwbVgotJvwQzqdvurpw3A1lUq10EZKgehwzKt5Ll2+nunjDjVlxx+C147e8BAVikbuolf05b7OO++TyWRrLpfro+YvWq9X04t+oFo9xxopF1Gh+MoO9D/wGw22G1lTOsnzK3OvEYkKJd2O4MlrykVUKPNF79Q43KfuGdcR+DmTyWSdd+Vr8ddgQ8l3dU2nNo+rhSIqlECwy3SwbDoWtzWC1jfv7rrrRYgKFRvHhWLe2zQycAXum2C78L78Mw/FiCAfbX/qCtx/McEqsHjmodCm4DS3TVbTeqB+3ATT0aCayIUu39VjIyyUtjvaR2f31MAiv4L3VR7vPj4ulHxXj4tGGpihgS3fQHsAd3TIeno/nX1SODNyc6LdVdq1tLnkarGgNRBUF6yGuEZ3ZMx/7wXcC6rTpOmah9/tL0Vw1t4bggzSxgLXNXtsnAp0tmg64ENO8K6wr5dG3W3V6IORGv2aOv5b/ANMrc+udVsTPwAAAABJRU5ErkJggg==>

[image7]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEEAAAAXCAYAAABUICKvAAADoUlEQVR4Xu2XSWhUQRCGXzCC4oZLjGaZnkyCIQouxAUXBIOIHiISFYSIF4nx4MHlIHhSJOhFCUHwIqgHQdwhiihBXC5qDqIoOaiI4IKICoGACxq/P6/b6TQzBhEml/fDz3TV+7unqrq73kwUJUiQIEGCBHlRU1MzPpVKtRtjPvP5AC4NNYUE33+HWN4qHngVe2aoEUpLS8fw/KPl43Q6PTeQFOFv0DP4nnUOa06giaJMJjMBQRc8UVFRMbqysnKVFuVzbagtBAh0WVVVldHYi+1nGE95eXkF/m5tYH19/UgliH1fc5wGe72Sh0tkozlLoW6WlJSMza4EcG5C1Iug3rqKGJ8MFywUtBnwiLOJrxW7H15jPEo+bRZ2J/Zd2Yoduw++htPlswW8r1wwizxdr3J26+sLRmlxf7IV78X+oV35Iy4QVADY4WyvCLfdDjJugr/0TLY9CS2ixvIxXimNcnFrKUeb6wXM4tDZjXiiE9si6It3O1+hoCRcIlF8p48pFhLeb33FSsIMsUleDs3O5+X7kitXOuBkMAdHr/GqbMXNdoE25xsOEN9CYvgCu9zVVPBKAvaR6DZ4hfEnPg95xXPXqh9/o/Npo/F1m7hJZpzT3aVBRdBELUD1TztfHhQR1HL0p0TmNOAbEYqE6urqqTxfEfpzge/dhfYV/A6P+h0du05JKz54Sz56RA3jd8TQHtn7r9il8YugHJWriQsY98D/LQKaPSbelX2WOmoXCWpSqKW7L2DdltCfD4qH71/Eei/gJcbT5Pdi7jfZo65rcwZ+g4vlKFgR9NxPWF0b3wH4EKY9qbQ7U3+5w/nAvNXE8svYK2GyJyGbSKwbSNrYPvYvRciY+D16r7a2dpwTp21HTnmdNRcUYOiL4lfsFua/gc2u0TE+ryKF4qHgJa0E15hsc8tZBBcz4zY7p8lpysrKpmA/s/Pjt2Eq2yhyvSIHLZALPK8LfQ70itkE9sSu068fN6EmhP0B1KOGHfhUUJ3M1nT2tT5UEQaau7+RytHEuQ7adD2YZ+Kfy5tl2wb2NOU1mULBdv6eoBHuUDKw050kdrQS+zk86M/zNaCYHI5TnIcUcrIcjLej+YB/ltX8gY7vRhMfuTZN4vP6cPxaFEz8W/8y3Ao74FdiOhc2W6t7b6/uI3K4EWpky29z0slQjvlPt6rFhEaSnxEV+ASE4E0yn1g2EPw6mI7yxKMTI529FjlfzWCEnkuX889TggQJHH4DIGJE7td5qsoAAAAASUVORK5CYII=>

[image8]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADkAAAAZCAYAAACLtIazAAAC0UlEQVR4Xu2WPWgUQRTH91BBUfwgnof3NfdlcaZQPBUC2igKFlqIIBILwUI74RrBRpt0EkQEISBioYUBwcJgcYWgjaaysFKRiBAUNCAqKIj+3u1M7mXcu0ti7tLsH/7smzdv3sfM7NsNghgxYsSI0WPUarVV2Wx2mzFmlz+3DFhBHluFIvuTiwLODsB38Do8K89UKrXWt+sHiF0tFAov8vn8Q+Tf8BM84tstCDgbxMlHnqedzga5hphQpj1HJpPJksvTYrG4V8b2Zr2VYsnnkG8/b9jT+yABlG5EdLCobXsN4g3DP3AimUyuEx3FXbS6R2z+an9NV+BgE4snZbfYvZTSO8fHtX2vYcJXReLO5kMuR63uJQcx4K/pChyUWDwNn7idEzCui2MpVtv3GtL8iHmQa1pxOtM63YbuE9htN+EtHIsi85ebNSHUUHz3i3S7x/W443TtIInlcrndQpH9eY1u8z7Enhzu21zOW3UC+Qy6HyZ8paQx/YRf4Ddyfy9EHl+yIm0QxxnWnAratH2u4A5f1wls3DETdtgxt0Ho9jOecD2EeOcY1+UpnOshaBbzX0WWy+UtBN3jxrYbPoOjyGu0rQD9sK/rBOzfwKv6BqTT6VylUlkvssRg/gH57uN5V/JurbZgogo/+0VKMlIkvKTtfUQ5tYFHpVgp2ullM9CNa9tO4NQN/i8EbW6FAH9D2Dyn8M3Ik1H5yK7I5Cs4ZcK/iybyre7acecjr0cIeW9Osn7GtK7ya3Q7fcMo2O9lQ40HZOPcCVok0N0g19tWnoosMrCGcBqDklOasGvJCVe1sQ+S3ujrNGy3HBTOt+mUSqUNxL0Hh5zOvla3EFc6HeOiCRtP8yCkSFh383NgHXzN//vHczNQTvsBW2DDqC4pZPwLjmhbcryiD0KKxPZxVC+YhQTgvTmM4Ymgz79zi4HcooL6A5KeovtKjBgxYiwb/gIZ0NG1rREp9wAAAABJRU5ErkJggg==>