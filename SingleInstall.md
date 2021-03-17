# SingleInstall

# Oracle Version Unzip Path
18c 이전 : 특정 unzip 경로 없음
18c : oracle home 경로 ( 사전 홈 경로 생성 필요 )

# Oracle Version Install Path
11.2
base : /oracle/app/11.2
home : /oracle/app/11.2/11.2.0.4
12.1
base : /oracle/app/12.1
home : /oracle/app/12.1/12.1.0.2
12.2
base : /oracle/app/12.2
home : /oracle/app/12.2/12.2.0.1
18
base : /oracle/app/18
home : /oracle/app/18/18.0.0

# Gui IP Check
who am i
export DISPLAY=IP:0.0

# runInstaller
./runInstaller
./runInstaller  -silent -executePrereqs (Silent)

# 구성 옵션
Create and configure a database : Oracle SW 설치 간에 Database 설치까지 진행 = DB Engine + DB
Install database software only(O) : Oracle SW 설치만 진행 = DB Engine

# 데이터베이스 설치 옵션
Single Instance database installation : 싱글 인스턴스 설치
Oracle Real Application Clusters database Installation : RAC 설치

# 데이터베이스 버전
Enterprise Edition : Oracle SW의 옵션 중 모든 옵션을 사용할 수 있다. 
Partioning, Parallel, Active Data Guard 등 사용 가능하다. 모든 옵션이 무료는 아니며 Extra cost option 이 존재
Standard Edition : Oracle SW의 모든 옵션을 사용하지는 못하며 CPU Core 수 사용에도 제한이 있다.. Partioning, Parallel, Active Data Guard 등 많은 기능의 제약이 있다. 

- EnterPrise Edition : 4 CPU 이상 확장 가능한 서버에 설치 가능
- Standard Edition 2 : 기존 Standard Edition, Standard Edition One이 만료되고 새로 추가된 Edition으로 2 CPU 까지 지원하지만 16 CPU Threads 지원
   * 해당 에디션은 에디션별 지원 옵션, 특징이 다르며 에디션은 변경 할 수 있으나 구매한 라이센스로 선택

# 설치 위치
oracle base 경로 및 oracle home 경로 지정
oracle home 경로를 oracle base 하위 경로에 존재해야 한다.
UNIX 및 LINUX 의 경우 oracle SW의 설치 유저의 bash_profile에 ORACLE_BASE, ORACLE_HOME 을 지정하였을 때 해당 변수의 값으로 지정된다.

* Oracle base : 오라클 설치 프로그램인 OUI를 저장하고, 오라클 트레이스 파일을 저장하는 디렉터리의 이름을 기록하는 오라클 환경 변수 명 입니다.
* Software location : $ORACLE_HOME 위치, DB 엔진 설치 경로로 Oracle base와 경로가 같아도 무방 합니다.
-	유저 환경변수(profile)에서 설정하면 자동으로 표시가 됩니다.
-	디렉토리는 모두 oracle계정의 read/write 권한이 필요 합니다.

oracle base = /oracle/app

[INS-32008] [INS-32056]
-	Oracle Base 위치가 oracle 계정의 홈디렉토리와 같은 경우 (그림은 12cR2이나 내용은 동일)
-	Inventory 경로가 이미 $ORACLE_BASE에 포함 되어 있을 경우 뜨는 경고
-	Yes 선택 후 Next 

# 인벤토리 생성
Oracle SW 설치 로그, ORACLE_HOME_NAME, 버전 등의 정보를 저장하며, Opatch 유틸리티 사용시 필수적으로 사용되는 경로이다.
* 이미 Inventory가 있을 경우 생략
/oracle/oraInventory      Group Name : dba

# 운영 체제 그룹
[OSDBA] : SYSDBA 의 권한을 가지고 OS 인증을 받고 접속할 수 있는 OS 그룹이다.
[OSOPER] : SYSOPER의 권한을 가지고 OS 인증을 받고 접속할 수 있는 OS 그룹이다. 운영체제 그룹(선택)
[OSBACKUPDBA] : RMAN, SQL*PLUS 사용하는 sysbackup 권한
[OSDGDBA] : Data Guard 조작을 수행 하는 SYSDG 권한
[OSKMDBA] : 암호화 키 저장소 조작하는 SYSKM 권한
[OSRACDBA] : RAC 관리하는 SYSRAC 권한

* 각각의 그룹을 지정하여 설치 권장

# 루트 스크립트 실행 : Check X
* 루트 스크립트 자동 수행 여부

# 필요 조건 검사
위 2개의 패키지는 ignorable 하지만 설치 미디어에 존재하는 elfutils-libelf-devel (ELF 바이너리 파일을 읽고 생성하고 수정할 수 있는 유틸리티와 라이브러리 모음) 은 설치 후  Ingore All 을 체크하고 진행한다.
check again : 수정사항을 수정 후 재확인한다.
fix & check again : 수정사항에 대한 fix 스크립트를 생성해준다.
show [all, failed, succeeded] 사전 체크시 발생한 내용에 대해서 필터링 또는 모두 보여준다.
Ignore All : 실패한 사전 체크를 모두 무시하고 진행한다.

# 요약
설치 간 진행했던 내용 요약을 보여준다.
Save Response File : 설정 사항을 응답파일로 저장한다. (여러 서버 또는 같은 설정사항으로 다른 곳에 설치하는 등에 사용된다.)
Install 클릭 시 설치가 진행되며 <oraInventory 경로>/logs 아래 installActions 로 시작하는 설치로그가 생성된다.

# 구성 스크립트 실행
orainstRoot.sh : /etc/oraInst.loc 에 oraInventory와 install group (여기서는 dba) 를 추가하고 권한을 수정한다.
                 oraInventory 퍼미션,그룹 변경 및 /etc/oraInst.loc가 없을 경우 생성 합니다.
root.sh : /etc/oratab 을 생성하고 권한을 수정한다.
          oratab 생성 , oracle 사용자를 위한 환경변수 설정(dbhome, oraenv, coraenv)
          
          * Root Script 수행
              /oracle/oraInventory/orainstRoot.sh
              /oracle/app/database/root.sh

# Choose Components
Oracle Partitioning : 대용량 테이블을 개별 파티션으로 나누어(Range, Hash, List 등) 관리, 가용성, 성능을 향상시킬 수 있다.
Oracle OLAP : On-Line Analytical Processing으로 데이터 분석 솔루션이다.
Oracle Label Security : row label 기반으로 정보 보호 및 데이터 분리 기능 제공한다.
Oracle Data Mining RDBMS Files : 데이터를 이용하여 새로운 패턴과 관계를 밝혀내는 Advanced Analytics 옵션이다.
Oracle Database Vault option : 사용자가 데이터 및 애플리케이션에 엑세스를 제어하며 악의적인 내부사용자 및 일반적인 보안 위협에 대해 보호하는 옵션이다.
Oracle Real Application Testing : 실제 응용 프로그램에 대한 포괄적인 테스트를 통해 시스템 변경에 대한 문제를 식별하여 변경에 대한 문제를 최소화할 수 있는 옵션이다.


( Patch 문서로 이동 )
