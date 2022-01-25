# 소켓 (Socket)

> 네트워크상에서 동작하는 프로그램 간 통신의 종착점 (Endpoint)
>
> IP 주소와 port 번호를 조합한 네트워크 주소
>
> 네트워크 상에서 서버와 클라이언트 2개의 프로그램이 특정 포트를 통해 양방향 통신이 가능하도록 만들어주는 소프트웨어 장치



소켓(Socket)이란 네트워크 상에서 동작하는 프로그램 간 통신의 종착점 (Endpoint) 이다.

즉, 프로그램이 네트워크에서 데이터를 통신할 수 있또록 연결해주는 연결부 라고 할 수 있다.



> Endpoint : IP Address 와 port 번호의 조합을 뜻하며 최종 목적지를 나타낸다.
>
> 예시로 최종목적지는 사용자의 디바이스(PC, 스마트폰 등) 또는 Server 가 될 수 있다.



데이터를 통신할 수 있도록 해주는 연결부이기 때문에 통신한 두 프로그램 (Client, Server) 모두에 소켓이 생성되어야 한다.



Server는 특정 포트와 연결된 소켓 (Server 소켓)을 가지고 컴퓨터 위에서 동작하게 되는데,

이 Server는 소켓을 통해 Client 측 소켓의 연결 요청이 있을 떄 까지 기다리고 있는다. (listening)



Client 소켓에서 연결요청을 하면 (올바른 port로 들어왔을 때) Server 소켓이 허락을 하여 통신을 할 수 있도록 연결 되는 것이다.



![image-20220125165929436](md-images/image-20220125165929436.png)





---

참고사이트



소켓이란 무엇인가 블로그 : https://www.daleseo.com/what-is-a-socket/

Socket 이란 블로그 : https://medium.com/@su_bak/term-socket%EC%9D%B4%EB%9E%80-7ca7963617ff

소켓 통신이란 블로그 : https://helloworld-88.tistory.com/215

TCP/IP 소켓이란 블로그 : https://popbox.tistory.com/66

