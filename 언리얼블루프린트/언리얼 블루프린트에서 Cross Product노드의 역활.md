# **언리얼 블루프린트: Cross Product (외적) 노드 이해하기**

**Cross Product(외적)** 노드는 두 벡터 ![][image1]와 ![][image2]를 입력받아, 두 벡터가 이루는 평면에 \*\*수직(90도)\*\*인 세 번째 벡터 ![][image3]를 출력합니다.

## **1\. 핵심 개념**

* **결과값의 방향**: 오른손 법칙(Right-hand Rule)을 따릅니다. 첫 번째 벡터(A)에서 두 번째 벡터(B) 방향으로 손가락을 감아쥐었을 때 엄지손가락이 가리키는 방향이 결과 벡터의 방향입니다.  
* **결과값의 크기**: 두 벡터가 이루는 평행사변형의 넓이와 같습니다. 두 벡터가 평행하면 결과값은 0, 0, 0이 됩니다.  
* **수식**: ![][image4] (단, ![][image5] 이고 ![][image6])

## **2\. 주요 활용 사례**

### **① 캐릭터의 좌/우 판별 (Left/Right Detection)**

캐릭터가 타겟을 바라볼 때, 타겟이 내 기준 왼쪽인지 오른쪽인지 판별할 때 가장 많이 사용됩니다.

* **계산 방법**:  
  1. 캐릭터의 **Forward Vector**(앞방향)와 캐릭터에서 타겟으로 향하는 **Direction Vector**(방향)를 구합니다.  
  2. 두 벡터를 **Cross Product** 합니다.  
  3. 결과 벡터의 **Z값**이 양수(+)면 오른쪽, 음수(-)면 왼쪽에 타겟이 있다고 판단할 수 있습니다. (언리얼의 Z-Up 좌표계 기준)

### **② 표면의 법선 벡터(Normal) 계산**

세 개의 점(정점)으로 이루어진 삼각형 면이 어디를 보고 있는지 계산할 때 사용합니다.

* **계산 방법**:  
  1. 삼각형의 두 변을 벡터로 만듭니다. (![][image7], ![][image8])  
  2. 두 변 벡터를 **Cross Product** 하면 해당 면의 **Normal Vector**(법선)를 얻을 수 있습니다.  
  3. 이는 커스텀 메쉬 생성이나 절차적 지형 생성 시 필수적입니다.

### **③ 이동 방향에 수직인 벡터 구하기**

플레이어가 벽을 타고 이동하거나, 특정 방향의 '옆' 방향을 구해야 할 때 유용합니다.

* 예: Actor Forward Vector ![][image9] World Up Vector (0,0,1) \= Actor Right Vector

## **3\. 블루프린트 사용 시 팁**

1. **입력 순서가 중요합니다**:  
   * A x B와 B x A는 결과 벡터의 방향이 정반대(180도)가 됩니다. 원하는 방향이 나오지 않는다면 입력 핀의 연결을 바꿔보세요.  
2. **Normalize(정규화)와 함께 사용하세요**:  
   * 외적의 결과값은 입력 벡터의 길이에 따라 크기가 커집니다. 단순히 '방향' 데이터만 필요하다면 Cross Product 노드 다음에 반드시 **Normalize** 노드를 연결하여 벡터 길이를 1로 만들어주는 것이 좋습니다.  
3. **두 벡터가 평행하면 안 됩니다**:  
   * 앞방향 벡터와 뒷방향 벡터처럼 완전히 평행한 두 벡터를 외적하면 결과가 0이 되어 방향을 찾을 수 없으므로 주의해야 합니다.

## **4\. 요약**

| 상황 | 해결 방법 |
| :---- | :---- |
| **두 방향에 동시에 수직인 방향을 찾을 때** | Cross Product 사용 |
| **대상이 내 왼쪽인가 오른쪽인가?** | Forward ![][image9] Target Dir 후 Z값 확인 |
| **바닥 면이 기울어진 방향(법선) 찾기** | 바닥의 두 축 벡터를 Cross Product |

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAYCAYAAAAlBadpAAABA0lEQVR4XmNgGAXUB4qKivJAoIkuThBIS0sLKygonAJqnoMuRxAANRUD8X+gAQvR5QgCOTm5RyDNQHxAVFSUB10eH2ABamoG4rcka5aVlbWFBtZDKJZEV4MVgGwBKl4tJSUlAqSvAvETYODJoKvDACBFQMVrQbS4uDg3kL0HiL8BXWKKrhYdMEL9CQplSSheDcT/gYHni64YBQAVGIP8BwplGAbyv4A0A3E0uno4MDY2ZgUqmAcKKGRxoAHlUJvLkcVRADAhhAMVdQOZjMjiQLEiqM2tyOIwwAjUaCEPCVVLdEmgWBBIM0YqA8ajGVDiG9RkGE6CyWORAzk/F9mMUUAkAACL7kg2gzJ3vwAAAABJRU5ErkJggg==>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAZCAYAAAA4/K6pAAABN0lEQVR4Xu2SrU8DQRDFr6EVKEJKUrivvb3UkOAumGoEmuLAoMA3wSP5DwgGhSE1hOAqQCNQ1UCCJSiQwG/Cdhk2pUgQfcnLZffNzL6XuSia4p8hSZJmURSHxpjjcUQ7SNO0HfZ5VFXVsNa2KNyh4Q1ewiXHTp7n13zfYY/yWtjvwYBdV7il77MsW+HuGT5CqzWNGuIpfOHFSgtETF3zE1zWmkccxwuIQ3jDgHmtcd4WZzg8k7ha88DmKkWvFJ9EKid3HWd/wIBF1fIdo/wMOIKbcJ/zFbyFXUpmwh4NnX/NfG1gA96J9YmvT8pP47r5XO0F/8Ks1jx+yi+QjYgz+CCutOYxyi/fUHORxMFQnIa6oI7Yl1fC/ZdlOcf9wA3f05oHuRIK7sP8nC3nc2mWjUS/bGGKv8AHDrlbY7OnBuIAAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAaCAYAAABozQZiAAABQklEQVR4XmNgGAVDHDAqKirqycnJrZCXl38HxK+A+KqsrKypkpISP5C9CIg10TUxyMjICCkoKKwCSv4Aaq5DElcB8k8CxS8C8WkgWxBZHwPQNqC4/BUgvovNZKChHkDxf0A8CUVCWVlZDCh4DojfAw0xQ5GEAmlpaRmg/BMgjkYWZwE6YzpQ8D+QLkeWQAZAeUmg7adQXAXkWALxTyC+DrRVHEk9CgC5Dqh5oqioKA9YQF1dnReo6TDIVnTnEAQgpwDxQyD+BooKdHkkwAiyCETDRZA0fwX61xihFhWAvAOUnwF3MgiIi4tzAzXuIWQzyEtA/6aji4PiLwPq5xx0ORAARR0o4YBSF7ocAzTJgWx/D3SaC7IcUMwJiNeC4hhZHAUgJUtQCjoPZDeCMNCwfpDX0NVjBdCA8QXiEFwpbRSQCQCmf0yaqs5/wwAAAABJRU5ErkJggg==>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAF8AAAAZCAYAAABXTfKEAAAESElEQVR4Xu2ZTWgdVRTHX0gEP6pSNU2aj3ebl2gogW5iK0JdaNJVEUQLFnVloboQFy3aD1Bai4viRipBCcWSSnEjlLZ+7iIKWrqom64UjKG0UJFCodJaTPz9M2fCvJPJZFLfmzzl/eHPzDsf95577r3n3klKpWWiXC4fDCHs8PIVRgsxTfJs9YoGgmL8VU+vqAIJHsHw5TT29va+o0a6urru9n4FQANYEJMWAzF/y3PMOxQN4qikxDcXo/IGD3mfKtDAToyU5DSqganOzs5271dvrAPJWGwX6v0QnOX3ae9TNHp6egaSMdpiFY8rRn4/431ygYYfDtH2bvO6FUQLSZ/QROjdKxsFtjNvv1xr5XlZg0BJb9jEG/4LMTZRBatjl9g6e7yuCLDh7qT/N+B4Bnd0d3c/6H1XAsPDw3eQq23EdA7OwCvw+0ql8gi8n/ePqf0bvV8a2mjoQxxm4VGvLAit/f39a/r6+jbZQK4S02aea5HxCK/B6/B3BvWody4Quo09BX8xPquFIwVxbiDms8h+hBe4LT7knRcAhy0Y34CzNDTh9UVCq4U4/oRfxIOKYRMwm6YrCEr8bjhDzo51dHTc4w2Ia7tilF72Xl8FbWOMv8RpnxqFk+3t7au8nQd2T9LB66VFOkD/OHzLy5cCcbxiwS8of8hftOTnirHG0I3rTet/XGXHGwjl6BvgssbhdR4tSrqorRyibf3d4ODgvd7QQ51jO0Znb/tAkI3Cr5nYnqQ8BzTAYyFaWaNJBWfSXcjP2OB3J3VFwMrhVXhJ56PXx0C/Fv5E/MNeVwUMhjQgW/3r4R/wNzXgbdNgE/AeHIsngPet8HMmtNPbLwXVSHwvwCkG2B3LlXja08fWDM/DfrKTsINwBG5bBkey2kxOPP0f8PokdG7R3kc6dL1uHpa44xhu0W8l3BJ/GVnF22eg1UrWOM/neX5FsA94ozwoRwfsLXhetVOJCdGfFab5PcHqG/Q+HvVIfohK6E14vbzUis4DS9SRkn3JJlbd7XQQH0QnM2d8CeC/K0RlRVdOLYaY2l3X1EdWkuqFRFznyM1qr182aOSbEM3o3ADtnv9DSKm3OVCL5Lfh/xm8Rf+bU3SfKgEsmledru6g36OW/BOlRS4YhhY7L7Ns5pJ1hUFOJ4nsL3XC+9PeIQM1KTuq8fhOhUXux8S0x5KfeRWuU9k5kqdvbPRB8knmTYybzRNpBmrcZniX16WhlgcuCRgN0VU3bXXFtyDFplK5KOqU/PiKmxbbPBjzXtp6ycvnYZ++Z7xcQP6uOimn3LE9LPE1u2rGfa9LuR8jeyzY1y3tDnl9vRGiFX0R/syu7PX6GOg/8LmYh77ICP59eMrrBJz3WwIyt5cQaviRpV2I7aQSXHaHvU2y5NN5/1ZSDxDDc/BveGJgYOA+p25F/sK/OO+ayAMra0NxySLp60uN/a/NJppoookmmvj/4x940nK1K65CGgAAAABJRU5ErkJggg==>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADgAAAAZCAYAAABkdu2NAAACw0lEQVR4Xu2XO2gUURSGZ42C4htcl9nHzL5wkYAii6KCjabVIgqKioUK9gpqI/jASrDQRoKoUURsbEQQqwgWShobGx/4QAykEYQUFhq/k7kjNyebmV2dCYvuDz+T3P/ck/Pfc+feieN0CM/zzsEBPd5FyPi+P8KzTwvTQNDhViyVShd4fiiXywv1nDmGGJlRHzxCA55S3zAx8/Sk3yDw/CycNAZX6DlzCdd1/Ra1CaUBk5i8pefEgolH/aD987XWJchgbFheI/lZi7Gga2U91oUQY52b++dAm/ewHUfhTzgOx+CzarW6Bi7n5xscOBv1vChks9kl5G3q8SRg3r2P0NWahpxO2+FbOGiflpVKZR0FvmD8OXyVz+dX2RPjkLLBr/AbNa7Xmg0xd0K6lsvlFmtRgOF9fnBK3XQ63OtpGSwWi4ukZjgRlV9OoJNSPBzSYghiquhjGD2mtTikZZCcB6nniVn4nVqfAq3dZNr8hRWpaz0EulxAL/+k0DQMyslOzsdysZvmDOqYsMUPJYDAs1q3UavVVpPwmhw0WotDCgbllbosHeR5xXTwlA6SrmyB3/2YPfy3SNogp/g2ar4nDRJjpoMXdZwYPG7EUQJXaj0pJGnQXFUP4Ab5Xc4E8SBbVceKwevG4F0n+mTMNBqNpfLUQjtI0qAfnPaX6GKep0veQ8bDI/tqC4On9m9L9xaIqcA7UqjW2kFSBsnRTx3veX4K6QcfI2JwZEZ9DB4wYlQHMyzAaXmhtdAukjDYbDYXUOdtOrfLHpe8jE/Ad9wIOVsLO/MZvuHrpDRNNCDBAPpV+QNaaxdJGGSR91LHkK7DM/ezP9vnGoO74Q/pYr1eX2ZJfYztJ/H9dq8G4rfKYtlbyGwjWcRxPW60MzqPguygzcS+hju0WCgUiiZ/9E0gK0NAvxd8bK914v7176GHHnr4n/ELDVLLJxLOPGYAAAAASUVORK5CYII=>

[image6]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADkAAAAZCAYAAACLtIazAAAC+0lEQVR4Xu2XPWgUURSFd0lExX8wLonZnexmNYhgsyhoKbHUQhMUtYqKoJ2CiiD4Q8DGJopIEEUlhWAhiNYRLJQ0FrFSMIgYFMQqhT9J/E72rXlzJ8nOhkGD7IHDG9559+09c+f9bCpVI3K53DHYafsXENJBELxXa4UIGNgzE7PZ7BXaETv+H0BmIvnBIzIJe21ABAy6LGJqqvU4CUeKxeJiG/M3QQ55Py++rkvu+b7L8aSNiQUCj8JBHhuttkCQxuxzcuzRsxVjoQ3YvgUImZufwf8SlL2Lsg/BCfgFjsIXhUJhI1zF8x3W61YbNxeampqWM+9j218L+JhWM8dVfr9/Ft5E785kMstsrA/tWjvhO7iXSZdUhHw+v4UJXtH/Er5paWlZ6wdWQxImS6XSIvLIMM9ucvgJh5UXbXNra+sGWu3+464/sPGCDJ6GE7O9CUwfQJ/kR+6mavz2kzBZgQqgPGCfkeThxiza1M50xon9VqyAMQX0Ucwet1o1JGyyT7myZPbMoPVKI8d7IYHSbkP4Bj9R9mJI9IDeDF+TbMlq1ZCUSc1DDoPwK9zka+3t7evoG4bjoReAqaV0PnHuL06HRKFJSPSWNh+rVUNSJmXMGRzUnJV+Ligr6RuAP+CJlL+c6NgOv8Ox+VQoLhI0eUgFgc+Yr0uUKfgRXoPNNkZBp1zQEAFrrJ4UEjR5G04wV7cMVchX+JD2M+3+lN0UXZBMDkTEMNIdHR0r1FohDpIwqSKoGHBUm6CvufNbx9s42i5f+7NTRXYjg6B8KX7gr4NakJDJEjmMwaf+GV6BPMgL486GhGD6G5+rkmkmOEfwYSvERRImdXS5XM9bzdt1VbDwEecqpEX7lltMNiQ6kFwn+nXdOKwWFwmYbCSHR0F5PUb+uGstSoPDOgWsLqP7gvJ1aEBbsSc10HdQizruscH4HXphJPLBp3uRv2y/0y7YeSw46tYzbkTUs6/phubyHyLXNl+LQJXiRzfnytuyDtoGO6aOOuqoo44q+A25lexUzbWBegAAAABJRU5ErkJggg==>

[image7]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEoAAAAZCAYAAACWyrgoAAADIklEQVR4Xu2Wz2tTQRDHX7GCoqKoMbZJ89JEDcWDlihe1JNCe6iCCgr21oM9K/6gVxE8CRUCIl4URLBFBGmRFlTwoH+ApScPiiAqXgQLUvzxGbMbJ9PXpC20afF94cvbNzP7dvb7dnY3CGLEiBEjRoz/Btlsdk0Yhhfhbc9MJnMd+yYdh71Px+AfhFkdI0ilUulkMrnO2hcTpLFX56bYrWIEg8bfp78DmvL5/DahtI0vaGJyW9ra2o7R8TucdoGrdIyMgu+OixkgPu9jisXi6nQ6vRN7CX5B6KLqu+iQn93e3h7yfMj4v+FzySeRSKzXMfA4vs/wGe0u49+OfZznGM8H8HUul9vl/RUwUBLnWxHC+gQiBr578KS28+F+hHkPn+KbkP5LLZQHY58XocjprvUJ+LmHyO0xAmzUdkRdS78nIrTME1MT71fhG7do/kHUxfFCBqpyODBAL74SzWbr85AEGykU4/ZI/jIPvVoEUjXYR2GntgvodwT7L3hC2Q7y/oM5ndGxAlHxfpRQBEvZjcjytj6NRgvFitnP+FNworW1dav2kdsV8roUROw9xF9zAlf2NJmDzEU0CWwfjDetUK7kSgx0Wtuj0GihGLsDfoUf5FAxvke25Bya8Q3bvFkUe7B9CyNWZ6XGtU02eT5wy9VuTTRaKL3P6hxkouR2QMd6qC2nqg/vLfCdY4vuU6lxf7zLX2GAMTa7HVWBs6DRQul9NlRlRPtCYMvHYUFC+Rp39S1LskTn3qqgGpiPUIifIu7UXCnx9hsRqOyz5HJODLQ74agN9FiQUKGrcVlJdDpKe0iOzqqgGlgGQlX2WTjgjv0hmYuN88iWL9wjNm/aOWwf4ctCobBB99EqdsNxgndXBdTBfIRaLDD2ZRFKcoH9tG8Es5SdhxeXvj3eJnOQucDhwF6J1DKcDMt1PS8sE6H8XeoTfGVPvyiQd1dYvked9baMu1v5Erb4e1TiHJtxJNaH3x+mZK+zzqVCpnxRnIY/5cS2/ij4BZKdeTOfnFVonB3WtpIg+5LsSTw3W189iEiIu49rxuG5XIdixIgRI8YKwx+yOw+3O6i0EgAAAABJRU5ErkJggg==>

[image8]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEoAAAAZCAYAAACWyrgoAAADkklEQVR4Xu1XS0iUURT+hwyKip426Yxz51GJRJRN0aaiRYIuLKigIHcuap1o4TaiIIgMhKgWPRAhLQLRRUIFLQrcFkLQohCiohZBQdhk39d/78yZ44yNwj8Q/R98zP3P4z7OPefeO54XIkSIECFC/DdIJpNLjDHd4HXHRCJxEfJV0g7yTmkDfR+YdHraQ74jHo9vYp/CNXBgvO1yboJtwoboU/pO2Q8QyWQy60m2lc6LxGKxtQ0NDQfh+A2ctoaLpA1Hge6mtemFfYY20Wh0Gb5vgBMwuY3fj2AOwb6QzWYXiz4CAzcmlUoZ/N7D2DPgE25YbW3tcmkDHrLze4x2q9JvgHwcv4/wOwi+SKfTm50+DwwUhfINA6F1BBcN3R3wiJTjux9B6fHsDmCCS/F9ixPG7xUnrwYw5mmOyw3TOgKbuxdzeogArJRyzhl+Iwy03dwIvs+BL23SFMDoQvGUAxUpLDBAB4OCZo2UQ/Ye/IRAb3My2GYZcPAz2CTtgwTGbef8uQ6ZLQSrBvIxsFnKCfgdgPwXeFjI9uD7B4J3XNoSjOJAqUDBmGU3yvTWOsg/2MnlzwO068C3YA6u+4V5oEDG7MKY38FX9fX166QO8zgrM18C9uf1GsRmD3jaB8KrdJAyW3L9GOiYlDsgbbcy+p440/CdNn6mfZWZFjQwXpPxs3gKGRRXuge65CxqoBtmUBgcJ+S8OX9TIjvzNS5lPOTRwbX5HMy2TGfAEda/1gcFec7KRXOh2Ojd0tZBHDlFPqZQFWSd9MnXOG8yfnNXeAtgsRuLDOcAfdDHJPp6XapUg4Q8Z03xUdDl6fKxWFCgXI3b+mZK8kbrKDKaA/apcBec0KmvgeDH0PfRSkl73UcJ5M9ZbPBJCtBuBse0ocOCAmVsjXORcGpBe6jS0rFnGR9x9+GzRus1AgpU/pwFe+21P8S1aDuHpP/gHtWBShTO2WeNjY0rpI+MYhs4DuMtRQZlYIN0iZRnGScrB68GMN4ZBopvKfAU2pe9MmXnYGxw4dvuZJw3gwcOe+pJJNNw0vh1XQmY7l1gtyduPtvXIHdG2AYOLpaLNv6z5fnfjgACAW01/jvqhJMl7NvKlbDGn6uSB/isK7EM0GEPfH6CU2i/c8T3F3AC7dXaJ0gk/IfiNJjjja31peASJDn7ZT5ZNtCmii/pIGD/QrVUck5qMEgI7k7c1vvm8xwKESJEiBD/CH4DkKAoIBMCdF0AAAAASUVORK5CYII=>

[image9]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA0AAAAVCAYAAACdbmSKAAAAyElEQVR4XmNgGAWjACswNjZmlZKSEkEXhwEZGRlOIBZCF2dQUFCQkJeXXy8nJ+cCEwMZBhSbDMQ5QC4jknIEUFRUBMrL7wdiK5AGoAEzgLiMAZcGGIBqPALE3UBcykBIAwwoKSmpATVMAtmGLocLMIKcBMQ7gRo10SWxAZiGGbKysiZA+iDIueiKkAEjKJSAeI64uDg3SACowQwUMDg1AiWDgXgp0D/8aOJWQLweFCXI4gzS0tLCQMEGdA0wANTkBJRPRxcnGgAABPUiS/6gP4QAAAAASUVORK5CYII=>