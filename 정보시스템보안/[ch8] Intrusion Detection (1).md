## 1. Intruder란?
hacker or cracker를 포함하여, 보안을 위협하는 이들   

### Intruder 분류

1. Type에 따른 분류
    - Cyber Criminals
    - 'Hactivists'- 해킹을 시도하는 이들
    - State-sponsored organizations- 악성코드 만드는 시스템을 개발하는 단체
    - Others
2. intruders의 기술 수준에 따른 분류
    - Appretice- 시중에 있는 걸 사용
    - Journeyman- 새로운 스킬을 만들어냄
    - Master

### Intrusion의 예시

생략

### "Intrusion detection system"

- **IDS**: intrusion **Detection** System
- **IPS**: intrusion **Protection** System

    → Intrusion 시스템들은 너무 정교하거나 타겟팅된 공격에는 효율이 좋지 않다.    

    → But, 이미 잘 알려진(흔한) 공격을 탐지&방어하는 데 매우 효율적   

### ✏ Intruder의 행동(behavior)

0. **Target acquisition and information gathering** (준비): 타겟을 정하고 정보를 모으는 단계   

1. **Initial access** (초기 접근)
2. **Privilege escalation** (권한 확대)
3. **Information gathering or system exploit** (시스템 사용 OR 정보 활용)
4. **Maintaining access** (접근권한 유지)
5. **Covering tracks** (로그파일 복구): 아무 일도 없었던 듯 로그 파일을 수정하여 사용자가 모르게 한다.

- - -

#### ✔ [0. Target acquisition: 타켓 모색]

- 웹사이트를 돌아다니며 타겟을 정한다.
- Gather information on target network using DNS → DNS 분석하여 타겟의 네트워크 정보 수집, WHOIS database까지 알아냄
- 메일로 쿼리문을 보내 응답자를 확인하는 방식도.

⇒ 잠재적 취약점을 알아내는 일 (Web Content Management System 이용)    

#### ✔ [1. **Initial access**: 초기 접근]   

- Web CMS의 password를 Brute force 공격으로 알아냄 (무작위로 입력해보며)
- 취약점을 활용해 접근 권한을 얻는다.
- 피싱 메일을 보내 접근 권한을 얻는다.

#### ✔ [2. **Privilege Escalation**: 권한 확대]

- local하게 활용하기 위해 시스템을 Scan
- 취약점을 이용해 권한을 업그레이드(elevated)
- Sniffer 설치 → 관리자의 password 캡쳐

    → 관리자의 pw 얻으면 다른 정보에도 접근할 수 있게 권한 변경   

#### ✔ [3. **Information gathering or system exploit**: 정보 수집 및 활용]

- file scan → 원하는 정보 얻기
- documents 를 다른 저장장치로 옮겨두기
- 알아낸 pw를 통해 네트워크 내의 다른 서버에도 접근하기

#### ✔ [4. Maintaining access: 접근 유지]

- rootkit (with Backdoor) 설치 → 나중에 또 접근할 수 있도록 (remote, later access)
- IDS 프로그램을 변경 또는 불구로 만들기

#### ✔ [5. Covering tracks: 트랙 관리]

- rootkit 이용 → 파일 숨기기
- 로그 파일 수정

---

### Components of IDS (3가지)

**1) Sensor**- 데이터 수집→ Analyzer에게 전송     

- 네트워크 패킷, 로그 파일, system call 추적 등...

**2) Analyzer**- intrusion이 일어나는 것을 알아채는 용도    

**3) UI** (User Interface)- 사용자가 시스템의 행동을 볼 수 있게..    

### IDS의 Types

1. **HIDS** (Host-based IDS): single host를 모니터링    
2. **NIDS** (Network-based IDS): 네트워크 위주로 모니터링- network traffic, analyzes network, transport, ....    
3. **Distributed or hybrid IDS**: 여러 개의 sensor를 중앙 analyzer에 결합시킴

### IDS의 필요성

- **Fast** → damage ↓
- **deterrent** (억제제 역할)- non-APT 공격을 막아줌 (타겟 정하지 않고 하는 공격들을 잘 막아줌)
- Intrusion 공격의 집합체 → 대응책 개발이 쉬워짐

---

### 중요_ false alarms
![Untitled](https://user-images.githubusercontent.com/61778930/121812078-84845c80-cca1-11eb-8949-0a834b888f5a.png)
   
⇒ IDS 시스템이 잘못된 결과를 내는 경우: **false alarm**   

### Base-rate fallacy

→ IDS에서 false alarm을 없애는 것은 어렵다
