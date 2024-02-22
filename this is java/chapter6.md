# chapter6 클래스
## 객체지향 프로그래밍 특징
### 캡슐화
- 외부 객체는 내부 구조를 알지 못한다
- 필드와 메소드를 캡슐화하여 보호하는 이유: 외부의 잘못된 사용으로 인해 객체가 손상되지 않도록 하기 위함
- 접근 제한자: 캡슐화된 멤버를 노출시킬 것인지, 숨길 것인지를 결정

### 상속
- 부모 객체가 필드와 메소드를 자식객체에게 물려줘 자식 객체가 사용할 수 있음
- 코드의 재사용성 증가
- 유지 보수 시간 최소화 - 부모 객체의 필드와 메소드 수정하면 모든 자식 객체들은 수정된 필드와 메소드 사용 가

## 객체와 클래스
- 클래스: 객체를 생성하기 위한 설계도
- 인스턴스: 클래스로 부터 생성된 객체
- ex) 한개의 설계도로 여러개의 자동차를 만드는 것

## 클래스 선언
- 한 개의 파일에 복수 개의 클래스 선언 가능
- 그러나 파일명과 동일한 클래스만 공개 클래스로 선언 가능
- 공개클래스: 어느 위치에 있든지, 패키지와 상관없이 사용할 수 있는 클래스
- `클래스 변수 = new 클래스()`
- new 연산자는 객체를 생성시킨 후 객체의 주소 리턴
- 라이브러리 클래스: 실행할 수 없으며 다른 클래스에서 이용
- 실행 클래스: 메소드를 가지고 있는 실행 가능한 클래스
- -> 일반적인 자바 프로그램은 하나의 실행 클래스와 여러 개의 라이브러리 클래스들로 구성, 실행 클래스는 실행하면서 라이브러리 클래스를 내부에서 이

## 클래스 구성
### 필드
: 객체의 데이터를 저장하는 역할

- 반드시 클래스 블록 안에서 선언
- 초기값을 제공하지 않는 경우, 객체 생성시 자동으로 기본값으로 초기화 됨
- 객체마다 동일한 값을 갖고 있다면 필드 선언 시 초기화 하는 것이 좋다

### 생성자
: new 연산자 객체를 생성할 때 객체의 초기화 역할 담당, 리턴 타입 없다

### 메소드
: 객체가 수행할 동작, 객체간의 상호작용을 위해 호출

## 생성자 선언과 호출
객체를 생성 후 초기화 -> 객체를 사용할 준비
### 기본 생성자
- 모든 클래스는 생성자 존재, 하나 이상 가질 수 있음
- 클래스에 생성자 선언이 없으면 컴파일러가 기본 생성자를 바이트 코드 파일에 추가함
```java
public class Korean {
  String nation = "대한민국";
  String name;
  String ssn;
  
  // 생성자 선언
  public Korean(String n, String s) {
    name = n;
    ssn = s;
  }
}
```

### 생성자 오버로딩
- 매개변수를 달리하는 생성자를 여러개 선언하는 것
- 객체 필드를 다양하게 초기화 하기 위함

### 다른 생성자 호출
```java
Car(String model) {
  this.model = model;
  this.color = "은색";
  this.maxSpeed = 250;
}

Car(String model, String color) {
  this.model = model;
  this.color = color;
  this.maxSpeed = 250;
}

Car(String model) {
  this.model = model;
  this.color = color;
  this.maxSpeed = maxspeed;
}
```
- 이와 같이 중복 코드가 발생할 경우에
- 공통 코드를 한 생성자에게만 집중적으로 작성하고 나머지 생성자는 this(...) 를 사용
- this(...) : 공통 코드를 가지고 있는 생성자 호출
```java
Car(String model) {
  this(model, "은색", 250);
}

Car(String model, String color) {
  this(model, color, 250);
}

// 공통 초기화 코드
Car(String model, String color, int maxSpeed) {
  this.model = model;
  this.color = color;
  this.maxSpeed = maxSpeed;
}
```

## 메소드 선언과 호출
- 메소드: 객체간의 상호작용 방법 정의
- 리턴값이 없는 메소드는 `void`로 작성
- 매개변수 생략 가능
- 매소드는 객체의 동작이므로 객체가 존재하지 않으면 메소드를 호출할 수 없다

### 가변길이 매개변수
```java
int sum(int ... values) {
}

int result = sum(1, 2, 3);

int[] values = {1, 2, 3};
int result = sum(values);
```
- 매개값들은 자동으로 배열 항목으로 변환되어 메소드에서 사용된다
- 그래서 직접 배열을 매개값으로 제공해도 된다

### 매소드 오버로딩
- 메소드 이름은 같되 매개변수의 타입, 개수, 순서가 다른 메소드를 여러개 선언
- 매개값 타입에 따라서 알아서 선택되어 실행됨

## 정적 멤버 - static
- 객체마다 가지고 있을 필요성이 없느 공용적인 필드는 정적 필드로 선언하는 것이 좋다 (ex.계산기에서 파이값)
- 반면, 인스턴스 필드를 변경하는 메소드는 인스턴스 메소드로 선언
- 정적 필드와 정적 메소드는 객체 참조 변수로도 접근이 가능하다
```java
Calculator myCalcu = new Calculator();
double result1 = 10 * 10 * myCalcu.pi;
int result2 = myCalcu.plus(10, 5)
```
- 그러나, 정적 요소는 클래스 이름으로 접근해야 정석이다

### 인스턴스 멤버 사용 불가
- 정적 멤버는 객체가 없어도 실행된다는 특징 때문에 내부에 인스턴스 필드나 인스턴스 메소드를 사용할 수 없다
    - 또한, 객체 자신의 참조인 this도 사용할 수 없다
- 정적 블록은 클래스가 메모리로 로딩될 때 자동으로 실행되기 때문
- 정적 메소드와 정적 블록에서 인스턴스 멤버를 사용하고 싶다면, 객체를 멈ㄴ저 생성하고 참조 변수를 접근해야함
```java
static void Method3() {
  // 객체 생성
  ClassName obj = new ClassName();
  // 인스턴스 멤버 사용
  obj.field1 = 10;
  obj.method1();
```

## final 필드와 상수
: 값 변경을 막고 읽기만 허용
- 초기값을 줄 수 있는 방법
- 필드 선언 시에 초기값 대입 / 생성자에서 초기값 대입
```java
// 1. 인스턴스 final 필드 선언
public class korean {
final String nation = "대한민국"
final String ssn;
}
// 2. 생성자 선언
public Korean(String snn, String name) {
this.ssn = ssn;
this.name = name;
```

### 상수 선언
- 파이 같은 상수는 객체마다 저장할 필요 없고, 여러개 값을 가져서도 안되기 때문에
- `static final 타입 상수 = 초기값;` 이렇게 선언

## 패키지
: 단순히 디렉토리가 아니라, 클래스의 일부분이며, 클래스를 식별하는 용도로 사용됨

- 패키지 디렉토리는 클래스를 컴파일하는 과정에서 자동으로 생성
- 컴파일러는 클래스의 패키지 선언을 보고 디렉토리를 자동 생성 시킴
- 패키지 이름 - 소문자, 중복되지 않록, 마지막에는 프로젝트 이름 붙여주기

### import
다른 두 패키지에서 같은 이름의 클래스를 사용할 경우
```java
import com.hankook.*;
import com.kumho.*;

public class Car {
  com.hankook.Tire tire1 = new com.hankook.Tire();
  com.kumho.Tire tire2 = new.com.kumho.Tire();
```

## 접근 제한자
: 중요한 필드와 메소드가 외부로 노출되지 않도록 해 객체의 **무결성**을 유지하기 위함

- public: 제한범위 없음
- protected: 같은 패키지 or 자식 객체만 사용 가능
- private: 객체 내부

### 클래스의 접근 제한
- 클래스는 public과 default 접근제한 가능
- 클래스 선언할 때 제한자 생략한 경우 - default -> 같은 패키지만 가능, 다른 패키지에서는 사용 불가
- public 제한자 사용 - 같은 패키지, 다른 패키지 가능

### 생성자의 접근 제한
1. public
- 생성자: 클래스
- 모든 패키지에서 생성자 호출 가능 = 모든 패키지에서 객체 생성 가능
2. (default)
- 생성자: 클래스
- 같은 패키지에서만 생성자 호출 가능 = 같은 패키지에서만 객체 생성 가능
3. private
- 생성자: 클래스
- 클래스 내부에서만 생성자 호출 가능 = 클래스 내부에서만 객체 생성 가능

### 필드와 메소드의 접근 제한
1. public
- 생성자: 필드, 메소드
- 모든 패키지에서 생성자 호출 가능 = 모든 패키지에서 객체 생성 가능
2. (default)
- 생성자: 필드, 메소드
- 같은 패키지에서만 생성자 호출 가능 = 같은 패키지에서만 객체 생성 가능
3. private
- 생성자: 필드, 메소드
- 클래스 내부에서만 생성자 호출 가능 = 클래스 내부에서만 객체 생성 가능

## Getter Setter
- 객체 지향 프로그래밍에서는 직접적인 외부에서의 필드 접근을 막고 **메소드를 통해 접근**하도록 한다
- 이유는 -> 메소드는 **데이터를 검증해서 유효한 값만 필드에 저장**할 수 있기 때문

### Setter
- private speed 필드일 경우, 접근 제한을 가지므로 외부에서 접근 불가
- 따라서 필드를 변경하기 위해서는 setSpeed() 메소드를 이용해야 한다
```java
public void setSpeed(double speed) {
  if(speed < 0) {
    this.speed = 0;
    return;
  } else {
    this.speed = speed;
  }
}
```

### Getter
- 필드값이 객체 외부에서 사용하기에 부적절한 경우 -> 메소드로 적절한 값으로 변환해서 리턴
```java
public double getSpeed() {
  double km = speed*1.6;
  return km;
}
```
- private speed 필드는 외부에서 읽지 못함
- km단위로 변환해서 외부로 리턴

