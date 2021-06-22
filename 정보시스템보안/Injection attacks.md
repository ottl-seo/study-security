# Injection attacks

# Injection attacks

input data가 프로그램 실행에 영향을 끼치도록 하는 공격

- Command injection attack
- SQL injection attack
- Code injection attack

## 1. Command injection attack

input으로 **command** 삽입 → 서버 내 파일 노출, 소스코드 노출 위험   

malicious input 예시)

```bash
xxx; echo attack success; ls -l *
```

→ 결과   

![Untitled](https://user-images.githubusercontent.com/61778930/122948874-33b5e780-d3b6-11eb-84c8-31d74e2a7803.png)

### → safer code (input validation)

![Untitled 1](https://user-images.githubusercontent.com/61778930/122948934-3c0e2280-d3b6-11eb-8a5a-351787396ace.png)
