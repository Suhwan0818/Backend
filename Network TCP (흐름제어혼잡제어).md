# TCP (흐름제어/혼잡제어)  
들어가기 전  
- **TCP 통신이란?**  
    - 네트워크 통신에서 신뢰적인 연결방식  
    - TCP는 기본적으로 unreliable network에서, reliable network를 보장할 수 있도록 하는 프로토콜  
    - TCP는 network congestion avoidance algorith을 사용  

- **reliable network를 보장한다는 것은 4가지 문제점 존재**  
    - 손실 : packet이 손실될 수 있는 문제  
    - 순서 바뀜 : packet의 순서가 바뀌는 문제  
    - Congestion : 네트워크가 혼잡한 문제  
    - Overload : receiver가 overload 되는 문제  

- **흐름제어/혼잡제어란?**  
    - 흐름제어 (endsystem 대 endsystem)  
        - 송신측과 수신측의 데이터 처리 속도 차이를 해결하기 위한 기법  
        - Flow Control은 receiver가 packet을 지나치게 많이 받지 않도록 조절하는 것  
        - 기본 개념은 receiver가 sender에게 현재 자신의 상태를 feedback 한다는 점  
    - 혼잡제어 : 송신측의 데이터 전달과 네트워크의 데이터 처리 속도 차이를 해결하기 위한 기법  

- **전송의 전체 과정**  
    - Application layer : sender application layer가 socket에 data를 씀.  
    - Transport layer : data를 segment에 감싼다. 그리고 network layer에 넘겨줌.  
    - 그러면 아랫단에서 어쨋든 receiving node로 전송이 됨. 이 때, sender의 send buffer에 data를 저장하고, receiver는 receive buffer에 data를 저장함.  
    - application에서 준비가 되면 이 buffer에 있는 것을 읽기 시작함.  
    - 따라서 flow control의 핵심은 이 receiver buffer가 넘치지 않게 하는 것임  
    - 따라서 receiver는 RWND(Receive WiNDow) : receive buffer의 남은 공간을 홍보함  

1. **흐름 제어(Flow control)**  
- 수신측이 송신측보다 데이터 처리 속도가 빠르면 문제가 없지만, 송신측의 속도가 빠를 경우 문제가 생긴다.  
- 수신측에서 제한된 저장 용량을 초과된 이후 데이터는 손실될 수 있으며, 만약 손실된다면 불필요하게 응답과 데이터 전송이 송/수신 측 간에 빈번히 일어난다.  
- 이러한 문제를 줄이기 위해 송신측의 데이터를 수신측에서 조절해야 한다.  