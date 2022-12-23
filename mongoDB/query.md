# mongoDB 쿼리문 기록하기

이 문서에서는 지금까지 사용했던 mongoDB의 쿼리문들을 정리할려고 한다.

## read

### ISODate로 구성된 날짜 조회하기

```
db.collection('document 이름').aggregate([
	{ $match: { "dataRegistDate": {$exists:true},"dataRegistDate": { $gte: new Date("ISO DATE"), $lte: new Date("ISO DATE") } } },
	{ $sort : { "dataRegistDate": -1} },
]).toArray()
```

<br />

```
{ $match: { "dataRegistDate": { $gte: new Date("ISO DATE"), $lte: new Date("ISO DATE") } } }
```
<br />

new Date()를 사용해 ISO DATE를 비교해준다.

<br />

### $lookup으로 join하기

```
db.getCollection("document 이름").aggregate([
    { $match : { "필드명" : "필드값", "필드명" : "필드값", "필드명" : "필드값"} },
    { 
        $lookup : { 
            from : "join할 document 이름", 
            let : { "파이프라인에서 사용할 필드명" : "$사용할 변수명" }, 
            pipeline : [
                { $match : { $expr : { $eq : [ "$필드명", "$$필드명" ] } } },
                { $project : { "필드명" : 1, "필드명" : 0} }
            ],
            as : "반환되는 그룹 이름"
        } 
    }
])
```

<br />

```
$lookup : { 
   from : "join할 document 이름", 
   let : { "파이프라인에서 사용할 필드명" : "$사용할 변수명" }, 
   pipeline : [
       { $match : { $expr : { $eq : [ "$필드명", "$$필드명" ] } } },
       { $project : { "필드명" : 1, "필드명" : 0} }
   ],
   as : "반환되는 그룹 이름"
} 
```
<br />

mongoDB에는 JOIN 대신 $lookup이란 쿼리문을 사용한다.

from : join할 document 이름을 적는다.

let : 파이프라인에서 사용할 필드명 : 변수명을 선언한다.

**pipeline**
$match : 비교할 실제 조건을 적는다. $$는 조인 대상 document의 필드이고 $는 현재 조회 대상 컬렉션의 필드이다.

$project : 반환할 필드명을 적는다. 0은 해당 필드가 반환되지 않고 1은 해당 필드가 반환된다.

as : 반환되는 값을 해당 선언한 그룹에 담아준다.

<br />

## update

### 배열로 구성된 field에서 조건에 맞는 경우 해당 field 업데이트

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

### 배열로 구성된 field에서 조건에 맞는 경우 해당 filed []값으로 처리

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

<br />



