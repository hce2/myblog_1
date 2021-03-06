---
title: 0616 자바 복습
date: 2022-06-16 16:30:05
tags:
- JAVA
- Hexo
- Github
- blog
- 헥소
categories: JAVA review
---





# 복습

오버라이딩

- 오버라이트로 기억, 덮어쓰기, 부모의 함수를 자식에서 재정의.

오버로딩

- 파라미터 타입이나 갯수가 다를때 이름이 같으면서 새롭게 함수 정의
- 장점 : 이름을 생각하는 고민의 시간을 줄여준다.
    
               이름은 그 함수가 하는 일이 함축되어 있기때문에 하는 일은 같음을 유추할 수 있다.
    

객체

- 우리가 프로그램으로 구현할 대상.
- 자바에서는 class로 객체를 설계한다.
- 생성자를 통해 인스턴스를 만든다.
- 인스턴스가 되어야 비로소 메모리에 올라가고 실체화 된다.
- 객체는 변수와 메소드의 묶음이다. 속성과 행동의 묶음이다.

생성자

- 인스턴스를 만들때 new 키워드와 함께 쓰인다.
- 함수와 비슷하게 생겼지만 이름이 클래스와 같고, 리턴타입이 없다.
- 인스턴스를 만들때 반드시 필요한 데이터를 강제하는 역할을 한다.

// 준비되지 않은 상태라면 인스턴스를 만들 수 없도록 한다.

- 생성자를 명시하지 않은 경우 컴파일러가 자동으로 부모의 디폴트 생성자를 호출한다. super();
    
    그렇기 때문에 부모의 사용 자정의 생성자가 있는데도, 자식에서 부모의 생성자를 호출하지 않는다면 컴파일 에러가 발생한다.
    
    - 자기의 생성자 안에는 반드시 부모의 생성자가 있어야 되는 이 메커니즘은 최종 조상인 Object를 생성할 때까지 계속 된다.
        
        자식 > 부모 > 조부모> 조부모의 부모 ….. > Object
        
    - 사용자 정의 생성자가 하나라도 있으면 디폴트 생성자는 호출되지 않는다.
    
    // super() 를 호출했는데 그 생성자는 없기 때문에
    

추상클래스

- abstract 키워드가 붙은 메소드 (추상메소드)가 하나라도 포함된 클래스를 말한다.
- 클래스 앞에도 abstract 키워드를 붙여줘야 한다.
- 추상클래스로는 인스턴스를 만들 수 없다.
- 목적 : 상속.
    - 미구현된 함수가 있으니 반드시 자식에서 오버라이딩해야함을 명시적으로 알려준다.
    - 미구현된 함수 말고는 자식에서 그대로 상속받아 사용할 수 있다.

상속

- 자식클래스 extends 부모클래스
- 를 하면  부모의 변수와 메소드를 자식에서 코딩하지 않고도 그대로 사용할 수 있다.
- 부모클래스는 하나밖에 못둔다. 동시에 2개 이상의 클래스로부터 상속받을 수는 없다.

- 내가 부모클래스를 만들어 상속을 만들려고 한다면 is a 관계이지 확인해야 한다.
- has a 관계로 상속관계를 구현하는 실수를 하기 쉽다.
    - 검은색(자식)은 색깔(부모)이다. → is a 관계 ⇒ 상속 가능
    - 차는 바퀴를 가지고 있다. → has a 관계 ⇒ 상속 불가
- 부모에 선언된 변수와 같은 이름으로 자식에 선언하면 자식의 것이 사용된다. (오버라이딩 개념)
    
    → 나로부터 타고 올라간다.
    
    부모에 변수가있어도 자식에 있으면 자식걸로 덮어쓰기 때문에
    
    → 최대한 부모에 있는 변수와 가튼 이름으로 자식에세 쓰지말고
    
    쓸경우가 생겼다면 부모에만 넣고 자식에 없애면
    
    예를들어 name이라는 값을 썼으면 먼저 나한테 그 변수가 있나 찾는다.
    
    없으면 나의 부모를 찾는다.
    
    부모에게 없다면 부모의 부모를 찾는다.
    
    이런식으로 최종 부모인 Object까지 훑고 올라간다.
    
    정리하자면 나부터 타고 올라가기 때문에 동시에 있다면 내것이 우선이 된다.
    
    나한테 없더라도 내 조상 누군가에게 있다면 그것을 쓸 수 있다.
    
    ⇒ 부모에 있는데 나한테 또 정의해놨을 경우
    
    부모에서 변수를 사용하면 부모의 것이 사용되지 자식의 것이 사용되지 않는다.
    
    자식에서 변수를 사용하면 자식의 것이 사용되지 부모의 것이 사용되지 않는다.
    

접근제한자

역할 : 접근을 제한하는 역할

왜 접근을 제한하나요? → 다른 사람(다른 클래스)가 함부로 나의 값에 접근하거나 수정하는 것을          막기 위해서

노출시키지 않고 싶거나, 노출시키면 안되는 정보들이 있기 때문에

private : 자기 클래스에서만 접근 가능

default : 해당 패키지 내에서만

protected : 해당 패키지 + 상속받은 클래스에서만

public : 이 프로젝트내 어디서나