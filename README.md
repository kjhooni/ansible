# 실습환경
- 컨트롤 노드 1ea
  - control node_OS: Debian Linux 12
- 인벤토리 호스트 4ea
  - web01,web02,db01_OS:CentOS 9
  - web03_OS: Ubuntu20.04 


# exercise1
inventory 작성 기초 및 모듈 사용
- vprofile/exercise1 폴더 생성 및 이동
- inventory 작성(web01)
- ansible ping모듈로 web01로 ping
- host key checking false

exercise2
===========
inventory 작성(그룹 포함)
- vprofile/exercise2 폴더 생성 및 이동
- 인벤토리파일의 hosts그룹에 web01, web02, db01
- web02, db01에 각각 핑
- 인벤토리파일의webservers그룹에 web, dbservers그룹에 db, dc_oregon그룹에 webservers, dbservers

exercise3
===========
inventory 작성(vars를 활용한 공통 변수 선언)
- vprofile/exercise2 폴더 생성 및 이동
- dc_oregon에 ansible_user, ansible_ssh_private_key_file 정의

exercise5
===========
ansible-playbook을 이용한 web, db 엔진 설치 및 시작
- vprofile/exercise5 폴더 생성 및 이동
- 아래의 플레이북 작성
  - webservers그룹 httpd 설치 후 service started, enabled
  - dbservers그룹 mariadb 설치 후 service started, enabled

exercise6
===========
ansible-playbook copy모듈
- vprofile/exercise6 폴더 생성 및 이동
- web.yaml파일 작성
  - copy모듈 이용. index.html파일 webservers에 복사

exercise7
===========
ansible.cfg파일 작성 및 database 모듈 활용
- vprofile/exercise7 폴더 생성 및 이동
- db.yaml파일 작성
  - mariadb설치, service started/enabled, accounts db 생성
  - pymysql설치
  - vprofile 데이터베이스 user 생성

exercise8
===========
group vars 및 debug 모듈
- vprofile/exercise8 폴더 생성 및 이동
- exercise6의db.yaml파일 dbservers그룹에 변수 지정
  - dbname= electric
  - dbuser: current
  - dbpass: tesla
- tasks에 문자가 아닌 변수를 전달
- register, debug 모듈 사용하여 디버그 메세지 출력

exercise9
===========
group vars, host vars 및 debug 모듈
- vprofile/exercise9 폴더 생성 및 이동
- web03서버 (Ubuntu) 추가 생성
- ping모듈로 ping
- print_facts.yaml작성
  - task: Print OS name
  - debug, setup모듈 사용하여 ansible_distribution 출력

exercise10
===========
조건문(when)
- vprofile/exercise10 폴더 생성 및 이동
- provisioning.yaml 파일 작성
  - when 사용하여 os별로 task 분기
  - centos에서 chrony패키지 설치, chronyd서비스 start/enable
  - ubuntu에서 ntp패키지 설치, ntp서비스 start/enable
