# Sparta_Unreal_5th
Fortress of Solitude

<details>
<summary> 템플릿 </summary>
 
- To Do List
    - [x]
    - [ ]

- Today I Learned
</details>

 
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
    - [x] 1-3 캐릭터와 에너미 알아보기 
  - Today I Learned
</details>

<details>
<summary> 25.08.01 </summary>

  - To Do List
    - [x] 본캠프준비
    - [x] 1-3 캐릭터와 에너미 알아보기 
  - Today I Learned
    - c++ 캐릭터와 에너미 구조 설계 학습 1

</details>

<details>
<summary> 25.08.04 </summary>

  - To Do List
    - [x] 내가생각하는 개발자의 삶은?
    - [x] 1-3 캐릭터와 에너미 알아보기 
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
    - [x] 새로운 레벨 구성
    - [x] 1-3 캐릭터와 에너미 알아보기 
  - Today I Learned
    - c++ 캐릭터와 에너미 구조 설계 복습
    - 여러 버그가 발생... 아직 해결중....
</details>

<details>
<summary> 25.08.06 </summary>

  - To Do List
    - [ ] 블루프린트 기초 로직 공부
    - [ ] 1-3 캐릭터와 에너미 마무리
  - Today I Learned
    - c++ 캐릭터와 에너미 구조 설계 복습
    - 라이더 언어 관련 깨짐 현상 디버그 방법 
    - 블루프린트 기초 로직 공부- 텍스트 슈팅게임 무기 추가 ( ref / copy )
    <details>
    <summary> 블루프린트 기초 로직 공부- 텍스트 슈팅게임 무기 추가 ( ref / copy ) </summary>
     
    #### Get a Copy (복사):
    구조체나 배열에서 값을 Get할 때 "Get a Copy"를 사용하면, 해당 위치의 데이터가 복사되어 반환됩니다.
    이 복사된 값은 원본과 별개이므로, 복사본을 변경해도 실제 원본 데이터에는 아무런 영향이 없습니다.
    예를 들어, Get a Copy로 구조체 데이터를 불러와 값을 변경해도, 원본 구조체는 그대로 유지됩니다.
    
    #### Get a Ref (참조):
    "Get a Ref"를 쓰면 해당 데이터의 **참조(Reference, 참조 주소)**가 반환됩니다.
    이 상태에서 값을 바꾸면 원본 데이터가 실제로 변경됩니다. 즉, 구조체의 멤버를 이 방식으로 바꿀 경우 구조체 변수 안의 실제 값이 함께 변경됩니다.
    
    #### 실제 사용 예:
    구조체 배열에서 특정 인덱스를 가져올 때 "Get a Copy"를 사용하면, 얻어온 값에 대한 조작은 원본 배열에는 영향이 없지만,
    "Get a Ref"로 가져오면 조작 결과가 원본 배열에도 반영됩니다.
    
    #### 블루프린트 핀 모양:
    참조(ref) 핀은 "마름모꼴" 모양이며, 변수 타입 옆에 (by ref)라는 설명이 붙습니다.
    복사(copy) 방식은 그냥 일반 핀으로 표시됩니다.
    
    #### 추가 설명:
    대부분의 블루프린트 변수 Get 노드는 기본적으로 "복사"로 작동합니다.
    "참조" 방식이 필요한 경우 함수 정의, 노드 옵션에서 (by ref)로 지정해야 하며, 설계 시 데이터 변경 목적에 따라 선택합니다.
    
    #### 요약:
    copy는 원본 값을 건드리지 않고, ref는 실제 원본 값을 변경할 수 있습니다.
    값 변경까지 원해야 할 때는 ref를, 값 확인/복사만 할 때는 copy를 사용합니다.

    #### 구조체를 이용한 텍스트 슈팅 게임 구현
    https://github.com/user-attachments/assets/c12d7e1a-92f8-4218-a8b0-2e3e8723da01


    </details>

    <details>
    <summary> 라이더 언어 관련 깨짐 현상 디버그 방법 </summary>
    라이더 로직 작성시 한글이 깨지는 현상 디버그
    시스템의 언어및 지역 -> 관련설정_기본언어설정 -> 시스템로캘변경 -> 세계언어지원을 위해 Unicode UTF-8 사용 체크
    </details>
</details>

<details>
<summary> 25.08.07 </summary>

  - To Do List
    - [x] 레벨디자인 제작    
  - Today I Learned
    - 블록아웃 구성 및 플러그인 활용
    
   <img width="990" height="650" alt="화면 캡처 2025-08-07 113313" src="https://github.com/user-attachments/assets/c313c91d-89b2-49c6-aa03-03b0aaff77b1" />

</details>

<details>
<summary> 25.08.08 </summary>

  - To Do List
    - [x] 레벨디자인에 인터렉션 게임 구성     
  - Today I Learned

    https://youtu.be/hWfCO5S9x1g?si=QmR7TpcE8yuBVrOr
</details>

<details>
<summary> 25.08.11 </summary>

  - To Do List
    - [x] 깃 학습
    - [x] 블루프린트 핵심 학습 _ Spline
    - [x] 챕터2 발제 정리 
  - Today I Learned
    - 버전 관리 툴 학습
    - Spline 응용 기술 학습
    - c++ 챕터2 발제 정리 

</details>

<details>
<summary> 25.08.12 </summary>

  - To Do List
    - [x] 1-4 언리얼 기본 예제로 공부 
    - [x] 블루프린트 핵심 학습 _ UMG
    - [x] C 언어 스터디 참가
  - Today I Learned
    - 언리얼 코드 구성 학습
    - UMG 구성 학습
    - c언어 기초 학습 - 로우 레벨
      <details>
      <summary> 1-4 언리얼 기본 예제로 공부 </summary> 
      
        ```c++
        #include "CoreMinimal.h"    // 기본적인 엔진의 기초 기능을 담고있다
        #include "Game1Character.generated.h"   // GENERATED_BODY() 를 사용하기 위함
              // generated.h 는 가장 마지막에 위치해야 작동한다 
         -> GENERATED_BODY() // 클래스의 정보를 언리얼로 인식 시켜주는 역할  
  
        class USpringArmComponent;
        class UCameraComponent;
        class UInputMappingContext;
        class UInputAction;
        // 복잡한 헤더파일의 참조 구조를 피하기 위해 선언만 해놓은 것 ( 전방선언 )
        struct FInputActionValue; // 구조체 -> 기본적으로 public으로 선언이 된다 
        
        UCLASS(config=Game) // 클래스의 정보를 언리얼에 알려주기 위한 것 ( 리플렉션 )
                // UPROPERTY 도 마찬가지 기능
        
        USpringArmComponent* CameraBoom;    // 객체를 포인터로 받는 이유 : 객체 자체를 가져오면 복사본이 되어 수정이 되지만
                                            //  포인터로 받아서 객체를 수정하기 위함
        
        void Move(const FInputActionValue& Value);  // const : 변하지 않는 상수값
            // FInputActionValue& Value -> & 뒤의 Value를 주소 안에 있는 값으로 받겠다
        
        virtual void SetupPlayerInputComponent(class UInputComponent* PlayerInputComponent) override;
            // class UInputComponent* : 전방선언 클래스
        
        FORCEINLINE class USpringArmComponent* GetCameraBoom() const { return CameraBoom; }
            // FORCEINLINE : 짧은 함수는 복사해서 그 위치에 붙여넣겠다는 뜻
            //  const : 해당 함수 내에서 변화를 일으키지 않겠다는 뜻
        ```
     </details>
</details>

<details>
<summary> 25.08.13 </summary>

  - To Do List
    - [x] c++ 문법 강의 진행 1주차
    - [x] 블루프린트 핵심 학습 _ Tag 관련 
    - [x] C 언어 스터디 참가 
  - Today I Learned
    <details>
    <summary> Get all actors with Tag 함수에서 get(copy) 만 나오는 이유 </summary>
    언리얼 엔진의 메모리 관리 구조와 액터 객체의 특성에 기인합니다.

    ## **핵심 이유: 액터는 참조 타입이지만 포인터 복사가 발생**
    
    언리얼 엔진에서 **액터(Actor) 객체들은 실제로는 포인터로 관리**됩니다. 배열에 저장되는 것은 실제 액터 객체가 아니라 액터 객체를 가리키는 포인터입니다. 따라서 "Get (a copy)"라고 표시되지만, 실제로는 다음과 같이 작동합니다:
    
    * 포인터 자체는 복사되지만, 포인터가 가리키는 실제 액터 객체는 동일합니다  
    * 즉, 포인터의 복사본을 얻더라도 동일한 메모리 주소의 같은 액터를 참조합니다  
    * 결과적으로 복사된 포인터를 통해 액터를 수정하면 원본 액터에 변경사항이 반영됩니다
    
    ## **언리얼 엔진의 객체 메모리 관리 구조**
    
    ### **1\. UObject 기반 메모리 관리**
    
    언리얼의 모든 액터는 `UObject`를 상속받으며, 이는 가비지 컬렉션 시스템으로 관리됩니다. 액터 객체들은:
    
    * `GUObjectArray`라는 전역 배열에 저장됩니다  
    * 각 객체는 포인터 형태로 참조됩니다  
    * 가비지 컬렉터가 자동으로 메모리를 관리합니다
    
    ### **2\. 배열에서의 포인터 저장 방식**
    
    TArray(언리얼의 동적 배열)는 액터 객체를 다음과 같이 저장합니다:
    
    * 실제 액터 객체 데이터가 아닌 \*\*객체의 메모리 주소(포인터)\*\*를 저장  
    * 배열의 각 요소는 액터 객체를 가리키는 포인터
    
    ## **"Get (a copy)" vs "Get (a ref)"의 실제 의미**
    
    ### **일반적인 데이터 타입의 경우**
    
    * Get (a copy): 값 자체를 복사하여 독립적인 데이터 생성  
    * Get (a ref): 원본 데이터의 참조를 반환하여 직접 수정 가능
    
    ### **액터/UObject의 경우**
    
    액터나 UObject의 경우, 배열에 저장된 것이 이미 포인터이기 때문에:
    
    * "Get (a copy)"로 표시되더라도 포인터의 복사일 뿐  
    * 복사된 포인터도 동일한 액터 객체를 가리킴  
    * 따라서 실질적으로는 참조와 동일한 효과를 가집니다
    
    ## **왜 "Get (a ref)" 옵션이 없는가?**
    
    1. 이미 포인터 기반: 액터는 이미 포인터로 관리되므로 추가적인 참조 레이어가 불필요합니다  
    2. 메모리 안전성: 언리얼 엔진의 가비지 컬렉션 시스템과의 호환성을 위해 포인터 복사 방식을 사용합니다  
    3. 일관성 유지: 모든 UObject 파생 클래스(액터 포함)에 대해 동일한 메모리 관리 방식을 적용합니다
    
    ### **실제 사용에서의 영향**
    
    `cpp`
    
    *`// 블루프린트에서 Get All Actors with Tag 사용 시`*  
    *`// "Get (a copy)"로 액터를 가져와도`*  
    *`// 실제로는 같은 액터 객체를 참조하므로`*  
    *`// 액터의 속성 변경이 원본에 반영됩니다`*
    
    따라서 `Get All Actors with Tag`에서 "Copy"로만 표시되는 것은 기술적으로는 포인터의 복사를 의미하지만, 실용적으로는 원본 액터에 대한 접근을 제공하는 것입니다. 이는 언리얼 엔진의 메모리 관리 아키텍처가 객체를 포인터 기반으로 관리하기 때문입니다.
    </details>
</details>

<details>
<summary> 25.08.14 </summary>
 
- To Do List
    - [x] 블루프린트 핵심정리
    - [x] 스터디 자료 공부 조건문, 반복문
- Today I Learned
  <details>
  <summary> C언어에서 이진수 덧셈과 OR 비트연산의 차이점 </summary>
   
  ## **핵심 차이점**

  C언어에서 이진수 덧셈(`+`)과 OR 비트연산(`|`)의 가장 큰 차이점은 캐리(carry) 처리에 있습니다.
  
  ## **1\. 연산 방식의 차이**
  
  이진수 덧셈(`+` 연산자):
  
  * 각 비트 위치에서 캐리를 고려한 실제 산술 덧셈 수행  
  * 1 \+ 1 \= 10 (이진수)로 캐리가 다음 비트로 전파  
  * 수학적으로 정확한 덧셈 결과 생성
  
  OR 비트연산(`|` 연산자):
  
  * 각 비트 위치에서 논리적 OR 연산만 수행  
  * 1 | 1 \= 1로 캐리 없이 단순히 1 반환  
  * 비트 단위의 논리 연산으로 제한
  
  C언어에서 이진수 덧셈과 OR 비트연산의 연산 결과 비교
  
  ## **2\. 구체적인 예시**
  
  `c`
  
  *`// 예시 1: 캐리가 없는 경우`*  
  `int a = 5;  // 0101`  
  `int b = 2;  // 0010`  
  `printf("덧셈: %d\n", a + b);   // 결과: 7 (0111)`  
  `printf("OR: %d\n", a | b);     // 결과: 7 (0111)`  
  *`// 이 경우는 동일한 결과`*
  
  *`// 예시 2: 캐리가 발생하는 경우`*  
  `int a = 6;  // 0110`    
  `int b = 3;  // 0011`  
  `printf("덧셈: %d\n", a + b);   // 결과: 9 (1001)`  
  `printf("OR: %d\n", a | b);     // 결과: 7 (0111)`  
  *`// 이 경우는 다른 결과`*
  
  ## **3\. 비트 연산을 활용한 덧셈 구현**
  
  실제로는 XOR과 AND 연산의 조합으로 덧셈을 구현할 수 있습니다:
  
  `c`
  
  `int bitwise_add(int a, int b) {`  
      `while (b != 0) {`  
          `int sum = a ^ b;        // XOR로 비트 합 계산`  
          `int carry = (a & b) << 1;  // AND와 시프트로 캐리 계산`  
          `a = sum;`  
          `b = carry;`  
      `}`  
      `return a;`  
  `}`
  
  이 방법에서:
  
  * XOR(`^`): 캐리를 제외한 각 비트의 합 계산  
  * AND(`&`) \+ 왼쪽 시프트(`<<`): 캐리 계산 및 다음 자릿수로 이동
  
  ## **이렇게 나눈 이유**
  
  ## **1\. 하드웨어 설계의 효율성**
  
  * 각 비트 연산자는 CPU의 ALU(Arithmetic Logic Unit)에서 서로 다른 회로로 구현  
  * OR 연산은 단순한 논리 게이트로 고속 처리 가능  
  * 덧셈은 복잡한 가산기(adder) 회로 필요
  
  ## **2\. 프로그래밍 목적의 구분**
  
  * 비트 마스킹: OR 연산은 특정 비트를 1로 설정하는 용도  
  * 플래그 조작: 여러 상태를 비트로 관리할 때 OR 사용  
  * 산술 연산: 덧셈은 수치 계산이 목적
  
  ## **3\. 성능 최적화**
  
  * 비트 연산은 산술 연산보다 빠른 실행 속도  
  * 특정 상황에서 덧셈을 비트 연산으로 대체하여 성능 향상 가능  
  * 컴팩트한 데이터 표현과 메모리 사용량 절감
  
  ## **4\. 명확한 의도 표현**
  
  * 코드의 가독성과 유지보수성 향상  
  * 비트 조작 의도인지 산술 계산 의도인지 명확히 구분  
  * 다른 개발자가 코드의 목적을 쉽게 이해할 수 있도록 함
  
  결론적으로, C언어에서 이진수 덧셈과 OR 비트연산을 구분한 것은 하드웨어의 효율적 활용, 명확한 프로그래밍 의도 표현, 그리고 다양한 용도에 최적화된 연산 제공을 위함입니다.
  </details>
  
  <details>
  <summary> 반전연산자 '~' </summary>
  <img width="773" height="537" alt="image" src="https://github.com/user-attachments/assets/06ae85d0-ed81-44e5-a53a-87bb2b2bb548" />

  0 -> 1 / 1 -> 0 으로 값을 반전
  이후 나온 결과 값을 0으로 짜맞추기 위한 값을 구해서 역으로 계산
  </details>

  <details>
  <summary> i++은 내부적으로 어떻게 구현되었길래 다음 줄부터 1이 증가 할 수 있을까?</summary>
   
  ## **기본 원리**

  * `i++`는 후위 증가 연산자로, 현재 값은 그대로 사용하고, 그 뒤에 값을 1 증가시킵니다.  
  * 내부적으로는 보통 임시 변수(temporary variable)를 하나 생성해서 현재 값을 저장해 둔 후,  
  * 변수 `i`의 값을 1 증가시키고,  
  * 임시 변수에 저장해 둔 원래 값을 반환하는 식으로 동작합니다.
  
  즉, 처리 흐름은 다음과 같습니다:
  
  1. 현재 변수 `i`의 값을 임시 변수에 저장.  
  2. 변수 `i`를 1 증가 (`i = i + 1`).  
  3. 임시 변수(변경 전 값)를 연산 결과로 반환.
  
  ---
  
  ## **C++ 후위 증가 연산자 오버로딩 구현 예시**
  
  ```cpp
    class MyInt
    {
    private:
        int value; 
    public:
        MyInt(int v) : value(v) {}
    
        // 후위 증가 연산자 (int는 구분용 dummy)  
        MyInt operator++(int)
        {  
            MyInt temp = *this;  // 현재 상태 복사  
            value++;             // 내부 값 1 증가  
            return temp;         // 증가 전 값 반환  
        }
    
        int getValue() const { return value; }
    };
  ```
  
  이처럼 임시 객체 `temp`에 현재 상태를 복사해 두고, 값은 증가했지만 호출한 곳에는 이전 값이 반환되는 구조입니다.
  
  ---
  
  ## **참고**
  
  * 반면에 `++i`는 전위 증가 연산자로, 값을 먼저 1 증가시키고 그 값을 반환합니다.  
  * 그래서 `++i`는 임시 변수를 만들지 않고, 한 번만 작업하면 되지만,  
  * `i++`는 증가 전 값을 유지해야 하므로 임시 변수를 만들고 값을 두 번 다루기 때문에 성능상 약간 더 비용이 들 수 있습니다.
  
  ---
  
  즉, `i++`가 "다음 줄부터 1이 증가한 값이 된다"라는 것은 후위 연산자가 자신의 값을 복사해서 반환하고, 변수 자체는 먼저 증가했기 때문에 가능한 동작입니다.
  ```cpp
    int main() 
    {
        int i = 5;
        int a = i++; // 후위 증가
        int b = ++i; // 전위 증가
        cout << a << " " << b << " " << i << endl;
        return 0;
    }
     -> 5 7 7
  ```
  </details>

  <details>
  <summary> do - While 문 </summary>
   
  ## **게임 프로그래밍에서 do-while문 활용 예시**
  * 게임 메뉴 반복 출력  
    사용자가 메뉴에서 유효한 선택을 할 때까지 메뉴를 반복 출력하고, 잘못된 입력이 들어온 경우 다시 묻는 상황에 사용합니다.  
    (최소 한 번은 메뉴를 보여주기 때문에 do-while문 적합)  
  * 플레이어 입력 처리  
    행동 선택, 명령 입력, 점수 입력 등 사용자의 입력을 받고 검증하는 과정에서, 입력을 처음에도 받고 조건에 따라 반복 확인할 때 사용합니다.  
  * 게임 루프 내 특정 이벤트 반복 실행  
    예를 들어, 적 AI가 특정 행동을 할 때 여러 번 조건을 검사하면서 행동을 반복하는 경우, do-while을 쓰면 최소 한 번은 액션이 수행됩니다.  
  * 레이스 게임에서 랩 완료 여부 확인  
    한 랩을 완주할 때까지 반복하는 동작을 do-while로 구현할 수 있습니다.
  
  </details>

</details>

<details>
<summary> 25.08.18 </summary>
 
- To Do List
    - [x] 코드카타 오답 정리
    - [x] 스파르타 c 공부

- Today I Learned
  - 너무 매몰되어 공부하지 않기 - 막히고 정체되면 빠르게 넘기기 - c++ 정규 과정 진도 빼기
    <details>
    <summary> 최대 공약수 / 최소 공배수  </summary>
    <img width="600" height="594" alt="image" src="https://github.com/user-attachments/assets/cd32f129-db36-4233-a8c1-a754d1e053da" /> || <img width="599" height="593" alt="image" src="https://github.com/user-attachments/assets/a2c07e3d-0323-4599-97d2-43bbc9f59233" />
    </details>

    <details>
    <summary> 전처리 지시자에 관하여 </summary>
    전처리 지시자는 C/C++ 프로그래밍에서 컴파일하기 전에 실행되는 명령어로, 모두 `#` 기호로 시작합니다. 전처리기(Preprocessor)가 소스 코드를 컴파일러에 넘기기 전에 이 지시자들을 처리하여 코드를 변환합니다.

    ## **전처리 과정의 이해**
    
    C++ 소스코드가 실행 가능한 프로그램이 되기까지는 다음 순서를 거칩니다:
    
    1. 전처리(Preprocessing) \- 전처리 지시자 처리  
    2. 컴파일(Compile) \- 소스코드를 기계어로 번역  
    3. 링크(Link) \- 실행 파일 생성
    
    전처리기는 단순히 텍스트 치환과 조작을 수행하며, C++ 문법을 완전히 이해하지는 않습니다.
    
    ## **주요 전처리 지시자 종류**
    
    ## **1\. 파일 포함 지시자 (\#include)**
    
    가장 흔히 사용되는 지시자로, 다른 파일의 내용을 현재 위치에 복사해 넣습니다.
    
    `cpp`
    
    `#include <iostream>     // 시스템 헤더 파일`  
    `#include "myheader.h"   // 사용자 정의 헤더 파일`
    
    * `<>` 사용: 컴파일러가 제공하는 표준 헤더 파일  
    * `""` 사용: 사용자가 만든 헤더 파일
    
    ## **2\. 매크로 정의 지시자 (\#define, \#undef)**
    
    코드에서 특정 키워드를 다른 값으로 치환합니다.
    
    `cpp`
    
    `#define PI 3.14159          // 매크로 상수`  
    `#define MAX(a,b) ((a)>(b)?(a):(b))  // 매크로 함수`
    
    *`// 사용 예`*  
    `double area = PI * radius * radius;`  
    `int maximum = MAX(10, 20);`
    
    `#undef PI  // 매크로 정의 해제`
    
    매크로 함수의 특수 연산자:
    
    * `#` 연산자: 매개변수를 문자열로 변환  
    * `##` 연산자: 두 매개변수를 연결
    
    `cpp`
    
    `#define str(x) #x`  
    `#define combine(a,b) a ## b`
    
    `cout << str(Hello);        // "Hello" 출력`  
    `combine(std::c, out) << "test";  // std::cout << "test"`
    
    ## **3\. 조건부 컴파일 지시자**
    
    특정 조건에 따라 코드 블록을 포함하거나 제외합니다.
    
    기본 형태:
    
    `cpp`
    
    `#if 조건`  
        `// 조건이 참일 때 포함될 코드`  
    `#elif 다른조건`  
        `// 다른 조건이 참일 때 포함될 코드`  
    `#else`  
        `// 모든 조건이 거짓일 때 포함될 코드`  
    `#endif`
    
    정의 여부 확인:
    
    `cpp`
    
    `#ifdef DEBUG        // DEBUG가 정의되어 있다면`  
        `cout << "디버그 모드" << endl;`  
    `#endif`
    
    `#ifndef MAX         // MAX가 정의되어 있지 않다면`  
        `#define MAX 100`  
    `#endif`
    
    ## **4\. 기타 지시자**
    
    오류 발생 (\#error):
    
    `cpp`
    
    `#ifndef REQUIRED_MACRO`  
        `#error "REQUIRED_MACRO must be defined!"`  
    `#endif`
    
    라인 제어 (\#line):
    
    `cpp`
    
    `#line 100 "newfile.cpp"  // 라인 번호와 파일명 변경`
    
    컴파일러 제어 (\#pragma):
    
    `cpp`
    
    `#pragma once        // 헤더 파일 중복 포함 방지`  
    `#pragma warning(disable: 4996)  // 특정 경고 무시`
    
    ## **미리 정의된 매크로**
    
    컴파일러가 자동으로 정의하는 매크로들입니다:
    
    * `__FILE__`: 현재 파일명  
    * `__LINE__`: 현재 라인 번호  
    * `__FUNCTION__`: 현재 함수명  
    * `__DATE__`: 컴파일 날짜  
    * `__TIME__`: 컴파일 시간  
    * `__cplusplus`: C++ 버전
    
    ## **실용적인 사용 예시**
    
    헤더 가드 구현:
    
    `cpp`
    
    `#ifndef MYHEADER_H`  
    `#define MYHEADER_H`
    
    *`// 헤더 파일 내용`*
    
    `#endif`
    
    디버그/릴리즈 모드 구분:
    
    `cpp`
    
    `#ifdef DEBUG`  
        `#define DBG_PRINT(x) cout << x << endl`  
    `#else`  
        `#define DBG_PRINT(x)`  
    `#endif`
    
    플랫폼별 코드:
    
    `cpp`
    
    `#if defined(WINDOWS)`  
        `#include <windows.h>`  
    `#elif defined(LINUX)`  
        `#include <unistd.h>`  
    `#endif`
    
    ## **주의사항**
    
    1. 전처리 지시자는 세미콜론(`;`)으로 끝나지 않습니다  
    2. 복잡한 매크로는 가독성을 해칠 수 있습니다  
    3. 매크로 함수는 타입 검사가 없어 위험할 수 있습니다  
    4. 전처리기는 단순 텍스트 치환이므로 예상치 못한 결과가 발생할 수 있습니다
    
    전처리 지시자는 C/C++ 프로그래밍에서 코드의 유연성과 재사용성을 높이는 강력한 도구이지만, 적절히 사용해야 코드의 가독성과 유지보수성을 해치지 않습니다.
    
    # **2 전처리 지시자**
    
    대부분의 전처리 지시자들은 세 가지로 분류될 수 있다:
    
    * ***매크로 정의macro definition***. `#define` 지시자는 매크로를 정의해준다; `#undef` 지시자는 매크로 정의를 없애준다.  
    * ***파일 추가file inclusion***. `#include` 지시자는 특정 파일의 내용물을 프로그램에 추가해준다.  
    * ***조건적 컴파일conditional compilation***. `#if`, `#ifdef`, `#ifndef`, `#elif`, `#else`, `#endif` 지시자들은 전처리가 확인할 주어진 조건에 따라 코드의 특정 부분을 프로그램에서 추가하거나 제외해줄 수 있다.
    
    나머지 지시자들인 `#error`, `#line`, `#pragma`는 좀 더 특수한 목적을 갖고 있어 그리 자주 사용되지는 않는다. 앞으로 단원들에서 전처리 지시자들을 좀 더 깊게 다뤄보도록 하자. 여기서 다루지 않을 지시자는 15.2단원에서 다룰 `#include`이다.
    
    그럼 본격적으로 시작하기 전에, 모든 지시자들에 적용되는 몇 가지 규칙을 알아보도록 하자:
    
    * *지시자는 언제나 `#` 기호로 시작한다*. `#` 기호 이전에 공란문자가 있기만 하면 굳이 해당 줄을 반드시 `#` 기호로 시작할 필요는 없다. `#` 기호 이후에는 지시자의 이름과 해당 지시자가 필요로 하는 다른 정보들이 온다.  
    * *지시자의 토큰 사이에는 띄어쓰기와 수평탭이 임의의 개수만큼 들어갈 수 있다*. 즉, 다음 예시를 보자:  
      \#    **define**      N    (100)  
    * *지시자는 명시적으로 이어붙히지 않는 한 반드시 첫번째 개행문자로 끝나야한다*. 지시자를 다음 줄로 넘겨주려면 반드시 현재 줄을 `\` 문자로 끝내야한다. 예를 들어 다음 지시자는 하드 디스크를 바이트 단위의 용량으로 표현한 매크로를 정의해준다:  
    * \#**define** DISK\_CAPACITY (SIDES \*             \\  
    *                        TRACKS\_PER\_SIDE \*   \\  
    *                        SECTORS\_PER\_TRACK \* \\  
    *                        BYTES\_PER\_SECTOR)  
    * *지시자는 프로그램 어디서든 등장할 수 있다*. 주로 `#define`이나 `#include` 지시자를 파일의 시작부에 놓기는 하지만 다른 지시자들은 나중에, 심지어 함수 정의 중간에서도 나올 가능성이 더 높다.  
    * *주석은 지시자와 같은 줄에 나올 수도 있다*. 사실 매크로 정의 끝부분에 주석을 놓아서 매크로의 의미를 설명해주는 것이 좋은 코딩 습관이다:  
    * \#**define** FREEZING\_PT (32.0f)    / 물의 어는 점 /
    
    ---
    
    ## **\#define \_CRT\_SECURE\_NO\_WARNINGS의 의미와 목적**
    
    \*\*`#define _CRT_SECURE_NO_WARNINGS`\*\*는 Visual Studio에서 C/C++ 프로그래밍을 할 때, 오래된 '안전하지 않은 함수'(예: `scanf`, `gets`, `strcpy`, `fopen`)를 사용할 경우 발생하는 경고 메시지를 숨기거나 무시하기 위해 사용하는 전처리 지시자입니다.
    
    ## **왜 필요한가?**
    
    * Visual Studio 2005 이상에서는 코드의 보안과 안정성을 강조하기 위해 위 함수들을 사용할 때 경고(C4996 등)가 발생합니다.  
    * Visual Studio는 더 안전한 함수(예: `scanf_s`, `gets_s`, `strcpy_s`, `fopen_s`) 사용을 권장하고, 기존 함수를 계속 사용하면 경고를 보여줍니다.  
    * 기존 함수 사용이 꼭 필요하거나 강의/예제 등에서 경고 없이 실행하고 싶을 때 `#define _CRT_SECURE_NO_WARNINGS`를 코드 맨 윗부분에 작성하면 경고가 나오지 않습니다.
    
    ## **주의 사항**
    
    * 경고를 없앨뿐, 실제로 함수는 여전히 안전하지 않습니다(버퍼 오버플로 등 위험 존재).  
    * 상업적·실전 개발에서는 경고를 무시하기보다는 안전한 함수를 사용하는 것이 바람직합니다.  
    * Visual Studio에서만 적용되는 매크로입니다. 다른 컴파일러에서는 효과가 없습니다.
    
    ## **정리**
    
    * 주로 학습, 예제, 기존 코드 실행에서 많이 사용  
    * 코드 최상단에 작성해야 효과가 있습니다  
    * 보안 및 안정성을 위해 가급적 새로운 안전한 함수를 사용하는 습관을 갖는 것이 좋습니다
    </details>
</details>

<details>
<summary> 25.08.19 </summary>
 
- To Do List
    - [x] 알고리즘 복습 및 정리 ( 포인터, 참조자 )
    - [x] 스파르타C 공부 ( 포인터 )
    - [x] c++ 문법 강의 진행 1주차
- Today I Learned
  - 좀더 순 공부량을 늘리기
  
  <details>
  <summary>포인터에 대한 전반적인 학습  </summary>

  # 1
  ```cpp
  int arr[] = {10, 20, 30, 40, 50};
  int* ptr = arr;
  ptr += 2;
  ptr[1] = *ptr + 10;
  ```
  #### **1\. `int arr[] = {10, 20, 30, 40, 50};`**
  
  * 5개의 정수형 요소가 있는 배열이 선언되고 초기화됩니다.  
  * `arr = 10`, `arr = 20`, `arr = 30`, `arr = 40`, `arr = 50`
  
  #### **2\. `int* ptr = arr;`**
  
  * `ptr`은 `int`형 포인터 변수입니다.  
  * 배열 이름 `arr`은 배열의 첫 번째 요소 주소를 의미합니다.  
  * 따라서 `ptr`은 `arr`의 주소를 가리키는 포인터가 됩니다.
  
  #### **3\. `ptr += 2;`**
  
  * 포인터 `ptr`에 2를 더하는 것은 포인터 연산입니다.  
  * 포인터의 값은 메모리 주소입니다.  
  * `ptr += 2;`는 포인터가 가리키는 위치를 배열 요소 2칸 앞으로 이동시킵니다.  
  * 즉, 원래는 `ptr`이 `arr`을 가리켰다면, 이제 `ptr`은 `arr` (값 30)을 가리키게 됩니다.
  
  #### **4\. `ptr = *ptr + 10;`**
  
  * 포인터 표기 `ptr`은 `*(ptr + 1)`과 동일합니다.  
  * 현재 `ptr`은 `arr`를 가리키고 있으므로, `ptr`은 `arr`을 의미합니다.  
  * `*ptr`은 `ptr`이 가리키는 값, 즉 `arr = 30`입니다.  
  * 따라서 `ptr = *ptr + 10`는 `arr = 30 + 10` 으로 `arr`에 40 대신 40을 다시 저장하는 게 아니라 40을 40으로 바꾸는 게 아닌,  
    * `arr`에 `30 + 10 = 40`을 저장하는 것이며 실제 `arr` 초기 값이 40이니 변경이 같은 값으로 재설정됩니다.  
  - **ptr\[1\]은 ptr에서 한 칸 더 앞으로 간 위치(arr\[3\])를 가리킴**  
   - **최종 배열 상태**
  
     | 인덱스 | 0 | 1 | 2 | 3 | 4 |
     | :---: | :---: | :---: | :---: | :---: | :---: |
     | 초기값 | 10 | 20 | 30 | 40 | 50 |
     | 최종값 | 10 | 20 | 30 | 40 | 50 |
  
  - `arr` 값이 40에서 40으로 변경된 것처럼 보이지만, 사실은 `arr`에 `*ptr + 10` 값을 대입한 결과입니다.
  
  ---
  
  **중요한 포인트**
  
  * 포인터에 숫자를 더하면 그 숫자만큼 그 타입 크기 단위로 이동합니다. (`ptr += 2`는 `ptr`이 int 2개 위치 앞으로 이동)  
  * `ptr`은 현재 포인터 위치에서 한 칸 더 떨어진 위치의 배열 요소를 의미합니다.  
  * `*ptr`은 현재 포인터가 가리키는 위치의 값을 의미합니다.
  
  ---
  
  이해를 돕기 위해 메모리와 배열 관계를 그림으로 설명하면:
  
  `arr:  주소     값`  
        `addr0 -> 10`  
        `addr1 -> 20`  
        `addr2 -> 30   <-- ptr이 여기 위치`  
        `addr3 -> 40   <-- ptr[1]이 이 위치`  
        `addr4 -> 50`
  
  포인터 `ptr`이 `addr2`에 위치했으므로, `ptr`은 `addr3` 위치가 되어 `arr`에 해당합니다.
  
  `ptr = *ptr + 10;`는 `arr = 30 + 10 = 40` 대입 입니다.
  
  ---
  # 2 
  ```cpp
  void foo(int& x, int* y) {
    x = *y;
    *y = 30;
    y = &x;
    *y = 40;
  }
  int main() {
      int a = 10;
      int b = 20;
      foo(a, &b);  // a와 b의 최종값은?
  }
  ```
  
  #### **1\. 함수 매개변수 이해**
  
  * `int& x` : `x`는 `int`형 변수에 대한 \*\*참조자 (reference)\*\*입니다. 즉, `x`는 함수 호출 시 전달된 변수 `a`의 별명(alias)입니다. 함수 안에서 `x`를 수정하면 `a`가 직접 수정됩니다.  
  * `int* y` : `y`는 `int`형 변수의 주소를 받는 포인터입니다. `y`를 통해 `*y`로 해당 주소가 가리키는 값을 읽거나 쓸 수 있습니다. 하지만 포인터 변수 `y` 자체를 변경해도 호출한 쪽 변수에는 영향이 없습니다(포인터가 전달된 값은 복사본이기 때문).
  
  #### **2\. `main` 함수에서의 호출**
  
  * `a = 10`, `b = 20`  
  * `foo(a, &b)` 호출  
    * `x`는 `a`에 대한 참조 → `x`는 `a`와 사실상 같은 변수  
    * `y`는 `b`의 주소를 가진 포인터
  
  #### **3\. `foo` 함수 내부 동작 분석**
  
  ##### **3-1. `x = *y;`**
  
  * `*y`는 `y`가 가리키는 값, 즉 `b`의 값 20  
  * `x`는 `a`를 참조하므로 `a = 20`이 됨
  
  상태: `a = 20`, `b = 20`
  
  ##### **3-2. `*y = 30;`**
  
  * `*y`는 `b`를 가리키므로 `b = 30`으로 변경됨
  
  상태: `a = 20`, `b = 30`
  
  ##### **3-3. `y = &x;`**
  
  * 포인터 변수 `y`에 `x`의 주소값을 대입  
  * `x`가 참조하는 것은 `a`이므로 `y`는 이제 `a`의 주소를 가리킴  
  * 그러나 `y` 자체는 함수 내에서만 사용되는 지역 복사본 포인터이므로, 이 변경은 `main` 함수의 변수 `b`나 `a`에 영향 없음
  
  상태: `a = 20`, `b = 30`, `y`는 `a` 주소를 가리킴 (함수 내에서만)
  
  ##### **3-4. `*y = 40;`**
  
  * `y`가 `a`의 주소를 가리키므로, `*y = 40`은 `a = 40`으로 변경
  
  상태: `a = 40`, `b = 30`
  
  ---
  
  #### **4\. 함수 종료 후 상태**
  
  * `a`는 참조자 `x`를 통해 최종적으로 40이 됨  
  * `b`는 포인터 `y`를 통해 30으로 변경됨 (포인터 복사본을 통해서 직접 `*y` 값을 수정했기 때문)  
  * `y = &x;`에서 `y` 자체가 다른 곳을 가리키도록 한 것은 함수 내에서만 의미 있고, 원본 `b` 변수에는 영향 없음
  
  ---
  
  #### **결론 a \-\> 40 , b \-\> 30**  
  ---
  
  **핵심 포인트**
  
  * 참조자(`int& x`)는 원본 변수(`a`)와 완전 동일한 별명이다.  
  * 포인터(`int* y`)는 주소를 전달받지만 포인터 변수 자체는 값 복사되어 전달된다.  
  * 포인터 변수 자체를 바꾸는 것은 함수 밖 변수에 영향을 주지 않는다.  
  * 그러나 포인터가 가리키는 값(`*y`)을 바꾸는 것은 원본 변수에 영향 준다.

  ---
  # 3
  ```cpp
  #include <stdio.h>

  int main(void) 
  { int num1 = 15; 
    int num2 = 30;
    int num3 = 45;
  
    int* ptr1 = &num1;
    int* ptr2 = &num2;
    int* ptr3 = &num3;
  
    ptr1 = ptr2;
    ptr2 = ptr3;
  
    *ptr3 *= 2;
    *ptr1 += *ptr3;
    *ptr2 *= 2;
  
    /* Q. 출력 결과는? */
    printf("%d %d %d", num1, num2, num3);
  
    return 0;
  }
  ```

  #### **1\. 초기값**
  
  `num1 = 15 \ num2 = 30 \ num3 = 45`  
  
  `ptr1 → num1 \ ptr2 → num2 \ ptr3 → num3`  
  
  #### **2\. `ptr1 = ptr2;`**
  
  * 이제 `ptr1`이 `ptr2`가 가리키는 곳(`num2`)을 가리키게 된다.
  
  `ptr1 → num2`    
  `ptr2 → num2`    
  `ptr3 → num3`
      
  #### **3\. `ptr2 = ptr3;`**
  
  * 이제 `ptr2`가 `ptr3`처럼 `num3`을 가리킨다.
  
  `ptr1 → num2 \ ptr2 → num3 \ ptr3 → num3`  
  
  #### **4\. `*ptr3 *= 2;`**
  
  * `ptr3`가 `num3`를 가리키고 있으니 `num3`은 2배가 된다.  
  * `num3 = 45 * 2 = 90`
  
  `num1 = 15 \ num2 = 30 \ num3 = 90 ` 
  
  #### **5\. `*ptr1 += *ptr3;`**
  
  * `ptr1 → num2`, `ptr3 → num3`  
  * `*ptr1 = num2`, `*ptr3 = num3`  
  * 따라서 `num2 += num3`  
  * `num2 = 30 + 90 = 120`
  
  `num1 = 15 \ num2 = 120 \ num3 = 90`  
  
  #### **6\. `*ptr2 *= 2;`**
  
  * `ptr2 → num3`를 가리키고 있으므로 `num3 *= 2`  
  * `num3 = 90 * 2 = 180`
  
  `num1 = 15 \ num2 = 120 \ num3 = 180`  
  
  **최종 결과**  
  `printf("%d %d %d", num1, num2, num3);`
  
  출력:
  `15 120 180`
  
  ---
  
  ✅ 정리:
  
  * 포인터가 중간에 다른 변수들을 가리키게 되면서 참조 대상을 바꾼다.  
  * 계산 과정에서 `num1`은 untouched (15 그대로), `num2`는 `30 + num3`으로 120, `num3`은 두 번 곱해져 45 → 90 → 180 이 된다.
  </details>
</details>

<details>
<summary> 스파르타 언리얼 TIL 문서 링크 </summary>

- TIL 작성 시간을 효율적으로 관리하기 위함
1.
https://docs.google.com/document/d/1BlRYbRRFQJxa-C-bVfFnIfcmVDrIIq5WtxDgaRUiWZ0/edit?usp=sharing
2.
https://docs.google.com/document/d/1NsANfFCW3V8koXTiTvR_UG3Cpobqzgd3aDPjZJCMg6w/edit?usp=sharing


</details>   


