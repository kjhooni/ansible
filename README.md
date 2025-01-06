# 실습환경
- 컨트롤 노드 1ea
  - control node_OS: Debian Linux 12
- 인벤토리 호스트 4ea
  - web01,web02,db01_OS:CentOS 9
  - web03_OS: Ubuntu20.04


# exercise1
inventory 작성 기초 및 모듈 사용
- exercise1 폴더 생성 및 이동
- inventory 작성(web01)
- ansible ping모듈로 web01로 ping
- host key checking false

exercise2
===========
inventory 작성(그룹 포함)
- exercise2 폴더 생성 및 이동
- 인벤토리파일의 hosts그룹에 web01, web02, db01
- web02, db01에 각각 핑
- 인벤토리파일의webservers그룹에 web, dbservers그룹에 db, dc_oregon그룹에 webservers, dbservers

exercise3
===========
inventory 작성(vars를 활용한 공통 변수 선언)
- exercise2 폴더 생성 및 이동
- dc_oregon에 ansible_user, ansible_ssh_private_key_file 정의

exercise5
===========
ansible-playbook을 이용한 web, db 엔진 설치 및 시작
- exercise5 폴더 생성 및 이동
- 아래의 플레이북 작성
  - webservers그룹 httpd 설치 후 service started, enabled
  - dbservers그룹 mariadb 설치 후 service started, enabled

exercise6
===========
ansible-playbook copy모듈
- exercise6 폴더 생성 및 이동
- web.yaml파일 작성
  - copy모듈 이용. index.html파일 webservers에 복사

exercise7
===========
ansible.cfg파일 작성 및 database 모듈 활용
- exercise7 폴더 생성 및 이동
- db.yaml파일 작성
  - mariadb설치, service started/enabled, accounts db 생성
  - pymysql설치
  - vprofile 데이터베이스 user 생성

exercise8
===========
group vars 및 debug 모듈
- exercise8 폴더 생성 및 이동
- exercise6의db.yaml파일 dbservers그룹에 변수 지정
  - dbname= electric
  - dbuser: current
  - dbpass: tesla
- tasks에 문자가 아닌 변수를 전달
- register, debug 모듈 사용하여 디버그 메세지 출력

exercise9
===========
group vars, host vars 및 debug 모듈
- exercise9 폴더 생성 및 이동
- web03서버 (Ubuntu) 추가 생성
- ping모듈로 ping
- print_facts.yaml작성
  - task: Print OS name
  - debug, setup모듈 사용하여 ansible_distribution 출력

exercise10
===========
조건문(when)
- exercise10 폴더 생성 및 이동
- provisioning.yaml 파일 작성
  - when 사용하여 os별로 task 분기
  - centos에서 chrony패키지 설치, chronyd서비스 start/enable
  - ubuntu에서 ntp패키지 설치, ntp서비스 start/enable

# exercise11
- exercise11 폴더 생성 및 이동
- provisioning.yaml파일 수정
  - CentOS에 아래의 패키지 설치
    - chrony, wget, git, zip, unzip
  - Ubuntu에 아래의 패키지 설치
    - ntp, wget, git, zip, unzip

# exercise12
- exercise12 폴더 생성 및 이동
- provisioning.yaml파일 수정
- 아래의 task 추가
  - template모듈을 활용하여 컨트롤러노드의 templates/chrony_conf, templates/ntp_conf를 
  - 인벤토리 호스트 CentOS의 chrony.conf, Ubuntu의 ntp.conf 덮어쓰기
(CentOS: /etc/chrony.conf, Ubuntu: /etc/ntp.conf)
- 아래의 task 추가
  - ntp, chronyd service restart
- groupvars/all에 ntp서버 변수 추가
  - ntp0: time.bora.net
  - ntp1: ntp.kornet.net
- templates/ntp_conf, templates/chrony_conf 에서 ansible 변수를 사용하도록 수정
- 아래의 task 추가
  - file모듈 이용하여 /opt/test21 디렉토리 추가

# exercise13
- exercise13 폴더 생성 및 이동
- provisioning.yaml파일 수정
- handlers를 이용하여 templates/conf파일이 변경 될 때에만 chronyd, ntp 서비스 restart

# exercise14
- exercise14 폴더 생성 및 이동
- roles 디렉터리 생성 후 해당 디렉토리에서 ansible-galaxy init post-install
- 결론적으로 provisioning.yaml파일을 아래와 같이 수정 후 ansible-playbook provisioning.yaml 실행했을때, 이전과 동일하게 동작 하도록 작업 진행
    
```yaml
- name: exercise14
  hosts: all
  roles:
    - post-install
```
## 팁: VI 편집기에서 공백 제거

명령어: 2칸 공백으로 시작하는 문자열의 공백을 제거

```bash
:%s/^    //
```


# exercise15
- exercise15 폴더 생성 및 이동
- aws에서 실습을 위한 iam 계정 생성 후 bashrc에 계정의 access_key, secret_key 변수 등록 필요
  - export AWS_ACCESS_KEY_ID=’accesskey’
  - export AWS_SECRET_ACCESS_KEY=’secretkey’
- test-aws.yaml파일 작성
  - us-west-2지역 키 생성(이름: sample)
    - 키가 정상적으로 생성됐을 경우 /root/sample.pem 저장
    
  - ec2인스턴스 상세정보
    - sample 키 이용
    - ec2 name: public-compute-instance
    - instance_type: t2.micro
    - security_group: default
    - image_id: ami-02d8bad0a1da4b6fd
    - 태그: Environment: Testing
