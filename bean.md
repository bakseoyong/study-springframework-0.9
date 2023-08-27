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
- Bean을 조회 (생성, 관리는 이후 버전)
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

BeanWrapper(interface)
- 빈(Bean) 객체의 프로퍼티에 접근하고 조작하는 기능을 제공하는 인터페이스
- 빈 객체의 프로퍼티 값을 가져오거나 설정
- Wrapper를 사용해 프로퍼티 값을 가져올 때, 필요한 타입으로 자동 변환을 수행
- 중첩된 프로퍼티도 지원. 즉, 빈 객체 내부의 다른 빈 객체의 프로퍼티에도 접근 가능
- 프로퍼티 값이 변경될 때 이벤트 리스너를 등록하여 이벤트를 처리
- 데이터 바인딩과 유효성 검사 기능도 제공. 이를 통해 웹 폼 데이터와 빈 객체 간의 상호 작용을 용이하게 처리
- setPropertyValue(String propertyName, Object value): 주어진 프로퍼티 이름에 해당하는 빈의 프로퍼티 값을 설정
- setPropertyValue(PropertyValue pv): PropertyValue 객체를 통해 프로퍼티 값을 설정. 더 복잡한 프로퍼티 값, 인덱스된 프로퍼티 값을 설정할 때 사용
- getPropertyValue(String propertyName): 주어진 프로퍼티 이름에 해당하는 빈의 프로퍼티 값을 반환
- getIndexedPropertyValue(String propertyName, int index): 인덱스가 있는 프로퍼티의 값을 반환(컬렉션 타입)
- setPropertyValues(Map m): Map을 사용하여 여러 프로퍼티 값을 한 번에 설정
- setPropertyValues(PropertyValues pvs): PropertyValues 객체를 사용하여 여러 프로퍼티 값을 한 번에 설정
- setPropertyValues(PropertyValues pvs, boolean ignoreUnknown, PropertyValuesValidator pvsValidator): 여러 프로퍼티 값을 한 번에 설정하면서, 변환, 유효성 검사 등을 세밀하게 제어
- getPropertyDescriptors(): 빈 객체의 프로퍼티 디스크립터(정보)들을 반환
- getPropertyDescriptor(String propertyName): 주어진 프로퍼티 이름에 해당하는 프로퍼티 디스크립터를 반환
- isReadableProperty(String propertyName): 주어진 프로퍼티 이름의 프로퍼티가 읽을 수 있는지 여부를 반환
- isWritableProperty(String propertyName): 주어진 프로퍼티 이름의 프로퍼티가 쓸 수 있는지 여부를 반환
- getWrappedInstance(): 이 BeanWrapper 객체로 래핑된 빈 객체를 반환
- setWrappedInstance(Object obj): 래핑된 빈 객체를 변경
- newWrappedInstance(): 새로운 래핑된 빈 객체를 생성
- newWrapper(Object obj): 새로운 래핑된 빈 객체를 생성하면서 미리 캐시된 정보를 사용
- getWrappedClass(): 래핑된 빈 객체의 클래스를 반환
- registerCustomEditor(Class requiredType, String propertyPath, PropertyEditor propertyEditor): 특정 프로퍼티나 타입에 대한 커스텀 프로퍼티 에디터를 등록
- findCustomEditor(Class requiredType, String propertyPath): 특정 타입이나 프로퍼티에 등록된 커스텀 프로퍼티 에디터를 탐색
- addVetoableChangeListener(VetoableChangeListener l): VetoableChangeListener를 등록
= removeVetoableChangeListener(VetoableChangeListener l): 등록된 VetoableChangeListener를 제거
그 외에도 VetoableChangeListener 관련 메서드들이 있으며, PropertyChangeListener도 동일한 방식으로 관리. 이벤트 관련 메서드들은 빈의 프로퍼티 변경 이벤트에 대한 리스너를 등록하고 관리하는 데 사용
- invoke(String methodName, Object[] args): 지정된 이름의 메서드를 호출. 이 메서드는 주로 빈 프로퍼티보다는 메서드 호출에 사용되며, 메서드 이름과 인자를 전달하여 호출

인덱스된 프로퍼티?
``` JAVA
MyBean bean = new MyBean();
String name = bean.getNames()[0];
```
- 프로퍼티의 값을 빠르게 검색(순회) 하거나 수정

프로퍼티 디스크럽터?
- 프로퍼티의 이름, 타입, 값, 그리고 프로퍼티에 대한 추가 정보를 포함
- 스프링 컨테이너는 Bean의 프로퍼티를 설정할 때 프로퍼티 디스크립터를 사용
- name: 프로퍼티의 이름
- type: 프로퍼티의 타입
- value: 프로퍼티의 값
- required: 프로퍼티가 필수인지 여부
- writable: 프로퍼티가 쓰기 가능 여부
- enumerable: 프로퍼티가 열거 가능 여부
- configurable: 프로퍼티가 구성 가능 여부

커스텀 프로퍼티 에디터?
- 스프링 컨테이너가 Bean의 프로퍼티 값을 설정할 때 사용하는 에디터
- 스프링 컨테이너는 기본적으로 제공되는 프로퍼티 에디터를 사용하지만, 필요에 따라 커스텀 프로퍼티 에디터를 등록하여 사용할 수 있음

InitializingBean(interface)
- 자신의 모든 프로퍼티가 BeanFactory에 의해 설정된 후에 반응해야 하는 빈들이 구현해야 하는 인터페이스
- 예를 들어 초기화를 수행하거나, 단순히 필수 프로퍼티가 모두 설정되었는지 확인하는 데 사용
- 스프링 프레임워크는 이 인터페이스를 구현한 빈 객체의 초기화 메서드를 자동으로 호출합니다.
- afterPropertiesSet() 메서드는 BeanFactory가 모든 속성을 설정한 후 호출. 초기화 작업이 실패하면 Exception을 throw



