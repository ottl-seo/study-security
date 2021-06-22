# Injection attacks

# Injection attacks

input data가 프로그램 실행에 영향을 끼치도록 하는 공격

- Command injection attack
- SQL injection attack
- Code injection attack

## 1. Command injection attack

input으로 **command** 삽입 → 서버 내 파일 노출, 소스코드 노출 위험   

ex)

```bash
xxx; echo attack success; ls -l *
```

→ 결과   

![Injection%20attacks%204c3c32ddf1874c4b918b79066f7d8fa0/Untitled.png](Injection%20attacks%204c3c32ddf1874c4b918b79066f7d8fa0/Untitled.png)

### → safer code (input validation)

![Injection%20attacks%204c3c32ddf1874c4b918b79066f7d8fa0/Untitled%201.png](Injection%20attacks%204c3c32ddf1874c4b918b79066f7d8fa0/Untitled%201.png)