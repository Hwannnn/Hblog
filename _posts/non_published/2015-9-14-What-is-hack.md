---
layout: post
published: False
title: What is Hacking?
categories:
- Security
tags:
- Security
---

전자 회로나 컴퓨터의 하드웨어, 소프트웨어, 네트워크, 웹 사이트 등 각종 정보 체계가 본래의 설계자나 관리자, 운영자가 의도하지 않은 동작을 일으키도록 하거나 체계 내에서 주어진 권한 이상으로 정보를 열람, 복제, 변경할 수 있께 하는 행위를 광범위하게 이르는 말

> 이전에 보안을 처음 시작하였을 때, 해킹 관련 서적[^1] 관련 기술을 정리한 자료이다.

## 시스템 해킹(System Hacking)
- 커널을 중심으로 이루어진다
- 커널이 관리하는 메모리, 레지스터 등의 영역을 침입해서 데이터를 빼내거나 루트 권한을 획득.

<!--more-->

### 루트킷(Root Kit)
* 루트킷(RootKit)은 루트 권한 획득 기능, 시스템 제어를 위한 백도어 기능, 백신 프로그램에 발각되지 않기 위한 위장기능을 가진 해킹프로그램
* 루트킷은 유저 모드, 커널 모드, 부팅 모드 3가지 종류가 존재

1. **유저 모드**
    * 상대적으로 검출이 쉬움.
    * 시스템에 대한 영향도가 낮음.

2. **커널 모드**
    * 커널에 별도의 코드를 추가, 기존 코드를 대체하는 방식으로 동작
    * 개발이 어려움
    * 시스템에 치명적인 영향을 줌.

3. **부팅 모드**
    * MBR(Master Boot Record), VBR(Volume Boot Record), 부트섹터에 영향을 줌.
    * 파일 시스템 전체를 암호화 하거나 시스템을 부팅 불가능 상태로 만들 수 있는 강력한 기능

### 백도어(Back Door)
* 백도어는 사용자 PC를 원격에서 제어할 수 있는 프로그램
* 해커는 클라이언트 기능을 하는 악성 코드를 인터넷에 배포, 백도어 서버를 가동해 클라이언트의 접속을 기다림.
* 백도어 클라이언트가 서버에 접속하면, 해커는 사용자 PC를 제어할 수 있음.


### 레지스트리 공격(Registry Attack)
윈도우에서 일종의 데이터베이스 역활을 하는 레지스트리에 있는 다양한 정보를 조작하여, 비밀번호 초기화 방화벽 설정 변경, DLL인젝션 등의 공격을 수행 가능

### 버퍼 오버플로우
프로세스에 비정상적인 데이터 입력을 통해서 메모리에 해커가 의도하는 데이터를 저장하고 실행될 수 있도록 만드는 공격

### 경합 조건 공격
경합 조건 공격(Race Condition)은 두 프로세스가 하나의 자원을 사용하기 위해 서로 경쟁하는 상황을 이용한 공격

### 형식 문자열 공격
다양한 형식 문자열을 입력 값으로 사용해 스택을 조작, 쉘을 얻을 수 있음


## 어플리케이션 해킹(Application Hacking)
* 사용자가 실행하는 프로그램을 중심으로 이루어진다.
* 애플리케이션에 악성코드가 담긴 DLL을 주입하거나, 디버깅을 통해 키보드 입력을 가로챌 수 있다.

### 메세지 후킹(Message Hooking)
* 키보드 입력 등의 메세지를 중간에 가로채 해커에거 전송하는 공격
* 메시지 후킹은 user32.dll의 SetWindowsHookExA() 메소드를 이용.
* 윈도우는 키보드, 마우스 등에서 들어오는 메시지를 훅체인을 통해 처리한다.
> 훅 체인은 메시지를 처리하는 일련의 함수 포인터를 모아놓은 리스트

### API 후킹(API Hooking)
* 콜백 메서드에 해킹코드를 섬어 놓아 해커가 원하는 동작을 수행하도록 하는 공격
* API 후킹은 운영체제에서 제공하는 디버깅 프로세스를 이용한다.  
1. 먼저 디버거를 이용해서 어플리케이션 특정 명령어에 BreakPoint를 설정하고 특정 메소드를 수행하도록 등록한다.
2. 애플리케이션은 작업을 수행하다가 중단점을 만나면 등록된 메서드(콜백 메소드)를 실행한다.

### DLL 인젝션(DLL Injection)
* DLL인젝션은 동적으로 사용할 수 있는 라이브러리인 DLL을 애플리케이션에 삽입하는 공격 기술.
* 레지스트리를 사용. 레지스트리의 특정 위치에 원하는 DLL이름을 입력. user32.dll을 호출하는 애플리케이션의 경우 해당 위치에 입력된 DLL 이름을 입력해 놓는다.
* 후킹함수 사용. 특정 이벤트가 발생했을 때 DLL을 로딩하는 후킹함수로 등록.
* 실행 중인 어플리케이션에 원격 스레드를 생성해서 DLL을 삽입. 윈도우에서는 CreateRemoteThread() 함수를 제공해서 원격 스레드 생성을 지원.

### 코드 인젝션(Code Injection)
**쉘 코드를 애플리케이션에 삽입하는 공격 기술**
>스레드를 활용한 DLL 인젝션 기법과 유사. 차이점은 DLL 대신 직접 실행 가능한 Shell Code를 삽입. 코드 인젝션의 장점은 DLL을 미리 시스템 특정 위치에 저장할 필요가 없고, 속도가 빠르며 노출이 쉽지 않다.
하지만, 셸 코드의 특성상 복잡한 해킹 코드를 삽입할 수 없다는 단점이 존재.

****

## 웹 해킹(Web Hacking)
- 인터넷 브라우저와 웹서버의 구조적 취약점을 이용.
- 상대적으로 진입장벽이 낮은 편.

### XSS(Cross-Site Scripting)
**게시판 게시물에 악성코드를 포함하는 스크립트를 심어놓고 게시물을 읽는 사용자 PC에서 개인정보를 추출하는 해킹 기법.**
* 악성 코드는 대부분 스크립트 코드이며 쿠키를 읽어서 특정 URL로 전송하는 기능을 수행.
* 개인정보 유출문제가 발생할 수 있음.
* 웹방화벽과 브라우저 보안 강화를 통하여 보안을 강화하고 있다.

### CSRF(Cross Site Request Forgery)
* 게시판에 악성 코드를 삽입하고, 사용자가 해당 게시물을 읽었을 때 공격이 수행된다는 점에서 XSS와 유사. 차이점은 XSS는 사용자 PC에서 개인정보를 유출하지만, CSRF는 사용자 PC를 통해 웹 서버를 공격. 해킹 유형은 서비스 거부 혹은 정보유출이 발생할 수 있다.

### 피싱(Phising)
* 은행이나 증권사이트와 비슷한 웹 사이트를 만들어 놓고, 사용자의 금융정보나 개인정보를 탈취하는 기법. 해커는 사용자에게 피싱을 위한 이메일을 전송한다. 사용자가 이메일을 열어 링크를 클릭하면 피싱을 위한 위장 사이트가 오픈된다. 자신이 잘 아는 사이트로 오인한 사용자는 아이디와 비밀번호를 입력하게 되고, 이 정보를 기반으로 해커는 2차 공격을 시도한다.

### 파밍(Pharming)
* 파밍은 DNS를 해킹해서 정상적인 도메인 이름을 호출해도 위장 사이트가 전송되게 하는 해킹 기술이다. 위장 사이트의 IP가 사용자 브라우저에 전송되면 사용자는 해커가 만든 웹사이트에 개인정보를 입력하게 된다.

### SQL Injection
* SQL 인젝션은 HTML input 태그를 활용한다. 비정상 SQL문을 반복적으로 입력하면서 응답 데이터를 관찰하면 시스템 해킹이 가능한 SQL문이나 특정 정보를 알아내는게 가능하다.

### Web Shell
* 웹 쉘은 웹에서 제공하는 파일 업로드 기능을 악용한다. 서버를 원격에서 조정할 수 있는 파일(웝 쉘 파일)을 웹 서버를 통해 업로드 한다. 해커는 업로드한 파일의 위치를 파악하고 접근 가능한 URL을 찾아낸다. 해커는 이 URL을 통해 웹 셸 파일을 실행해서 운영체제를 통제할 수 있는 강력한 권한을 획득할 수 있다. 웹 셸을 최근 웹 해킹에서 SQL Injection과 더불어 가장 강력한 기법으로 활용되고 있다.

****

## Network Hacking
- 인터넷을 기반으로 이루어진다.
- Dos공격, Spoofing[네트워크 패킷을 엿봄]

### Port Scanning
- IP는 서버를 식별하는 논리적인 주소
- 포트는 하나의 IP를 여러 개의 애플리케이션이 공유하기 위한 논리적인 단위
- IP는 IP프로토콜에서 식별자로 사용되고, 포트는 TCP/UDP프로토콜에서 식별자로 사용
- 대표적으로 80(HTTP),443(HTTPS),21(TELNET)

>포트스캐닝은 서비스를 위해 방화벽이나 서버에서 개방한 포트 목록을 알아내는 기술.
>UDP, TCP기반의 패킷을 전송해서 확인
>각 기법별로 성능과 은닉성의 차이가 있으므로, 상황에 알맞은 기술을 선택해서 사용.

### 패킷 스니핑(Packet Sniffing)
- TCP/IP통신을 하는 이더넷 기반 동일 네트워크 환경에서는 MAC주소 기반으로 패킷이 동작.
- 하나의 PC에서 다른 PC로 데이터를 송수신할 때, 전체 PC에 데이터를 브로드캐스트.
- 패킷의 목적이 MAc주소가 자신의 것과 같으면 받아들여서 처리하고 그렇지 않으면 드랍.
>패킷 스니퍼는 모든 패킷을 버리지 않고 처리해서 동일 네트워크에서 이동하는 모든 데이터의 흐름을 한눈에 파악 할 수 있다.

### 세션 하이재킹(Session Hijacking)
- HTTP세션 하이재킹과 TCP 세션 하이재킹으로 구분.
- HTTP세션 하이재킹은 웹 서비스 인증 정보를 저장한 쿠키의 SessionID값을 탈취해서 해킹에 이용.
- TCP세션 하이재킹은 TCP패킷 정보를 탈취하는 방식.

* TCP프로토콜은 통신 상대방을 인증하기 위해 IP,Port, Sequence Number 3개 요소를 사용.
* TCP 세션 하이재킹은 패킷 스니핑을 통해 알아낸 인증 정보를 가지고 클라이언트와 서버 사이의 통신을 중간에 가로챈다.
* 해커는 클라이언트와 서버 사이의 연결을 잠시 끊고 발신지 IP를 해커 PC로 변경해서 서버와 커넥션을 재설정.
* 서버는 통신 연결이 잠시 끊겼다가 다시 연결됐다고 생각하고, 해커 PC를 클라이언트로 인식.

### 스푸핑(Spoofing)
- 스푸핑의 사전적 의미는 '위장'
- 네트워크 관점에서 DNS,IP,ARP 3개의 자원에 대해서 위장을 통한 공격이 가능.

* ARP는 IP주소를 가지고 MAC주소를 알아내는 프로토콜.
* PC 내부에 IP와 MAC주소가 저장된 ARP 테이블이 존재.
* ARP 테이블에 존재하지 않는 정보는, ARP프로토콜을 통해 IP에 해당하는 MAC정보를 찾을 수 있음.
* 이 때, ARP프로토콜은 보안이 고려되지 않아, ARP Reply 패킷을 통해 상대방의 ARP 캐시 테이블을 간단하게 조작 가능.

### 서비스 거부 공격(Denial of Service)
- 대량의 사용자가 동시에 서버에 접속하였을 경우, 서버에 부하가 생겨 이를 처리하지 못하는 경우가 있다. Dos 공격은 이와 같은 상황을 유도한다.
- SYN 패킷의 발신지 주소를 변경하거나, SYN 패킷만 지속적으로 전송하고, 대량의 IP패킷을 작은 단위로 쪼개서 전송 하는 등의 방법을 통해서 시스템을 서비스 불능 상태로 만들 수 있음.
- 이뿐 아니라, DoS는 정상적인 패킷을 대량으로 발생시켜 서비스를 마비시킬 수 있다.

**바이러스를 배포해서 불특정 다수의 PC를 좀비 PC로 만들고, 원격에서 대량의 트래픽을 발생시키도록 하는 분산 서비스 거부 공격(DDoS)도 Dos의 일종.**

****

## 기타 해킹 기술

### 무선랜 해킹 기술
공유기 등의 무선 AP등은 WEP,WPA,WPA2등 다양한 매커니즘을 통해서 데이터를 암호화하는데 이를 공격하는 해킹 기술

1. **WEP**
* 무선랜 보안을 위해 최초에 개발된 알고리즘.
* RC4 스트림 암호화 기버을 사용
* 취약성이 상당히 노출되어 있음.

2. **WPA**
* TKIP를 사용하여 보안성을 높임.
* 최초 인증과정에서 WPA키를 숨기는 취약점을 지님.
* 해커는 인증된 세션을 강제 종료하고 재인증을 유도함으로써 WPA 키를 쉽게 탈취 가능.
* 다양한 해킹 도구를 통해 WPA 해킹 시도 가능.

3. **WPA2**
* AES-CCMP를 사용해서 WPA의 취약성을 보완
* 가변 키 크기를 가지는 수학적 암호화 알고리즘을 사용하여 암호 키를 특정 시간이나 일정 크기의 패킷 전송 후 자동으로 변경하는 기능을 지님.
* 키 값의 탈취는 어려움.
* ARP스푸핑에 취약

## 암호 해킹 기술
**암호화된 정보를 키 값을 알아내서 복호화하거나 혹은 암호에 대해서 시도하는 공격**

**암호 해킹**

1. 암호문 단독 공격
- 공격자가 암호문만을 가지고 평문과 암호키를 찾아내는 방식
- 높은 난이도

2. 기지 평문 공격
- 암호문과 평문 일부를 가지고 공격을 시도
- 단독 공격보다는 낮은 난이도

3. 선택 평문 공격
- 공격자가 암호화를 수행할 수 있는 상태에서 공격을 시도.

4. 선택 암호문 공격
- 공격자가 복호화 할 수 있는 상태에서 공격을 시도.

**비밀번호 추출**

1. 무차별 대입 공격(Brute Force)
* 가능한 모든 경우의 수를 대입하여 비밀번호를 알아내는 공격.

2. 사전 대입 공격(Dictionary Attack)
* 사전에 높은 가능성이 있는 문자열이나 값들의 조합을 통해서 공격을 시도.

## 사회공학 해킹 기술
**기술적인 방법이 아닌 사람들 간의 기본적인 신뢰를 기반으로 정보를 획득하거나 조작하는 공격 기법**

크게 네 가지의 범주로 구분 가능.
1. 신뢰
2. 약점
3. 무능
4. 도청

기술적인 능력이 부족하여도 손쉽게 정보를 탈취할 수 있음.

[^1]: [파이썬 해킹 입문 - 공격의 언어 파이썬을 이용한 해킹연습] 책의 내용을 요약 정리하였다. 책에서는 추가적으로 관련된 내용의 스크립트도 제공되니 실제로 실습해보면 도움이 많이 된다.