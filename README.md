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
    - [ ] 내가생각하는 개발자의 삶은?
    - [ ] 1-3 캐릭터와 에너미 알아보기 
  - Today I Learned
    - c++ 캐릭터와 에너미 구조 설계 학습 2
    - [ hicpp-multiway-paths-covered ]- 코드 경로 관련 오류
    - [포인터] - 멤버 접근 연산자 '->' 학습
    - 여러 버그가 발생 내일 튜터님께 문의
   
      <details>
      <summary> [ hicpp-multiway-paths-covered ]- 코드 경로 관련 오류 </summary>
      - 위 경고는 C++에서 switch문 또는 다중 if-else문을 사용할 때, 모든 가능한 분기(코드 경로)가 명확하게 처리되지 않아 발생합니다.   
      - 특히, switch문에 default 라벨이 없거나, if-else-if 체인에 마지막 else가 없는 경우에 뜨는 경고입니다.

      ### 왜 이 경고가 중요한가?

      * 특정 값(예: enum의 새로운 값, 입력 데이터의 예상외 값 등)이 들어왔을 때 아예 코드가 실행되지 않을 수 있으므로, 논리적 오류 또는 예기치 않은 동작의 원인이 되기 때문입니다.  
      * High Integrity C++(HICPP) 코딩 표준에서는 모든 코드 경로가 확실하게 다루어져야 한다고 권장합니다.  
      * 일부 경우는 컴파일러 자체 경고로도 전달될 수 있지만, clang-tidy의 hicpp-multiway-paths-covered 체크는 코드의 잠재적 위험을 더 엄격하게 잡아냅니다
      
      ### 해결 방법
      
      * switch문에는 항상 default 라벨을 추가하세요. 만약 실제로 아무 것도 할 필요 없다면, 명시적으로 주석을 추가합니다.
        ```C++
        switch(color)
        { 
          case RED: /* ... */ break;
          case GREEN: /* ... */ break;
          default:
            // 다른 값에 대해서는 아무 처리도 안 함
            break;
        }
        '''
      
       * if-else-if문에도 마지막 else를 추가하세요. 아무 처리도 필요 없다면 빈 블록과 주석을 넣어 의도를 명확히 하면 됩니다.
         ```C++
         if (x > 0) { /* ... */ }
         else if (x < 0) { /* ... */ }
         else { 
           // 0일 때는 특별히 할 것이 없음`
         }
         '''
       </details>

      <details>
      <summary> [포인터] - 멤버 접근 연산자 '->' 학습 </summary>
       
      ### 1. '->' 연산자란?

      * '->'는 포인터가 가리키는 객체(object)의 멤버에 접근하는 **멤버 접근 연산자(member access operator)**입니다.  
      * 즉, 포인터 변수가 어느 객체를 가리키고 있을 때, 그 객체의 멤버 함수나 멤버 변수를 사용하고자 할 때 씁니다.
       
      ### 2. 왜 쓰이는가?
       
       `Enemy* enemy;  // 'enemy'는 Enemy 객체를 가리키는 포인터 변수`  
       `enemy->GetPosition();  // enemy가 가리키는 객체의 GetPosition() 멤버 함수 호출`

      * `enemy`는 객체 자체가 아니라 **객체의 주소값을 저장하는 포인터**입니다.  
      * 포인터 변수에 대해 멤버에 접근하려면, 아래 둘 중 하나여야 합니다:  
        * 직접 객체인 경우: '.' 연산자 사용 (ex: enemy.GetPosition())  
        * 포인터인 경우: '->' 연산자 사용 (ex: enemy-\>GetPosition())

      ### 3. 내부 동작 방식
      
      * `enemy->GetPosition()`은 사실상 `(*enemy).GetPosition()`의 축약형입니다.  
      * 이것은 먼저 포인터 `enemy`가 가리키는 메모리 주소에 있는 실제 객체를 역참조(dereference)하여 `*enemy`를 얻고,  
        여기서 `GetPosition()` 멤버 함수를 호출하는 의미입니다.
      
      ### 4. '->' 연산자의 역할
      
      * 포인터 역참조 + 멤버 함수(또는 변수) 호출의 결합 
      * 포인터가 가리키는 객체의 멤버에 바로 접근 가능하게 하여 코드 가독성 향상  
      * 만약 '.' 연산자를 쓰려면 객체여야 하기 때문에, 포인터 객체의 멤버 접근에는 반드시 '->'를 써야 함
      
      ### 5. 전체 표현 분석
      
      `enemy->GetPosition() == character->GetPosition() + 1`
      
      * `enemy`와 `character`는 아마 `Enemy*`, `Character*` 같은 포인터 변수입니다.  
      * 각각 해당 객체가 저장된 메모리 주소를 저장하고 있음.  
      * `enemy->GetPosition()` 은 enemy가 가리키는 객체의 위치 값을 반환.  
      * `character->GetPosition()` 은 character가 가리키는 객체 위치 값을 반환.  
      * 후자는 1을 더해서 두 위치를 비교하는 코드.
      
      ### 6. 요약
      
      | 연산자 | 용도 | 예시 | 의미 |
      | :---- | :---- | :---- | :---- |
      | -> | 포인터가 가리키는 객체 멤버 접근 | `ptr->member` | `(*ptr).member` 와 같음 |
      | . | 객체 자체의 멤버 접근 | `obj.member` | 객체 변수 내 멤버에 접근 |
      
      결론적으로, '->'는 포인터 변수로 객체를 가리키면서 그 객체의 멤버 함수나 변수를 호출/접근할 때 사용하는 표준 C++ 연산자입니다. 이는 포인터 역참조(*)와 멤버 접근(.)를 간결하게 표현한 형태입니다.
</details>

<details>
<summary> 25.08.05 </summary>

  - To Do List
    - [ ] 새로운 레벨 구성
    - [ ] 1-3 캐릭터와 에너미 알아보기 
  - Today I Learned
    - c++ 캐릭터와 에너미 구조 설계 복습
    - 여러 버그가 발생... 아직 해결중....
   
      
</details>


      


