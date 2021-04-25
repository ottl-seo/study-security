# Bloom-filter

특정 원소가 집합 내에 **존재하는지 확인**하는 데 사용되는 자료구조

## 배경

bad password dictionary 사용 시, 공간 및 시간 낭비 발생   

→ Solution: **Bloom filter**

## 원리

bad password dictionary = **D**   
input password = **B**

### **1)** D에 원소 **추가**

해시함수(h1, h2, ... , hk) 사용하여 특정 bit 해싱   
→ 해시값만 저장

### **2)** D에 B 원소 있는지 **검사**

input B 들어왔을 때, B도 동일한 방식으로 해싱    
→ D의 해시값과 비교

#### ⇒ k개의 해시 계산이 병렬적으로 수행될 수 있다면 속도측면에서 큰 이점을 얻을 수 있다!   

**※** 정확도는 떨어짐- ****hash collision 때문에 ***False positive*** 발생 가능   
![Untitled](https://user-images.githubusercontent.com/61778930/116006286-8ccafe80-a645-11eb-8537-8482ad92509b.png)

[Bloom Filter 실습 사이트](https://llimllib.github.io/bloomfilter-tutorial/)

## 특징

- 특정 원소가 집합 내에 **존재하는지 확인**하는 데 사용되는 자료구조
- 정확도 **↓,** 대신 ****공간 사용 ****최소화할 목적
- **false positive 발생 가능**
- false negative는 발생 **X**
![Untitled 1](https://user-images.githubusercontent.com/61778930/116006295-93f20c80-a645-11eb-9cb2-b5a94566d8c8.png)


## Error rate 줄이는 방법
![Untitled 2](https://user-images.githubusercontent.com/61778930/116006300-994f5700-a645-11eb-880a-1a36f1141827.png)


1. **k** 늘리기 (해시함수 개수 **많이**)
2. **N/d** : 딕셔너리 크기(d)에 따른 해시테이블 사이즈(N)
![Untitled 3](https://user-images.githubusercontent.com/61778930/116006303-9eaca180-a645-11eb-982c-f7e2a98d51a6.png)
