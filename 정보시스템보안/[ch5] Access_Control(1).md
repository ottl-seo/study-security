# 5. Access Control

## Access Control 이란

multi user 시스템에서   
누가, 어디에 접근할 수 있는지 권한 부여하는 **security policy**

—> 권한 가진 사람만 접근할 수 있게 함

## Access Control Context

*1. Authentication   
2. Authorization   
3. Audit*

![Untitled](https://user-images.githubusercontent.com/61778930/115970314-802ea380-a57c-11eb-8878-0392493a8f5b.png)

### 1. Authentication (인증)

### 2. Authorization (권한 부여)

Authorization database에 which user / what file / what way 내용이 저장되어 있음

### 3. Audit (모니터링)

위 과정 (인증→ 권한 확인 → match → 접근 허용)을 모니터링함

## Access Control Policies

— *정책 종류*

*1. **DAC** (Discretionary access control)   
2. **MAC** (Mandatory ...)   
3. **RBAC** (Role-based ...)   
4. **ABAC** (Attribute-based ...)*

### 1. DAC

- 사용자의 identity에 기초하여 접근 제어 (ex. ID, name ...)   
- "**Discretionary**": 임의의

    since an entity might have access rights that permit the entity, by its own volition, to enable another entity to access some resource       
    ; 접근 권한을 가진 사람이 원하면 다른 사람에게 권한을 **임의로** 부여할 수 있음.

### 2. MAC

**Mandatory** ↔ discretionary의 반대

- 절대적인 기준에 따라 접근 제어

    comparing security labels with security clearance
    ; 정해진 보안 기준과 (사용자의) 권한 비교

- more **secure**, but **inconvinient...** 불편해
- "**Mandatory**": 절대적인

    since an entity that has clearance to access a resource **may not**, just by its own volidation, enable another entity to access that resource.
    ; 다른 사람에게 권한을 부여할 수 없음. 

- 군대 등 Top secret 다룰 때 쓰임

### 3. RBAC

'**Role**' 기반 
+ 어떤 접근이 유저에게 가능한지-를 설명하는 규칙에 기반

— DAC과 비교
비슷한데, 유저의 identity 대신 역할 role을 부여하여, 그 역할에 맞게 접근을 제어한다.
![Untitled 1](https://user-images.githubusercontent.com/61778930/115970339-a2c0bc80-a57c-11eb-950b-776bbd92672d.png)

### 4. ABAC

유저 role에+ **현재 상황**까지 고려한 방식.

유저는 role을 가짐, role은 attribute를 가짐.   
—> attribute에 기반하여 접근이 제어됨 (role 내에서도 상황별 attribute가 여러개)   
- - -
## Basic elements

- **Subject** ; 접근하는 주체 ( Owner / Group / World )   
- **Object** ; 접근 제어되는 대상 ex. 파일, 디렉토리, 프로그램 등...   
- **Access right**   
; Read/ Write/ Execute/ Delete/ Create/ Search 등에 대한 권한   
