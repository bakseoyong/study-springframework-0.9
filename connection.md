트랜잭션을 제공하기위해 어떻게 커넥션을 만들어서 전달하는가에 대한 궁금증을 해소하기 위해 공부한 내용을 정리하는 마크다운

ConnectionHolder(class)
- JDBC Connection 객체의 래핑 클래스
- 중첩된 JDBC 트랜잭션에 대한 롤백 전용 지원을 제공
- DataSourceTransactionManager는 이 클래스의 인스턴스를 특정 DataSource에 대해 스레드에 바인딩하여 사용

javax.sql.DataSource
- 데이터베이스 연결 생성, 풀링, 관리 등의 작업을 쉽게 처리할 수 있는 인터페이스
- 데이터베이스에 연결하기 위해 직접 JDBC 드라이버를 호출할 필요 X
- 데이터베이스 연결을 관리하고 재사용 가능한 연결 풀을 제공
- 연결 획득: 애플리케이션이 데이터베이스 연결을 필요로 할 때, 미리 생성된 연결을 풀에서 가져옵니다.
- 연결 반환: 연결을 사용한 후에는 다시 DataSource에 연결을 반환하여 연결 풀에 재활용
- 연결 설정 관리: DataSource는 연결 생성 시 필요한 설정 정보(데이터베이스 URL, 사용자 이름, 비밀번호 등)를 관리
- 연결 풀링: 미리 생성된 연결을 풀에 유지하여 필요할 때마다 연결을 재사용
- 데이터베이스 연결 오류를 처리하기 위해 예외 처리를 지원
- 대표적인 DataSource 구현체로는 BasicDataSource, HikariCP, C3P0

DataSourceUtils(abstract class)
- 

중첩된 JDBC 트랜잭션?

바인딩?


