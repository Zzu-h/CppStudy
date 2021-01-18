# 함수 객체
함수 객체: 함수처럼 동작하는 객체
- '( )'연산자를 오버로딩한 객체
- 함수 객체 == 함수자 (Functor)라고도 한다.
- 정의 방법
	```c++
	struct Functor{
		void operator() (매개변수){
			...
		}
	}
	int main(){
		...
		Functor(매개변수);
		...
	}
	```

### 함수 객체를 왜 사용할까?
- 다른 멤버 변수와 멤버 함수를 가질 수 있다.
- 일반 함수에서 하지 못하는 지원을 받을 수 있다.
- 함수 객체의 서명이 같더라도 객체 타입이 다르면 서로 전혀 다른 타입으로 인식한다.
- 속도가 일반 함수보다 빠르다.
- 콜백의 경우 함수포인터는 인라인될 수 없지만, 함수 객체는 인라인될 수 있다.
	- 컴파일러가 쉽게 최적화가 가능

> 인라인 함수란? 코드라인 내부에 들어간 함수    
> 인라인 함수가 컴파일 되면, 컴파일러는 해당 함수가 선언된 코드에 함수 자체의 내용이 복사가 된다.    
> 실행속도가 훨씬 빠르며, 함수 오버헤드가 제거된다.
    
> 오버헤드: 프로그램의 실행흐름 도중에 동떨어진 위치의 코드를 실행시켜야 할 때 , 추가적으로 시간,메모리,자원이 사용되는 현상

```c++
#include<iostream>
using namespace std;

class Adder {
	int total;
public:
	explicit Adder(int n = 0):total(n){}
	int operator()(int n) {
		return total += n;
	}
};
int main() {
	Adder add(0);

	cout << add(10) << endl;
	cout << add(20) << endl;
	cout << add(30) << endl;

	return 0;
}
```

operator() 이 함수는 클래스 내부에 정의되어있다.    
객체를 생성하였으므로, 해당 객체의 메소드로 바로 사용이 가능하다.    
즉, 암묵적으로 인라인 함수가 되었다.

<br>
<br>

# 함수 객체 구현
less, greater: 조건자 9장 내용

```c++
#include<iostream>
#include<functional>
using namespace std;

typedef less<int> Less;

int main() {
	Less l;
	cout << l(10, 20) << endl; // 1.
	cout << l(20, 10) << endl; // 1.
	cout << l.operator()(10,20) << endl; // 2.
	cout << endl;
	cout << Less()(10, 20) << endl; // 3.
	cout << Less()(20, 10) << endl; // 3.
	cout << Less().operator()(10, 20) << endl; //4.

	return 0;
}
```
1. l 객체로 암시적 함수 호출
2. l 객체로 명시적 함수 호출
3. 임시 객체로 암시적 함수 호출
4. 임시 객체로 명시적 함수 호출

> typedef: 프로그래머가 타입의 별칭을 생성하고, 실제 타입 이름 대신 별칭을 사용할 수 있다    
> 사용방법: `'typedef 자료형 별칭'`

<br>
<br>

---

## Quiz
1. 빈칸 채우기
	1. 함수처럼 호출 가능한 클래스 객체를 가리켜 (`함수자(Functor)`)라 합니다.
	2. 함수처럼 호출 가능한 클래스 객체는 (`'( )'`) 연산자를 오버로딩해 생성합니다.
2. 다음 Equal 클래스의 객체가 cmp일 때 두 정수가 같으면 true, 아니면 false를 반환하는 Equal 클래스를 작성
	```c++
	class Equal {
	public:
		bool operator()(int a, int b) {
			return (a == b);
		}
	};
	```
3. 다음 Adder 클래스의 객체가 add일때 두 정수의 합을 반환하는 Adder 클래스를 작성
```c++
class Adder {
public:
	int operator()(int n, int m) {
		return n+m;
	}
};
```