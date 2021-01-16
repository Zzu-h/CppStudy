# 함수 포인터

정의
- 함수 이름: 함수가 시작하는 시작 주소    
- 함수 포인터: 함수의 시작 주소를 저장하는 포인터    
- 함수 시그니처: 함수의 반환 타입과 매개변수 리스트

함수 포인터 선언 방법: 함수 시그니처와 같게 선언한다.
```c++
int func(int a, int b){ }
...
int (*pf)(int, int);
pf = func;
```

### 함수 포인터는 메모리 접근 연산자(*)를 붙여도 함수의 주소이다.
pf(10) == *pf(10);

# 함수 포인터의 종류
함수 호출 방법
- 정적 함수 호출(정적 함수)
- 개체로 멤버 함수 호출(멤버 함수)
- 객체의 주소로 멤버 함수 호출(멤버 함수)

예시
```c++
#include<iostream>
using namespace std;

void Print() {
	cout << "정적 함수 Print()" << endl;
}

class Point {
public:
	void Print() {
		cout << "멤버 함수 Print()" << endl;
	}
};

int main() {
	Point pt;
	Point* p = &pt;

	Print();
	pt.Print();
	p->Print();
	
	return 0;
}
```

## 정적 함수 호출
지금까지 했던 함수 호출 방식이랑 같다.

> 함수 호출 규약: 함수 호출 시 전달되는 인자의 순서나 함수가 종료될 때 함수의 스택을 정리하는 시점 등을 약속한 것.     
> ex) stdcall, cdecl, thiscall, fastall etc    
> 정적 함수 기본 함수 호출 규약: cdecl     
> 멤버 함수 기본 함수 호출 규약: thiscall    
> 따라서, 정적 함수 포인터와 멤버 함수 포인터를 다르게 선언


## 객체와 주소로 멤버 함수 호출
멤버 함수 포인터는 함수 포인터 선언에 어떤 클래스의 멤버 함수를 가리킬 것인지 이름을 지정해야 한다.    
`void Print::Print(int n)`의 멤버 함수 포인터: `void (Point::*pf)(int)`     

멤버 함수 호출 방법
- 객체로 멤버 함수 호출 시 `.*`연산자를 이용한다.
- 주소로 멤버 함수 호출 시 `->*`연산자를 이용한다.
- 함수 호출 시 `.*/->*` 연산자 사이에 () 연산자를 사용해야 함
    - 연산자 우선순위로 인한 오류 방지

```c++
#include<iostream>
using namespace std;


class Point {
private:
	int x, y;
public:
	explicit Point(int x_ = 0, int y_ = 0) :x(x_), y(y_) {}
	void Print() const { cout << x <<", " << y << endl; }
	void PrintInt(int n){ cout << "Test num: " << n << endl; }
};

int main() {
	Point pt(2, 3);
	Point* p = &pt;

	void (Point:: * pf1)() const;
	pf1 = &Point::Print;

	void (Point:: * pf2)(int);
	pf2 = &Point::PrintInt;

	pt.Print();
	pt.PrintInt(10);
	cout << endl;

	(pt.*pf1)();
	(pt.*pf2)(10);
	cout << endl;

	(p->*pf1)();
	(p->*pf2)(10);

	return 0;
}
```

# 클라이언트 코드와 서버 코드
정의
- 서버 코드: 어떤 기능이나 서비스를 제공하는 코드
- 클라이언트 코드: 기능을 제공 받는 코드
- 콜(call): 클라이언트가 서버를 호출
- 콜백(callback): 서버가 클라이언트를 호출
    - 윈도우의 모든 프로시저는 시스템이 호출하는 *콜백 함수*이다.

```c++
#include <iostream>
using namespace std;

void Client();

// 서버 코드
void PrintHello() {
	cout << "Hello!" << endl;
	Client();
}

// 클라이언트 코드
void Client() {
	cout << "I'm client" << endl;
}

int main() {
	PrintHello();

	return 0;
}
```

콜백 메커니즘을 구현하기 위해 클라이언트가 서버를 호출할 때 서버에 클라이언트 정보를 제공해야한다.    
-> 함수 포인터 매개변수를 이용해 콜백 함수의 주소를 전달    
(기타 다른 방법: 함수 객체, 대리자, 전략 패턴 등)

for_each문을 통한 예제2
```c++
#include <iostream>
using namespace std;


// 서버 코드
void For_each(int* begin, int* end, void(*pf)(int)) {
	while (begin != end) {
		pf(*begin++);
	}
}


// 클라이언트 코드
void Print1(int n) {
	cout << n << ' ';
}
void Print2(int n) {
	cout << n*n << ' ';
}
void Print3(int n) {
	cout << "정수: " << n << endl;
}

int main() {
	int arr[5] = { 10, 20, 30, 40, 50 };

	For_each(arr, arr + 5, Print1);
	cout << "\n\n";
	For_each(arr, arr + 5, Print2);
	cout << "\n\n";
	For_each(arr, arr + 5, Print3);

	return 0;
}
```

For_each문만 3번을 호출하였지만, 동작되는 것은 클라이언트 코드에 의해 결정되었다.     

<br>
<br>
<br>

---
## Quiz
1. 함수 포인터 정의 빈칸 채우기1
    ```c++
    void Print(int data){ ... }
    ```
    - Print() 함수가 정적 함수나 전역함수라면 함수 포인터는 (`void *pf(int)`)와 같이 선언합니다.
    - Print() 함수가 MyClass 클래스의 멤버 함수라면 함수 포인터는 (`void (MyClass::*pf)(int)`)와 같이 선언합니다.
    - 함수 포인터 pf가 전역 함수 Print()의 주소라면 (`pf(data)`)처럼 전역 함수를 호출합니다.
    - 함수 포인터 pf가 MyClas 클래스의 멤버 함수 Print()의 주소고, 객체가 obj라면 (`(obj.*pf)(data)`)처럼 멤버 함수를 호출합니다.
    
2. 빈칸 채우기 2
    - 서버는 클라이언트의 정책을 반영하려고 서버 측 코드에서 클라이언트 측 함수를 호출합니다. 이때 서버가 호추라는 클라이언트 함수를 가리켜 (콜백) 함수라 합니다.
