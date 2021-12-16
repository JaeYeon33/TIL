# 목차
- [현재 개발 트렌드 - 언어, DB](#현재-개발-트렌드---언어-db)
- [무엇이 문제인가?](#무엇이-문제인가)
- [객체와 관계형 데이터베이스의 차이](#객체와-관계형-데이터베이스의-차이)

<br>
<br>
<br>

## JPA 소개
-----
: JPA와 모던 자바 데이터 저장 기술


<br>
<br>

## 현재 개발 트렌드 - 언어, DB
- 현재 개발 언어 트렌드 -> 객체 지향 언어
- 현재 데이터베이스 세계의 헤게모니 -> 관계형 DB

-> 객체를 관계형 DB에 관리하는 것이 문제

-> 개발자가 객체로 데이터를 가공했지만 DB에 저장할 땐 결국 SQL

-> SQL 중심적인 개발이 되버린다.

<br>
<br>

## 무엇이 문제인가?
1. 기능 하나한 추가해서 테이블 생성될 떄마다 CRUD SQL을 만들어줘야 한다.

    → Jdbc Template, MyBatis가 Mapping에 도움을 주는 것은 있지만 그래도 한계가 있다.

   - Example : 회원 객체를 만들고 DB에 CRUD를 하는 기능이 있는 상태
      1. 기존 회원 객체와 테이블 기능쿼리 구현
            ```java
            /*회원 객체*/
            public class Member {
                private String memberId;
                private String name;
            }
            
            /*쿼리*/
            INSERT INTO MEMBER(MEMBER_ID, NAME) VALUES ...
            SELECT MEMBER_ID, NAME FROM MEMBER M
            UPDATE MEMBER SET ...
            ```
            
            <br>

      2. 기획자가 전화번호 필드를 추가해 달라고 한 상황
            
            ```java
            /*회원 객체*/
            public class Member {
                private String memberId;
                private String name;
                private String tel;
            }
            
            /*쿼리*/
            INSERT INTO MEMBER(MEMBER_ID, NAME, TEL) VALUES ...
            SELECT MEMBER_ID, NAME, TEL FROM MEMBER M
            UPDATE MEMBER SET ...TEL = ?
            ```
            
         - 모든 CRUD 쿼리에 TEL을 하나하나 추가해 줘야 한다.

    > 결론: SQL에 의존적인 개발을 할 수밖에 없다.


2. 패러다임의 불일치 - 객체 vs 관계형 데이터베이스

    -> 관계형 데이터베이스와 객체지향은 서로 가지고 있는 사상이 다르다.

     - 객체지향개발: 추상화, 캡슐화, 정보은닉, 상속, 다형성 등 시스템의 복잡성을 제어하는 다양한 장치

     - 관계형데이터베이스: 데이터를 잘 정규화해서 저장하는 것이 목표

    -> 객체를 영구 보관하는 저장소


<br>
<br>
<br>

**객체를 관계형 데이터베이스에 저장**
![picture1](https://github.com/JaeYeon33/TIL/blob/main/JPA/Inflearn/img/Untitled.png?raw=true)

- 객체를 SQL로 변환해서 RDB에 저장하는 변환과정을 개발자가 해야 한다.

    -> 노가다 시작


<br>
<br>
<br>


## 객체와 관계형 데이터베이스의 차이

### 1. 상속
![picture2](https://github.com/JaeYeon33/TIL/blob/main/JPA/Inflearn/img/Untitled1.png?raw=true)

- 객체의 상속관계와 유사한 관계형 데이터베이스의 개념으로 Table 슈퍼타입, 서브타입 관계가 있다.
- 객체 상속 관계에서는 그저 `extends` 나 `implements` 로 상속 관계를 맺고 캐스팅도 자유롭다.
- 하지만,  상속 받은 객체(`Album` , `Movie` , `Book`)을 데이터베이스에 저장하려면 어떻게 해야하는가?
    1. 객체 분해 : Album객체를 Item과 분리한다.
    2. Item Table에 하나, Album테이블에 하나 두 개의 쿼리를 작성해서 저장한다.
- Album 객체를 DB에서 조회하려면 어떻게 해야하는가?
    
    → ITEM과 ALBUM을 조인해서 가져온 다음 조회한 필드를 각각 맞는 객체(ITEM, ALBUM)에 매핑시켜서 가져와야 한다.

<br>

>결론: DB에 저장할 객체는 상속관계를 쓰지 않는다.


<br>
<br>

### 2. 연관관계

![picture3](https://github.com/JaeYeon33/TIL/blob/main/JPA/Inflearn/img/Untitled2.png?raw=true)


- 객체를 **참조**를 사용한다 : member.getTeam();
- 테이블은 **외래 키**를 사용한다 : JOIN ON M.TEAM_ID = T.TEAM_ID

❓ Member와 Team간에 참조가 가능한가?

- 객체: `Member` → `Team` 은 member.getTeam()을 통해 가능, `Team` → `Member` 는 참조할 객체가 없기 때문에 불가능하다.
- 테이블: 서로가 서로를 참조할 키(FK)가 있기 때문에 양측이 참조 가능하다. `Member` ←→ `Team`


❗객체를 테이블에 맞춰 모델링하게 된다.

```java
class Member {
    String id;        //MEMBER_ID 컬럼 사용
    Long teamId;      //참조로 연관관계를 맺는다.
    String username;  // USERNAME 컬럼 사용
}

class Team {
    Long id;      //TEAM_ID 컬럼 사용
    String name;  //NAME 컬럼 사용
}

/*쿼리*/
INSERT INTO MEMBER(MEMBER_ID, TEAM_ID, USERNAME) VALUES ...
INSERT INTO TEAM(TEAM_ID, NAME) VALUES...
```

- 하지만, 위 코드는 객체지향적이지 못한 것 같다.
    
    Why? : Member에서는 Team을 참조하는데 참조값을 가져야 하지 않을까?
    
    ```java
    Long teamId -> Team team;
    
    Team getTeam(){
    	 return team;
    }
    ```

    <br>
    
    - 이렇게 설계를 바꾼다면 외래키를 DB에 어떻게 저장하는가?
    
        ```java
        member.getTeam().getId();
        ```
    
    - 참조되는 Team객체에서 id를 가져와서 넣어줘서 저장한다.
    - 하지만, 조회할 때는 어떻게 하는가?
    
    <br>
    <br>

    객체 모델링 조회
    
    ```java
    SELECT M.*, T.*
      FROM MEMBER M
      JOIN TEAM T ON M.TEAM_ID = T.TEAM_ID
    
    public Member find(String memberId){
            //SQL 실행
            Member member = new Member();

            //데이터터베이스에서 조회한 회원 관련 정보를 모두 입력
            Team team = new Team();

            //회원과 팀 관계 설정
            member.setTeam(team);
            return member;
    }
    ```

    <br>

- 객체 그래프 탐색
  
    : 객체는 자유롭게 객체 그래프를 탐색할 수 있어야 한다. (ex : memer.getXXX())

    ![picture4](https://github.com/JaeYeon33/TIL/blob/main/JPA/Inflearn/img/Untitled3.png?raw=true)

- `Member` 객체에서 엔티티 그래프를 통해 `Category` 까지도 접근이 가능해야 한다.


    <br>
    <br>

    처음에 실행하는 SQL에 따라 탐색 범위가 결정된다.

    ```java
    SELECT M.*, T.*
    FROM MEMBER M
    JOIN TEAM T ON M.TEAM_ID = T.TEAM_ID

    member.getTeam(); //OK
    member.getOrder(); //null
    ```

    <br>

    엔티티 신뢰문제가 발생한다.

    ```java
    class MemberService {
        ...
        public void process(){
                Member member = memberDao.find(memberId);
                member.getTeam(); //????
                member.getOrder().getDelivery(); //????
        }
    }
    ```

    - 정말 자유롭게 member안의 모든 참조를 자유롭게 참조할 수 있을까?
    - 새로 투입된 개발자가 새로운 필드를 추가했는데 조회 로직에서 해당부분 매핑을 빼놨다면?
    - 실제로 모든 쿼리와 쿼리를 분석해보기전까지는 엔티티 그래프 검색이 어디까지 되는지 확신할 수 없다.


    <br>

    그렇다고 모든 객체를 로딩할 수는 없다.

    - 상황에 따라 동일한 회원 조회 메서드를 여러개 생성
        ```java
        memberDAO.getMember(); //Member만 조회
        memberDAO.getMemberWithTeam(); //Member와 Team조회
        memberDAO.getMemberWithOrderWithDelivery(); // Member, ORder, Delivery 조회
        ```


<br>


> 결론: 계층형 아키텍처 <br>
> 진정한 의미의 계층 분할이 어렵다.


<br>
<br>
<br>



❓동일한 식별자로 조회한 두 객체 비교결과는?

```java
String memberId = "100";
Member member1 = memberDAO.getMember(memberId);
Member member2 = memberDAO.getMember(memberId);

member1 == member2; // false 다르다

class MemberDAO{
    public Member getMember(String memberId){
        String sql = "SELECT * FROM MEMBER WHERE MEMBER_ID = ?";
        ...
        //JDBC API, SQL 실행
        return new Member(...);
    }
}
```
member를 조회할 때마다 new Member()를 통해 새로운 객체를 만들어서 조회 하기 때문에 두 인스턴스 비교는 내용물이 같더라도 다르다.

<br>
<br>
<br>

자바 컬렉션에서 조회한 객체를 비교

```java
String memberId = "100";
Member member1 = list.get(memberId);
Member member2 = list.get(memberId);

member1 == member2; // true 같다.
```

<br>

> 참고: 동일성과 동등성(Identical & Equality) <br>
자바에서 두 개의 오브젝트 혹은 값이 같은가 라는 말은 주의해서 사용해야 한다. <br>
(identical)동일한 오브젝트라는 말은 같은 레퍼런스를 바라보고 있는 객체라는 말로 실제론 하나의 오브젝트라는 의미이고, <br>
(Equivalent)동등한 오브젝트라는 말은 같은 내용을 담고 있는 객체라는 의미이다.


<br>

> 결론: 객체지향적으로 모델링을 할 수록 매핑작업만 늘어나고 불일치가 늘어나서 사이드펙트가 커지기만 한다. <br>
그렇다면, 객체를 자바 컬렉션에 저장하듯이 DB에 저장할 수 없을까? 고민하다 나온 것이 JPA이다.


