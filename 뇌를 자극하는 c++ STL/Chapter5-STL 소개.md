# STL이란?
STL(Standard Template Library): 표준 C++라이브러리의 일부분     
자료구조와 알고리즘을 템플릿으로 제공하는 라이브러리    

<br>

STL의 구성 요소
- 컨테이너(Container): 객체를 저장하는 객체 (= 컬렉션, 자료구조 라고도 함)
- 반복자(Iterator): 컨테이너의 원소를 가리킴, 다음 원소를 가리키게 하는 기능 제공
- 알고리즘(Algorithm): 정렬, 삭제, 검색, 연산 등을 해결하는 일반화된 방법을 제공하는 함수 템프릿
- 함수 객체(Function Object): 컨테이너와 알고리즘 등에 클라이언트 정책을 반영
- 어댑터(Adapter): 구성 요소의 인터페이스를 변경, 새로운 인터페이스를 갖는 구성 요소로 변경
- 할당기(Allocator): 컨테이너의 메모리 할당 정책을 캡슐화한 클래스 객체
	- 모든 컨테이너는 자신만의 기본 할당기를 가지고 있다.

<br>

STL의 특징
- 효율성
- 재사용성
- 확장성

<br>

---

## 컨테이너
같은 타입을 저장, 관리 목적으로 맏늘어진 클래스

정렬 기준에 따라
- 표준 시퀀스 컨테이너(standard sequence container): 컨테이너 원소가 자신만의 삽입 위치를 가짐
	- vector, deque, list
- 표준 연관 컨테이너(standard associative container): 저장 원소가 삽입 순서와 다르게 특정 정렬 기준에 의해 자동 정렬됨
	- set, multiset, map, multimap

저장 단위에 따라
- 배열 기반 컨테이너(array-based container): 데이터 여러 개가 하나의 메모리 단위에 저장
	- vector, deque
- 노드 기반 컨테이너(node-based container): 데이터 하나를 하나의 메모리 단위에 저장
	- list, set, multiset, map, multimap

<br>



## 반복자
### 역할
- 포인터와 비슷하게 동작한다.    
- 컨테이너에 저장된 원소를 순회, 접근하는 일반화된 방법을 제공한다.    
- 컨테이너와 알고리즘이 하나로 동작하게 묶어주는 인터페이스 역할을 한다.
- 알고리즘이 컨테이너로부터 독립적이면서도 컨테이너와 결합하여 동작할 수 있게 한다.

### 특징
- 컨테이너 내부의 원소를 가리키고 접근할 수 있어야 한다.
	- *연산자를 제공한다.
- 다음 원소로 이동, 컨테이너의 모든 원소를 순회할 수 있어야 한다.
	- ++, !=, == 연산자를 제공한다.

컨테이너 원소의 집합을 순차열이라고 한다.    
순차열: 원소의 순서있는 집합

STL의 모든 컨테이너는 자신만의 반복자를 제공
- begin(): 순차열의 시작을 가리키는 반복자 반환
- end(): 순차열의 끝을 가리키는 반복자 반환
	- 실제 마지막 원소가 아닌 끝을 표시하는 원소이다.

<br>

### 반복자의 종류
- 입력 반복자: 현 위치의 원소를 한 번만 읽을 수 있는 반복자
- 출력 반복자: 현 위치의 원소를 한 번만 쓸 수 있는 반복자
- 순방향 반복자: 입력, 출력 반복자 기능에 순방향으로 이동(++)이 가능한 재할당될 수 있는 반복자
- 양방향 반복자: 순방향 반복자 기능에 역방향으로 이동(--)이 가능한 반복자
	- list, set, multiset, map, multimap
- 임의 접근 반복자: 양방향 반복자 기능에 +, -, +=, -=, []연산이 가능한 반복자
	- vector, deque

> 모든 컨테이너는 양방향 반복자 이상을 제공

<br>


## 알고리즘
순차열의 원소를 조사, 변경, 관리, 처리를 수행    
알고리즘은 한 쌍의 반복자(시작과 끝)를 필요로 한다.    
<br>
알고리즘은 특정 컨테이너나 원소 타입에 종속적이지 않다.    
ex) find 알고리즘은 순방향 반복자를 요구, 순방향 반복자만 지원하는 컨테이너라면 알고리즘을 수행 가능

알고리즘 범주
- 원소를 수정하지 않는 알고리즘(nonmodifying algorithms)
- 원소를 수정하는 알고리즘(modifying algorithms)
- 제거 알고리즘(removing algorithms)
- 변경 알고리즘(mutating algorithms)
- 정렬 알고리즘(sorting algorithms)
- 정렬된 범위 알고리즘(sorted range algorithms)
- 수치 알고리즘(numeric algorithms)


<br>

## 함수 객체
클라이언트가 정의한 동작을 다른 구성 요소에 반영하려 할 때 사용됨    
많은 STL알고리즘이 함수 객체, 함수, 함수 포인터 등의 함수류를 인자로 받는다.

<br>

## 어댑터
구성 요소의 인터페이스를 변경한다.
STL의 어댑터
- 컨테이너 어댑터(container adaptor)
	- **stack**, queue, priority_queue
- 반복자 어댑터(iterator adaptor)
	- **reverse_iterator**, back_insert_iterator, front_insert_iterator, insert_iterator
- 함수 어댑터(function adaptor)
	- 바인더, **부정자**, 함수 포인터 어댑터


각 대표 어댑터의 사용
- stack: container adaptor
	- 일반 컨테이너를 LIFO 방식의 스택 컨테이너로 변환한다.
	- empty, size, push_back, pop_back, back 인터페이스를 지원하는 컨테이너 모두 사용 가능
	- 시퀀스 컨테이너는 모두 지원하므로 stack 컨테이너 어댑터의 컨테이너로 사용 가능
	- 디폴트 컨테이너: deque
	- 사용예제
		```c++
		#include<iostream>
		#include<vector>
		#include<stack>
		using namespace std;

		int main() {
			stack<int, vector<int>> st;

			st.push(10);
			st.push(20);
			st.push(30);

			cout << st.top() << endl;
			st.pop();
			cout << st.top() << endl;
			st.pop();
			cout << st.top() << endl;
			st.pop();

			if (st.empty())
				cout << "원소 없음" << endl;

			return 0;
		}
		```
- reverse_iterator: iterator adaptor
	- 정방향 반복자와 반대로 동작(--, ++)
	- 역방향 반복자가 가리키는 원소와 실제 가리키는 값이 다르다.
		- 반복자가 가리키는 원소 다음 원소의 값을 참조한다.
		- 알고리즘 수행 시 정방향 반복자와 호환되도록 하기 위해서이다.
	- 정방향 반복자가 [begin, end)라면 역방향 반복자는 [end, begin)이다.
	- 역방향 반복자를 위한 rbegin(), rend() 메소드를 제공
	- --연산자를 사용하지 않고 ++연산만으로 정방향 및 역방향 순회가 가능하게 한다.
		- 대부분의 알고리즘이 ++연산자만으로 구현되어 있기에 이를 통해 역방향도 가능하게 되었다.
	- 사용예제
		```c++
		#include<iostream>
		#include<vector>
		using namespace std;

		int main() {
			vector<int> v;

			v.push_back(10);
			v.push_back(20);
			v.push_back(30);
			v.push_back(40);
			v.push_back(50);

			vector<int>::iterator normal_iter = v.begin() + 3;
			vector<int>::reverse_iterator reverse_iter(normal_iter);

			cout << "정방향 반복자의 값: " << *normal_iter << endl;
			cout << "역방향 반복자의 값: " << *reverse_iter << endl;

			for (vector<int>::iterator iter = v.begin(); iter != normal_iter; ++iter)
				cout << *iter << " ";
			cout << endl;
			for (vector<int>::reverse_iterator riter = reverse_iter; riter != v.rend(); ++riter)
				cout << *riter << " ";
			cout << endl;

			return 0;
		}
		```
- not2: function adaptor
	- 조건자 함수 객체를 반대 의미의 조건자 함수 객체로 변경하는 어댑터
	- not1 단항 조건자, not2 이항 조건자에 사용
	- 사용예제
		```c++
		#include<iostream>
		#include<functional>
		using namespace std;

		int main() {
			cout << less<int>()(10, 20) << endl;
			cout << not2(less<int>())(10, 20) << endl;

			less<int> l;
			cout << l(10, 20) << endl;
			cout << not2(l)(10, 20) << endl;

			return 0;
		}
		```

<br>

## 할당기
컨테이너의 메모리 할당 정보와 정책(메모리 할당 모델)을 캡슐화한 STL 구성 요소이다.
- 템플릿 클래스이다.
- 모든 컨테이너는 기본 할당기를 사용함
- 사용자가 직접 STL의 할당기를 정의 및 사용 할 수 있다.
	- 사용자가 직접 메모리 할당 방식을 제어할 수 있다.
	- 다중 스레드에 최적화되고 사용자 메모리 할당 모델이 필요한 경우
	- 사용자가 컨테이너에 맞는 메모리 할당 모델을 설계할 경우
	- 특졍 구현 환경에서 최적화된 메모리 할당 모델을 구축할 경우
- 모든 컨테이너는 템플릿 매개변수에 할당기를 인자로 받는다.
- 사용 예제
	```c++
	#include<iostream>
	#include<vector>
	#include<set>
	using namespace std;

	int main() {
		vector<int, allocator<int>> v;
		v.push_back(10);
		cout << *v.begin() << endl;

		set<int, less<int>, allocator<int>> s;
		s.insert(10);
		cout << *s.begin() << endl;

		return 0;
	}
	```

<br>
<br>

---

## Quiz

1. 빈칸 채우기1
	1. STL 구성 요소에서 객체들을 저장하는 객체를(`컨테이너`)라 합니다.
	2. 컨테이너의 원소를 순회하고 참조하는 객체를(`반복자`)라 합니다.
	3. 여러 가지 문제 해결을 위한 반복자와 동작하는 함수 템플릿을 (`알고리즘`)라 합니다.

2. 빈칸 채우기2
	1. 컨테이너 원소가 자신만의 삽입 위치를 갖는 것을 (`표준 시퀀스`) 컨테이너라 합니다.
	2. 컨테이너 원소가 특정 정렬 기준에 의해 자동 정렬된 것을 (`표준 연관`) 컨테이너라 합니다.

3. 빈칸 채우기3
	1. 배열 기반 컨테이너인 vector와 deque는 (`임의 접근`) 반복자를 제공하며, 그외 모든 STL 컨테이너는 (`양방향`) 반복자를 제공합니다.
	2. (`순차열`)은 원소의 순서 있는 집합을 의미하며, (`순차열`)은 반복자 쌍으로 표현합니다.

4. 다음 구간[begin, end), [begin, iter), [iter, end)의 순차열을 쓰세요.
	- [begin, end): A, B, C, D, E
	- [begin, iter): A, B
	- [iter, end): D, E

5. 다음 중 양방향 반복자가 지원하지 않는 연산자를 고르세요.(4, 5, 6)
	- [], +=, -=은 임의 접근 반복자만 지원한다.

6. 빈칸 채우기4
	1. STL 컨테이너는 자신이 지원하는 반복자를 반환하기 위한 멤버함수(`begin()`)와 (`end()`)를 제공하며 각각 시작 원소의 반복자와 끝 표시 반복자입니다.
	2. iter 반복자가 가리키는 원소를 참조하기 위해(`*`)연산자를 사용합니다.

7. 빈칸 채우기5
	1. (`어댑터`)는 구성 요소의 인터페이스를 변경합니다.
	2. stack, queue, priority_queue는 (`컨테이너 어댑터`)이며, reverse_iteartor, insert_iterator 등을 (`반복자 어댑터`)라 합니다.
	3. (`함수 어댑터`)에는 바인더, 부정자 등이 있습니다.