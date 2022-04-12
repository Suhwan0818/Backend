# [TCP] 3 way handshake & 4 way handshake  

    연결을 성립하고 해제하는 과정을 의미한다.  

### **3 way handshake - 연결 성립**  

TCP는 정확한 전송을 보장해야 한다. 따라서 통신하기 앞서, 논리적인 접속을 성릭하기 위해 3 way handshake 과정을 진행한다. 한번 8000번과 15000번을 사용해서 예시를 들어보겠다.  

![3wayHand](/img/3wayHand.png)  

1. 클라이언트가 서버에게 SYN(Synchronize sequence numbers) 패킷을 보냄 (sequence: x)  

2. 서버가 SYN(x)을 받고, 클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보냄 (sequence: y, ACK: x + 1)  

3. 클라이언트는 서버의 응답은 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보냄  

이렇게 3번의 통신이 완료되면 연결이 성립한다.(**3번이라 3 way handshake라고 부른다.**)  

### **4 way handshake - 연결 해제**  

연결 성립 후, 모든 통신이 끝났다면 해제해야 한다. 위 같은 예시에 숫자 없이 x, y로 예시를 보겠다.  

![4wayHand](/img/4wayHand.PNG)  

1. 클라이언트가 서버에게 연결이 종료됬다는 FIN 플래그를 보낸다.  

2. 서버는 FIN 신호를 받고, 확인했다는 ACK를 클라이언트에게 보낸다. (이때 모든 데이터를 보내기 위해 CLOSE_WAIT 상태가 된다.)  

3. 데이터를 모두 보냈다면, 연결이 종료되었다는 FIN 플래그를 클라이언트에게 보낸다.  

4. 클라이언트는 FIN 신호를 받고, 확인했다는 ACK를 서버에게 보낸다. (아직 서버로부터 받지 못한 값이 있을 수 있으므로 TIME_WAIT 상태로 기다린다.)  
    - 서버는 ACK를 받은 후 소켓을 닫는다 (Closed)  
    - TIME_WAIT 시간이 끝나면 클라이언트도 닫는다 (Closed)  

이렇게 4번의 통신이 완료되면 연결이 해제된다. 한번 전체과정을 그려보겠다.  

![handShakeTotal](/img/handshakeTotal.PNG)  

윗 내용이 모두 이해가 되었다면 전체 과정이 어느정도 눈에 들어올것이다. 이것이 연결과 해제를 위한 TCP 과정이다.  