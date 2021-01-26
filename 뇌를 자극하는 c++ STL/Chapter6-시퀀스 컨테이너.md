# vector 컨테이너

## 주요 인터페이스 및 특징
- vector는 시퀀스 컨테이너이다.
	- 원소 추가 및 제거의 메소드 제공: push_back(), pop_back()
	- 첫 원소와 마지막 원소를 참조하는 메소드 제공: front(), back()
	- 지정한 위치에 원소를 삽입하는 메소드 제공: insert()
- vector는 앞쪽이 막혀 있는 형태이다.
	- 앞쪽에는 원소를 추가/제거가 불가
	- 뒤쪽에만 추가 및 제거 기능 수행
- 사용예제
	```c++
	#include<iostream>
	#include<vector>
	using namespace std;

	int main() {
		vector<int> v;

		for (int i = 1; i <= 5; i++)
			v.push_back(i * 10);

		for (int i = 0; i < v.size(); i++)
			cout << v[i] << endl;

		return 0;
	}
	```
	- `for (int i = 0; i < v.size(); i++)`에서 경고가 뜬다.
	- size가 unsigned int를 반환하기 때문이다.
	- i를 unsigned int로 바꾸면 되긴 하나 다음의 방법이 더 좋다.
		- vector 내에 typedef된 멤버 형식을 사용
		- `for(vector<int>::size_type i = 0; i < v.size(); i++)`
	- size_type: vector의 크기, 첨자 형식을 위한 typedef 멤버 형식이다.

- vector 크기 반환 함수
	- size(): 저장 원소 개수
	- capacity(): 실제 할당된 메모리 공간 크기
		- vector만 유일하게 가지는 멤버함수
	- max_size(): 컨테이너가 담을 수 있는 최대 원소의 개수

- vector는 원소를 컨테이너에 계속 추가할 수 있다.
	1. 초기 할당된 메모리에 원소를 저장한다.
	2. 할당된 메모리가 다 차게되면 이전 메모리 크기의 1/2만큼을 새로 할당한다.
	3. 이전 데이터를 새로 할당한 주소에 복사하고 할당 해제한다.
		- 이건 컴파일러마다 조금씩 다름
- reserve(): 미리 메모리를 할당할 수 있는 메모리 예약 함수
	- 원소가 계속 삽입될 때마다 메모리를 할당 복사 삭제를 반복하게 된다.
	- 하지만 넉넉하게 메모리를 할당할 경우 해당 작업이 조금 덜 일어난다.
	- 따라서, 미리 메모리를 할당할 수 있는 예약 함수를 제공

- resize(): 컨테이너의 size를 변경
	- resize(size, initnum): size로 크기 변경, 확장될 경우 해당 원소들에 initnum을 저장
	- size를 키우면 capacity도 늘어난다.
	- <u>**size를 줄이면 capacity는 줄지 않는다.**</u>

- capacity를 0으로 만드는 방법?
	- swap을 이용한다.
		```c++
		#include<iostream>
		#include<vector>
		using namespace std;

		int main() {
			vector<int> v(5, 10);

			cout << "size: " << v.size() << " capacity: " << v.capacity() << endl;

			vector<int>().swap(v);

			cout << "size: " << v.size() << " capacity: " << v.capacity() << endl;

			return 0;
		}
		```
	- 임시객체를 생성해 swap함수와 v를 바꾸어 capacity를 0으로 만든다.

- swap(): 두 컨테이너의 원소를 교환
- front(): 첫 번째 원소 참조
- back(): 마지막 원소 참조
	- front와 back은 참조이므로 수정이 가능.

- 임의 위치 원소 참조
	- [ ]: <u>범위 점검을 하지 않아</u> 속도가 빠름
	- at(): <u>범위를 점검하여</u> 속도가 느리지만 안전함
	- 위 두개 모두 기능이 같다.

- assign(): 일관적으로 값을 할당
	- 모든 시퀀스 컨테이너가 이 메소드를 제공
	- assgin(n, x): n개의 원소에 x값을 할당
	- 구간으로 할당이 가능하다.
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

				vector<int> v2(v.begin(), v.end()); //1.

				vector<int> v3;
				v3.assign(v.begin(), v.end()); //2.

				return 0;
			}
			```
			1. 생성자가 반복자를 통해 초기화가 됨.
			2. assgin 함수를 통해 구간 할당됨.

- 컨테이너는 모든 원소의 시작과 끝을 가리키는 반복자 쌍을 begin()과 end()멤버 함수로 제공
	- 컨테이너의 모든 원소는 [begin(), end())로 표현 가능함

- 임의 접근 반복자
	- +, -, +=, -=, [] 연산 가능
	- ++iter: 다음 원소로 이동
	- iter += 2: 현재 위치부터 2 다음의 원소를 가리킴
	- iter -= 1: 현재 위치부터 1 이전의 원소를 가리킴
	- *iter: 현재 가리키는 원소를 참조
- 만일 반복자가 가리키는 원소의 값을 변경하지 않는다면
	- const_iterator(상수 반복자)를 사용하는 것이 좋다.


- **const 반복자 비교**
	- `vector<int>::iterator iter`
		- 다음 원소로 이동 **가능**
		- 원소의 변경이 **가능**
	- `vector<int>::const_iterator citer`
		- 다음 원소로 이동 **가능**
		- 원소의 변경이 **불가능**
	- `const vector<int>::iterator iter_const`
		- 다음 원소로 이동이 **불가능**
		- 원소의 변경이 **가능**
	- `const vector<int>::const_iterator citer_const`
		- 다음 원소로 이동이 **불가능**
		- 원소의 변경이 **불가능**
		
- 역방향 반복자
	- `vector<int>::reverse_iterator riter`
	- 처음과 끝 구간: [v.rbegin(), r.rend())
	- 정방향 반복자와 반대로 흘러감

	
- insert(): 반복자가 가리키는 위치에 원소 값을 추가
	- 삽입 시 기존에 있던 원소는 뒤로 밀림
	- 삽입을 수행하고 현재 위치의 반복자를 반환함
	- 반복자 쌍(구간)을 이용하여 원소를 삽입할 수 있다.
		- 사용 예제
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

				vector<int>::iterator iter = v.begin() + 2;

				v.insert(iter, 3, 100); //1.

				vector<int> v2;

				v2.push_back(100);
				v2.push_back(200);
				v2.push_back(300);

				iter = v2.begin() + 1;
				v2.insert(iter, v.begin(), v.end()); //2.

				return 0;
			}
			```
			1. 반복자가 가리키는 곳부터 원소 3개를 100으로 채운다.
			2. 반복자가 가리키는 곳부터 구간[v.begin(), v,end()) 원소들을 삽입한다.
- erase(): 반복자가 가리키는 위치의 원소 제거
	- 제거 시 삭제 한 원소의 다음 원소가 앞으로 당겨짐
	- 삭제를 수행하고 다음 원소의 위치를 반환함
	- 반복자 쌍(구간)을 이용하여 원소를 삭제할 수 있다.
		- 사용 예제
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

				vector<int>::iterator iter = v.begin() + 2;
				vector<int>::iterator iter2;

				iter2 = v.erase(iter); //1.

				iter2 = v.erase(v.begin() + 1, v.end()); //2.

				return 0;
			}
			```
			1. 반복자가 가리키는 곳의 원소를 삭제하고 다음 원소를 가리키는 반복자를 반환한다.
			2. 반복자가 가리키는 곳부터 구간[v.begin() + 1, v,end()) 원소들을 제거한다.

- 비교 연산
	- v1 == v2 : v1과 v2의 ***모든 원소가 같다면 참***, 아니면 거짓을 반환
	- v1 != v2 : v1과 v2의 ***모든 원소가 같다면 거짓***, 아니면 참을 반환
	- v1 < v2 : v1과 v2의 순차열의 원소를 ***하나씩 순서대로 비교***, v2 원소의 크다면 참, 아니면 거짓을 반환

### 특징 정리
- 임의 접근 반복자를 지원하는 배열 기반 컨테이너이다.
- 원소가 하나의 메모리 블록에 연속으로 저장된다.
- 메모리 할당 크기를 알 수 있는 capacity() 메소드를 제공
- 한번에 메모리를 예약할 수 있는 reserve() 메소드를 제공
- insert, erase, push_back 등이 빈번하게 호출 될 경우 다른 컨테이너를 고려해야 함
- 시퀀스 기반 컨테이너이다.
	- 하지만 push_front(), pop_fron()는 제공하지 않는다.
- index 정수로 빠르게 접근하도록 at(), []연산자를 제공
	- [ ]: 범위 점검을 하지 않아 속도가 빠름
	- at(): 범위를 점검하여 속도가 느리지만 안전함

# deque 컨테이너
vector와 다른 점
- 메모리 블록 할당 정책
	- vector는 원소가 추가될 때마다 메모리 재할당과 이전 원소 복사의 반복이었다.
		- 성능이 크게 저하가 되는 문제를 발생시켰다.
	- deque는 여러 개의 메모리 블록을 할당을 하고 하나의 블록처럼 보이게 한다.
		- 원소의 추가 시 메모리가 부족할 때마다 일정 크기의 새로운 메모리 블럭을 할당한다.
		- 이전 메모리를 제거하거나 이전 원소를 복사하는 등의 연산을 수행하지 않는다.

- deque는 앞쪽 원소를 추가/제거할 수 있다.
	- push_front(), push_pack() 삽입 메소드 제공
	- 원소를 앞쪽에 추가하면 새로운 메모리 블록을 할당하고 원소를 저장한다.

vector와 같은 점
- 배열 기반 컨테이너이다.
	- 임의 접근 반복자를 지원한다.
	- +, -, +=, -=, [] 연산을 모두 수행 가능
- 새로운 원소를 순차열 중간에 insert() 및 erase()가 가능하다.
	- 다만 벡터와 다르게 원소의 개수가 **작은 쪽**으로 밀어낸다.

### 특징 정리
- 임의 접근 반복자를 지원하는 배열 기반 컨테이너이다.
- 여러 메모리 블록에 나뉘어 저장된다.
- 원소를 앞쪽과 뒤쪽 모두 추가 가능
- vector보다 효율적이다.
- 시퀀스 기반 컨테이너이다.
- index 정수로 빠르게 접근하도록 at(), []연산자를 제공
	- [ ]: 범위 점검을 하지 않아 속도가 빠름
	- at(): 범위를 점검하여 속도가 느리지만 안전함
