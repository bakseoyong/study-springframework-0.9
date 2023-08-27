JavaBeans
- Java에서 객체 지향 프로그래밍을 위한 표준 프레임워크
- JavaBeans의 속성 값은 String, int, double과 같은 기본 타입 또는 Date, Color와 같은 사용자 정의 타입일 수 있음

java.beans.PropertyEditor(interface)
- 자바 빈에서 프로퍼티 값을 문자열로 표현하거나, 문자열을 프로퍼티 값으로 변환하기 위한 인터페이스
- 이 인터페이스를 구현한 클래스는 해당 프로퍼티의 값을 문자열로 변환하거나, 문자열을 프로퍼티 값으로 역변환할 수 있음
- setAsText(String text): String 타입의 값을 속성 값으로 설정
- getAsText(): 속성 값을 String 타입으로 반환
- getValue(): 속성 값을 반환
- setValue(Object value): 속성 값을 지정

java.beans.PropertyEditorManager(class)
- PropertyEditor 인터페이스의 구현체를 관리
- 자바 빈(JavaBeans) 프로퍼티를 문자열로 변환하거나, 문자열을 자바 빈 프로퍼티로 변환하기 위한 메커니즘을 관리하는 클래스
- 일반적으로, 각각의 자바 빈 클래스는 자체적으로 PropertyEditor 구현체를 가지고 있다
- 하지만, 때로는 특정 자바 빈에 대한 커스텀한 PropertyEditor를 만들어 사용하거나 이미 구현된 PropertyEditor를 재활용해야 할 경우가 있음
- PropertyEditorManager는 이러한 커스텀 PropertyEditor 구현체를 관리하고 등록하는 클래스로, 빈 클래스의 인스턴스를 생성하거나 값을 변환하는 과정에서 사용
- findEditor(Class<?> targetType): 지정된 타입의 속성 값을 편집하는 데 사용할 PropertyEditor 객체를 반환
- registerEditor(Class<?> targetType, Class<?> editorClass): 지정된 타입의 속성 값을 편집하는 데 사용할 PropertyEditor 객체를 등록
- PropertyEditor editor = PropertyEditorManager.findEditor(Class<?> targetType);

BeanFactory(interface)
- Bean을 생성, 관리, 조회
- (0.9버전에 존재하는 메서드)
  - getBean(String name) : 주어진 빈 이름의 인스턴스(공유되거나 독립적일 수 있음)을 반환
  - getBean(String name, Class requiredType) : 빈이 일치해야 할 유형을 추가적으로 요구
  - isSingleton(String name) : 싱글톤 객체인지 확인
  - getAliases(String name) : 빈 이름의 별칭 반환
- (0.9버전 이후에 존재하는 메서드)
  - getBean(String beanName): 지정된 Bean 이름의 Bean을 조회
  - getBean(Class<?> beanClass): 지정된 Bean 클래스의 Bean을 조회
  - getBeansOfType(Class<?> beanClass): 지정된 Bean 클래스의 모든 Bean을 조회
  - createBean(String beanName): 지정된 Bean 이름의 Bean을 생성
  - createBean(Class<?> beanClass): 지정된 Bean 클래스의 Bean을 생성
  - registerBeanDefinition(String beanName, BeanDefinition beanDefinition): 지정된 Bean 이름과 BeanDefinition을 등록
  - removeBeanDefinition(String beanName): 지정된 Bean 이름의 BeanDefinition을 제거

그러면 0.9에서는 빈을 어떻게 생성한거지?

InitializingBean(interface)
- 자신의 모든 프로퍼티가 BeanFactory에 의해 설정된 후에 반응해야 하는 빈들이 구현해야 하는 인터페이스
- 예를 들어 초기화를 수행하거나, 단순히 필수 프로퍼티가 모두 설정되었는지 확인하는 데 사용
- 스프링 프레임워크는 이 인터페이스를 구현한 빈 객체의 초기화 메서드를 자동으로 호출합니다.
- afterPropertiesSet() 메서드는 BeanFactory가 모든 속성을 설정한 후 호출. 초기화 작업이 실패하면 Exception을 throw



