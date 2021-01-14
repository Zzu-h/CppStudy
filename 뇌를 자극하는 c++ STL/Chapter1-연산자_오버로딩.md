# 연산자 오버로딩
컴파일러와 약속된 함수를 이용해 사용자 정의 타입에 연산이 가능하도록 제공하는 것
사용자 정의 타입에도 연산자를 사용할 수 있게 하는 문법    

기본적인 연산자는 컴파일러에 연산이 정의가 되어 있어 사용하는데 문제가 없다.    
하지만 사용자 정의 타입은 컴파일러에 정의가 되어 있지 않아 오류가 발생한다.

장점: 직관성과 가독성 향상

<br>

## 연산자 오버로딩을 하는 방법
- 멤버 함수를 이용한 연산자 오버로딩
- 전역 함수를 이용한 연산자 오버로딩

<br>


# 연산자 오버로딩 정의 및 사용
오버로딩 방법: `returnType operator symbol (getType){}`

```c++
#include<iostream>
using namespace std;

class Point {
private:
	int x;
	int y;
public:
	Point(int _x = 0, int _y = 0):x(_x),y(_y){}
	void Print() const {
		cout << x << ',' << y << endl;
	}

	void operator+(Point arg) {
		cout << "operator+() 함수호출" << endl;
	}
};

int main() {
	Point p1(2, 3), p2(5, 5);

	p1 + p2;

	return 0;
}
```
여기서 `p1 + p2;`는 곧 `p1.operator+(p2);`가 된다.

<br>

> const 멤버 함수: 함수 내에서 객체의 멤버 변수를 변경하지 않음을 보장하는 함수
> const 객체는 const멤버 함수만 호출 가능하다.

<br>

# 단항 연산자 오버로딩
단항 연산자: !, &, ~, *, +, -, ++, --

## ++ / -- 연산자 오버로딩
전위 연산자와 후위 연산자로 구분하여 오버로딩이 가능하다.

- 전위 연산
    ```c++
    const Point operator++(){ //or operator--()
        this->x++;
        this->y++;
        return *this;
    }
    ```
- 후위 연산
    ```c++
    const Point operator++(int){ //or operator--(int)
        Point tmp(this->x, this->y);
        this->x++;
        this->y++;
        return tmp;
    }
    ```

<br>

# 이항 연산자 오버로딩
이상 연산자: +, -, *, /, ==, !=, <, <= 등

## == / != 연산자 오버로딩
- ==연산자
    ```c++
    bool operator==(const Point& arg) const{
        return this->x == arg.x && this->y == arg.y;
    }
    ```
- !=연산자
    ```c++
    bool operator!=(const Point& arg) const{
        return !(*this == arg);
    }
    ```


<br>

# 전역 함수를 이용한 연산자 오버로딩
지금까지는 멤버 함수를 이용하여 오버로딩을 했다.    
멤버 함수를 이용한 연산자 오버로딩을 사용할 수 없을 때 ***전역 함수 연산자 오버로딩***을 한다.

### 멤버 함수를 이용한 연산자 오버로딩이 불가한 경우
왼쪽 항이 사용자가 정의한 타입의 객체, 연산자 오버로딩한 객체가 아니라면 사용이 불가하다.     
ex) `4 + p1;`

### 컴파일러가 해석하는 연산자 오버로딩 (p1 == p2)
- 멤버 함수로 `p1.operator==(p2);`로 해석
- 전역 함수 `operator==(p1, p2);`로 해석

<br>

전역 함수를 이용한 연산자 오버로딩    
오버로딩 방법: `returnType operator symbol (getType, getType){}`

사용 예시
```c++
#include<iostream>
using namespace std;

class Point {
private:
	int x, y;
public:
	Point(int _x = 0, int _y = 0):x(_x),y(_y){}
	void Print() const {
		cout << x << ',' << y << endl;
	}
	int getX() const { return x; }
	int getY() const { return y; }
};
const Point operator + (const Point& argL, const Point& argR) {
	return Point(argL.getX() + argR.getX(), argL.getY() + argR.getY());
}

int main() {
	Point p1(2, 3), p2(5, 5);

	Point p3 = p1 + p2;
    
    p3.Print();
	
    return 0;
}
```

> freind: (함수 프렌드, 클래스 프렌드)
> 함수나 클래스를 프렌드로 지정하면 모든 클래스 멤버(private, protected, public)를 제한 없이 사용 가능함.

<br>

# STL에 필요한 주요 연산자 오버로딩
## 함수 호출 연산자 오버로딩
함수 호출 연산자 오버로딩 (): 객체를 함수처럼 동작하게 하는 연산자     
함수 객체: 함수 호출 연산자를 정의한 객체
```c++
#include<iostream>
using namespace std;

struct FuncObject
{
public:
	void operator()(int arg)const {
		cout << "정수: " << arg << endl;
	}
};

void Print1(int arg) {
	cout << "정수: " << arg << endl;
}

int main() {
	void(*Print2)(int) = Print1;
	FuncObject Print3;

	Print1(10);
	Print2(10);
	Print3(10);

	return 0;
}
```
여기서 Print2는 함수 포인터이다. 이는  [2장 내용](./Chapter2-함수_포인터.md)에서 볼 수있다.    
Print3는 객체이다. 여기서 객체를 함수처럼 사용한 것임을 볼 수 있다.


## 배열 인덱스 연산자 오버로딩
배열 인덱스 연산자 오버로딩 []: 배열에 사용하는 []연산자를 객체에도 사용이 가능하다.     
ex) vector class     
일반적으로 많은 객체를 저장하고 관리하는 객체에 사용된다.(컨테이너 객체에 사용됨)


사용방법: `int operator[](int idx) const{ ... }`

[ ]연산자 오버로딩은 const와 비 const 함수 모두 제공해야 하므로 다음과 같이 2번 정의해야한다.
```c++
int operator[](int idx) const{ ... }
int& operator[](int idx) { ... }
```

## 메모리 접근, 클래스 멤버 접근 연산자 오버로딩
메모리 접근, 클래스 멤버 접근 연산자 오버로딩 *,->: 스마트 포인터나 반복자 등의 특수한 객체에 사용됨     
스마트 포인터: 동적으로 할당한 메모리를 알아서 해제해준다.    

오버로딩 방법: `returnType operator symbol (getType){}`
```c++
#include<iostream>
using namespace std;

class Point{
private:
	int x, y;
public:
	Point(int _x = 0,int _y= 0):x(_x),y(_y){}
	void Print() const { cout << x << ", " << y << endl; }
};

class PointPtr {
private:
	Point* ptr;
public:
	PointPtr(Point *p):ptr(p){}
	~PointPtr() {
		delete ptr;
	}
	Point* operator->() const {
		return ptr;
	}
	Point& operator*() const {
		return *ptr;
	}
};
int main() {
	PointPtr p1 = new Point(2, 3);
	PointPtr p2 = new Point(5, 5);

	p1->Print();
	p2->Print();

	return 0;
}
```

<br>

# 타입 변환 연산자 오버로딩
사용자가 정의한 타입 변환
- 생성자를 이용한 타입 변환
	- 사용자가 타입에 맞는 생성자를 일일이 생성해준다.
	- 생성할 때 입력되는 타입의 다양성을 제공한다.
	- 다른 타입을 자신의 타입으로 변환
		```c++
		#include<iostream>
		using namespace std;

		class A {};
		class B {
		public:
			B() { cout << "B()생성자" << endl; }
			B(A& _a) { cout << "B(A _a)생성자" << endl; }
			B(int n) { cout << "B(int n)생성자" << endl; }
			B(double d) { cout << "B(double d)생성자" << endl; }
		};

		int main() {
		
			A a;
			int n = 10;
			double d = 5.5;

			B b;
			b = a;
			b = n;
			b = d;
		
			return 0;
		}
		```
- 타입 변환 연산자 오버로딩을 이용한 타입 변환
	- 자신의 타입을 다른 타입으로 변환
	- 타입변환 연산자는 반환 타입을 <u>지정하지 않는다!</u>
		```c++
		#include<iostream>
		using namespace std;
		
		class A {};
		class B {
		public:
			operator A() const {
				cout << "operator A() 호출" << endl;
				return A();
			}
			operator int() const {
				cout << "operator int() 호출" << endl;
				return 10;
			}
			operator double() const {
				cout << "operator double() 호출" << endl;
				return 5.5;
			}
		};
		
		int main() {
		
			A a;
			int n = 10;
			double d = 5.5;
		
			B b;
			a = b;
			n = b;
			d = b;
			
			return 0;
		}		
		```
	- const 객체, 비 const 객체 모두 타입 변환이 가능하게 const 멤버 함수로 정의한다.

> explicit: 생성자를 이용한 형변환을 의도하지 않는다면 명시적 호출만 그낭하도록 한다.

<br>
<br>
<br>

---
## Quiz

1. 이항 연산자 같은 의미 찾기 (2, 3)
	```c++
	Point p1, p2;
	p1 + p2
	```
	- `p1.operator+(p2);`
	- `operator+(p1, p2);`
2. 함수 호출 연산자() (4)
	```c++
	func(10, 20, 30);
	```
	- int operator()(int, int, int);
3. 배열 인덱스 연산자[ ] (3)
	```c++
	array[0];
	```
	- int operator[](int);
4. 최소한의 String class 작성1
	```c++
	class String {
		const char* str;
	public:
		String(const char* _str) {
			str = _str;
		}
		operator const char* () const {
			return str;
		}
	};
	```
5. 최소한의 String class 작성2
	```c++
	class String {
		const char* str;
	public:
		String(const char* _str) {
			str = _str;
		}
		operator const char* () const {
			return str;
		}
	};
	```