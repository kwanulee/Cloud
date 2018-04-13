## Linux 파일/디렉토리 접근권한 조정하기

리눅스는 멀티 유저 시스템이므로, 여러 사람이 혹은 여러 그룹이 하나의 파일이나 디렉토리를 접근하여 읽거나 쓰거나 실행시킴니다. 따라서 소유자 혹은 그룹에 따라 파일 혹은 디렉토리에 대한 세밀한 접근 권한을 부여함으로써 파일 시스템에 대한 보안을 강화하고 있습니다.

---
### 파일 정보

```
drwxr-xr-x  2  root  root  4096 Apr 22 16:59 conory
```
- 파일타입: **d** (디렉토리), **-** (일반파일)
- 권한정보 
	- 권한대상: 소유자,그룹,기타사용자
	- 권한종류: **r** (읽기), **w** (쓰기), **x** (실행)
	- 예제
		- rwxr-xr-x
			- 소유자: 읽기,쓰기, 실행
			- 그룹: 읽기, 실행
			- 기타사용자: 읽기, 실행 
- 링크수: 해당 파일이 링크된 수
- 소유자: 해당 파일의 소유자
- 소유그룹: 해당 파일을 소유한 그룹이름! 특별한 변경이 없을 경우 소유자가 속한 그룹이 소유그룹으로 지정
- 용량: 파일의 용량
- 생성날짜: 파일이 생성된 날짜
- 파일이름: 파일이름이죠


---
### 사용자를 그룹에 추가하기 (useradd 혹은 usermod)

#### 새로운 그룹 생성하기
- 형식

	**groupadd** [옵션] 생성할_그룹
- 예제

	```
	sudo groupadd www-group
	```
	- **www\-group**을 새로이 생성

#### 기존 그룹에 사용자 추가하기
- 기존 사용자를 기존 그룹에 추가

	```
	sudo usermod -a -G www-group ec2-user
	```
	- 기존 사용자 ec2-user를 기존 2차 그룹인  www\-group에 추가


- 새로운 사용자를 기존 그룹에 추가

	```
	sudo useradd -G www-group kwlee
	```
	- 새로운 사용자 kwlee를 기존 그룹인 www\-group에 추가

---
### 소유자 변경하기 (chown)
파일의 소유자를 다른 사람으로 변경

- 형식

	**chown** [소유자][:그룹] 변경할_파일
	
- 예제

	```
	sudo chown -R ec2-user:apache /var/www
	```
	- **/var/www**의 소유자를 **ec\-user**로, 소유그룹을 **apache**로 변경

---
### chmod
- 파일이나 디렉토리의 모드(권한)을 변경할 때 사용
- 파일 소유자나 슈퍼유저만 모드를 변경할 수 있음

- 참고링크 : https://www.ibm.com/support/knowledgecenter/ko/ssw_ibm_i_73/rzahz/rzahzchmod.htm



---
### find


