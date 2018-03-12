## Amazon EC2 개요

---
### Elastic Compute Cloud (EC2)란?

- 가상 서버를 필요할 때, 필요한 만큼 사용할 수 있고, 사용한 만큼만 비용을 지급하는 확장식 컴퓨팅 서비스
	- 하드웨어에 선 투자할 필요 없어, 더 빠르게 애플리케이션을 개발하고 배포
	- 원하는 만큼 가상 서버를 구축하고, 보안 및 네트워크 구성과 스토리지 관리가 가능
		- AWS Management Console, AWS 명령줄 도구(CLI) 또는 다양한 언어의 SDK를 사용 가능
	- 갑작스런 수요 증대 등 변동사항에 따라 확장하거나 축소할 수 있어 트랙픽 예측 필요성이 줄어 듦

<iframe width="560" height="315" src="https://www.youtube.com/embed/TsRBftzZsQo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

---
### EC2 주요 개념 및 기능
- [인스턴스, 인스턴스 유형](#instance)
- [인스턴스 요금](#price)
- [EC2의 스토리지](#storage)
- [아마존 머신 이미지 (AMI)](#ami)
- [인스턴스 생명주기](#lifecycle)
- [리전 및 가용 영역](#region)
- [인스턴스 로그인 정보 보호](#login)
- [보안그룹과 방화벽 기능](#sg)

---
<a name="instance"></a> 
#### 인스턴스, 인스턴스 유형
- **인스턴스**: 가상 컴퓨팅 환경
- **인스턴스 유형 (Instance type)** : 인스턴스에 사용되는 호스트 컴퓨터의 사양 (CPU, 메모리, 스토리지, 네트워크 대역)을 정의한 것
- 인스턴스에서 실행하려는 애플리케이션에 따라 [인스턴스 유형](https://aws.amazon.com/ko/ec2/instance-types/) 선택
	- 범용 인스턴스
		- T2
		
			<img src=images/t2-instances.png width=400>
			- 기본 수준의 CPU 성능 + [성능 순간 확장 가능](https://aws.amazon.com/ko/ec2/instance-types/#burst)
			- 웹서버, 개발자 환경 등, 가끔 빠른 성능이 필요한 경우
		- M4
			- 최신 세대 범용 인스턴스로, 컴퓨팅, 메모리, 네트워크 리소스를 균형 있게 제공
	- 컴퓨팅 최적화 인스턴스
	- 메모리 최적화 인스턴스
	- 가속화된 컴퓨팅 인스턴스
	- 스토리지 최적화 인스턴스

---
<a name="price"></a> 
#### 인스턴스 요금
- **온디맨드 인스턴스**
 	- 장기 약정이나 선결제 금액 없이 시간 단위로 사용한 인스턴스를 기준으로 비용을 지불하는 방식
	- 온디멘드 인스턴스가 적합한 경우
		- 선결제 금액이나 장기 약정 없이 저렴하고 유연하게 Amazon EC2를 사용하기 원하는 사용자
		- 단기의 갑작스럽거나 예측할 수 없는 워크로드가 있으며, 중단되어서는 안 되는 애플리케이션
		- Amazon EC2에서 처음으로 개발 또는 테스트 중인 애플리케이션

- **예약 인스턴스**
	- 저가의 요금(온디맨드 요금의 최대 75%)을 일시불로 선결제하여 1년 또는 3년 계약 기간 동안 인스턴스를 예약하고 해당 인스턴스를 매우 저렴한 요금으로(시간 단위) 사용하는 방식
	- 예약 인스턴스가 적합한 경우:
		- 수요가 꾸준한 애플리케이션
		- 예약 용량이 필요할 수 있는 애플리케이션
		- 총 컴퓨팅 비용을 절감하기 위해 1년 또는 3년 동안 EC2를 사용하기로 약정할 수 있는 고객

- **스팟 인스턴스**
	- 특정 인스턴스 유형을 실행하기 위해 지불할 의사가 있는 시간당 최고 가격을 지정하는 방식
	- 스팟 가격은 Amazon EC2에서 책정, 스팟 인스턴스 용량에 대한 수요와 공급에 따라 주기적으로 변동
	- 스팟 인스턴스가 적합한 경우:
		- 시작 및 종료 시간이 자유로운 애플리케이션
		- 컴퓨팅 가격이 매우 저렴해야 만 수익이 나는 애플리케이션
		- 대량의 서버용량 추가로 긴급히 컴퓨팅 파워가 필요한 사용자

- **전용 호스팅**
	- 전용 호스팅은 고객 전용의 물리적 EC2 서버
	- Windows Server, SQL Server, SUSE Linux Enterprise Server(라이선스 약관에 따름)를 비롯한 기존 서버 한정 소프트웨어 라이선스를 사용할 수 있으므로 비용을 절감 가능
	- 기업 규정 준수 및 규제 요구 사항을 충족하는 인스턴스 배포 가능

---
<a name="storage"></a> 
#### EC2의  스토리지
- **EBS**
	- 높은 가용성과 내구성을 가진 스토리지
	- 인스턴스 수명에 관계없이 지속됨
- **인스턴스 스토어**
	- 인스턴스 전용의 일시적인 스토리지 (인스턴스 정지 혹은 삭제 시엔 복원 불가능)

|    | EBS | 인스턴스 스토어 |
|--- | --- | --- |
| 부팅시간 |1분 이하| 5분이하 |
| 크기제한 | 16TiB | 10GiB |
| 데이터 지속성 |Amazon EBS 볼륨의 데이터는 기본적으로 인스턴스 종료 후에도 유지 |모든 인스턴스 스토어의 데이터는 인스턴스 수명 주기 동안만 유지 |
| 내구성 |99.5%~99.999% |EBS보다 낮음|
|성능|랜덤 액세스에 강함|순차적인 액세스에 강함|
|용도|OS나 DB등 지속성과 내구성이 필요한 스토리지|임시파일, 캐시, 등 삭제되도 문제 없는 스토리지|

** 출처: https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/Storage.html

---
<a name="ami"></a> 
#### 아마존 머신 이미지 (AMI)
- **아마존 머신 이미지 (AMI)**는 필요한 소프트웨어 (예, 운영체제, 애플리케이션 서버, 애플리케이션)가 이미 구성되어 있는 템플릿
	- 하나의 AMI로 여러 인스턴스를 실행시킬 수 있음

	![](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/images/architecture_ami_instance.png)
	
- 공개된 다양한 AMI
	- AWS에서는 자주 사용되는 다양한 AMI를 공개하여 게시하고 있음
	- AWS 개발자 커뮤니티 회원들이 올린 자체 구성 AMI
	- 사용자 정의 AMI

- AMI 종류
	- **인스턴스 스토어 기반 (Instance Store-Backed)** AMI
		- AMI의 인스턴스가 실행되는 [루트 디바이스](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/RootDeviceStorage.html)가 아마존 S3에 저장된 템플릿에서 생성된 인스턴스 스토어 볼륨인 경우 
			
	- **아마존 EBS 기반 (Amazon EBS-Backed)** AMI
		- AMI의 인스턴스가 실행되는 [루트 디바이스](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/RootDeviceStorage.html)가 아마존 EBS 볼륨인 경우 


** 출처: https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/ComponentsAMIs.html

---
<a name="lifecycle"></a> 
#### 인스턴스 수명주기
- **인스턴스 스토어** 기반 인스턴스는 **중지 후 시작을 할 수 없음**, 반면에 **EBS** 기반 인스턴스는 **중지 후 시작 가능**
- **running** 상태 있는 동안 비용 청구, (주의, 인스턴스에 접속하지 않아도 비용청구됨)
- **stopped** 상태에서는 인스턴스 사용 요금, 데이터 전송 요금은 부가되지 않으나, EBS 볼륨에 대한 요금은 부과됨
- 인스턴스 **재시작(중지 후 시작)**시, 
	- 이전 호스트 컴퓨터의 인스턴스 스토어 볼륨에 있는 데이터가 모두 손실
	- 모든 Amazon EBS 볼륨이 인스턴스에 연결된 상태로 유지되고 해당 데이터도 남아 있음
	- 프라이빗 IP 주소는 유지, 퍼블릭 IP는 변경됨 (EC2-VPC 인 경우)
- 인스턴스 **재부팅**시, 인스턴스가 동일 호스트 컴퓨터에서 유지되며 해당 퍼블릭 DNS 이름, 프라이빗 IP 주소 및 인스턴스 스토어 볼륨의 모든 데이터가 그대로 유지됨.

![](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/images/instance_lifecycle.png)

** 출처: https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html

---
<a name="region"></a> 
#### 리전 및 가용영역
- **리전**
	- AWS의 각 서비스가 제공되는 지역, 예: 서울, 미국동부 등
	- 리전에 따라 사용할 수 있는 서비스가 다름

- **가용영역**
	- 독립된 데이터 센터
	- 모든 리전에 두 개 이상의 가용 영역이 존재
	- 한 리전의 가용 영역들은 고속 회선을 통해 연결됨
	- 복수의 가용 영역에 걸쳐 인스턴스 배포시, 단련적 IP 주소를 사용하여 한 가용영역에서 발생한 인스턴스의 장애를 다른 가용 영역의 인스턴스로 마스킹 할 수 있음. 

![](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/images/aws_regions.png)

** 출처: https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/using-regions-availability-zones.html

---
<a name="login"></a> 
#### 인스턴스 로그인 정보 보호
- AWS에서는 공개 키 암호화를 사용하여 인스턴스에 대한 로그인 정보를 암호화 및 해독합니다.
- 키 페어는 **공개키**와 **개인키**로 구성된다.
	- 사용자는 개인키(프라이빗 키)는 디지털 서명을 생성하는 데 사용
	- AWS는 공개키를 사용하여 디지털 서명을 검증

	<img src=images/public-private-key.png width=500>
	
- 인스턴스 로그인 과정
	1. [키 페어 생성 (준비작업)](create-key-pair.html)
	2. 인스턴스 시작시 키 페어 이름 지정
	3. 인스턴스 연결 시 개인키 제공

---
#### 보안 그룹 생성
- 보안 그룹은 인스턴스에 대한 방화벽 역할을 하여 인스턴스 수준에서 통신의 입출력을 제어하기 위해 있는 기능
	- 서버에서 나가는 통신 (아웃바운드) 포트는 모두 열려 있음
	- 서버에 들어오는 통신 (인바운드) 포트는 기본적으로 모두 닫혀 있고, 필요한 포트만 열어 사용

- 여러 리전에서 인스턴스를 시작하려면 각 리전에서 보안 그룹을 생성해야 함.
- [**실습**] [보안 그룹 생성하기](create-security-group.html)
