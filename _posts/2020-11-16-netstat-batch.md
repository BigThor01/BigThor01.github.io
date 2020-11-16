---
title: 접속 IP 로그 기록을 위한 배치 파일 만들기 (netstat)
category: Etc.
tag: ["netstat", "Batch file"]
---

이번 포스트에서는 **내 PC에 접근한 ip를 기록하는 로그 파일 생성 방법**을 소개할께.

누가 언제 내 PC로 접근했는지 기록하는 것은 보안 관리를 위한 목적, 또는 내가 제공하는 서비스의 사용자 정보를 확인하는 목적에서 사용될 수 있어.

나는 후자의 목적으로 내가 만든 서비스에 접근한 사용자 ip를 기록하는 로그 파일을 생성했어.

로그 파일은 다음과 같이 구성된 **배치(batch) 파일**을 만들어서 실행하면 얻을 수 있었어.

 - **배치(batch) 파일의 구성**
 
   1. **netstat 명령어**를 이용해서 현재 접속한 ip 확인
   2. netstat의 결과를 "**>>**" 명령어로 **로그를 저장할 파일에 출력**
   3. **timeout / goto 명령어**를 이용해 120초 간격으로 1 & 2를 실행
  
그럼 위에서 말한 배치 파일과 그 안에 들어가는 netstat, timeout, goto 명령어를 알아보고, 실제 만들어진 배치 파일을 살펴보자.

---

## 배치(batch) 파일 구성

**배치 파일**은 **명령 프롬프트(cmd)에서 실행하는 명령어를 파일로 만들어서 실행하는 파일**이야. 

Notepad 에 명령어를 구성하고 **.bat** 확장자로 저장하면 배치 파일이 생성되고, 생성된 배치 파일을 실행하면 그 안에 있는 명령어가 자동으로 실행이 돼.

### netstat (cmd 명령어)

**netstat**(network statistics)는 내 PC에 연결된 네트워크 상태를 확인할 수 있는 cmd 명령어야. 

어떤 포트가 열려있고, 현재 포트에 연결되어 있는 상대방 정보 등을 알 수 있어. 

netstat 명령어 뒤에는 여러 옵션을 사용할 수 있지만, 오늘은 자주 사용하는 2개의 옵션만 설명하도록 할께.

 - -a : 모든 연결 및 수신대기 포트를 표시하는 옵션
 - -n : 주소나 포트형식을 숫자로 표시하는 옵션

```powershell
C:/Users/82107> netstat -an
```

위의 명령어를 cmd에서 실행하게 되면 모든 네트워크(수신 대기 + 연결) 정보를 결과로 보여줘. 

<a href="https://i.imgur.com/yIeGHgj"><img src="https://i.imgur.com/yIeGHgj.png" width="800px" title="source: imgur.com" /></a>

결과는 4개 항목으로 구성되어 있어.

  - 프로토콜 : 사용 프로토콜
  - 로컬주소 : 열려있는 내 PC의 ip/포트번호
  - 외부주소 : 내 PC와 연결된 상대방 ip/포트번호
  - 상태 : 연결상태
    + LISTENING : 현재 서비스를 대기 중
    + ESTABLISHED : 연결된 상태
    + TIME_WAIT : 연결이 종료되었으나 당분간 포트는 열어놓은 상태
    + CLOSED : 완전히 연결이 종료된 상태

netstat 명령어를 사용하면 모든 네트워크 정보가 나타나는데, 이 중 내가 제공하는 서비스 접속 포트 정보만 확인하기 위해서는 **find** 명령어를 사용하면 돼.

예를들어 내 서비스의 포트가 5601 일 때, 해당 포트로의 접속 정보만 확인하는 명령어는 다음과 같아.


```powershell
C:/Users/82107> netstat -an | find "5601"
```


### timeout (cmd 명령어)


### goto (cmd 명령어)



---

## 예제 : 로그 기록을 위한 배치 파일 

```powershell
:: log.bat 파일
@echo off
  :1
    echo ----------------- %time% ----------------- >> C:/user_log/log_%date%.txt
    
    netstat -an | find "192.168.35.239:5601" >> C:/user_log/log_%date%.txt
    
    timeout 120 /nobreak > NUL
    
    goto 1
```




