# Transaction

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
- TransactionDefinition에 @Nullable이 명시되지 않은 버전

AbstractPlatformTransactionManager(abstract class)
- PlatformTransactionManager를 구현한 추상 클래스
- 이 클래스를 DataSource, Hibernate, Jta, Jdo 가 상속받는다.
- commit, rollback메서드가 템플릿 메서드 디자인 패턴으로 구현되어 있다. (일반적인 로직과 상태 체크를 확인하고, 실제 커밋은 서브클래스들이 구현)
- 코드에 대한 공부를 하기 위한 목적이니까 commit, rollback의 코드를 살펴보자
- handle propagation 과 non-transactional behavior check하는 getTransaction()메서드가 존재
- 
