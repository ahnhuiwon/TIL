## mongoDB
### `mongoDB란 무엇인가?`

MongoDB는 C++로 작성된 오픈소스 문서지향적 크로스 플랫폼 데이터 베이스이며,

뛰어난 확장성과 성능을 자랑한다.

현존하는 NoSQL 데이터 베이스중 인지도 1위를 유지하고 있다.

### `NoSQL이란?`

NoSQL은 SQL이 없는 데이터베이스인가? 라는 생각을 할 수 있지만,

진짜 의미는 Not Only SQL이다. 기존 RDBMS의 한계를 극복하기 위해 만들어진 새로운 형태의 데이터 저장소이다.

관계형 DB가 아니기 때문에 RDMS처럼 고정된 스키마 및 JOIN이 존재하지 않는다.

### `SQL과 NoSQL 비교`

![mongo_pic](https://user-images.githubusercontent.com/94499416/182065364-ad2e7168-ea81-4f87-923f-6f92a3c76596.png)

<br />

![mongo_table](https://user-images.githubusercontent.com/94499416/182065368-08b5f71d-dc2d-4001-9951-9dce42ae7e46.png)

<br />

성능면과 확장성면에서는 NoSQL이 SQL보다 우수하다. 또한 유연하고 복잡성이 낮은것이 특징이다.

허나 좋은점만 있는것은 아니며 ACID 트랜잭션(원자성 / 일관성 / 고립성 / 영구성)을 보장받기 위해서는 RDBMS를 쓰는 것이 좋다.

은행 혹은 회사 업무 같은 중요한 DB는 RDBMS를 쓰느 것을 권장한다.

결론적으로 SQL, NoSQL 뭐가 더 좋은가를 가리는게 아니라 사용하는 용도에 따라 선택하면 된다.

### `MongoDB 알아보기`

#### `1. Document`

Document를 RDBMS에서의 Row와 동일한 개념으로 본다.

예를 들어 아래 코드와 같은 JSON 형태의 key-value 쌍으로 이루어진 데이터 구조를

하나의 Document라고 보면 된다.

<br />

```

{
    "_id": "5f2ad6b54866e5109dd2367b"
    "username": "홍길동",
    "hashedPassword": "비밀번호",
}

```

<br />

각 Document를 `_id`를 갖고 있는데 이 값은 유일하며 Primary Key랑 동일한 개념이다.

특이하게도 RDBMS처럼 스키마로 정해진 무언가가 없는데 따라서 username, hashedPassword 밑에다가

email 추가한 Document를 새로 생성해도 문제 없이 동작한다.

<br />

#### `2. Collection`

Collection은 Document의 그룹이다. RDBMS로 따진다면 Table과 비슷한 개념.

다만 위에서 언급했듯이 스키마를 가지고 있지 않다.

#### `3. Database`

Database는 Collection들의 물리적인 컨테이너이자 가장 상위 개념이다. 

RDBMS에서의 Database와 동일하다.

<br />

### `mongoDB에서 join은?`

RDBMS에서는 항상 join이 일상적으로 많이 쓰인다.

그렇다면 mongoDB에서는 일반적으로 join을 쓰지 않는다.

post와 comment가 있다고 가정해보자.

하나의 post에 comment가 여러개 있으니까 1대 다 관계이다.

이런 경우 RDBMS를 사용했다면 2개의 테이블로 분리시키는게 일반적이지만

mongoDB는 분리시키지 않고 post 내부에 comment를 넣는다.

이를 서브다큐멘트라고 한다. 따라서 아래와 같은 형태가 나온다.

```

{
   _id: POST_ID,
   title: POST_TITLE,
   content: POST_CONTENT,
   username: POST_WRITER,
   tags: [ TAG1, TAG2, TAG3 ],
   comments: [
     { 
        username: COMMENT_WRITER,
        mesage: COMMENT_MESSAGE,
        time: COMMENT_TIME
     },
   ]
}

```
