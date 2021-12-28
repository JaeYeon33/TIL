## TCP, UDP

### 인터넷 프로토콜 스택의 4계층

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%2011.png?raw=true)



<br>

<br>

### 프로토콜 계층

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%2012.png?raw=true)

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%2013.png?raw=true)





<br>

<br>

### IP 패킷 정보

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%2014.png?raw=true)



<br>

<br>

### TCP/IP 패킷 정보

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%2015.png?raw=true)



<br>

<br>



### TCP 특징

전송 제어 프로토콜 (Transmission Control Protocol)

- 연결지향 - TCP 3 way handshake (가상 연결)
- 데이터 전달 보증
- 순서 보장
- 신뢰할 수 있는 프로토콜
- 현재는 대부분 TCP 사용

<br>

<br>



### TCP 3 way handshake

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%2016.png?raw=true)



<br>

<br>



### 데이터 전달 보장

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%2017.png?raw=true)



<br>

<br>

### 순서 보장

![image](https://github.com/JaeYeon33/TIL/blob/main/Network/images/Untitled%2018.png?raw=true)



<br>

<br>

### UDP 특징

사용자 데이터그램 프로토콜 (User Datagram Protocol)

- 하얀 도화지에 비유 (기능이 거의 없음)
- 연결지향 - TCP 3 way handshake X
- 데이터 전달 보증 X
- 순서 보장 X
- 데이터 전달 및 순서가 보장되지 않지만, 단순하고 빠름
- 정리
  - IP와 같다. + PORT + 체크섬 정도만 추가
  - 어플리케이션에서 추가 작업 필요

<br>