# 3-Password

## A model for user authentication

사용자 인증 모델    

: 등록 → 인증

## authentication 수단

- something the individual **knows** ;password, PIN, ...
- something the individual **possesses** ;key, physical key, smart cards, ...
- something the individual **is** ; 생체정보(지문, 홍채, 얼굴 등)
- something the individual **does**; voice pattern, 손글씨 등

—> 각자의 방법은 문제가 있따    

*ex.*    

*password* ;잃어버리거나, stolen되거나, 추적(guess)당할 수 있다, 관리의 번거로움    

*biometrics* ;false positive / false negative 문제 발생, 한 번 해킹 당하면 변경이 어려움    

**—> Multifactor authentication 으로 해결**

# Password-based authentication

### Vulnerability of pw

- 

### Hashed and Salted passwords 방법
![Untitled](https://user-images.githubusercontent.com/61778930/116435145-05200280-a886-11eb-8390-4f8997305fc2.png)

password file: ***(id, salt, h)*** 쌍으로 저장   ※pw 자체는 저장하지 않는다!!!    
![Untitled 1](https://user-images.githubusercontent.com/61778930/116435170-094c2000-a886-11eb-8310-926a69d62261.png)


사용자의 pw 입력값에 1)salt 붙이고    

2)**해싱**하여    

3)hash와 **compare**   

⇒ ***`h = H(salt || password)`*** 

### UNIX의 예시

8 bytes long!

1. 7-bit ASCII, this makes 56-bit DES key k
2. salt 정하기 → [a-zA-Z-0-9./] 에서 2개 문자 골라
3. for slowness → 0^64를 25번 암호화

# Password 공격

- **Offline dictionary attack**
- **Specific account attack** ; 특정 계정을 노리고 추측하는 online attack
- **Popular password attack** ; 쉽고 유명한 pw 넣어보기(ex. 12345678)
- **Password guessing against single user** ; ex.birthday attack- 모든 pw를 모두에게 시행해보는 공격
- **Workstation hijacking**
- **Exploit user mistakes** ; 유저의 실수로 유출하는 경우 or **Social engineering**(공유 문서 등에 패스워드 노출하는 사례)
- **Exploiting multiple password use** ; 여러 사이트에서 같은 pw 사용
- **Electronic monitoring** ; 도청 등

---

# **Entropy**

shannon entropy가 가장 잘 알려져 있음

### idea develop 과정 —-

**1)** n bits string의 password **x** 에서,    

- 한 bit 맞을 확률 =1/2
- **n bits** 맞을 확률 = **1/2^n** == 이걸 **P[X=x]** 라고 하자.

  

**2)** 수식 정리   

P = 1/2^n = 2^(-n)    

→ -n = log2 (P)    

**→ n = - log2 (P)**    

**3)** entropy 정의하기   

;모든 일어날 수 있는 사건에 대한 (확률 × 정보량) 값을 합친 것   
![Untitled 2](https://user-images.githubusercontent.com/61778930/116435218-16690f00-a886-11eb-9583-634d6f8dc856.png)

참고
![Untitled 3](https://user-images.githubusercontent.com/61778930/116435312-2ed92980-a886-11eb-9532-37701afeeced.png)


## Min-entropy

= guessing entropy
![Untitled 4](https://user-images.githubusercontent.com/61778930/116435327-33054700-a886-11eb-8bf0-e92eda80b0f7.png)
![Untitled 5](https://user-images.githubusercontent.com/61778930/116435338-34367400-a886-11eb-9282-010195e93b36.png)


## Password와 Entropy 사이 관계

**Min-entropy가 작다.**     

⇒ PW **쉽다**.    

⇒ Entropy 는 크다.   

- *예시.*

    x가 000..0 일 확률 **P**[X=0^n] = **1/2** 라고 하자.    

    *→ **max(P)** = 1/2*    

    **⇒ min-entropy** 구하기: H∞(x) = -log max(P) 이므로, 값은 1이 된다 (***small*** 값)    

    **⇒ entropy** 구하기: H(x) = - ∑ P log P, P=1/2^n 이므로 H(x) = 약 **n/2 (big)**    

- 그러므로 Entropy를 계산하여 쉬운 비밀번호를 판별할 수 있다 → 일정 수준보다 쉬운 경우 reject

# Password checking

- ***Reactive**:* 즉각 반응
- ***Proactive***: 사전에 예방 → 정책 강화

    방법1) 조건 두기. ex. 16자 이상, 대문자 사용, 특수문자 사용 등.    

    방법2) **'bad' password 딕셔너리** 만들어 해당될 경우 **reject**

# Bloom filter

### 배경

bad password dictionary 사용 시, 공간 및 시간 낭비 발생   

→ Solution: **Bloom filter**

### 특징

- 특정 원소가 집합 내에 **존재하는지 확인**하는 데 사용되는 자료구조
- 정확도 **↓,** 대신 ****공간 사용 ****최소화할 목적
- **false positive 발생 가능**
- false negative는 발생 **X**

### 원리

bad password dictionary = **D**   
input password = **B**

**1)** D에 원소 **추가**

해시함수(h1, h2, ... , hk) 사용하여 특정 bit 해싱   

→ 해시값만 저장

**2)** D에 B 원소 있는지 **검사**

input B 들어왔을 때, B도 동일한 방식으로 해싱    

→ D의 해시값과 비교

⇒ k개의 해시 계산이 병렬적으로 수행될 수 있다면 속도측면에서 큰 이점을 얻을 수 있다!   

**※** 정확도는 떨어짐- ****hash collision 때문에 ***False positive*** 발생 가능   
![Untitled 6](https://user-images.githubusercontent.com/61778930/116443368-88dded00-a88e-11eb-9eab-28c3fe9608dd.png)

[Bloom Filter 실습 사이트](https://llimllib.github.io/bloomfilter-tutorial/)
![Untitled 7](https://user-images.githubusercontent.com/61778930/116443392-8e3b3780-a88e-11eb-8f2d-4526d2fb4950.png)

### Error rate 줄이는 방법
![Untitled 8](https://user-images.githubusercontent.com/61778930/116443405-909d9180-a88e-11eb-8e56-05e2bb02cf05.png)

1. **k** 늘리기 (해시함수 개수 **많이**)
2. **N/d** : 딕셔너리 크기(d)에 따른 해시테이블 사이즈(N)
![Untitled 9](https://user-images.githubusercontent.com/61778930/116443432-9c895380-a88e-11eb-9bea-eabf8d4ade38.png)

## Renewing passwords
![Untitled 10](https://user-images.githubusercontent.com/61778930/116443444-a01cda80-a88e-11eb-9952-4649de3aae5d.png)
