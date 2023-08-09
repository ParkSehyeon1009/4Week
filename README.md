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

**프렌드의 도입**

**오버로딩 연산자 : 멤버 함수와 멤버가 아닌 함수**

**오버로딩 보충 : Vector 클래스**

**자동 변환과 클래스의 데이터형 변환**
