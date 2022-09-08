---
title: 0622 자바 복습
date: 2022-06-22 9:12:05
tags:
- JAVA
- Hexo
- Github
- blog
- 헥소
categories: JAVA review
---




# 복습

Collection Framework :

자바에서 미리 만들어놓은 자료구조

- 직접 만드는 것보다 일관성 있고, 빠르고, 안정적이고 품질이 높다.
- 만드는 법 : 객체 만드는 법과 비슷한데 제네릭이 들어감.
    - 객체 만드는 법 : 객체타입 이름 = new 객체타입(파라미터);
    - 컬렉션타입<데이터타입> 이름 = new 컬렉션타입<데이터타입>(파라미터);
- 데이터타입은 원시형 데이터타입이 들어갈 수 없다. 참조형 데이터타입만 들어갈 수 있다.
1. Set : 집합
2. List : 크기가 고정되지 않은 배열
3. Queue : 먼저 들어온 놈이 먼저 나감
4. Map : 키와 벨류의 쌍

각각의 자료구조에 미리 만들어져있는 함수를 활용해서 CRUD를 한다.

CRUD = Create, Read, Update, Delete 만들고 읽고 수정하고 지우고

함수에는 get, add, remove, size, indexOf 이런 것들이 있다.

Iterator : 반복자.

향상된 for문에서 뒤에 들어갈 수 있는 데이터타입이 이터레이터이다.

for(자료형 이름 : 이터레이터) { 

모든 원소에 대해 실행할 본문;

}

while(iterator.haxNext()) {

이터레이터명.next();   // 다음 원소 읽어오기

}

제네릭 : Generic.

- 함수 정의 시 파라미터의 이름이 무엇으로 넘어오든 상관없이 정의할 때 정한 이름으로 쓰듯이 데이터타입을 정의 시에는 대명사처럼 받고, 인스턴스 생성 시 결정되도록 하는 문법
- 즉, 어떤 데이터타입으로 넘어오든지 그 넘어온 데이터타입으로 쓰겠다. 라고 하는 것
- 데이터타입이 다를 때마다 오버로딩 해줘야 되는 작업을 없애준다.
    - 예) List<String> list = new ArraryList<String>();
    - List를 만든 사람은 List를 정의할 때 제네릭을 사용했다.
    - 그래서 List의 인스턴스를 만드는 사람은 <String>을 하면 String형태의 리스트가 만들어
    - <Integer>를 하면 Integer 형태의 리스트가 만들어지고
    - <Double>을 하면 Double형태의 리스트가 만들어진다.
    - 하지만 List정의 시의 코드는 하나다. 하나가 저것들을 다 처리할 수 있다. 제네릭을 썼기 때문

ERD : Entity Relationship Diagram. 객체관계도

RDB(관계형 데이터 베이스) 오라클, mysql은 설계에 따라서 성능차이가 엄청남

<제약조건>

- PK : primary Key. 기본키.
    - 한테이블에서 유일한 행을 결정짓도록 하는 컬럼들의 조합
    - 한테이블에 pk는 무조건 하나만 있어야한다. 0개, n개 안된다.
    - 그 말이 한테이블에 pk는 하나만의 컬럼으로만 구성되어야 한다는 말이 아닌데 그렇게 이해하기 쉬우니 유의해야 한다.
- NN : Not Null. 빈값이 있으면 안된다.
- Unique : 이미 테이블에 기존값이 있으면 중복되어서 들어갈 수 없다. 유일해야 한다.
- FK : Foreign Key. 외래키.
    - 다른 테이블의 데이터를 참조해서 사용할 때 다른 테이블의 하나의 행을 유일하게 식벽해서 가져와야하기 때문에 다른 테이블의 PK를 참조하게 되는데 그것이 내 테이블에서는 FK라고 불린다.
- CHECK : 조건에 부합하는 데이터만 들어올 수 있다.

- FK를 가지고 올때 2가지 타입이 있다. 식별관계랑 비식별관계.
    - 식별관계 : 참조테이블의 PK가 내 테이블의 PK로 들어오는 경우.
        - ERD에서 실선으로 표현
        - 내 테이블에 데이터가 있을때 부모테이블에 그 데이터가 있음을 보장한다.
        - 부모테이블에서 다른 테이블에서 참조되고있는데이터를 지우려고 하면 못 지우거나, 참조데이터까지 지워야되기 때문에
    - 비식별관계 : 참조테이블의 PK가 내테이블의 일반칼럼으로 들어오는 경우
        - ERD에서 점선으로 표현
        - 다른테이블에서 참조하고있더라도 부모테이블에서 참조되고있는 데이터를 지울 수 있다.
    

Cardinaliry(관계 차수) : 

Optional : 있을 수도 있고, 없을 수도 있다.

1 : 0, 1, n 관계를 ‘목적어’ 쪽에 표현해준다.