# 템플릿
- 함수 템플릿: 여러 함수를 만들어내는 함수의 틀
- 클래스 템플릿: 여러 클래스를 만들어내는 클래스의 틀

함수 오버로딩을 하면 매개변수 리스트가 다른 함수들을 정의할 수 있다.    
하지만, 클라이언트가 매개변수 타입을 미리 알고 있다는 전제로 만들어진다.    
클라이언트가 어떤 타입을 사용하여 서버 코드를 호출할지 미리 알지 못하므로 함수 오버로딩의 한계가 있다.

<br>

-> 템플릿을 이용    
템플릿 함수를 사용하면
1. 컴파일러는 클라이언트의 함수 호출 인자 타입을 확인
2. 템플릿 함수의 매개변수 타입을 결정
3. 실제 함수인 템플릿 인스턴스 함수를 만듦

- 서버 함수는 일반화된 기능만 제공, 클라이언트가 함수의 매개변수 타입을 결정
- 사용방법: `template<typename T> void Print(T a, T b);`

```c++
#include<iostream>
using namespace std;

template<typename T>
void Print(T a, T b) {
	cout << a << ", " << b << endl;
}

int main() {
	
	// 암시적 함수 호출
	Print(10, 20);
	Print(0.123, 1.123);	
	Print("ABC", "abcde");

	// 명시적 함수 호출
	Print<int>(10, 20);
	Print<double>(0.123, 1.123);
	Print<const char*>("ABC", "abcde");
	return 0;
}
```
이 예제를 보자.    
컴파일러가 해석한 코드는 다음과 같다.
```c++
#include<iostream>
using namespace std;

void Print(int a, int b) {
	cout << a << ", " << b << endl;
}
void Print(double a, double b) {
	cout << a << ", " << b << endl;
}
void Print(const char* a, const char* b) {
	cout << a << ", " << b << endl;
}

int main() {
	
	// 암시적 함수 호출
	Print(10, 20);
	Print(0.123, 1.123);	
	Print("ABC", "abcde");

	// 명시적 함수 호출
	Print<int>(10, 20);
	Print<double>(0.123, 1.123);
	Print<const char*>("ABC", "abcde");

	return 0;
}
```
즉, `Print<int>()`, `Print<double>()`, `Print<const char*>()` 세 함수를 만들어 냈다.    
이러한 함수를 ***템플릿의 인스턴스***라고 한다.

### 템플릿도 여러개의 매개변수를 가질 수 있다.
- 사용방법: `template<typename T1,typename T2> void Print(T1 a, T2 b);`

> `template<typename T>` & `template<class T>`    
> class는 C++표준화 이전부터 사용하던 형식, 호환성을 위해 대부분의 컴파일러가 지원하기는 함.
---

<br>


### swap 예제
```c++
template<typename T>
void Swap(T& a, T& b) {
	T temp = a;
	a = b;
	b = temp;
}

int main() {
	int n1 = 10, n2 = 20;
	double d1 = 1.1, d2 = 2.2;
	cout << n1 << ", " << n2 << endl;
	Swap(n1, n2);
	cout << n1 << ", " << n2 << endl;
	cout << endl;

	cout << d1 << ", " << d2 << endl;
	Swap(d1, d2);
	cout << d1 << ", " << d2 << endl;
	cout << endl;

	return 0;
}
```

## 템플릿의 매개변수 타입 객체로 어떤 타입이든 상관이 없을까?
템플릿 함수 정의의 연산이 가능한 객체: 인터페이스를 지원하는 객체라면 모두 가능하다.
- 복사 생성자를 지원해야 한다.
	- T temp = a 문장처럼 복사생성자를 호출할 수도 있다.
- 대입 연산자를 지원해야 한다.
	- b = temp 문장처럼 대입 연산자를 호출할 수도 있다.
	- 근데 이건 어차피 같은 타입이라 가능하지 않나...?

### 함수 템플릿의 매개변수로 정수 등도 가능하다.
- 사용방법: `template<typename T,int size> void PrintArray(T* arr);`
- 다만, 클라리언트 코드에서 `PrintArray<int, 5>(arr1)`과 같이 명시적으로 호출해야 한다.
	- 함수 인자가 arr1의 정보만을 제공, 5의 템플릿 매개변수 인자를 컴파일러가 클라이언트 코드로부터 추론할 수 없다.
- double, string안됨, 기타 클래스도 안됨.
- char, int, short, bool 등 정수값이 저장되는 것들만 가능.

---
<br>

## 템플릿 특수화 (함수)
특수화된 버전의 함수 템플릿을 더 제공하는 것.
```c++
class Point {
	int x, y;
public:
	explicit Point(int x_ = 0, int y_ = 0) :x(x_), y(y_) {}
	void Print() const { cout << x << ',' << y << endl; }
};

template<typename T>
void Print(T a) {
	cout << a << endl;
}

// 특수화 함수 템플릿
template<>
void Print(Point a) {
	cout << "Print 특수화 버전: ";
	a.Print();
}

int main() {
	int n = 10;
	double d = 2.5;
	Point pt(2, 3);

	Print(n);
	Print(d);
	Print(pt);

	return 0;
}
```
만일 특수화된 함수 템플릿이 없다면, `Print(pt);`는 `cout<< pt << endl;`가 되어 에러가 발생한다.     
에러 해결방법
- Point클래스에 << 연산자 오버로딩 함수를 추가
	- 라이브러리 상태 등과 같은 이유로 클래스를 수정하지 못할 수도 있다.
	- 이러한 상태에서 에러를 해결하는 방법은 아래와 같다.
- 위와 같이 Point 객체만의 특수화된 함수 템플릿을 지원

<br>
<br>

# 클래스 템플릿
클래스 템플릿: 클래스를 만들어내는 틀(메타코드)   

예를들어 Array클래스를 만든다고 하자.    
이 배열은 int를 저장할 수도 있고, double도 저장할 수도 있고, 혹은 사용자 정의 타입일 수도 있다.    

### 클래스 템플릿을 이용하면 단 하나의 클래스로 여러 타입의 Array클래스를 생성할 수 있다.
템플릿 매개변수 인자를 통해 클라이언트가 클래스에 사용될 타입을 결정하게 한다.
- 사용방법: `template<typename T> class Array`
- 사용 예제
	```c++
	template<typename T>
	class Array {
		T* buf;
		int size;
		int capacity;
	public:
		explicit Array(int cap = 100) :buf(0), size(0), capacity(cap) {
			buf = new T[capacity];
		}
		~Array() { delete[] buf; }

		void Add(T data) { buf[size++] = data; }
		T operator[](int idx)const { return buf[idx] };

		int GetSize() const { return size; }
	};

	int main() {
		Array<int> iarr;
		iarr.Add(10);
		iarr.Add(20);
		iarr.Add(30);

		for (int i = 0; i < iarr.GetSize(); i++)
			cout << iarr[i] << " ";
		cout << endl;

		Array<double> darr;
		darr.Add(10.12);
		darr.Add(20.12);
		darr.Add(30.12);

		for (int i = 0; i < darr.GetSize(); i++)
			cout << darr[i] << " ";
		cout << endl;

		Array<string> sarr;
		sarr.Add("abc");
		sarr.Add("ABC");
		sarr.Add("Hello!");

		for (int i = 0; i < sarr.GetSize(); i++)
			cout << sarr[i] << " ";
		cout << endl;

		return 0;
	}
	```
함수 템플릿과 같이 컴파일러가 해당 타입에 맞는 실제 클래스를 생성한다.

템플릿의 매개변수는 디폴트 매개변수 값을 지정할 수 있는데, 함수 템플릿도 가능하다.     
(책 저자가 함수템플릿에서 깜빡한거 같다.)
- 사용방법: `template<typename T = int, int capT = 100> class Array`


## 템플릿 특수화 (클래스)
- 일반 버전의 템플릿을 사용할 수 없는 경우나 성능 개선이나 특수 기능 등을 위해 사용
- 사용방법: `template<> class ObjectInfo<string>`
- 사용 예제
	```c++
	template<typename T>
	class ObjectInfo {
		T data;
	public:
		ObjectInfo(const T& d) : data(d) { }
		void Print() {
			cout << "타입: " << typeid(data).name() << endl;
			cout << "크기: " << sizeof(data) << endl;
			cout << "값: " << data << endl;
			cout << endl;
		}
	};
	template<>
	class ObjectInfo<string> {
		string data;
	public:
		ObjectInfo(const string& d) : data(d) { }
		void Print() {
			cout << "타입: " << "string" << endl;
			cout << "문자열 길이: " << data.size() << endl;
			cout << "값: " << data << endl;
			cout << endl;
		}
	};

	int main() {
		ObjectInfo<int> d1(10);
		d1.Print();

		ObjectInfo<double> d2(5.5);
		d2.Print();

		ObjectInfo<string> d3("Hello!");
		d3.Print();

		return 0;
	}
	```


<br>
<br>

# STL을 위한 템플릿 예제
- 템플릿을 이용한 For_each()문
	- 일반 함수 템플릿
		```c++
		template<typename IterT, typename Func>
		void For_each(IterT begin, IterT end, Func pf) {
			while (begin != end)
				pf(*begin++);
		}
		template<typename T>
		void Print(T data) {
			cout << data << " ";
		}

		int main() {
			int arr[5] = { 10, 20, 30, 40, 50 };
			For_each<int*, void(*)(int)>(arr, arr + 5, Print<int>);
			cout << endl;

			string sarr[3] = { "abc","ABCDE","Hello!" };
			For_each<string*, void(*)(string)>(sarr, sarr + 5, Print<string>);
			cout << endl;

			return 0;
		}
		```
	- 함수 객체 템플릿
		```c++
		template<typename IterT, typename Func>
		void For_each(IterT begin, IterT end, Func pf) {
			while (begin != end)
				pf(*begin++);
		}
		template<typename T>
		struct PrintFunctor {
			string sep;
			explicit PrintFunctor(const string& s = " "):sep(s){ }
			void operator()(T data) const {
				cout << data << sep;
			}
		};

		int main() {
			int arr[5] = { 10, 20, 30, 40, 50 };
			For_each(arr, arr + 5, PrintFunctor<int>());
			cout << endl;

			string sarr[3] = { "abc","ABCDE","Hello!" };
			For_each(sarr, sarr + 5, PrintFunctor<string>());
			cout << endl;

			return 0;
		}
		```
		- 일반 함수를 이용할 때보다 부가적인 서비스를 제공이 가능해졌다.
			- 출력 구분자를 입력하여 출력 가능하게 됨
- 템플릿을 이용한 Pair()문
```c++
template<typename T1, typename T2>
struct Pair {
public:
	T1 first;
	T2 second;
	Pair(const T1 & ft, const T2& sd):first(ft),second(sd){}
};

int main() {
	Pair<int, int> p1(10, 20);
	cout << p1.first << ',' << p1.second << endl;
	Pair<int, string>p2(1, "one");
	cout << p2.first << ',' << p2.second << endl;

	return 0;
}
```

<br>

### <u>**템플릿의 매개변수와 함수 객체를 결합하면 반환 타입과 함수 매개변수 타입을 클라이언트가 결정하는 유연한 함수 객체를 만들 수 있다**</u>
예제
```c++
template<typename RetType, typename ArgType>
class Functor {
public:
	RetType operator()(ArgType data){
		cout << data << endl;
		return RetType();
	}
};

int main() {
	Functor<void, int>functor1;
	functor1(10);

	Functor<bool, string>functor2;
	functor2("Hello!");

	return 0;
}
```

<br>
<br>

---

## Quiz
1. 배열의 원소를 복사하는 함수 템플릿 Copy()를 작성
```c++
template<typename T>
void Copy(T* arr2, T* arr1, int size) {
	for (int i = 0; i < size; i++)
		arr2[i] = arr1[i];
}

int main() {
	int arr1[5] = { 10,20,30,40,50 };
	int arr2[5];
	Copy(arr2, arr1, 5);
	for (int i = 0; i < 5; i++)
		cout << arr2[i] << " ";
	
	return 0;
}
```
2. 최소한의 Stack 클래스 작성
```c++
template<typename T>
class Stack {
	T* arr;
	int capacity;
	int size;
public:
	Stack(int Cap = 100) :arr(0), capacity(Cap), size(0) {
		this->arr = new T[capacity];
	}
	bool Empty() {
		return(size ? false : true);
	}
	void Push(T data) {
		arr[size++] = data;
	}
	T Pop() {
		return arr[--size];
	}
};


int main() {
	Stack<int>st;

	st.Push(10);
	st.Push(20);
	st.Push(30);

	if (!st.Empty())
		cout << st.Pop() << endl;
	if (!st.Empty())
		cout << st.Pop() << endl;
	if (!st.Empty())
		cout << st.Pop() << endl;

	return 0;
}
```
3. 최소한의 Queue 클래스 작성
```c++
template<typename T>
class Queue {
	T* arr;
	int capacity;
	int size;
	int iter;
public:
	Queue(int Cap = 100) :arr(0), capacity(Cap), size(0),iter(0) {
		this->arr = new T[capacity];
	}
	bool Empty() {
		return(size ? false : true);
	}
	void Push(T data) {
		arr[size++] = data;
	}
	T Pop() {
		return arr[iter++];
	}
};


int main() {
	Queue<int>q;

	q.Push(10);
	q.Push(20);
	q.Push(30);

	if (!q.Empty())
		cout << q.Pop() << endl;
	if (!q.Empty())
		cout << q.Pop() << endl;
	if (!q.Empty())
		cout << q.Pop() << endl;

	return 0;
}
```