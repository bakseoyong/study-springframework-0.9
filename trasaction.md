# Transaction

전파(Propagation)?

트랜잭션 추상화?

TransactionDefinition(interface)
- 트랜잭션의 ACID 속성 중 개발자가 제어 가능한 부분( propagation, isolation, read-oly, timeout ) 을 추상화
- isolation은 당시 java.sql.Connection 객체의 추상화를 그대로 사용한 것으로 보임
- propagation 추상화
- 얼핏 보기에 지금의 enum클래스로 보이는데 당시에 enum이 존재하지 않아 인터페이스로 구현한 것인가? 라는 생각이 들었다.

TransactionStatus(class)
- TransactionPlatformManager에서 commit, rollback 메서드의 매개변수타입으로, 커밋과 롤백을 결정하는 클래스이다.
- 0.9버전에서는 class로서 trasaction objet와 새로운 트랜잭션인지 확인하는 newTransaction boolean type 그리고 rollbackOnly boolean type이 존재한다.
- 나중 버전에서 인터페이스로 변경되며 flush()와 hasSavepoint()라는 메서드로 변경되는 듯 하다.

PlatformTransactionManager(interface)
- getTransaction, commit, rollback 메서드를 선언
- 0.9버전 당시에는 TransactionManager라는 클래스를 상속받지 않았다.(TransactionManager라는 클래스가 존재하지 않았다.)
- TransactionDefinition에 @Nullable 어노테이션이 추가되지 않아 추가적인 null작업을 개별적으로 수행했어야 했을 듯

AbstractPlatformTransactionManager(abstract class)
- PlatformTransactionManager를 구현한 추상 클래스
- 이 클래스를 DataSource, Hibernate, Jta, Jdo 가 상속받는다.
- commit, rollback메서드가 템플릿 메서드 디자인 패턴으로 구현되어 있다. (일반적인 로직과 상태 체크를 확인하고, 실제 커밋은 서브클래스들이 구현)
- 코드에 대한 공부를 하기 위한 목적이니까 commit, rollback의 코드를 살펴보자
- handle propagation 과 non-transactional behavior check하는 getTransaction()메서드가 존재
- getTranscation() : 트랜잭션을 반환하는 메서드
  - 구현체로 부터 doGetTranscation()메서드를 통해 트랜잭션을 가져온다.
  - 예) DataSourceTransactionManager의 doGetTransaction()
    - 


트랜잭션 동기화?
배경
- 비즈니스 로직에서 여러 쿼리를 처리해야 할 때 Connection객체를 dao 쿼리마다 넘겨주는 방식은 비즈니스 로직에서 Connection 객체를 관리해야 한다는 부담감을 부여한다.
- 그렇다고 dao의 쿼리마다 Connection 객체를 생성한다면 한 트랜잭션에 진행되어야 할 쿼리들이 각각의 Connection을 하며 트랜잭션이 아닌 개별 작업이 되어버린다.
설명
- 비즈니스 로직을 담은 객체에서 만든 Connection 객체를 특별한 저장소에 보관해두고, 이후에 호출되는 DAO의 메서드에서는 저장된 Connection을 가져다가 사용하게 하는 방식
- 트랜잭션 동기화 저장소는 스레드마다 Connection 객체를 독립적으로 관리하므로 멀티 스레드 환경에서 충돌이 발생하지 않는다

TransactionSynchronization(interface)
- 트랜잭션 완료 후 콜백(afterCompletion(int status)을 위한 인터페이스
- STATUS_COMMITTED, STATUS_ROLLED_BACK, STATUS_UNKNOWN 상태값을 가지고 있다.

TransactionSynchronizationManager(interface)
- init 메서드를 통해 현재 스레드의 동기화 목록 프로퍼티에 ArrayList 자료구조를 할당시킨다. (특별한 저장소)
- ThreadLocal(ArrayList 자료구조)에 register(TransactionSynchronization synchronization) 메서드를 통해 현재 트랜잭션 동기화 목록에 저장하면서 멀티 스레드 환경에서의 충돌을 방지한다.
- triggerAfterCompletion(int status)를 통해 트랜잭션 완료 후 콜백을 호출. 주어진 상태에 따라 등록된 모든 동기화 작업의 afterCompletion 메서드를 호출

