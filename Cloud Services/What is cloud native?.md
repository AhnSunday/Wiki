# What is cloud native?

![cloud-native](https://github.com/user-attachments/assets/47e51bbf-32d5-484b-b4a4-00423111f4e2)

### **Development Process (개발 프로세스)**

- **1980~1990**: Waterfall (폭포수) 방식
    
    → 단계별로 개발을 진행하며, 한 번에 전체 프로젝트를 계획함
    
- **2000**: Agile (애자일)
    
    → 유연하고 반복적인 개발 방식으로 변경됨
    
- **2010 - Cloud**: DevOps
    
    → 개발과 운영이 긴밀하게 협력하여 빠른 배포와 유지보수가 가능해짐
    

### **Application Architecture (애플리케이션 아키텍처)**

- **1980~1990**: Monolithic (모놀리식)
    
    → 애플리케이션이 하나의 커다란 단위로 구성되어있음
    
- **2000**: N-Tier (엔티어)
    
    → 데이터베이스, 애플리케이션 로직, 프레젠테이션 층이 분리되어있음
    
- **2010 - Cloud**: Microservices (마이크로서비스)
    
    → 기능별로 독립적인 작은 서비스로 나누어 관리
    

### **Deployment & Packaging (배포 및 패키징)**

- **1980~1990**: Physical server (물리 서버)
    
    → 애플리케이션이 물리적 서버에 직접 배포
    
- **2000**: Virtual server (가상 서버)
    
    → 가상화 기술을 사용하여 자원을 효율적으로 활용
    
- **2010 - Cloud**: Container (컨테이너)
    
    → 컨테이너 기술을 통해 애플리케이션을 더욱 가볍고 이식성 있게 배포
    

### **Application Infrastructure (애플리케이션 인프라)**

- **1980~1990**: Data center (데이터 센터)
    
    → 물리적인 데이터 센터에 의존
    
- **2000**: Hosted (호스팅)
    
    → 외부 서비스 제공자가 서버를 호스팅
    
- **2010 - Cloud**: Cloud (클라우드)
    
    → 클라우드 인프라를 통해 확장성과 유연성을 제공받음
    

***Cloud Native란 클라우드 환경을 최대한 활용하도록 설계된 애플리케이션을 개발하고 운영하는 접근 방식을 의미합니다.*** 

## **주요 특징**

1. **마이크로서비스 아키텍처**
    
    → 애플리케이션을 독립적인 작은 서비스로 나누어 개발하고 배포함으로써 유지보수와 확장이 용이함
    
2. **컨테이너 기술 활용**
    
    → Docker, Kubernetes와 같은 컨테이너 기술을 통해 이식성, 자원 효율성, 자동화를 지원
    
3. **DevOps 및 CI/CD**
    
    → 개발(Development)과 운영(Operations)을 통합하여 지속적인 통합(Continuous Integration)과 지속적인 배포(Continuous Deployment) 가능
    
4. **클라우드 인프라 최적화**
    
    → 퍼블릭, 프라이빗, 하이브리드 클라우드에서 동작하도록 설계되어 자동 확장과 자원 할당을 쉽게 처리
    
5. **자율 복구 및 탄력성**
    
    → 장애가 발생해도 빠르게 복구하고 시스템의 안정성을 유지
    

## **장점**

- **빠른 배포**: 신규 기능을 신속히 배포 가능.
- **확장성**: 트래픽에 따라 서비스 확장 또는 축소 가능.
- **유연성**: 다양한 환경(온프레미스, 클라우드 등)에서 실행 가능.
- **비용 효율성**: 필요한 만큼의 자원을 사용할 수 있어 비용 절감.