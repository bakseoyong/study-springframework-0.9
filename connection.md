트랜잭션을 제공하기위해 어떻게 커넥션을 만들어서 전달하는가에 대한 궁금증을 해소하기 위해 공부한 내용을 정리하는 마크다운

ConnectionHolder(class)
- JDBC Connection 객체의 래핑 클래스
- 중첩된 JDBC 트랜잭션에 대한 롤백 전용 지원을 제공
- DataSourceTransactionManager는 이 클래스의 인스턴스를 특정 DataSource에 대해 스레드에 바인딩하여 사용


중첩된 JDBC 트랜잭션?

바인딩?


