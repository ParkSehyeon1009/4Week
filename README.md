# 4Week
# 10장 객체와 클래스

OOP의 중요한 기능 5가지는 아래와 같다.
1. 추상화
2. 캡슐화 와 데이터 은닉
3. 다형
4. 상속
5. 코드의 재활용

클래스는 위와 같은 OOP 기능들을 구현하고 결합하는 데 사용되는 가장 중요한 도구이다.

<br> **추상화와 클래스** <br>
<br>
컴퓨터 분야의 추상화 : 정보를 사용자 인터페이스로 표현하는 것
<br>
아래의 코드는 추상화 예제이다
```cpp
#include <iostream>
#include <vector>
using namespace std;

class Shape{
  public:
    virtual void Draw() {cout << "Shape::Draw" << endl;}
};

class Rect : public Shape{
  public:
    virtual void Draw90 {cout << "Rect::Draw" << endl;}
};

class Circle : public Shape{
  public:
    virtual void Draw() {cout << "Circle::Draw" << endl;}
};

void main(){
  vector<shape*> v;
  while(true){
    int cmd;
    cin >> cmd;
    if(cmd == 1) v.push_back(new Rect);
    else if(cmd == 2) v.push_back(new Circle);
    else if(cmd == 9) {
      for(auto p : v)
        p->Draw();
    }
  }
}
```
데이터형을 서술하는것은 아래와 같은 의미를 가진다
1. 데이터 객체에 필요한 메모리의 크기를 결정한다.
2. 메모리에 있는 비트들을 어떻게 해석할 것인지 결정한다 (예 long형과 float형의 비트 수가 같아도 값이 다름)
3. 데이터 객체를 사용하여 수행할 수 있는 연산이나 메서드를 결정

클래스는 추상화를 사용자 정의 데이터형으로 변환해 주는 C++의 수단이다.<br>
클래스의 서술은 아래와 같이 두가지 분류로 구분된다.<br>
1. 클래스 선언 : 데이터 멤버와 public 인터페이스, 멤버 함수를 이용하여 데이터 표현을 서술한다. (클래스 개요 제공)
2. 클래스 메서드 정의 : 클래스 멤버 함수가 어떻게 구현 되는지를 서술한다. (세부 사항 제공)
아래는 예제이다.
```cpp
class Sampple //클래스 선언 부분
{
  private: // pbulic 멤버 함수를 통해서만 접근 가능한 멤버들
    int int_Sample;
  public:
    float float_Sample;
}
```
만약 클래스 멤버가 private 인지 public 인지 명시하지 않았다면 defult 값은 private이다.<br>
<br>
멤버 함수 정의는 리턴형과 매개변수를 가질수 있다. 그들은 아래와 같은 두가지 특성을 가지고 있다.
1. 멤버 함수를 정의할 때 그 멤버 함수가 어느 클래스에 속하는지를 나타내기 위해 사용 범위 결정 연산자를 사용해야 한다.
2. 클래스 메서드는 그 클래스의 private 부분에만 접근할수 있다.

아래는 1번 특징의 예시이다.
```cpp
void Stock::update()
//Stock 클래스의 update() 함수를 정의한다는 뜻이다.
void Buffoon::update()
//Buffon 클래스의 update() 함수를 정의한다는 뜻이다.
위 처럼 사용 범위 결정 연산자(::) 를 통해 어떤 클래스에 속한 함수를 정의할것인지 지정할수 있다.
```
또한 2번 특징은 Stock 클래스의 private 멤버가 a,b,c 만 있다고 가정할때<br>
클래스 메서드 부분은 a,b,c 아닌 멤버들이 접근한다면 컴파일러가 즉각 멈추게 한다.
<br>
<br> **클래스 생성자와 파괴자** <br>

생성자의 이름은 클래스의 이름과 동일해야한다.<br>
예를들어 클래스의 이름이 Sample 라면 생성자의 이름도 Sample 이어야 한다.<br>
생성자와 멤버함수와의 차이점은 반환형의 유무이다.<br>
```cpp
class Sample{
  int test;
  Sample(int a){ // Sample의 생성자
    test = a;
  }
}
```
생성자 중에는 디폴트 생성자라는 생성자가 있다.<br>
이 생성자는 다른 생성자와 다르게 파라미터가 없다.<br>
```cpp
class Sample
{
  private:
    int test;
  public:
    Sample(){
    }
};
```
일반 생성자와 동일하지만 정의를 안하면 컴파일러가 알아서 해준다는 것이 차이점이다.<br>
<br>
파괴자는 생성자의 반대이다.<br>
객체의 수명이 끝나는 시점에서 파괴자를 자동으로 호출한다.<br>
파괴자와 생성자의 차이점은 파괴자는 매개변수를 가질수 없다는 점이다.<br>
```cpp
class Sample
{
  pivate:
    int test;
  public:
    ~Sample(){ //파괴자
    }
}
```
파괴자가 호출되는 시점은 아래와 같다.
1. 스코프를 벗어났을때
2. new를 사용하여 객체를 생성하고 delete 하였을때
3. 임시 객체를 생성했을 경우에 프로그램이 객체의 사용을 마쳤을때

파괴자가 필요한 이유는 메모리누수가 발생하지 않게 하기 위해서이다.
<br>
<br> **this 포인터** <br>
<br>
this 포인터는 클래스의 실제 인스턴스에 대한 주소를 가르키는 포인터이다.<br>
아래는 예시이다.
```cpp
class Sample
{
  private:
    int int_sample;
  public:
    Sample(int num)
    {
      int_sample = num;
    }
    void Print()
    {
      cout << int_sample << endl;
    }
}
void main()
{
  Sample sam(10);
  sam.Print();
}
```
결과값은 아래와 같다.
```
10
```
우리가 주목해야할 점은 sam.Print() 이다.<br>
이 코드는 인수 없이 함수를 호출하는것처럼 보이지만 sam.Print(&sam)처럼 존재하고 있는것이다.<br>
즉 매개변수로 전달된 인스턴스의 주소 이름이 this인 포인터 변수에 저장되는 것이다.<br>
this 포인터를 사용하는 경우는 아래와 같다.<br>
1. 클래스의 멤버 변수와 매개 변수가 동일한 경우
2. 객체 자신의 주소를 리턴할 경우

단 this 포인터는 static 멤버 함수에서 사용이 불가능한다.<br>
(static 함수에서 this 포인터를 사용할수 없는 이유는 static함수는 객체가 없어도 생성이 되어 있을 수 있기 때문이다.)<br>
<br>
<br> **객체배열** <br>
<br>
일반 자료형도 배열을 사용하듯 객체 또한 배열로 만들어 사용할수 있다.
```
클래스명 객체명[]
```
아래는 예제이다
```cpp
class Sample(){
  private:
    int a;
  public:
    Sample()
    {
      a = 0;
    }
    void Set(int num){a = num}
    void Set() {cout << a << endl;}
}

void main()
{
  Sample Sam[3];
  for(int i = 0; i < 3; i++)
  {
    Sam[i].Set(i);
    Sam[i].Set();
  }
  
}
```
결과값은 아래와 같다.
```
1
2
3
```
<br> **클래스 사용 범위** <br>
<br>
클래스 사용 범위는 클래스 데이터 멤버들의 이름이나 클래스 멤버 함수들의 이름과 같이<br>
클래스 안에서 정의되는 이름들에 적용된다.<br>
아래는 예시이다
```cpp
class Sample
{
  private:
    int A; // A는 클래스 사용 범위를 가진다
  public:
    Sample(int f = 9) {A = f;} // A가 사용범위 안에 있다.
}
void Sample::ViewSample() const // Sample::가 ViewSample 를 사용범위 안에 넣는다. 
{
  cout << A << endl; // 클래스 메서드 안이므로 A는 사용범위
}
void main()
{
  Sample * pik = new Sample 
  Sample ee = Sample(8) // 생성자는 클래스 이름을 갖기 때문에 사용 범위
  ee.ViewSample(); // 클래스 객체가 ViewSample를 사용 범위에 넣음
  pik->ViewSample() // Sample을 지시하는 포인터가 ViewSample를 사용 범위에 넣음
}
```
또한 아래와 같은 예시가 있다.
```cpp
class Sample
{
  private:
    const int A = 12;
}
```
위의 코드는 동작하지 않는다. 이유는 클래스를 선언하는 것은 객체가 어떻게 생겨있는가를 서술하는것이다.<br>
즉 그 객체를 생성하는것은 아니기에 객체가 생성될 때까지 기억공간은 마련되지 않는다.<br>
그래서 C++은 static 키워드를 사용해 클래스 안에 상수를 정의하는 방법을 제공한다.
```cpp
class Sample
{
  private:
    static const int A = 12;
}
```
위에 처럼 코드를 작성하면 정적 변수들과 함께 저장되는 A라는 하나의 상수를 생성해준다.
<br> **추상화 데이터형** <br>
추상화 데이터형은 기능의 구현 부분을 나타내지 않고 순수한 기능이 무엇인지 나열한것을 뜻한다.<br>
예시로 전자제품 사용설명서 같은것이다. 어떤 버튼을 누르면 기능이 실행되고 두개를 동시에 누르면 특정 작업이 실행되는등<br>
그 제품의 기능이 무엇인지를 설명해주지만 버튼을 누를시 내부회로에서는 어떤 동작을 하는지는 설명을 안해준다.<br>
추상화 데이터형은 사용설명서와 같이 기능과 사용 방법을 정의한 것이다.<br>
아래는 예시이다
```cpp
class Student()
{
  string name;
  int Number;
  int grade;
  int class;

  Student(String name, int Numner, int grade, int class)
  {
    this->name = name;
    this->Number = Number;
    this->grade = grade;
    this->class = class;
  }
}
```
# 11장 클래스의 활용
**연산자 오버로딩**
먼저 예시를 살펴보겠다.
```cpp
class NUMBOX
{
private:
	int num1, num2;
public:
	NUMBOX(int num1, int num2) : num1(num1), num2(num2) { }
	void ShowNumber() 
	{
		cout << "num1: " << num1 << ", num2: " << num2 << endl;
	}
};

int main()
{
	NUMBOX nb1(10, 20);
	NUMBOX nb2(5, 2);
	NUMBOX result = nb1 + nb2;

	nb1.ShowNumber();
	nb2.ShowNumber();
}
```
코드를 실행시키면 아래와 같은 에러가 발생한다
```
IntelliSense: 이러한 피연산자와 일치하는 "+" 연산자가 없습니다.
```
아래와 같이 코드를 작성하면 위와 같은 에러를 지울수 있다.
```cpp
class NUMBOX
{
private:
	int num1, num2;
public:
	NUMBOX(int num1, int num2) : num1(num1), num2(num2) { }
	void ShowNumber() 
	{
		cout << "num1: " << num1 << ", num2: " << num2 << endl;
	}
	NUMBOX operator+(NUMBOX &ref)
	{
		return NUMBOX(num1+ref.num1, num2+ref.num2);
	}
};

int main()
{
	NUMBOX nb1(10, 20);
	NUMBOX nb2(5, 2);
	NUMBOX result = nb1 + nb2;

	nb1.ShowNumber();
	nb2.ShowNumber();
	result.ShowNumber();
}
```
우리가 살펴봐야할 부분은 바로 operator+ 이다.<br>
operator는 연산자 오버로딩을 사용하기 위해 필요한 함수이다. 사용 방법은 아래와 같다.<br>
```cpp
리턴 타입 operator(연산자) (연산자가 받는 인자)
```
**프렌드의 도입**
프렌드는 A클래스가 B클래스에 직접적으로 접근할수 있게 해주는 함수이다.<br>
사용 방법은 아래와 같다.
```cpp
friend 리턴값 프렌드함수명(파라미터)
{
  행위
}
```
프렌드를 사용한다면 C++의 장점인 캡슐화를 해치기 때문에 사용이 권장되진 않는다.
**오버로딩 보충 : Vector 클래스**
Vector를 사용하기 위해서는 일단 Vector를 include 해줘야 한다.
```cpp
#include <vector>
```
그후 Vector 를 선언시켜주면 된다
```cpp
vector<T> v;
```
벡터는 템플릿 클래스이기 때문에 벡터의 타입을 <T> 안에 적어주어야 한다.<br>
선언할때 크기의 한계가 정해진 배열의 한계를 극복한 것이 Vector 클래스이기 때문에 배열보다 훨씬 유용한 사용이 가능하다.<br>
```cpp
int main() {
	vector<int> v;
	cout << "벡터의 크기: " << v.size() << endl;
	cout << "벡터가 비어있나요? " << (v.empty() ? "true" : "false") << endl;
	v.push_back(1);
	v.push_back(2);
	v.push_back(3);
	v.push_back(4);
	v.push_back(5);
	cout << "벡터가 비어있나요? " << (v.empty() ? "true" : "false") << endl;
	cout << "벡터의 크기: " << v.size() << endl;
	cout << "0번째 원소: " << v[0] << endl; //이렇게 []로도 접근 가능하고
	cout << "3번째 원소: " << v.at(3) << endl; //이렇게 at() 함수로도 인덱스에 접근 가능
	for (int i = 0; i < v.size(); i++) {
		cout << v[i] << " ";
	}
	cout << endl;
	v.pop_back();
	for (int i = 0; i < v.size(); i++) {
		cout << v[i] << " "; //마지막 원소였던 5가 삭제된 것을 볼 수 있음
	}
	cout << endl;
	v.clear(); //벡터의 모든 원소 삭제
	cout << "벡터의 크기: " << v.size() << endl;
	cout << "벡터가 비어있나요? " << (v.empty() ? "true" : "false") << endl;
	cout << endl;
	return 0;
}
```
**자동 변환과 클래스의 데이터형 변환**
C++은 표준 자료형의 값이 다른 표준 자료형과 호환이 될 때 암시적 형 변환이 이루어진다.<br>
만약 자동 데이터형 변환을 못하게 막고싶다면 explicit라는 키워드를 사용하면 된다.<br>
```cpp
explicit Stonewt(double lbs)
```
혹은 사용자 정의 강제 데이터형 변환을 하고싶을 경우 변환 함수를 사용하면 된다.<br>
변환 함수 작성 방법은 아래와 같다.
```cpp
operator typeName();
```
변환함수는 아래와 같은 규칙이 있다.
```cpp
변환 함수는 클래스의 메서드여야 한다
변환 함수는 리턴형을 가지면 안 된다.
변환 함수는 매개변수를 가지면 안 된다.
```
