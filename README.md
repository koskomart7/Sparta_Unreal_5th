# Sparta_Unreal_5th
Fortress of Solitude

### 25.07.28
- To Do List
  - [ ] github 정리 및 블로그용 레포지토리 생성
  - [ ] 사전강의 C++ 언어로 게임 만들기 / 라이더 생태계 적응 
- Today I Learned
```C++
//Actor.h
#pragma once  // 한번만 해석한다 
#include <string>
using namespace std;

class Actor
{
public:
    Actor(int hp, int ad, string name);
    void SetPosition(int position);  // 1차원 x축만 사용  *void : 함수의 기능만 실행하고 반환 값이 없다 -> 위치만 설정
    int GetPosition();             // 메소드 (멤버함수) : 함수들중에서 클래스 안에 속한 함수
    int GetHP();
    virtual void Damage(int damage);  // 가상함수 : 상속된 자식이 새로운 내용을 재구성 할 수 있다 
    virtual void Attack(Actor* hit);  // 포인터 : 변수의 주소를 가리킨다
/* 포인터 학습
 A 아파트
 101호 가
 102호 나

 B 아파트
 101호 : A아파트 101호 (주소)
 102호 : B아파트 102호 (주소)
 */
    virtual void Move(bool front);
    ~Actor();                   // ~ 소멸자 : 액터가 삭제될때 실행되는 내용 

protected:    // 상속받은 자식과 부모 자신만 쓸수있다 
    string Name;        // 변수 내용
    int HP;
    int AD;
    int Position;
};

//Actor.cpp
#include "Actor.h"
#include <iostream>
using namespace std;

Actor::Actor(int hp, int ad, string name)   //Actor 클래스의 Actor를 구현한다 
{
    HP = hp;
    AD = ad;
    Name = name;
    Position = 0;
}

void Actor::SetPosition(int position)
{
    Position = position;
}

int Actor::GetPosition()
{
    return Position;    
}

int Actor::GetHP()
{
    return HP;
}

void Actor::Damage(int damage)
{
    HP -= damage;
}

void Actor::Attack(Actor* hit)
{
    hit->Damage(AD);        // 포인터 사용시 (*hit).Damage(AD); -> hit가 가리키고 있는 대상에게 대미지 적용
                            // 위 포인터 기능을 '->' 화살표로 간소화 
}

void Actor::Move(bool front)
{
    if (front)
    {
        Position += 1;
    }
    else
    {
        Position -= 1;
    }
}

Actor::~Actor()
{
    cout<<Name<<"죽다\n";
}
