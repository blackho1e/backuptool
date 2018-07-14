# backuptool
MariaDB 백업 / 복구 Tool

Percona XtraBackup 툴을 간편하게 사용할 수 있게 bash 스크립트로 작성했다.
일반적으로 MySQL 백업으로 mysqldump를 사용하는데 매번 전체를 백업하는 방식이므로 데이터가 커질수록 백업에 걸리는 시간이 길어지고 부담이 커진다. 뿐만 아니라 백업된 데이터를 복구하는데도 무척 긴 시간을 필요로 한다. 하지만 XtraBackup을 사용하면 마지막 백업으로부터 변경된 사항만 이어서 백업하는 증분 백업이 가능하다.
XtraBackup은 MySQL의 데이터 디렉토리 자체를 복사하는 것과 같기에 복구에 걸리는 시간은 단지 파일을 복사하는데 걸리는 시간과 같으며 InnoDB 엔진을 적용한 테이블의 경우 MySQL이 실행중인 상태에서도 테이블 락을 걸지 않고 백업이 가능하다. MyISAM 엔진을 사용한 테이블은 락이 걸리므로 주의하자.

## 요구사항
backuptool를 사용하려면 Percona XtraBackup이 설치된 환경에서 사용이 가능하다.

## Percona XtraBackup CentOS 설치 방법
```
yum install -y epel-release
yum install -y libev
yum install -y http://www.percona.com/downloads/percona-release/redhat/0.1-4/percona-release-0.1-4.noarch.rpm
yum install -y percona-xtrabackup-24 qpress
```

## Install
```
$ sudo curl -L https://github.com/blackho1e/backuptool/raw/master/backuptool -o /usr/local/bin/backuptool
$ sudo chmod +x /usr/local/bin/backuptool
```

## Usage
```
$ # 백업방법
$ backuptool backup -u 계정 -p 암호 -w 백업할 경로

$ # 복구방법
$ backuptool restore -u 계정 -p 암호 -w 백업한 경로 -r 복구할날짜
```

## Crontab 등록
```
# 월~토 오전 2시 전체 백업
0 2 * * 1-6 /usr/local/bin/backuptool backup -u 계정 -p 암호 -w 백업할 경로
# 월~토 오전7시~오후8시까지 4시간주기로 증분 백업
0 7-20/4 * * 1-6 /usr/local/bin/backuptool backup -u 계정 -p 암호 -w 백업할 경로
```
