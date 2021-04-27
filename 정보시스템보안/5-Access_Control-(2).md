# 5-Access_Control-(2)

DAC, RBAC, ABAC에 대하여 더 자세히 알아보기-

# **1. DAC**

## DAC 구현 방식

*1) **Access matrix**
2) **ACL** (Access Control List)
3) **Capability tickets**
4) **Authorization table***

### *1) Access matrix*

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled.png)

- **단점**

User A의 File 2에 대한 권한은 none.   

—> empty 발생 (불필요한 메모리 점유)

### 2) ***ACL** (Access Control List)*

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%201.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%201.png)

; column별(File)로 나누어서 리스트업

- 각 **파일별** 유저의 권한을 볼 수 있다.
- LinkedList로 empty 문제 해결!
- **Good for**: 리소스 기준으로 보기 쉽다(ex.리소스 하나에 대해 모든 유저의 권한 확인)
- **Bad for**: 한 유저가 가진 권한 모아보기 불편, 데이터 많아지면 탐색이 어렵다.

### 3) ***Capability tickets***

1번 매트릭스의 방식 + **divided by rows**

- **Good for**: 한 유저가 어떤 권한 가지고 있는가 (== ACL의 불편한 점 해결)
- **Bad for**: 한 리소스에 대해 유저별로 모아보기 어렵다 (== ACL 장점)

### 4) ***Authorization table***

subject (유저) / 권한 / object(파일) 로 테이블 만들기

- sort 가능
- 위의 장단점 해결

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%202.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%202.png)

---

## A general model for DAC

### Extending objects and their access rights

- **Processes**; 프로세스의 진행 상황
- **Devices**; 디바이스별로 접근 제어 ok - ex.디스크 별로 다르게 접근 제어
- **Memory locations**
- **Subjects**

*— 매트릭스로 나타내기*

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%203.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%203.png)

### Access control system commands

- **A[S, X]** 의 뜻

    subject S가 파일 X에 접근할 수 있다.    

※참고 —> A[S ,X]를 *attribute*라고 부름   

- **copy flag**
- 다양한 commands 정리

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%204.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%204.png)

### Protection domain

- 

## UNIX file access control

— inode 이용하여 10 bits를 access control에 사용한다 (ex. mode 644 )

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%205.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%205.png)

### For directory

- **Read**: 리스트 보기 권한 (**ls** directory)
- **Write**: 디렉토리 내 파일 Create/Rename/Delete 권한
- **Execute**: 디렉토리 내부로 들어가거나(**cd** directory), 파일 이름으로 검색 권한

⇒ 해당 파일에 권한이 없어도, 상위 디렉토리에 권한이 있으면 위와 같은 권한으로 접근 가능! 하다는 점 기억

### remaining 3 bits... (특수 권한)

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%206.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%206.png)

### 1) **SetUID (4)**

슈퍼유저(root)만 접근할 수 있는 파일에 다른 사용자가 접근하게 하고자 할 때 SetUID 사용     
(실행 순간만 다른 사용자가 소유자 권한을 빌려와 실행한다고 생각하면 쉬움)   

**※** *setuid 설정 시 주의할 점*      
: root 권한이 필요없는 프로그램에도 setUID 권한을 남발하면, root가 아닌 사용자가 권한을 부여하게 될 수 있음 → **권한 상승 우려** 때문에 setuid는 최소화해야 함.   

*ex.*

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%207.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%207.png)

`**-rwsr-xr-x 1 root root`** 에서    

'**s**' → setUID 설정 ***//일반 사용자도 루트 권한으로 패스워드 변경 가능***      

**root** → 소유자는 root   

— 자동으로 passwd 변경 시 setuid 사라지는 예시 **↓**    

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%208.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%208.png)

일반 사용자 계정으로 패스워드 변경(execute) → 변경 후 다른 디렉토리 ( /shadow 또는 /passwd 등)에 접근 할 수 없게 됨    
자동으로 passwd 변경 시 setuid 사라진다.   

### 2) **SetGID (2)**

setuid는 디렉토리에 권한 부여 불가,    
**setGID는 가능하다**는 내용 → 권한 부여 후 새로 생긴 파일들에 대해서만 그룹이 권한 부여 받음

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%209.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%209.png)

### 3) **'sticky bit' (1)**

sticky bit을 디렉토리에 적용 시 —    

- **Only the owner** can Rename, Move, or Delete
- useful for shared temporary dir (ex. /tmp. /var/tmp 등...)

# 2. RBAC

### RBAC model

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%2010.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%2010.png)

### Role의 계층 구조

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%2011.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%2011.png)

### 표현 예시

B가 A에 대한 권한 가지는 경우 → 아래 두번째 그림처럼 Access Right을 생략하여 표현 가능

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%2012.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%2012.png)

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%2013.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%2013.png)

# 3. ABAC

### RBAC 과 어떻게 다른가

- RBAC은 서브젝트와 role 간의 관계만 복잡하게 할 수 O,    
- ABAC은 뿐만 아니라 role-Object 간 관계도 설정 가능

— 접근권한 제어에 있어, **resource**와 **subject 둘 다**에 기초하고 있는 게 ABAC의 특징

### ABAC attributes

- **Subject** attributes
- **Object** attributes
- **Environment** attributes (!)

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%2014.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%2014.png)

### ABAC policies

- **policy**: 정책. subject와 Object가 어떻게 relation 되어 있는지를 정의해둔 내용
- **privileges**: 권한. subject에 권한을 부여하는 행위
- **Policy rule base (=policy store)**: 여러 policy rules를 저장
- **Access control decision process:** evaluation of applicable policy rules in the policy store

### ABAC policy model

— 예시   

![5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%2015.png](5-Access_Control-(2)%20d51d3d030a2f4afbb0d8c4c8d32f53a9/Untitled%2015.png)

### 장단점

**Pros:** 유연함, 더 많은 표현이 가능해짐   

**Cons:** 좀 더 복잡한 과정을 표현해야 함   

---

[UNIX 예제 코드]() 출처: [https://eunguru.tistory.com/115](https://eunguru.tistory.com/115)