# mongoDB 쿼리문 기록하기

이 문서에서는 지금까지 사용했던 mongoDB의 쿼리문들을 정리할려고 한다.

## update

### `배열로 구성된 field에서 조건에 맞는 경우 해당 field 업데이트`

```
db.collection('document 이름').updateOne(
  { _id: new require('mongodb').ObjectID('ObjectID명'), filter : { $elemMatch : { 필드명 : '필드값',  필드명 : '필드값' } } },
  { $set: { 'filter.$.more': '변경할 값' }
})
```

<br />

updateOne은 단일 수정을 말한다.

<br />

```
{ _id: new require('mongodb').ObjectID('ObjectID명'), filter : { $elemMatch : { 필드명 : '필드값',  필드명 : '필드값' } } }
```
<br />

new require().ObjectID를 사용해 nodeJS mongoDB 기본 드라이브에서 문자열('ObjectID명')을 ObjectId로 변환할 수 있다.

$elemMatch를 사용하면 배열로 구성된 filed에 key와 key value값이 일치할 경우 조건에 맞는 데이터를 선택한다.

<br/>

```
{ $set: { 'filter.$.more': '변경할 값' }
```

<br />

'$set을 통해 특정 필드의 값을 수정하고 $연산자를 사용해 배열 필드의 하위 문서를 읽는다.'

<br />

### `배열로 구성된 field에서 조건에 맞는 경우 해당 filed []값으로 처리`

```
db.collection('document 이름').update(
	{ _id: new require('mongodb').ObjectID('$ObjectID명'), '필드명': '필드값' },
        { $unset: { 'keywords.$.more': '', 'keywords.$.type': '' }}
)
```

<br />

```
{ _id: new require('mongodb').ObjectID('ObjectID명'), filter : { $elemMatch : { 필드명 : '필드값',  필드명 : '필드값' } } }
```
<br />

new require().ObjectID를 사용해 nodeJS mongoDB 기본 드라이브에서 문자열('ObjectID명')을 ObjectId로 변환할 수 있다.

<br />

```
{ $set: { 'filter.$.more': '변경할 값' }
```

<br />

'$unset은 해당 필드를 제거하는 연산자이지만 배열의 요소에 사용할 경우 제거하지 않고 null 값으로 교체한다.'

**현재 사용중인 데이터베이스를 확인하고 싶다면 db명령어를 입력한다.**

```csharp
> db
first_mongodb
```

**내가 만든 데이터베이스 리스트를 확인하고 싶다면 show dbs 명령어를 입력한다.**

```csharp
> show dbs
local 0.000GB
```

<aside>
⚠️ 리스트에 방금 만든 데이터베이스를 보려면 최소 한개의 Document를 추가해야합니다.

</aside>

### Database 삭제 - db.dropDatabase()

Database를 제거할땐 db.dropDatabase() 명령어를 사용한다.

이 명령어를 사용하기 전, use DATABASE_NAME으로 삭제하고 

싶은 데이터베이스를 선택해야한다

**first_mongodb 데이터베이스를 삭제한다.**

```csharp
> use first_mongodb
switched to db mongodb_tutorial
> db.dropDatabase()
{ "dropped" : "mongodb_tutorial", "ok" : 1 }
```

### Collection 생성 - db.createCollection()

Collection을 생성할때는 db.createCollection(name, [options]) 명령어를 사용한다.

- name - 생성하려는 collection의 이름
- option - document 타입으로 구성된 해당 컬렉션의 설정값

**first_mongodb 데이터베이스에 users 컬렉션을 옵션 없이 생성.**

```csharp
> use test
switched to db test
> db.createCollection("**users**")
{ "ok" : 1 }
```

**first_mongodb 데이터베이스에 lists 컬렉션을 옵션과 함께 생성.**

```csharp
> db.createCollection("lists", {
	capped: true,
	autoIndex: true,
	size; 6142800,
	max:10000
})
{ "ok" : 1 }
```

**내가 만든 collection 리스트를 확인하려면 show collections 명령어를 사용**

```csharp
> show collections
users
lists
```

### Collection 제거 - db.COLLECTION_NAME.drop()

collection을 제거 할 땐 drop() 메소드를 사용한다.

물론 이 명령어를 실행하기 전에 사용할 데이터베이스를 우선 설정해야한다.

**example 데이터베이스의 posts 컬렉션을 제거.**

```csharp
> use example
switched to db test
> show collections
posts
users
lists
> db.posts.drop()
users
lists
```

### Document 추가 - db.COLLECTION_NAME.insert(document)

insert() 메소드를 사용하여 Document를 추가할 수 있다.

이 명령어 역시 사용하기 전, 데이터를 추가할 데이터베이스를 선택해줘야한다.

배열형식의 인자를 전달해주면 여러 다큐먼트를 동시에 추가할 수 있다.

**한개의 document를 books 컬렉션에 추가**

```csharp
> db.books.insert({"name" : "NodeJs", "author" : "anonymous"})
WriteResult({ "nInserted" : 1 })
```

**두개의 document를 books 컬렉션에 추가**

```csharp
> db.books.insert([
	{"name" : "book1", "author" : "anonymous"},
	{"name" : "book2", "author" : "anonymous"}
])
BulkWriteResult({
        "writeErrors" : [ ],
        "writeConcernErrors" : [ ],
        "nInserted" : 2,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
})
```

**컬렉션의 다큐먼트 리스트를 확인할때는 db.COLLECTION_NAME.find() 명령어를 사용**

```csharp
> db.books.find()
{ "_id" : ObjectId("56c08f474d6b67aafdeb88a4"), "name" : "NodeJS", "author" : "anonymous" }
{ "_id" : ObjectId("56c0903d4d6b67aafdeb88a5"), "name" : "book1", "author" : "anonymous" }
{ "_id" : ObjectId("56c0903d4d6b67aafdeb88a6"), "name" : "book2", "author" : "anonymous" }
```

### **Document 제거 - db.COLLECTION_NAME.remove(criteria, justOne)**

books 컬렉션에서 name이 book1인 document 제거
