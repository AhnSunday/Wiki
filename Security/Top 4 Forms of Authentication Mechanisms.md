# Top 4 Forms of Authentication Mechanisms

### 1. SSH Keys

- **개념**:
    - SSH(Secure Shell) 키는 공개 키 암호화를 기반으로 한 인증 방식으로, 원격 시스템과 서버에 안전하게 접근하기 위해 사용
    - 패스워드보다 강력한 보안 제공
- **작동 방식**:
    1. **Key Pair 생성**:
        - 클라이언트는 `ssh-keygen` 명령어를 사용해 공개 키(public key)와 비밀 키(private key) 쌍을 생성
        - 공개 키는 서버에 저장되고, 비밀 키는 클라이언트가 보관
    2. **공개 키 배포**:
        - 서버는 `~/.ssh/authorized_keys` 파일에 클라이언트의 공개 키를 추가하여 인증 설정
    3. **인증 과정**:
        - 클라이언트는 비밀 키를 사용해 서버에서 보낸 데이터를 암호화하여 응답
        - 서버는 클라이언트의 공개 키로 암호화된 데이터를 해독하여 인증 확인
- **장점**:
    - 패스워드 없이 인증 가능, 비밀번호 탈취 위험 제거
    - 대규모 서버 환경에서 효과적 관리 가능
- **보안 강화 팁**:
    - 비밀 키를 암호화하여 저장
    - 키 교체 주기를 정기적으로 설정

---

### 2. OAuth Tokens

- **개념**:
    - OAuth(Open Authorization)는 사용자의 자격 증명을 직접 제공하지 않고, 제3자 애플리케이션이 제한적으로 사용자 데이터를 액세스할 수 있도록 하는 인증 방식
    - 토큰 기반으로 동작하며, 사용자는 비밀번호를 노출하지 않아도 됨
- **작동 방식**:
    1. **Access Token 요청**:
        - 클라이언트는 Client ID와 Client Secret을 사용해 인증 서버에 접근 토큰 발급 요청
        - 사용자 권한 승인이 필요한 경우, 리다이렉트를 통해 사용자 승인을 거침
    2. **Identity Validation**:
        - 인증 서버는 클라이언트의 자격 증명을 검증하고 Access Token을 발급
        - Access Token은 제한된 유효 기간을 가짐
    3. **API 호출**:
        - 클라이언트는 API 요청 시 Authorization 헤더에 Bearer Token을 포함하여 서버에 전달
        - 서버는 전달받은 토큰을 검증하여 요청 승인 여부 결정
    4. **응답 코드 확인**:
        - 200 OK: 요청 성공
        - 400 Bad Request: 요청 형식 오류
        - 401 Unauthorized: 토큰이 유효하지 않거나 만료됨
- **장점**:
    - 제3자 애플리케이션이 사용자의 비밀번호에 접근하지 않아도 데이터를 공유 가능
    - 세분화된 접근 권한 설정
- **실제 사례**:
    - Google, Facebook, GitHub 로그인

---

### 3. SSL Certificates

- **개념**:
    - SSL(Secure Sockets Layer) 인증서는 서버와 클라이언트 간 통신을 암호화하여 데이터를 보호
    - 현재는 TLS(Transport Layer Security)가 SSL을 대체
- **작동 방식**:
    1. **클라이언트 요청**:
        - 클라이언트가 HTTPS 프로토콜을 통해 서버에 접속
    2. **SSL 인증서 제공**:
        - 서버는 클라이언트에 인증서를 전달하며, 인증서에는 서버의 공개 키와 발급 기관(CA) 정보가 포함
    3. **인증서 검증**:
        - 클라이언트는 인증서의 유효성(만료 여부), 발급 기관 신뢰성, 도메인 이름 일치 여부 확인
        - 브라우저는 사전에 설치된 CA 리스트를 사용하여 인증서를 신뢰할 수 있는지 평가
    4. **암호화 키 교환**:
        - 클라이언트는 서버의 공개 키로 세션 키를 암호화하여 서버로 전달
        - 서버는 개인 키를 사용해 세션 키를 복호화하고, 이후 세션 키로 대칭 암호화를 통해 데이터 전송
    5. **보안 연결 설정**:
        - 이후 모든 데이터는 암호화된 채널을 통해 전송
- **장점**:
    - 중간자 공격(MITM) 방지
    - 데이터 유출 및 위조 방지
- **보안 강화 팁**:
    - 최신 TLS 버전 사용
    - 강력한 암호화 알고리즘 설정

---

### 4. Credentials

- **개념**:
    - 사용자 이름(username)과 비밀번호(password)를 이용해 사용자를 인증하는 가장 기본적인 방식
    - 대부분의 시스템에서 가장 널리 사용됨
- **작동 방식**:
    1. **HTTPS 연결**:
        - 클라이언트가 SSL/TLS를 통해 서버와 안전하게 통신 채널을 설정
    2. **비밀번호 전달 및 암호화**:
        - 사용자가 입력한 비밀번호는 암호화되어 서버로 전달
        - 서버는 전달받은 데이터를 복호화
    3. **사용자 인증**:
        - 서버는 데이터베이스에 저장된 해시된 비밀번호와 클라이언트에서 전달받은 비밀번호를 비교
        - 일치하면 인증 성공
    4. **추가 단계**:
        - 2단계 인증(OTP, SMS)을 추가하여 보안을 강화할 수 있음
- **장점**:
    - 구현이 간단하고 빠르게 배포 가능
- **단점**:
    - 비밀번호가 유출되거나 약한 비밀번호 사용 시 보안 취약점 발생
    - 브루트 포스 공격 위험
- **보안 강화 팁**:
    - 강력한 비밀번호 정책 적용
    - 해시 알고리즘(Bcrypt, Argon2)을 이용해 비밀번호 암호화

---

### 결론

- 각 인증 메커니즘은 특정한 보안 요구 사항에 따라 적합한 환경에서 사용
- **SSH Keys**: 원격 시스템 및 서버 접근에 가장 적합하며, 비밀번호 없이 높은 보안 제공
- **OAuth Tokens**: 사용자 데이터에 대한 제3자 접근 권한 제어를 지원하며, 주로 웹 애플리케이션에서 사용
- **SSL Certificates**: 클라이언트-서버 간 안전한 데이터 통신을 보장하며, 모든 웹사이트에 필수
- **Credentials**: 간단한 애플리케이션 인증에서 사용되지만, 2단계 인증으로 보안 강화 필요