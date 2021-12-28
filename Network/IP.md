## IP

### IP 주소 부여

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%204.png?raw=true)

> 송신 / 수신 클라이언트에서 정보를 주고 받을 때 사용하는 정보 위주의 프로토콜



<br>

<br>

### 인터넷 프로토콜 역할 (IP)

- 지정한 IP 주소 (IP Address)에 데이터 전달
- 패킷 (Packet)이라는 통신 단위로 데이터 전달

<br>

<br>



### IP 패킷 정보

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%205.png?raw=true)



<br>

<br>

### 클라이언트 패킷 전달

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%206.png?raw=true)

<br>

<br>



### 서버 패킷 전달

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%207.png?raw=true)



<br>

<br>

### IP 프로토콜의 한계

- 비연결성
  - 패킷을 받을 대상이 없거나 불능 상태여도 패킷 전송
- 비신뢰성
  - 중간에 패킷이 사라지면?
  - 패킷이 순서대로 안오면?
- 프로그램 구분
  - 같은 IP를 사용하는 서버에서 통신하는 어플리케이션이 둘 이상이라면?



<br>

<br>

### 대상이 서비스 불능, 패킷 전송

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%208.png?raw=true)



<br>

<br>

### 패킷 손실

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%209.png?raw=true)



<br>

<br>

### 패킷 전달 순서 문제 발생

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%2010.png?raw=true)

패킷의 용량이 클 때 나눠서 보내는데 2번이 먼저 도착할 수가 있다. 그래서 IP 프로토콜만으로 해결할 수 없다.