# **Set Vector Parameter Value 노드의 역할**

언리얼 엔진 블루프린트에서 **Set Vector Parameter Value** 노드는 **머티리얼 인스턴스(Material Instance)** 내에 정의된 \*\*벡터 파라미터(Vector Parameter)\*\*의 값을 런타임(게임 플레이 중)에 동적으로 변경하는 데 사용됩니다.

## **1\. 주요 기능**

이 노드는 주로 캐릭터의 색상 변경, 라이트의 강도 조절, 혹은 특수 효과의 색상을 실시간으로 업데이트할 때 사용됩니다. 머티리얼 에디터에서 생성한 'Vector Parameter' 노드와 통신하여 데이터를 전달하는 통로 역할을 합니다.

## **2\. 노드 구성 요소 (Inputs)**

| 입력 항목 | 설명 |
| :---- | :---- |
| **Target** | 값을 변경할 대상입니다. 보통 **Primitive Component**나 **Material Instance Dynamic (MID)** 객체가 연결됩니다. |
| **Parameter Name** | 머티리얼 에디터에서 설정한 파라미터의 정확한 \*\*이름(Name)\*\*입니다. 대소문자를 구분하므로 정확히 입력해야 합니다. |
| **Value** | 새롭게 설정할 벡터 값입니다. 일반적으로 ![][image1] 형태의 **Linear Color** 구조체를 사용합니다. |

## **3\. 작동 프로세스**

이 노드를 성공적으로 사용하기 위해서는 다음과 같은 단계가 필요합니다:

1. **머티리얼 설정**: 부모 머티리얼에서 특정 색상 노드를 우클릭하여 Convert to Parameter를 선택하고 이름을 지정합니다 (예: BaseColor).  
2. **다이나믹 인스턴스 생성**: 블루프린트의 BeginPlay 등에서 Create Dynamic Material Instance 노드를 사용하여 해당 머티리얼을 동적 인스턴스로 변환합니다.  
3. **값 변경**: 게임 로직(예: 피격 시, 버튼 클릭 시)에 따라 Set Vector Parameter Value 노드를 호출하여 새로운 색상 값을 입력합니다.

## **4\. 주요 활용 사례**

### **🎨 캐릭터 커스터마이징**

플레이어가 옷의 색상을 선택하면, 해당 색상 값을 벡터 파라미터로 전달하여 캐릭터의 외형을 즉시 변경할 수 있습니다.

### **🔥 환경 및 효과 변화**

* 독 안에 들어갔을 때 화면이 초록색으로 변하는 포스트 프로세스 효과.  
* 체력이 낮아질수록 캐릭터 몸에서 붉은 빛이 강해지는 이미시브(Emissive) 효과 제어.

### **🗺️ UI 피드백**

게이지 바의 색상을 체력 수치에 따라 녹색(![][image2])에서 적색(![][image3])으로 부드럽게 보간(Lerp)하여 변경할 때 사용합니다.

## **5\. 주의사항**

* **정확한 이름**: Parameter Name에 오타가 있으면 노드가 작동하지 않으며 오류 메시지도 표시되지 않는 경우가 많습니다.  
* **MID 필수**: 일반적인 Material Instance Constant는 런타임에 값을 수정할 수 없습니다. 반드시 **Dynamic Material Instance**를 생성하여 타겟으로 지정해야 합니다.  
* **성능**: 매 프레임(Tick)마다 너무 많은 파라미터를 수정하는 것은 성능에 영향을 줄 수 있으므로, 필요할 때만 호출하거나 타임라인(Timeline)과 함께 사용하는 것이 좋습니다.

**Tip**: 3개 이하의 숫자 데이터(예: 단순 스칼라 값)를 조절할 때는 Set Scalar Parameter Value를 사용하고, 색상이나 좌표값 같은 4개의 컴포넌트 데이터는 Set Vector Parameter Value를 사용하는 것이 표준입니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFgAAAAWCAYAAABEx1soAAAEuklEQVR4Xu2YTYgcRRTHZ0gExS9ElyX7VbPD4qIRFRYjioIHleQQEfwEPYiXGDGXhRCIIoqIiCAY0EMMSJSwKBGVGARdJMSDHwl6MUSCAZUlIRtUFLKocRN//+7qmeo33T3dO3Nz/vCndt6r9+p11atXVVurDTDAAAMMMMD/CaOjo2PT09OXWvkAPWJsbOyiiYmJjxqNxhNWNzw8fLFz7mm4Fy7D8/ATuBObr73s6Pj4+E3WtiyazebV+PgS/ksMu2nXTE5O3kC7jzFeon0f7qFr3doWAZtbFSf8wcetvyMyzru0S/APuMHarhTEexf+lq3wUYRfkcFXphQB0F8Df4VHRkZGrkrkBNpAdhyexs/a0KYE6tg8gO0/cF47KFQqHtx/g+487aZQVwXY75APK8fnzcjPrDD2DpAol+PzYGosCTW58KmgbwfI0HtkSCBv1YJMGhoaugT5AengbGDSFfS/z8U7YF5xWL3AeNvQn6W9zerKwO/Aefin1akcIv/Cf9dGq68KVQB8HYJLLaECR3CKdibo2wH6vKhAbCYpm5EfydIVgRKwDpvf4XHtAqtPoA+X/3DXVAH2TexP6sMLdH/DW6y+CiizU/j43MVlaaGl8BN3iMGuCPqnEGTpGbsQzM165OfgUbvF86CaT/99Ls76wp3jJ3gvf662ujLAdoMfZ4dR1ZG94HU7Z2ZmLjD6KpCvV32pXQN/jqRMzoX82N/tA4KVTmWSVg3dMeQ/4uvG0KYIwaIswEmrD8EYo2S7s/KycH7nwUcS2dTU1GX8fgP+Bbf2OLkqn7fj5wMlYpKMkUJZ6+KasSttkkZQf3+hfdO1T+OfkG1RRlqbIjh/6LguC9srgvqrsRZ9/ItwmUV+T4ljbVYC/M1pkvV3aoKdT2cG2hYaWLh2/d3sbaJrlAKukrlCL4diVYT1NyyBvkTtcvHt5claxeufBT5er7UTReVCV8pyE1xUf5GdhXO1ClmYjOlyTm5dzVx8q4l2CX0+pD2scmT7doNr19+OHRpM/imSZdrqy0LnDj5m8Xd/QpLuYKRMPrZogl3O/dfrNMEHtAihvAjKJBeXpcwJVj30QUeHIMG+pjhRrbJ9u8Fl1N9Alyx0R+JUQJ34nnfBA8bzcKQNBtmetmsj7/4r+OD367AM5V2w2sWvQtl2fLgQ3E/7cv9VObN6+ZV/V+KgzYMWhm//NEPeTlgX16J5BRT0ieC367dwKeMprIlShu3WD/TXOV+LaGelywteWerilZb9c/YUR77H23ccgkzWsItfjpFtqAvh4keMTYw69tc3/OsQPmPHTvwX+faLpyved9xIhqyeMR9s/cDRJjouhHdYZHe49v8dQp4Ir0wuPiS+h4/Dz9Ctk9xnh56gv+VtP3/QvOL9LmqhnP8fAXwYuy20j+XYvQ3P+ckLsQr5O95nHhXzx9hea2wjJP4zfAtZ/lsvRGzudPH1s/1UdnGNPcHHrG8JS0IZjtON2N7r4jqZgrZK3gQn0BOZ7L97wh8Qisf2yQJ9m4z7spX3C/30rS2tMlHpNtANOviUBXooWF0/4M+GrVbeL/TVNxOxFh7rlm1VQAY8hL9naz3eMbPg/0E1t5KrWxnIf6++/wN6LJ4B/VO+iQAAAABJRU5ErkJggg==>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAC4AAAAXCAYAAAB0zH1SAAACFklEQVR4Xu2Wv0scQRTH90gKA/nBES5B7sfe7l1ypYSUaptOi0QxqL29IHYhjUU6/4CAxMbaQmxSCEKIXps2haBYhFwhaCN4+by4I+Pz7nh7i91+4XEz3/dm5jO3sz+CIFeuXLnuRc1m82mtVlsPw7BDnNIe1zUp9IA55plju1QqPdZJizweYTnoyRPH8TMKvhNfK5XKI/Fo/6lWq9O6tp/q9foIY1aIn8QF0SX2hgH3eaQPx7uePCz6kcQZu3rrPNobAiGT+LUD9JCJJ9n4K8ZFxNGw4D14Cnd4kn9qJ1lo1A2mcJX+Jb8TzrNK5hkW3MzjLdLGLKrCLrHsPKuygJt5oigao3OmF6G/kBSuOc+qLOBmHjlHdM51If6UFHLpvjnPqizgZh5zYQrl4IN4MGKMU2K/1Wo9cYUULEmh3BTOsyojuI2HRhGjnSykHz9d4r3zrMoIbueh84bokFz0vF/012kWpM/d/hLvtwxm959dXS/xkqhRd0L8EBCdTwCOiUjnRJqn0Wi80DxO8maaJfmXWAuvHz27/ltTPgXwNokrajf8wU5y/hKoO+EfufD6DAvYzZtayedZYN7DUPHcUrlcfs6AKWImUDtzIhcz0Rftp5VsZAD4fyU8MwC/DvrwmCUfOux+RftpJOderhpXsaxz96Lkq22LBZs6l0ZcsTnAPwVZ/0Wr5KYE/IP204o5NvueV6P+Ab3P5ZyOmWOvAAAAAElFTkSuQmCC>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAC4AAAAXCAYAAAB0zH1SAAACi0lEQVR4Xu2WOYgUQRSGe3ADxQuPcdi5ai6cwEDFTA1ExEwDV1EYc40F0UQxWdBsM5MF0UQFEwMRxUAw8YgEDTcQFBHRYMFIdP3+6ereN+X0OgcLBvPDo7v+eu/V33W9jqIJJphgglVFqVQqV6vVhyE/IHLOucPYW+wz9oVc1wuFwvrQ8V9otVqbiJ1THp6vsAOhT1Sr1fZg93H6hP3ClkKfQUDcjBe8X20JZsB75H6az+c3hP5ZaDQam8nxDJtXu1KpHOX9K8/jPY7lcrlE8kPM9jael0cR7gd7idBbNHMJT3sf/CJ5zxj3FSFfxSjWUznlVX6N0+OcAIdLowgn7ghxvxVvebhp7AP2gOaU7esHRK/F95GPmU54r+snz4PWP8UYwrtxWMfyRvhCvV4v2L5+MP5vyLkl4U3+C9Y/xajCiZlXHPHHLK/BJcLFh6xh+/qBj9uN7yL23J4L2h0vfNb6pxhVOEt8u59wDS4R2I/q8p7NhHzkGwpXXuXXONY/xUS4x38vnJhZxWEnLF8sFrfDvXfBLZEFxm+4uBa8aLfbGxMewef8xPTcWinGEN49PGFiifWie4Rkobp8mPtdh0sumJgEKtlX+gnXVQa/oD6+/lrYD6ZIfpO+1ypkCUn7vItL/66E8wI+YvWEs4Dfi30n5qzazWZzB+13tOciU9zsvlJCa+lXU13X8X7HxUVG1fEv4LOVvicS7+IV0Pb55oJZcvFYEpa151UpT/nYjs/3OLNqDgLtQRLdCHmDNRKEndQ/RtYPlpZ+BeFdaOWUB8E7IzvTo0A/Onz9xZAfBrottGr6Rwr7VgX+R+ouA7bCvmHAip1G+NVo3FkcFDqUCJ8J+WGhszLWfgV/ANvX3R3gTwRmAAAAAElFTkSuQmCC>