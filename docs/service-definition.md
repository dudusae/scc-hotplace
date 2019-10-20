# 핫플검색 API 명세 & DB Schema
핫플검색 웹서비스의 API를 게시합니다.

* DB Schema written in Mongoose template.
* First draft as of 2019-10-20


## DB Schema and List of API

**SearchResult : 검색결과**
```
{
    keywords: //list of possible keywords,
    area: {type: areaSchema} //or id?
}
```
- `GET` /api/v1/search/{keyword}


**Areas : 인기지역**
* 인기지역 리스트 (34개)  
: 모든 영문표기는 **한글그대로** 읽는 발음
    가로수길 garosugil
    강남역 gangnamyeog
    건대 geondae
    경리단길 gyeonglidangil
    광화문 gwanghwamun
    남양주 namyangju
    대학로 daehakro
    망원 mangwon
    명동 myeongdong
    문래 munrae
    북촌 bugchon
    분당 bundang
    상수 sangsu
    샤로수길 sharosugil
    서래마을 seoraemaeul
    서촌 seochon
    성수 seongsu
    송도 songdo
    압구정 apgujeong
    양재 yangjae
    양평 yangpyeong
    여의도 yeouido
    연남 yeonnam
    을지로 euljiro
    이태원 itaewon
    익선동 ikseondong
    인사동 insadong
    일산 ilsan
    잠실 jamsil
    종로 jongro
    청담동 cheongdamdong
    한남동 hannamdong
    합정 hapjeong
    해방촌 haebangchon
    홍대 hongdae
```
{
    name: {type: String, required:true, unique: true},
    image: imageSchema,
    //FIXIT:: define list, define bound of list
    venues: {type: venueSchema}//list of venues
    id: mongoose.Schema.Types.ObjectId
}
```
- `GET` /api/v1/areas/{*id}
- `POST` /api/v1/areas/
- `PUT` /api/v1/areas/{id}
- `DELETE` /api/v1/areas/{id}

// Where to place offset and limit? (limit=6, offset=0)
// rank 구현 https://m.blog.naver.com/PostView.nhn?blogId=secret245&logNo=220609053013&proxyReferer=https%3A%2F%2Fwww.google.com%2F

**Venues : 핫플 (식당)**
```
{
    name: {type: String, required:true, unique: true},
    description: {type: String, maxlength: 50},
    num_of_posts: {type: Number, required: true}, 
    url_detail: {type: String},
    url_naver: {type: String},
    url_instagram: {type: String},
    isInNaver: {type: Boolean, required: true, default: true},
    category: {type: String, default: restaurant},
    area: {type: ObjectId, name: {type: String}}, //point to its parent area
    id: mongoose.Schema.Types.ObjectId
}
```
- `GET` /api/v1/venues/{*id}
- `POST` /api/v1/venues/
- `PUT` /api/v1/venues/{id}
- `DELETE` /api/v1/venues/{id}


imageSchema
{
    width: Number,
    height: Number
}



## API Definition

### 1. GET Search Result for a Keyword
----
  키워드에 해당하는 area를 반환합니다.
  Returns area mapped to the keyword.

* **URL**

  /api/v1/search/{keyword}

* **Method:**

  `GET`
  
*  **URL Params**

   keyword : String

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    //FIXIT🔨
    **Content:** `{ id : {type: ObjectId of areaSchema}}`
 
* **Error Response:**

  * **Code:** 404 NOT FOUND <br />
    **Content:** `{ error : "Try different keyword." }`

* **Sample Call:**

  ```javascript
    $.ajax({
      url: "/api/v1/search/신논현",
      dataType: "json",
      type : "GET",
      success : function(r) {
        console.log(r);
      }
    });
  ```


### 2. GET List of Areas or Area
----
  인기지역(Area) 목록 또는 인기지역을 반환합니다.
  Returns list of area or area.

* **URL**

 /api/v1/areas/{*id}

* **Method:**

  `GET`
  
*  **URL Params**

   *id : String
   (* option for **List of Area**)

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
    ```
    {
        name: {type: String, required:true, unique: true},
        image: imageSchema,
        //FIXIT🔨:: define list, define bound of list
        venues: {type: venueSchema}//list of venues
        id: mongoose.Schema.Types.ObjectId
    }
    ```
 
* **Error Response:**

  * **Code:** 404 NOT FOUND <br />
    **Content:** `{ error : "Area with the given id does not exist. Please check the id." }`

* **Sample Call:**

인기지역 목록 List of Areas
  ```javascript
    $.ajax({
      url: "/api/v1/areas",
      type : "GET",
      success : function(r) {
        //prints list of areas
        console.log(r);
      }
    });
  ```

인기지역
  ```javascript
    $.ajax({
      url: "/api/v1/areas/garosugil",
      dataType: "json",
      type : "GET",
      success : function(r) {
        //prints garosugil's data
        console.log(r);
      }
    });
  ```

... Work in progress.