# **Destroy Actor 노드 상세 가이드**

언리얼 엔진의 블루프린트에서 Destroy Actor 노드는 게임 월드 내에서 더 이상 필요하지 않은 액터를 제거하는 데 사용되는 핵심적인 노드입니다.

## **1\. 주요 역할**

Destroy Actor 노드가 실행되면 다음과 같은 일들이 순차적으로 발생합니다.

* **월드에서 제거**: 액터가 게임 화면에서 즉시 사라지고, 물리 연산이나 충돌 판정 등 모든 월드 내 상호작용이 중단됩니다.  
* **생명주기(Lifecycle) 종료**: 해당 액터의 EndPlay 이벤트가 호출됩니다. 이때 EndPlayReason은 Destroyed로 설정됩니다.  
* **가비지 컬렉션(Garbage Collection) 예약**: 액터가 'Pending Kill' 상태로 표시됩니다. 이후 언리얼 엔진의 가비지 컬렉터가 실제 메모리에서 해당 데이터를 정리하게 됩니다.

## **2\. 주요 특징**

* **Target 입력**: 소멸시키려는 액터의 참조(Reference)를 연결합니다. 기본값은 Self로 설정되어 있어, 노드를 호출한 본인 액터를 파괴합니다.  
* **자식 액터 처리**: 특정 액터를 파괴하면, 그 액터에 부착(Attach)되어 있던 자식 액터들도 함께 파괴될 수 있습니다. (설정에 따라 다름)  
* **즉각적인 비활성화**: 노드 호출 즉시 게임 로직 상에서 해당 액터는 '무효(Invalid)' 상태가 됩니다.

## **3\. 자주 사용되는 상황**

1. **전투 시스템**: 캐릭터의 체력(![][image1])이 ![][image2]이 되었을 때 캐릭터를 제거.  
2. **발사체(Projectile)**: 총알이 벽에 부딪히거나 일정 시간이 지났을 때 제거.  
3. **아이템 습득**: 플레이어가 아이템 상자에 닿아 아이템을 인벤토리에 넣은 후, 필드에 놓인 상자 액터를 제거.  
4. **성능 최적화**: 플레이어로부터 너무 멀리 떨어져서 더 이상 보이지 않거나 필요 없는 액터를 제거하여 리소스를 확보.

## **4\. 주의사항 (Best Practices)**

* **유효성 검사 (IsValid)**: 액터를 파괴한 직후나 다른 곳에서 해당 액터를 참조할 때는 반드시 IsValid 노드를 사용하여 액터가 존재하는지 확인해야 합니다. 이미 파괴된 액터의 함수를 호출하려고 하면 에러(Accessed None)가 발생합니다.  
* **메모리 관리**: Destroy Actor는 즉시 메모리를 해제하는 것이 아니라 가비지 컬렉션 대상으로 등록하는 것입니다. 따라서 아주 짧은 순간 동안은 메모리에 남아있을 수 있습니다.  
* **Replication (멀티플레이어)**: 서버에서 Destroy Actor를 호출하면 해당 액터가 Replicated 설정이 되어 있을 경우 모든 클라이언트에서도 자동으로 파괴됩니다. (일반적으로 파괴 로직은 서버에서 실행해야 합니다.)

## **5\. 블루프린트 예시 구조**

\[Event AnyDamage\] \-\> \[Health \- Damage\] \-\> \[Less or Equal 0?\] \--(True)--\> \[Destroy Actor\]

위와 같이 로직의 마지막 단계에서 액터의 생명 주기를 마감하는 용도로 주로 배치됩니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACEAAAAZCAYAAAC/zUevAAAB/UlEQVR4Xu1UO0gDURC8kAgWgojGT+6Sy6cxlcghIljaiGghdgFbGxGxsbRKoWUqiYJYpAtYBRuL9IKVKYUggoXYBBUkEJ0x7+K79fKxv4GBy5vZvc3b3TOMAAECDIBoNDqSSCSObNsuCq5CjkDb89GKeg7TNMeTyeSx9Gg8gb4Ia1iP0xHOZDKTMC6B73jpNpJaCBqGFlIvmIV2D7bwvE9dT+A4zlAqlZpC7Do8TfABnAdnFM/AL/A6nU6P6rEeIMEyTFXejI+WhvYM1i3LMqXuAvomXwb/BX6GNCmE85IqZFc79wKBhzDk5TkRj8c3VIKKuiFfQC8oX05InSIQfym0DiIwlPkyKRBuchYqNRfQxuC5BRtozZyuxWKxCZzXVBE7utYBrxiGOq9damwPtCrYZMuk7gJ6FnxlISxIaGxTC7zpOhMIWqGJvbT/Tjav8ROs8R/JWBfQc3a7FQUhhXH2wgJwC9NC+wUMeSZAEVs+PFXJS4Z32HSE1B+g7w3PjySen8AGuGb0WE+DgwZTBWxJjcD5OZN37aXh6TlX3JF6X3AObLV+Ppo7bD2TY6AX4PmglzFS7wtt/cpS44tZQL/kvCWVQ87DYGCgSnAgNTc5+210n4ef9aav24r3BD/JCL6zfdaPn2KcX6kC5cenA3e97fZ6ZqUeIECA/+Ib8oCphI+4q+EAAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAWCAYAAAD5Jg1dAAAA3klEQVR4XmNgGNFARUWFT05OboK8vPw7IH0SiK3R1TAoKSnxAxXsAeI5MjIynLKysm5A9msg7YeiUEFBIQIo8QloijFUiBHIng8UOwEyBKaIAyiwFYgfArEkTDNQYTmQ/xtI24AFQJJQRaeBgoJoCv8DcRFMwBjI+QrEB0RFRXmQFPqCFAJtXEg7hUpAgedAfFhdXZ0XphCoIB2kEORWmEJBkEfksfv6PxAHwcRAPp8EMhVkOj4xkKChPCTqYkB8ZWVlMSD/CpA/AchlhCuEAWlpaWGQJ4CxoYZVwcAAAGi+RqhBZojtAAAAAElFTkSuQmCC>