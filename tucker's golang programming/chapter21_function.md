# 고급힘수

## 가변 인수 함수
- 함수 인수 개수가 고정적이지 않은 함수
- ex. ```fmt.Println(1,2,3,4,5,6,7)```

```go
func sum(nums ...int) int {
	...
}
```
- 해당 타입의 인수를 여러 개 받는 가변 인수
```go
func Print(args ...interface{}) string {
	for _, arg := range args {
		switch f := arg.(type) { // 인수 타입에 따라 동작
		case bool:
			val := arg.(bool)
		case float64:
			...
        }       
    }   
}
```
- 여러타입을 인수로 섞어 쓸 수 있다
- interface{} 빈 인터페이스 타입이 모든 타입을 포함
- 모든 타입의 가변 인수를 받을 수 있고
- 함수 내부에서 인터페이스 변환 기능을 이용해 타입별로 다르게 동작 시킨다


## defer 지연 실행
- 배경
  - 프로그램 -> OS 파일 작업 시작할 때 파일 핸들 요청
  - OS -> 프로그램 프로그램은 파일 핸들을 만들어서 프로그램에 알려준다
  - 프로그램 -> OS 작업을 완료하고 파일 핸들 반환(내부 자원을 돌려주지 않으면 고갈되어 더는 파일을 만들 수 없기 때문)
- 목적
  - 파일 핸들을 반환해야하기 때문에 **함수 종료전에 처리해야하는 코드가 있을 때 실행**하기 위함
- defer 명령어, defer 명령어, defer 명령어 - 이렇게 세개의 defer 을 지정해 놓으면 함수 종료 직전 역순으로 호출된다
```go
defer func() {
		if err := tp.Shutdown(ctx); err != nil {
			logger.Errorf("[Error] shutting down tracer provider: %v", err)
		}
	}()
```
- lama main() 에서 사용

## 함수 타입 변수
- 함수가 호출되면 해당 함수 시작점으로 프로그램 카운터가 변경되듯이
- 함수도 포인터 처럼 숫자로 표현할 수 있다 -> 변수의 값이 될 수 있다
```go
func add(a,b int) int {
    return a + b
}

func getOperator(op string) func (int, int) int {
	if op == "+" {
		return add
    }
	...
}
main() {
	var operator func (int, int) int // 함수 타입 정의
	operator = getOperator("+")
	...
}
```

## 함수 리터럴
- 이름 없는 함수
- 다른 프로그래밍 언어에서는 익명함수 or 람다
```go
func main() {
	i := 0
	
	f := func() {
		i += 10
    }
	i++
	
	f() // 함수가 호출되는 시점 i값으로 사용
}
```
- 함수가 호출되는 시점은 i++ 이후이기 때문에
- 함수 내 i값은 1이다
- 캡쳐: 함수 리터럴에서 외부 변수를 내부 상태로 가져올 때 **값 복사가 아닌 인스턴스 참조**로 가져온다

### 함수 리터럴 내부 상태 주의점 - 캡처의 주의점
```go
for i := 0; i < 3; i++ {
	f[i] = func() {
		fmt.Println(i)
    }   
}
for i := 0; i < 3; i++ {
	f[i]()
}
```
- f[1] = 3,f[2] = 3, f[3]=3
- i 변수를 캡쳐할 때 캡쳐하는 순간의 i값이 복사되는 것이 아니라 i 변수가 참조되기 때문
- i 값은 최종적으로 3 -> i의 본래값을 참조하기 때문에 3 입력

```go
for i := 0; i < 3; i++ {
	v := i
    f[i] = func() {
        fmt.Println(v)
    }   
}
```
- i 값을 저장하는 변수를 새로 만들어 그 변수를 캡쳐하도록 한다
- v는 for문 내부에서 선언됐기 때문에 매 루프마다 새로운 v 변수 생성
- 루프 돌때마다 새로운 v 캡쳐하게 됨 -> 1,2,3 호출 성공