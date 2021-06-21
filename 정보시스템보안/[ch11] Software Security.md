# [ch11] Software Security

## Defensive programming?

***== secure programming***  

- 소프트웨어에서 발생할 수 있는 bug들을 예방하도록 설계하는 방식
- 공격을 받았을 때에도 소프트웨어가 기존 기능을 수행하도록 설계함
- - 기존 프로그래밍: 기능에만 초점,   
→ defensive 프로그래밍: exceptional case를 고려하여 악성행위에 대비함

### Handling program input

- ***input size and buffer overflow***   
→ 일정 크기를 넘어가는 input은 buffer overflow를 발생시킬 수 있으므로,    
자동으로 **discard**하여 계속 프로그램이 돌아가게
- ***Interpretation of program input***   
→ Text data vs binary data

### → 예시: Heartbleed 공격

Heartbeat 프로토콜의 Bug에 해당하는 공격   

- OpenSSL 라이브러리가 메시지의 **length**는 verify하지 않는다는 점을 이용
- "Buffer over-read"

![%5Bch11%5D%20Software%20Security%20ebb120aa87794f6c9a37e6843b9a92b1/Untitled.png](%5Bch11%5D%20Software%20Security%20ebb120aa87794f6c9a37e6843b9a92b1/Untitled.png)

# Injection attacks

input data가 프로그램 실행에 영향을 끼치도록 하는 공격

- Command injection attack
- SQL injection attack
- Code injection attack