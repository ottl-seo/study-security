# [ch8] Intrusion Detection (2)

# 2. Analysis approaches

### -Analysis 종류

- Anomaly detection (변칙적인)
- Signature or heuristic detection (잘 알려져있는 공격 알아채기)

## 2-1. Anomaly detection

→ normal 모델을 develop시켜 악성행위를 잡아낸다.   

- 장점: 알려지지 않은 공격까지 잡아낼 수 있다
- 단점:    
- **false alarm 잘 일어남**     
- 분석이 어렵다

### -탐지 과정

학습(**Training**) 단계: normal 행동으로 학습

→ 탐지(**Detection**) 단계: 관찰된 데이터가 normal인지 아닌지 체크

### -Anomaly detection의 종류

1) **Statistical** (통계적 접근)    

- 장점: 단순, 효율적
- 단점: 적절한 데이터를 찾기 어렵다

2) **Knowledge based** (지식 기반)    

→ 학습 단계에서 데이터 분류    

- 장점: 유연성 +튼튼함
- 단점: 시간이 오래 걸림

3) **Machine Learning**     

- 장점: 유연성+융통성+공통점 찾기 좋다
- 단점: false alarm 높다!!! , outside of data는 탐지 어려울 수 있다

## 2-2. Signature/heuristic detection

→ 알려진 악성 행위 패턴(=시그니처)를 사용    

→ 공격 규칙(=heuristic) 사용해서 공격을 **정의**    

- 장점: 빠르다, 효율적이다.
- 단점: 오직 알려진 데이터만 탐지할 수 있다.

### 1) Signature approaches

→ 시그니처 잡아서 빠르게 대응    

안티바이러스 제품, network traffic proxy(네트워크 트래픽 프록시), NIDS 등에 많이 쓰인다.   

- 장점:  효율성, 높은 활용성
- 단점: 새로운 공격마다 계속 개발되어야 함. DB도 유지해야함

### 2) Rule-based heuristic idetification

→ 알려진 취약점 등을 활용, 체험적인 방식.... (?)

---

# 3. Types of IDS

1) HIDS    
2) NIDS   
3) Distributed or hybrid IDS

## 1) HIDS (Host-based IDS)

소프트웨어에 layer를 더해서 만든다.   

→ intrusion 탐지, log 작성 후 알림 보낸다!!!

- **Data sources**: 시스템콜 추적, 로그 파일 관찰, 무결성 체크, 레지스터리 체크
- **Sensors**: 데이터를 모으고, filter (필터링), 포맷에 맞추기, IDS analyzer에 보내기

### ✔ Anomaly HIDS

- 분석 방법 여러개 → ANN, SVM 등등...
- 탐지 비율: **95~99%**, False positive: 5% 이하
    - **↑ 시스템콜** 추적(DLL) 방식이 많이 쓰임!
- **로그파일** or **레지스터리 분석** 방식의 경우 탐지 비율 **80%** 정도.

#### ***'Cryptographic checksums'*** 방식   

→ 중요한 파일에 대한 change를 관찰    

MAC(P) = 타우p 로 저장함으로써 DB화!!!   

- 단점:     
- 한 번 작동하면 change 탐지가 어렵다!     
- DB를 보호해야 한다   
- known good copies를 얻기 어렵다

### ✔ Signature or Heuristic HIDS

→ 안티바이러스 프로그램에 자주 쓰임, 매우 흔하고 효율적   

→ zero-day attacks에 약하다     

### ✔ Distributed (H)IDS
![Untitled](https://user-images.githubusercontent.com/61778930/122236284-58631880-cef9-11eb-8004-4c22dc596795.png)

[고려해야할 사항]   

- Data exchange → 각각 다른 환경에서 다른 센서로 들어온 데이터가 교환된다
- 네트워크 transmission 하는 동안 **Sensor를 보호**해야 한다
- **중앙화 여부**
![Untitled 1](https://user-images.githubusercontent.com/61778930/122236301-5c8f3600-cef9-11eb-997e-660150e536fd.png)

⇒ 네모가 각각 센서...   

각 센서가 독립적으로 실행되고 시스템이 모니터링됨!

## 2) NIDS (Network-based IDS)

→ 네트워크의 특정 포인트에서 **네트워크 트래픽을 모니터링**   

→ Encryption이 많아질 수록, NIDS는 각 컨텐츠에 접근하기 어려워진다 → 어떤 일이 일어나는지 모니터링 어려워짐   

### -Network sensors 타입

**1) Inline sensor**

- 트래픽은 무조건 sensor를 거쳐야 한다 (***Inserted into a network segment***)
- 
![Untitled 2](https://user-images.githubusercontent.com/61778930/122236327-631dad80-cef9-11eb-9542-91e69305b09c.png)

↑ 이렇게 생김

- LAN switch나 firewall(방화벽) 등이 inline sensors의 기능을 할 수도 있음
- detected attack을 막을 수 있다 → 탐지된 공격은 방어 가능

**2) passive sensor**

- more common
- 네트워크 트래픽 복사본을 모니터링    
→ No packet delay   
→ No IP 주소
- 분리된 인터페이스   
![Untitled 3](https://user-images.githubusercontent.com/61778930/122236353-6a44bb80-cef9-11eb-8714-241f87cbeae5.png)

### Network 센서에 대하여
![Untitled 4](https://user-images.githubusercontent.com/61778930/122236382-6fa20600-cef9-11eb-8859-747ac900804a.png)

- Wired or Wireless → Wireless로 연결되어있으면 그 NIDS는 inline이다.
- WIDS란? : wireness network에 초점을 맞춘 NIDS
![Untitled 5](https://user-images.githubusercontent.com/61778930/122236413-74ff5080-cef9-11eb-8a2f-ec6e29657d87.png)


#### [**1**]번 위치

- external firewall을 침투한 **외부 공격** 탐지 가능
- DMZ에서 작동하는 서비스를 타겟으로 하는 공격을 탐지 가능

#### [**2**]번 위치

- unfiltered된 모든 트래픽을 관리 가능
- 네트워크를 타겟으로 한 공격의 수와 종류를 알아챌 수 있다

#### [**3**]번 위치 

***⇒ Major backbone network** (internal server와 data resource에 맞닿아있음)    *****

- 네트워크 트래픽을 다수 모니터링 가능 → 탐지 가능성이 높아진다!
- 내부자(insider)에 의한 unauthorized activity 탐지 가능

#### [**4**]번 위치

생략

⇒ 각 위치별로 장점이 다르니 위치에 맞게 용도 사용

### Intrusion detection 기술:

**1) signature detection**

- Application layer: DNS, Finger, FTP, HTTP, IMAP 등의 프로토콜 분석 가능

    → 공격 예시: buffer overflow, 패스워드 추측, 악성코드 transmission, ...   

- Transport layer: TCP, UDP 트래픽을 분석 가능

    → 공격 예시: unusual packet fragmentation, 취약한 포트 스캔, TCP 공격 가능(ex. SYN floods)   

- Network layer: IPv4, Ipv6 등등 분석 가능

    → IP주소 스푸핑, illegal IP 헤더값 등.

**2) anomaly detection**

→ unusual network 기술

- DoS 공격: 급격한 패킷 트래픽 증가→ connection 불안정하게
- Scanning:
- Worm detection ← scanning으로 탐지될 수 있음

something detected 되었을 때 →
