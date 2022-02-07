---
cover: ../.gitbook/assets/Frame 75.png
coverY: 222.98553948832037
---

# 레디스 튜닝

## 1. 환경 설정 파일

레디스의 환경 설정하는 방법은 다음과 같다.

1. **레디스 프로세스가 시작할 때 환경 설정 파일을 읽도록 한다. **_****_ ⇒ `redis.conf` 파일
2. **레디스의 명령을 사용하여 실시간으로 설정값을 반영한다.   **_****_  ⇒ `config set` 명령



기본으로 제공하는 `redis.conf` 설정 파일은 크게 다음으로 나뉘어진다.

* _기본 설정_
* _영구 저장소 설정_
* _복제 설정_
* _보안 설정_
* _제한 설정_
* _루아 설정_
* _고급 설정_

__

### 1.1. 기본 설정

네트워크, 로그 및 프로세스와 관련된 설정값을 지정한다.

#### port

인스턴스가 사용할 서비스 **포트**를 지정한다.

* example : `port 1234`
* default : `port 6379`

#### bind

인스턴스에 **접속을 허용할 IP**를 지정한다.

* example : `bind 192.168.10.123`
* default : `bind 127.0.0.1 -::1`
  * 서버 내부에서만 접속을 허용

#### timeout

연결된 클라이언트의 idle **대기 시간**을 초 단위로 설정한다.

즉, timeout 값으로 지정된 시간동안 데이터의 송수신이 발생하지 않으면 해당 클라이언트의 연결을 끊는다.

* example : `timeout 5`
* default : `timeout 0`
  * 클라이언트의 연결을 끊지 않음

#### loglevel

레디스 인스턴스가 동작 중에 출력하는 **로그의 레벨**(`debug`, `verbose`, `notice`, `warning`)을 지정한다.

* example : `loglevel debug`
* default : `loglevel notice`

#### logfile

로그가 저장되는 **경로와 파일명**을 지정한다.

* example : `logfile ./log/redis.log`
* default : `logfile ""`
  * 로그를 남기지 않음

#### database

레디스는 database 라는 이름의 **논리적으로 분리된 데이터 저장공간**을 제공한다.

* example : `database 2`
* default : `database 16`
  * 16개의 분리된 데이터 저장공간을 사용



### 1.2. 스냅샷 설정

레디스는 데이터를 영구 저장하기 위해 `dump.rdb` 파일에 메모리 내용을 기록한다.

#### save

레디스의 **스냅샷 이벤트가 발생하는 간격**을 지정하는 설정이다.

스냅샷 이벤트가 발생하면 메모리에 저장된 데이터를 `dump.rdb` 파일에 저장한다.

설정은 `save <단위시간> <키의 변경횟수>` 와 같은 형식으로 지정한다.

*   example

    ```python
    # 300초 동안 최소 10개의 키가 변경되면 스냅샷 이벤트를 발생시킨다.
    save 300 10

    # 여러 개의 save 설정을 지정
    save 900 1
    save 300 10
    save 60 10000
    ```
*   default

    ```python
    # 스냅샷 이벤트가 발생하지 않는다.
    save ""
    ```

#### stop-writes-on-bgsave-error

스냅샷 이벤트에 의해서 dump.rdb 파일을 저장하는 도중에 오류가 발생하면 레디스는 모든 쓰기 요청을 거부한다.

스냅샷 파일의 생성에 **실패하더라도 쓰기 요청을 처리**하고 싶다면 `stop-writes-on-bgsave-error` 설정을 `no` 로 지정한다.

* example : `stop-writes-on-bgsave-error no`
* default : `stop-writes-on-bgsave-error yes`

#### rdbcompression

레디스 스냅샷 파일을 저장할 때 **파일의 압축 여부**를 설정한다.

압축을 사용하면 스냅샷이 동작할 때의 CPU의 사용률이 높아지지만, rdb 파일의 크기가 작아진다.

* example : `rdbcompression no`
* default : `rdbcompression yes`

#### rdbchecksum

레디스 스냅샷 파일을 저장할 때 파일의 정합성을 검증하기 위한 **체크섬의 사용 여부**를 설정한다.

`rdbchecksum` 을 `yes` 로 지정하면 스냅샷 파일을 생성하거나 읽을 때 10% 정도의 성능 하락이 발생한다.

#### dir

레디스의 **작업 디렉터리**를 지정한다.

스냅샷 파일인 `dump.rdb` 파일과 `aof` 파일 그리고 로그 파일이 생성되는 디렉터리를 의미한다.

* example : `dir /usr/local/etc`
* default : `dir ./`
  * `redis-server` 명령을 실행한 위치를 의미한다.

#### dbfilename

스냅샷 파일의 **파일명**을 지정하는 설정이다.

통상적으로 운영상의 편의를 위해서 절대경로로 지정하며 `<서비스 포트 또는 노드이름>.rdb` 와 같은 형식을 사용하여 저장한다.

* example : `dbfilename /data/redis/snapshot/master01-6379.rdb`
* default : `dump.rdb`

``

### 1.3. AOF 설정



