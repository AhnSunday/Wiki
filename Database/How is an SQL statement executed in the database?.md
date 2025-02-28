# How is an SQL statement executed in the database?

![sql_execution_order_in_db](https://github.com/user-attachments/assets/22592c8b-201a-4c8d-9380-4a7610d3e9a7)


# **SQL 명령어가 데이터베이스에서 실행되는 과정**

## **1 - SQL 명령어 전송**

### **설명**

사용자가 SQL 명령어를 작성하면 데이터베이스로 전송이 됩니다

주로 TCP/IP 같은 전송 계층 프로토콜을 사용합니다, 그리고 이 과정에서 클라이언트와 데이터베이스 간의 연결이 생성됨

### **프로세스**

1. 사용자가 애플리케이션에서 `SELECT`, `INSERT` 등 SQL 명령어를 작성
2. 네트워크 인터페이스가 명령어를 데이터베이스 서버에 전달
3. 클러스터 환경(여러 대의 서버가 하나의 시스템처럼 작동하는 환경)에서는 명령어를 적합한 노드로 라우팅

## **2 - 명령어 분석, 그리고 파싱**

### **설명**

SQL 명령어가 문법적으로 맞는지 확인하고 의미가 적합한지 검사를 합니다

이 과정에서 Query Tree가 생성됨

### **프로세스**

1. SQL 명령어의 구조를 분석해서 문법 오류를 체크
    1. 예시) `SELECT * FROM` 뒤에 테이블 이름이 없다? 오류 반환!
2. 의미 분석 단계에서 테이블, 컬럼, 데이터 타입이 존재하는지 확인
    1. 예시) `users` 테이블이 없다? 바로 오류 반환!
3. 명령어를 Query Tree라는 내부 데이터 구조로 변환
    1. 해당 트리는 명령어를 실행 가능한 논리적 단계로 구성된 트리

## **3 - 쿼리 최적화**

### **설명**

Query Tree를 기반으로 가장 효율적인 실행 계획을 생성합니다

데이터 처리의 성능을 높이기 위해 다양한 실행 경로를 평가함

### **프로세스**

1. 여러 실행 경로를 평가
    1. 테이블 스캔(Full Table Scan)과 인덱스 사용(Index Scan)을 비교
    2. 예시) `WHERE id = 5`와 같은 조건이 있다면 인덱스를 사용하는 것이 빠르다고 판단
2. 비용 기반 분석
    1. 각 실행 경로의 비용(Cost)을 계산하고, 가장 낮은 비용의 계획을 선택
    2. 여기서 비용은 디스크 I/O, CPU 사용량, 네트워크 트래픽 등을 포함
3. 최적화된 실행 계획을 생성

## **4 - 쿼리 실행**

### **설명**

최적화된 실행 계획을 바탕으로 실제 데이터를 검색하거나 변경하는 작업을 수행합니다

### **프로세스**

1. 쿼리를 실행하며 필요한 데이터를 스토리지 엔진에서 가져옴
2. 검색된 데이터를 반환하거나, 데이터 삽입/수정/삭제 작업을 처리함
3. 실행 중 오류가 발생하면 롤백하거나 오류 메시지를 반환

예시) `SELECT name FROM users WHERE id = 10;`명령어를 실행하면?

   `users` 테이블에서 `id`가 `10`인 행의 `name` 컬럼 값을 반환

**스토리지 엔진이 뭐지?**

혹시 InnoDB라고 들어봤나요? InnoDB가 MySQL의 스토리지 엔진중 하나입니당

<aside>
💡

**스토리지 엔진의 역할**

1. **데이터 저장 및 검색**
    
    데이터를 디스크나 메모리에 저장하고, 필요할 때 데이터를 검색해 제공합니다.
    
2. **데이터 구조 관리**
    
    데이터를 저장하기 위한 다양한 데이터 구조(예: B-Tree, Hash Table 등)를 구현.
    
3. **데이터 읽기/쓰기 최적화**
    
    SQL 명령어를 실행할 때 최적화된 방식으로 데이터를 읽고 씁니다.
    
4. **트랜잭션 처리**
    
    트랜잭션을 지원해 데이터의 일관성과 무결성을 유지합니다.
    
5. **동시성 제어 및 락 관리**
    
    여러 사용자가 동시에 데이터를 수정하거나 읽을 때 충돌을 방지합니다.
    
</aside>

<aside>
💡

**스토리지 엔진의 주요 기능**

1. **인덱스 관리**
    
    데이터를 빠르게 검색하기 위해 인덱스를 생성하고 유지합니다.
    
2. **데이터 형식 관리**
    
    데이터 형식(예: INT, VARCHAR)을 정의하고 저장 방식에 따라 관리합니다.
    
3. **파일 입출력 관리**
    
    데이터를 디스크에 효율적으로 저장하거나 디스크에서 데이터를 읽어오는 방식을 최적화합니다.
    
4. **캐싱**
    
    자주 사용하는 데이터를 메모리에 캐싱하여 속도를 향상시킵니다.
    
5. **복구**
    
    장애 발생 시, 손상된 데이터를 복구하거나 트랜잭션을 롤백할 수 있는 기능을 제공합니다.
    
</aside>

## **5 - 스토리지 접근**

### **설명**

데이터를 검색하거나 수정하기 위해 스토리지 엔진에 접근합니다

인덱스와 같은 데이터 구조를 사용하여 검색 속도를 높임

### **프로세스**

1. 인덱스를 사용할지, 테이블 전체를 스캔할지 결정
    1. 예시) `WHERE id = 5` 조건에서는 인덱스 스캔!
    2. 예시) `WHERE age > 20` 조건에서는 테이블 스캔!
2. 필요한 데이터를 스토리지 엔진에서 검색

## **6 - 버퍼 관리**

### **설명**

데이터를 메모리와 디스크 간에 효율적으로 관리하여 속도를 최적화합니다

읽기 작업에서 캐싱된 데이터를 우선적으로 사용하여 성능을 높입니다

### **프로세스**

1. 캐시 확인
    1. 요청된 데이터가 메모리에 있다면 디스크 접근을 생략
2. 캐시에 없는 경우 디스크에서 데이터를 읽어와 캐시에 저장

예시) `SELECT` 문 실행 시, 자주 사용하는 데이터를 캐시에 저장해 반복적인 요청을 빠르게 처리

## **7 - 트랜잭션 관리**

### **설명**

데이터 변경 작업(`INSERT`, `UPDATE`, `DELETE`)을 관리 

그리고 트랜잭션의 ACID (원자성, 일관성, 격리성, 지속성)을 보장합니다

### **프로세스**

1. 트랜잭션 시작
    1. 데이터 변경 작업이 시작되면 트랜잭션이 열림
2. 커밋 또는 롤백
    1. 모든 작업이 성공하면 **Commit**으로 데이터베이스에 반영 🙂
    2. 중간에 오류가 발생하면 **Rollback**으로 원상 복구!

예시) 은행 이체에서 계좌 A에서 출금과 계좌 B로 입금이 모두 완료되지 않으면 트랜잭션 전체를 취소하겠죠?

## **8 - 락 관리**

### **설명**

동시에 여러 사용자가 데이터를 수정하려고 할 때 충돌을 방지합니다

데이터의 일관성을 유지하고 동시성 제어를 수행

### **프로세스**

1. 락 설정
    1. 데이터 변경 작업 중 다른 트랜잭션이 동일 데이터를 수정하지 못하도록 락을 설정
2. 락 해제
    1. 트랜잭션이 완료되면 락을 해제하여 다른 작업이 접근 가능하도록 함

예시) 두 사용자가 동일한 테이블의 동일한 행을 동시에 수정하려는 상황?

   한 사용자만 먼저 수정할 수 있도록 락 설정!

## **정리**

| 순서 | 과정 |
| --- | --- |
| 1 | SQL 명령어는 네트워크를 통해 데이터베이스로 전송 |
| 2 | 명령어는 파서(Parser)에서 구문과 의미가 분석됨 |
| 3 | 최적화 단계에서 효율적인 실행 계획이 생성됨 |
| 4 | 스토리지 엔진과 상호작용하며 데이터를 처리 |
| 5 | 데이터는 버퍼 관리자를 통해 효율적으로 읽히거나 쓰여짐 |
| 6 | 데이터 변경 시 트랜잭션과 락 관리가 실행됨 |