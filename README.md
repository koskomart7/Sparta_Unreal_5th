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





      


