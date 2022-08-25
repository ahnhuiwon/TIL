## 크롤링 준비하기

> npm install axios
npm install cheerio
> 

- axios는 서버에서 외부 api 주소를 요청해야 하기 위해 설치
- cheerio는 html에서 원하는 데이터를 가져오기 위해 설치

---

```jsx
const axios = require('axios');
const cheerio = require('cheerio');
```

require() 메소드를 이용해 사용할 라이브러리를 갖고 온다.

```jsx
const getHtml = async () => {
  try {
    return await axios.get('https://maple.inven.co.kr/');
  } catch (error) {
    console.error(error);
  }
};
```

axios 라이브러리를 통해 서버에서 외부 api에 요청한다.

```jsx
getHtml()
.then(async (res) => {
    const $ = cheerio.load(res.data);
    const list = $('#imid_df94d42dc038eed > ul > li');
    let temp_arr = [];

    await list.each((i, tag)=>{
        let href = $(tag).find('a').attr("href");
        let text = $(tag).find('.tit').text();

        return temp_arr.push({ link : href, myText : text})
    })

    return temp_arr;
})
.then((res) => log(res));
```

cheerio 라이브러리를 사용해 html에서 원하는 데이터를 가져온다.

<aside>
💡 참고로 cheerio를 사용하면 jquery처럼  Node.js에서 HTML 데이터를 다룰 수 있다.

</aside>

### jQuery란?

- HTML의 요소들을 조작하기 편리한 javascript 라이브러리
- DOM 객체를 조작하거나, 데이터를 가져올 때 편리하게 사용 가능

JavaScript

```jsx
document.getElementById("temp_id").style.display = "none";
```

jQuery

```jsx
$("#temp_id").css("display", "none")
```

![image](https://user-images.githubusercontent.com/94499416/186624139-a7e7606e-4007-482f-ac0e-a79fedb4a189.png)

우리가 원하는 데이터들의 DOM을 파악한 뒤 변수에 할당해준다.

필자는 리스트들의 주소와 제목을 추출하기 위해 each() 반복문을 사용하였고

$(tag).find(”element”)를 사용해 DOM의 값을 가져왔다.

아래는 return된 크롤링 데이터들을 담은 temp_arr을

콘솔로 찍어낸 값이다.

![image](https://user-images.githubusercontent.com/94499416/186623723-6dd16f8b-c9ea-49f2-83b0-d6bc91e12e5d.png)
