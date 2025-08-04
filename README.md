# Sparta_Unreal_5th
Fortress of Solitude
 
<details>
<summary> 25.07.28 </summary>

  - To Do List
    - [ ] github 정리 및 블로그용 레포지토리 생성
    - [ ] 사전강의 C++ 언어로 게임 만들기 / 라이더 생태계 적응 
  - Today I Learned
    - 전반적인 기초 재점검
    - <details>
      <summary> endl 과 \n 사용 경고 문구 </summary>
      C++ 코드에서 다음과 같이 작성하신 경우
      cout << Name << "죽다" << endl;

      Clang-Tidy 등의 정적 분석 도구에서
      'Do not use 'std::endl' with streams; use '\n' instead [performance-avoid-endl]' 라는 경고가 나타나는 이유는
      
      _**std::endl이 줄바꿈과 함께 출력 버퍼를 강제로 flush(비우기)하기 때문입니다. 빈번한 flush는 성능 저하를 유발할 수 있습니다.**_
      

      해결 방법
      std::endl 대신 '\n' 문자 리터럴을 사용하며, 코드를 다음과 같이 변경해 주세요:

      ```cpp
      cout << Name << "죽다\n";
      ```
      '\n'은 줄바꿈만 수행하고, 강제 flush는 발생시키지 않습니다. 따라서 성능상 더 효율적입니다.

      요약
      
      << endl → << '\n'으로 변경하면 경고가 사라지고 성능이 향상됩니다.

      만약 정말로 출력 버퍼를 즉시 비워야 할 필요가 있다면 endl을 사용하세요. 대부분의 경우는 '\n'이 적합합니다.
      </details>
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
        virtual void Damaged(int damage);  // 가상함수 : 상속된 자식이 새로운 내용을 재구성 할 수 있다 
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
    
    void Actor::Damaged(int damage)
    {
        HP -= damage;
    }
    
    void Actor::Attack(Actor* hit)
    {
        hit->Damaged(AD);        // 포인터 사용시 (*hit).Damage(AD); -> hit가 가리키고 있는 대상에게 대미지 적용
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
    '''
</details>
    
<details>
<summary> 25.07.29 </summary>

  - To Do List
    - [ ] 조모상 ( ~ 7/31 )
    - [ ] 1-3 캐릭터와 에너미 알아보기 
  - Today I Learned
</details>

<details>
<summary> 25.08.01 </summary>

  - To Do List
    - [ ] 본캠프준비
    - [ ] 1-3 캐릭터와 에너미 알아보기 
  - Today I Learned
    - c++ 캐릭터와 에너미 구조 설계 학습 1

</details>

<details>
<summary> 25.08.04 </summary>

  - To Do List
    - [ ] 본캠프준비
    - [ ] 1-3 캐릭터와 에너미 알아보기 
  - Today I Learned
    - c++ 캐릭터와 에너미 구조 설계 학습 2

</details>

