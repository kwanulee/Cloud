<style>
div.polaroid {
  	width: 400px;
  	box-shadow: 0 10px 30px 0 rgba(0, 0, 0, 0.2), 0 16px 30px 0 rgba(0, 0, 0, 0.19);
  	text-align: center;
	margin-bottom: 0.5cm;
}
</style>

# AWS ElasticBeanstalk
- [AWS ElasticBeanstalk 소개](#1)
- [AWS ElasticBeanstalk 시작하기](#2)
- [AWS Elastic Beanstalk 활용하기](#3)

---
<a name="1"> </a>
## 1. AWS Elastic BeansTalk 소개

<iframe width="560" height="315" src="https://www.youtube.com/embed/SrwxAScdyT0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

https://youtu.be/Ul6FW4UANGc

### 1.1 AWS Elastic Beanstalk 란?
- Elastic Beanstalk는 AWS 클라우드에 웹 애플리케이션 및 서비스를 쉽고 빠르게 배포하고 관리할 수 있게 해주는 서비스
    - Elastic Beanstalk가 지원하는 애플리케이션 타입: Java, PHP, .NET, Node.js, Python, Ruby 등
- 단순히 애플리케이션을 업로드만 하면, Elastic Beanstalk가 용량 프로비저닝, 로드 밸런싱, 자동 크기 조정부터 시작하여 애플리케이션 상태 모니터링에 이르기까지 포를 자동으로 처리
- 애플리케이션을 실행하는 데 필요한 AWS 리소스(예, EC2 인스턴스)를 완벽하게 제어 가능
- Elastic Beanstalk 사용방법
    - AWS 관리 콘솔, AWS 명령 줄 인터페이스 (AWS CLI),  Elastic Beanstalk를 위해 특별히 고안된 High-level CLI
- Elastic Beanstalk 자체의 사용 비용은 무료이나, 애플리케이션이 사용하는 AWS 리소스는 비용을 지불해야 한다.

### 1.2 Elastic Beanstalk 사용 프로세스

![](images/elastic_beanstalk_process.png)

1. 애플리케이션 개발
2. 애플리케이션 버전 업로드
  - 소스 번들 형태
    - Java: .war 파일
3. 환경 시작
  - 애플리케이션 실행을 위한 AWS 리소스 생성 및 설정
4. 환경 관리
  - 새로운 애플리케이션 버전 배포 및 환경 설정 변경


### 1.3 Elabstic Beanstalk 구성요소
- Elastic Beanstalk 애플리케이션
    - 애플리케이션 버전 (Application Version), 환경 (Environment), 환경 구성 (Environment Configuration)의 논리적 집합
    - Elastic Beanstalk에서 애플리케이션은 개념적으로 폴더와 유사

- 애플리케이션 버전 (Application Version)
    - 웹 애플리케이션의 특정한 배포가능한 코드 (예, Java WAR 파일)

- 환경 (Environment)
    - 웹 애플리케이션을 실행하는데 필요한 인프라

- 환경 구성 (Environment Configuration)
    - 환경과 관련된 리소스들이 어떻게 행동해야 하는지를 정의하는 파라미터 및 설정 값들의 묶음


### 1.4 환경 티어 (Environment Tier)
- Elastic Beanstalk 환경의 시작 설정시에, 선택하는 환경의 종류
	- **웹 서버 환경 (Web server environment)**
		- HTTP 요청을 처리하는 웹 애플리케이션을 위한 환경
	- **작업자 환경 (Worker environment)**
		- 애플리케이션에서 완료하는 데 오래 걸리는 작업을 처리하기 위해서, Amazon Simple Queue Service 대기열에서 작업을 가져오는 애플리케이션을 위한 환경

#### 1.4.1 웹 서버 환경
- 아래 그림에서 파란색 실선으로 표시된 것이 환경
![](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/images/aeb-architecture2.png)

- 환경을 생성하면 **Elastic Beanstalk**가 애플리케이션을 실행하는 데 필요한 리소스를 프로비저닝합니다.
	- 위 예에서는 ELB 1개, Auto Scaling 그룹 1개, EC2 인스턴스 4개

- **호스트 관리자(HM)**라고 하는 소프트웨어 구성 요소는 각 Amazon EC2 서버 인스턴스에서 실행됩니다. 호스트 관리자는 다음 역할을 담당합니다.
	- 애플리케이션 배포
	- 콘솔, API, 명령줄을 통해 검색을 위해 이벤트와 측정치 집계
	- 인스턴스 수준 이벤트 생성
	- 애플리케이션 로그 파일을 모니터링하여 심각한 오류 검출
	- 애플리케이션 서버 모니터링
	- 인스턴스 구성 요소 패칭
	- 애플리케이션의 로그 파일을 교체하고 이를 Amazon S3에 게시

#### 1.4.2 작업자 환경
![](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/images/aeb-messageflow-worker.png)

- 부하가 높은 시간에도 애플리케이션이 계속 응답하도록 하려면 **오래 걸리는 작업을 수행하는 프로세스에서 웹 애플리케이션 프런트 엔드를 분리**하는 것이 일반적인 방법입니다.

- 작업자 환경 티어에 대해 생성된 AWS 리소스에는 **Auto Scaling 그룹, 하나 이상의 Amazon EC2 인스턴스, IAM 역할**이 포함
- 작업자 환경 티어를 시작하면 Elastic Beanstalk가 Auto Scaling 그룹의 각 EC2 인스턴스에 선택한 **프로그래밍 언어에 필요한 지원 파일과 데몬을 설치**
- **데몬**은 **Amazon SQS 대기열에서 요청을 가져온 후** 그러한 메시지를 처리할 작업자 환경 티어에서 실행되는 **웹 애플리케이션에 데이터를 보내는 역할**을 담당

<a name="2"> </a>
## 2. [AWS Elastic BeansTalk 시작하기](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/GettingStarted.html)

<a name="3"> </a>
## 3. AWS Elastic BeansTalk 활용하기
- 사전 준비
	- [	PHP 개발 환경 설정](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/php-development-environment.html) 
- [Laravel 애플리케이션을 Elastic Beanstalk에 배포하기](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/php-laravel-tutorial.html)
- [CakePHP 애플리케이션을 Elastic Beanstalk에 배포하기](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/php-cakephp-tutorial.html)
