# Firewalls

### Firewall이란?

- 규칙에 따라, allows or blocks network traffic → 공격으로부터 네트워크 보호

***"Defense in depth"*** → 새로운 취약점도 **firewall의 규칙을 바꿈으로써** 핸들링될 수 있다.

### Design 목표

1. inside↔outside의 모든 트래픽은 방화벽을 거쳐야 한다
2. 보안정책에 따라 "Authorized된 트래픽만" pass할 수 있다
3. 방화벽 그 자체는 침투에 대한 면역을 가진다

### Access policy (접근 정책)

트래픽을 필터링하는 데 쓰이는 특징들    

- **IP 주소 and 프로토콜 value**: IP주소, 포트번호, direction of flow(inbound/outbound)
- **Application 프로토콜**: authorized된 application protocol data에 대한 접근 제어
- **User identity**
- **Network activity**: ex. 발생 빈도, 비즈니스 아워 등

### Firewall의 Capabilities: 장점

- 인증되지 않은 사용자가 보호된 네트워크에 접근하지 못하도록 한다
- 보안 관련 이벤트를 모니터링
- 안전하지 않은 인터넷 함수를 쉽게 관리해주는 플랫폼 (**NAT**: network address translator)
- **IPSec 플랫폼**: VPN(virtual private network)을 보충하는 데 사용됨

### 한계

- bypassing (우회하는) 공격을 막지 못함
- Internal threats; 방화벽 내에서 일어나는 공격
- 부적절하게 보호된 wireless LAN(무선랜)
- 이미 감염된 laptop, 스마트 디바이스, usb 장치 등...

# Overview

![Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled.png](Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled.png)

- 네트워크 트래픽 감시→ low-level 네트워크 패킷부터 application 프로토콜까지
- **Positive filter**: 일정 기준을 충족하는 패킷만 pass를 허락한다
- **Negative filter**: 특정 기준을 충족하지 못하는 패킷은 pass할 수 없다.

### -firewall의 종류

1) Packet filtering firewall (패킷 필터링 방화벽)   
2) Stateful inspection firewall (상태유지 검사 방화벽)   
3) Application-level gateway    
4) Circuit-level gateway (회로...)

## 1) Packet filtering firewall

![Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%201.png](Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%201.png)

- incoming or outgoing IP 패킷 각각을 규칙에 따라 허락하거나 거절함

    → 출발/도착하는 IP 주소와 포트 번호   

- Transport protocol types: transport계층의 프로토콜 타입
- Interface
- examine each rule: 규칙을 검토한다
- 첫 번째 match는 패킷을 처리하기 위해 호출된다.
- No match: 'default' rule을 따른다.

### → 'Default' rule?

1) **discard** 가 default인 경우   

→ 특별히 허가된 경우가 아니면 무조건 block   

- 서비스는 case-by-case 원리에 따라 추가되어야 한다. (허가된 case만 출입 가능)
- **Inconvinient**, but **safer** → 접근성이 떨어져서 불편하지만 더 안전하다!

2) **forward** 가 default인 경우    

→ 특별히 비허가된 경우가 아니면 항상 허용   

- open된 단체에서 선호하는 방식

### - Example

✔ **목표**: inbound & outbound 의 'email' 트래픽을 허용한다. 단, 나머지 트래픽은 전부 block한다.    

⇒ **허용 규칙**   

- 25번 포트: SMTP incoming → 외부 소스로부터 오는 inbound mail
- inbound SMTP connection으로의 response
- 외부 소스에 보내지는 outbound mail과 그에 대한 response
- Default policy는 특별히 작성된 것

    ![Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%202.png](Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%202.png)

✔ **Problem**: 4번째 rule의 경우, 1023 이상의 포트로 향하는 external 트래픽은 항상 허용되어야 한다는 문제 발생 → 외부의 트래픽 공격이 들어올 수 있음     

**⇒ Solution**: 2,4번째 규칙의 경우에는 **Src 포트번호**를 **25**로 설정,   

1,3번째 규칙의 경우에는 Src port를 1023 이상으로 설정한다.   

→   

- 하지만, 여전히 포트번호 25를 사용하는 외부 공격이 침입할 수 있다.
- 4번째 규칙은 여전히 공격에 사용될 수 있음.

**⇒ 이에 대한 solution: ACK flag** 를 이용하는 방법이 있다 → SMTP에 대한 응답만을 허용

![Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%203.png](Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%203.png)

### -장점

: 단순, 사용자 친화적, 빠름   

### -**단점**

- upper-layer data를 시험하거나 이해할 수 없음      

- "한 application을 허용한다"의 의미 == 해당 application의 모든 함수를 허용한다 → 위험.   

- Logging이 낮은 수준이다 (??)   

- 잘못 인식하기 쉽다(misconfigure)   

### 공격 예시

🧨 **IP주소 스푸핑 공격**   

(*Spoofing*이란? → IP주소를 변조한 후 합법적인 사용자인 것처럼 위장하여 시스템에 접근)

- 스푸핑된(변조된) 소스 주소(내부 주소)로 패킷 전송 → 악성 패킷을 내부에 전송
- 간단한 소스 주소 기반 filter에 침투할 목적
- **대응 방법:** 외부 인터페이스에서 전송된 내부 소스 패킷을 차단

🧨 **Source routing 공격 (소스 라우팅 공격)**   

- source에 '경로'(route)를 지정
- 보안 수단을 우회할 목적으로..
- **대응 방법**: 이러한 옵션을 사용하는 모든 패킷을 차단

🧨 **Tiny fragment 공격**   

- 매우 작은 조각을 만든다 → 1st fragment 헤더는 filtering에 대한 충분한 정보를 포함하고 있지 않다.
- **대응 방법**: 1st fragment의 transport 헤더가 충분한 정보를 담고 있지 않을 경우, 차단 (그 subsequent 조각까지 전부)

## 2) Stateful inspection firewall

→ packet filtering 방화벽과 유사한 구조   

BUT, TCP connection 정보가 **저장**되고, filtering이 이를 나타낼 수 있다.   

![Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%204.png](Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%204.png)

### -Example

![Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%205.png](Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%205.png)

- **'TCP connection'**은 주로, 1024이하의 포트 ↔ [1024,65535] 사이의 포트 사이의 connection이다.

    **→** '*Packet filtering firewall*'의 경우, 이를 허용하려면 큰 숫자의 포트를 사용하는 모든 인바운드 트래픽을 허용해야 한다(높은 포트만 쓰면 내부로 향하는 모든 트래픽을 허용해야 하는 문제)    

    **→** '***Stateful inspection firewall***'의 경우에는, 현재의 TCP connection과 연관된 패킷만을 허용할 수 있다. (선택적으로)   

- TCP sequence number나 잘 알려진 프로토콜의 application data가 검토될 때도 (??)

## 3) Application-level gateway

**== application proxy** 라고도 불린다.   

![Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%206.png](Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%206.png)

→ telnet, FTP 등 프로토콜을 위한 application-level의 traffic 릴레이   

- User는, remote host에대한 User ID, authentication info를 제공하는 gateway를 거친다.
- Gateway는, remote host의 application, TCP 세그먼트의 릴레이에 접근한다. → two endpoint 사이에 오고가는 application data에 접근 가능

### -장점

- 패킷 필터보다 더 안전하다 (more secure)   

- Logging at higher level  

### -단점

: 각 connection 사이에 추가적인 프로세싱 **overhead**가 필요하다

## 4) Circuit-level gateway

**== circuit-level proxy** 라고도 불린다.   

![Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%207.png](Firewalls%2092e66ad08bc240eb9e2b94b486ffde98/Untitled%207.png)

- 두 TCP connection을 설정한다 → user와 gateway, gateway와 remote host간의 연결

    → contents를 확인하지 않고 TCP segments 릴레이 커넥션을 설정할 수 있다

### → example

내부 사용자를 신뢰할 수 있는 경우, gateway를 구성하여    
inbound 연결 시 애플리케이션 레벨 서비스 및    
outbound 연결을 위한 회로 레벨 기능을 지원할 수 있다.   

When internal users are trusted, gateway can be configured to support application-level service on inbound connection, and circuit-level functions for outbound connection   

→ 외부에 보내지는 데이터에 대한 overhead를 줄일 수 있다.

# Firewall basing

; 방화벽의 기반이 되는 곳은?   

- Bastion host
- Host-based firewall
- Network device firewall
- Virtual firewall
- Personal firewall

## 1) Bastion host

## 2) Host-based firewall

## 3) Network device firewall

## 4) Virtual firewall

## 5) Personal firewall

# Firewall location and configurations

; 방화벽 위치와 구성   

- DMZ network
- VPN (Virtual private network)
- Distributed firewall

## 1) DMZ network

## 2) VPN

## 3) Distributed firewall