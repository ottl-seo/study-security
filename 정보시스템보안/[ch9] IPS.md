# [ch9] IPS

# Intrusion prevention system

⇒ **IDS의 확대 버전** _   

→ 악성 행위를 차단하거나 탐지하는 능력을 가진 시스템

→ IDS와 동일하게 분류할 수 있다

- host-based, network-based, distributed/hybrid
- anomaly detection, signature/heuristic detection

## 1. Host-based IPS (=HIPS)

- 공격을 알아채고 차단하기 위해 anomaly detection(변칙적 탐지) 또는 signature/heuristic detection(규칙대로 탐지)을 사용한다.
- Sandbox approach 또한 사용될 수 있다

### -HIPS로 막을 수 있는 공격

- 시스템 리소스 수정
- 권한 확대 공격
- Buffer overflow 공격
- email contact 리스트에 접근
- 웹 서버를 가로지르는 directory

## 2. Network-based IPS (=NIPS)

Inline NIDS와 비슷한데, **패킷을 수정/삭제**할 수 있는 권한을 가지며, **TCP connection을 허물 수 있음**.

### -NIPS의 공격 탐지 방식

- Pattern matching
- Stateful matching
- Protocol anomaly
- Traffic anomaly
- Statistical anomaly

## 3. Distributed/hybrid IPS

여러 host와 network-based 센서들에게서 데이터를 모아온다.   

- **Central system**이 데이터를 분석하고, 모든 연관된 시스템에 대한 시그니처와 행동 패턴들을 리턴해준다.
- 각각의 시스템은 이를 사용하여, 응답 and 악성행위에 맞선다.

### -example

⇒ ***'digital immune system'***

# → Digital immune system

![%5Bch9%5D%20IPS%203a820b2685ac4d1695a9585cd6c580ba/Untitled.png](%5Bch9%5D%20IPS%203a820b2685ac4d1695a9585cd6c580ba/Untitled.png)

- **Sensor**는 잠재적 악성코드 Scanning, 감염, 수행 행위를 **탐지**한다.
- Sensor는 탐지된 악성코드의 **복사본**을 **'Correlation server'에 보낸다.**

    **→ Correlation server? : 비슷한 악성코드를 탐지해낸다.**

- 잠재적 악성코드는 **sandboxed** 환경에서 테스트된다
- Software patches가 만들어지고 테스트된다.

    → 만약 patch가 효율적이라면, application host에게 전송되어 적용된다

## Snort inline

: Snort system의 IPS 버전을 변경한 것