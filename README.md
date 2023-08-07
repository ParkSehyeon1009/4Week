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

