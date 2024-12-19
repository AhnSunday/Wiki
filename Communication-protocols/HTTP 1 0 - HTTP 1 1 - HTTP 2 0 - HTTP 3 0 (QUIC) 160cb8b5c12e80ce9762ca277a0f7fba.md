# HTTP 1.0 -> HTTP 1.1 -> HTTP 2.0 -> HTTP 3.0 (QUIC)

![http3.jpg](HTTP%201%200%20-%20HTTP%201%201%20-%20HTTP%202%200%20-%20HTTP%203%200%20(QUIC)%20160cb8b5c12e80ce9762ca277a0f7fba/http3.jpg)

# **각 HTTP세대별로 해결하고자 한 문제**

## **HTTP 1.0**

### **가능했던 기능**

- 기본 요청-응답 모델을 지원
    - 클라이언트가 요청을 보내면 서버가 응답을 반환하는 가장 기초적인 웹 통신 구조를 제공했습니다.
- 단일 TCP 연결
    - 매 요청마다 새 TCP 연결을 열고 닫기 때문에 단순한 방식을 제공했습니다.
- 기본적인 HTTP 메소드(GET, POST, HEAD)와 상태 코드 지원
    - 이를 통해 웹의 기본 골격을 형성했습니다.

### **문제점**

같은 서버에 대한 요청마다 새로운 TCP 연결을 맺어야 했습니다.

요청과 응답마다 TCP 연결 비용이 큼 → 응답 시간이 느려짐

그래서 각 요청마다 매번 connection을 새로 열어야 하는 비효율적인 방식을 개선에 대한 필요성을 인식했습니다.

## **HTTP 1.1**

### **가능했던 기능**

- Persistent Connection
    - 한 번 연결을 맺으면 여러 요청을 지속적으로 처리가 가능했습니다.
    - 결론적으로 연결을 재사용해서 성능을 개선했습니다.
- Host 헤더 필수화
    - 하나의 IP, 즉 서버에서 여러 도메인 호스팅이 가능했습니다.
- Chunked Transfer Encoding
    - 응답 본문을 조각으로 나눠서 전송이 가능했습니다.
    - 이로 읺서 동적인 콘텐츠 스트리밍이 용이해졌습니다.
- HTTP 메소드 확장
    - 기존의 GET, POST, HEAD에 PUT, DELETE 등의 추가 표준화가 진행됐습니다.

### **문제점**

- Head-of-Line Blocking
    - 하나의 연결에서 요청들을 순서대로 처리하는 특성을 가지고 있어서, 앞선 요청이 지연된다면 뒤 요청도 대기를 했습니다.
- 브라우저 동시 연결 제한
    - 브라우저는 병렬 요청을 위해 여러 TCP 연결을 열었지만, 개수 제한이 있어서 HOL 블로킹 문제를 완벽하게 해결하지 못했습니다.
- Pipelining가 비실용적이었음
    - 요청을 미리 파이프라인 형태로 보내도 응답 순서 제약으로 인해서 실질적으로 개선이 제한되었습니다.

## **HTTP 2.0**

### **가능했던 기능**

- Multiplexing
    - 단일 TCP 연결 위에서 여러 스트림으로 동시 요청/응답을 처리합니다.
    - 이를 통해 1.1의 어플리케이션 레벨 HOL 블로킹을 해결했습니다.
- Binary Framing
    - 텍스트 기반에서 binary로
        
        <aside>
        💡
        
        HTTP 프로토콜은 요청/응답 메시지를 사람이 읽기 쉬운 텍스트 형태(ASCII)로 주고받았습니다. 예를 들어, 요청 헤더나 응답 헤더는 줄바꿈 문자(\r\n)로 구분되는 텍스트 기반 포맷이었죠.
        
        이 때 서버나 클라이언트는 텍스트를 일일이 읽어들이면서 줄 단위로 구분하고, 헤더 이름과 값을 파싱하는 등 문자열 처리 과정을 거쳐야 했습니다. 이 과정은 문자열을 계속 분석해야 하므로 비교적 느리거나 비효율적일 수 있습니다.
        
        HTTP/2는 요청과 응답을 바이너리 프레임(이진 데이터 덩어리)로 처리합니다. 즉, 텍스트 기반 파싱 대신, 미리 정의된 바이너리 형식에 따라 고정된 크기나 명확한 필드 구조를 갖는 패킷 형태를 사용합니다.
        이진 데이터는 컴퓨터가 직접 해석하기 쉬운 형태이므로, 각 필드에 바로 접근할 수 있고, 줄바꿈 등 텍스트 처리 과정 없이 정해진 오프셋(offset)에서 필요한 정보를 바로 읽어올 수 있습니다.
        
        </aside>
        
    - 텍스트 기반에서 바이너리 기반 프로토콜로 전환했습니다.
    - 효율적으로 파싱을 해서 성능을 개선했습니다.
- Header Compression
    - 반복되는 헤더 정보를 압축이 가능해졌습니다.
    - 크기가 줄어들어 속도가 빨라졌지요
- Server push
    - 서버가 클라이언트가 아직 요청하지 않은 자원을 미리 푸시합니다.
    - 페이지 로딩 시간이 단축될 수 있겠지요

### **문제점**

- TCP 기반 HOL
    - TCP 계층에서 패킷 손실이 발생하면, 해당 패킷을 기다리느라 같은 연결 내 모든 스트림이 지연됐습니다.
    - 여전히 전송 게층 수준의 HOL 문제가 존재했지요.
- 지연 최소화의 한계
    - 연결 재수립 시 TCP Handshake 및 혼잡 제어로 인해서 지연이 존재했습니다.
    - **TCP 핸드셰이크(TCP Handshake)란?**
        
        <aside>
        💡
        
        **TCP**
        
        TCP(Transmission Control Protocol)는 신뢰성 있는 데이터 전송을 위해 사용되는 전송 계층 프로토콜입니다. 웹 통신에서 HTTP/1.x나 HTTP/2는 기본적으로 TCP 위에서 동작합니다.
        
        **핸드셰이크 과정**
        
        TCP는 클라이언트와 서버 간에 연결을 맺을 때, 단순히 “연결해!” 하고 바로 통신하는 것이 아니라 “3-way handshake”라 불리는 일종의 인사(합의) 과정을 거칩니다.
        
        3-way handshake
        
        1.	**SYN:** 클라이언트 → 서버 : “연결할 준비 됐어?(SYN)”
        
        2.	**SYN+ACK:** 서버 → 클라이언트 : “나도 준비 완료!(SYN), 너 신호 받았어(ACK)?”
        
        3.	**ACK:** 클라이언트 → 서버 : “신호 잘 받았고, 그럼 이제 데이터 주고받자(ACK).”
        
        이 과정을 통해 양측은 통신할 준비가 되었음을 서로 확인하고, 이후 데이터 전송을 시작합니다. Handshake 덕분에 네트워크 패킷 손실이나 지연 문제에도 불구하고 연결 상태를 명확히 파악할 수 있고, 신뢰성 있는 통신 환경을 구축할 수 있습니다.
        
        </aside>
        

## **HTTP 3.0**

### **가능했던 기능**

- QUIC 기반 UDP 전송
    - TCP 대신 QUIC를 사용했습니다
    - **QUIC(Quick UDP Internet Connections)란?**
        
        <aside>
        💡
        
        • **정의**
        
        QUIC는 구글(Google)에서 개발하고, 이후 IETF(Internet Engineering Task Force)에서 표준화한 새로운 전송 계층 프로토콜입니다.
        
        기존 HTTP는 TCP 위에서 동작하였는데, HTTP/3는 TCP 대신 QUIC를 기반으로 합니다.
        
        • **특징**
        
        • **UDP 기반**
        
        QUIC는 TCP 대신 UDP(사용자 데이터그램 프로토콜) 위에서 동작합니다. UDP는 TCP보다 단순하고 연결 설정에 대한 복잡한 handshake가 필요 없어, 빠른 반응성을 제공할 수 있습니다.
        
        • **TCP의 기능 + 추가 개선**
        
        QUIC는 UDP 위에서 동작하면서도 TCP가 제공하는 혼잡 제어, 신뢰성 보장, 순서 정렬 등을 자체적으로 구현하고 있습니다. 즉, UDP의 장점을 살리면서 TCP가 하던 일을 QUIC 프로토콜 레벨에서 직접 처리하는 것이지요.
        
        </aside>
        
- TLS 통합, 0-RTT 핸드셰이크
    - 보안이 프로토콜 자체에 내장되고, 초기 연결 수립 지연을 최소화했습니다.
    - **TLS 통합과 0-RTT 핸드셰이크란?**
        
        <aside>
        💡
        
        • **TLS 통합**
        
        기존 HTTP/1.1, HTTP/2는 TCP로 연결을 맺은 뒤, 별도로 TLS(보안 계층) 핸드셰이크를 통해 암호화를 시작했습니다. 즉, 연결을 수립하고, 추가로 보안 채널을 맺는 순서였지요.
        
        하지만 QUIC는 프로토콜 자체에 TLS가 내장되어 있습니다. 즉, QUIC 연결을 맺는 순간부터 이미 암호화된 통신 환경이 바로 확보됩니다.
        
        이를 통해 보안과 연결 설정 단계를 통합하여 초기 연결 시간을 단축할 수 있습니다.
        
        • **0-RTT(제로 라운드 트립 타임) 핸드셰이크**
        
        전통적인 TLS/1.2나 TCP 연결은 몇 번의 왕복이 필요합니다. 즉, 클라이언트와 서버가 “인사”하고 “암호화 여부 확인” 등을 하는데 일정한 왕복 시간이 소요되지요.
        
        QUIC의 0-RTT는 이전에 연결한 적이 있는 서버라면, 재접속 시 추가적인 핸드셰이크 지연 없이(또는 매우 최소화된 형태로) 바로 데이터를 보내는 것이 가능하게 합니다.
        
        이는 초기 연결 지연(Latency)을 크게 줄여, 페이지 로딩이나 리소스 요청을 더 빠르게 시작할 수 있습니다.
        
        </aside>
        
- 독립적인 스트림 처리
    - 한 스트림에서 패킷 손실이 발생해도 다른 스트림에 영향이 없어졌습니다.
    - 이로 인해 HOL 블로킹을 근본적으로 해결했습니다.
- 더 빠른 접속과 복원력
    - 네트워크 경로 변경 시에도 연결 재수립을 최소화 합니다.
    - 모바일 환경에서 유용합니다.

### **문제점**

- 인프라 적응 필요
    - UDP 기반 QUIC는 기존 방화벽, 네트워크 장비의 UDP 처리 특성을 고려할 필요가 있습니다.
- 초기 도입하는 기술이라 일부 환경에서는 제한적으로 지원합니다.

최종 간단한 정리

| 버전 | 특징 | 해결한 문제 | 한계 |
| --- | --- | --- | --- |
| HTTP/1.0 | 모든 요청마다 새로운 연결 | 기본적인 웹 요청/응답 체계 확립 | 비효율적 연결 관리, 낮은 성능 |
| HTTP/1.1 | 지속적 연결, Host 헤더, 기본 성능 개선 | 연결 재사용으로 성능 개선, 가상호스팅 가능 | HOL Blocking 문제 여전 |
| HTTP/2.0 | Multiplexing, Binary, Header Compression, Server Push | 앱 레벨 HOL 제거, 다중 요청 병렬 처리 | TCP 기반 HOL 문제 지속, 전송 계층 한계 |
| HTTP/3.0 | QUIC(UDP) 기반, 스트림 독립적 처리, 0-RTT | HOL Blocking 근본적 해결, 빠른 연결, 보안 통합 | 초기 도입기, 인프라 적응 필요 |

• HTTP/1.0 → 1.1: 매 요청마다 TCP 연결을 여는 비효율성을 개선, 지속 연결(persistent connection)도입

• HTTP/1.1 → 2.0: 단일 연결에서 다중 스트림을 통해 HOL Blocking(어플리케이션 레벨) 감소, 헤더 압축 및 서버 푸시 등 성능 개선

• HTTP/2.0 → 3.0: QUIC 프로토콜 기반으로 TCP에 내재된 HOL Blocking 문제 해결, 더 빠른 연결 수립, 패킷 손실에 대한 탄력적인 대응