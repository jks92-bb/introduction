---
description: 간단한 검색창을 이용해보았습니다.
---

# 검색창 만들어보기

네이버검색창과 구글 검색창을 form태그로 구성하여 실행해보았습니다.

````html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
   
</head>
<body>
    <!-- form태그 :: 입력양식 선언, 입력값 전송(submit타입 input 필수 필요)-->
    <!-- form태그를 통해서 다른 html 파일을 불러올 수도 있음 -->

    <!-- form태그 속성 :: method, action(다른 html 또는 url 불러옴) -->
    <form action="https://search.naver.com/search.naver" method="GET" target="_blank">
        <div class="input-group">
            <span class="input-text">NAVER</span>
            <input name="query" type="text" class="input" placeholder="검색어를 입력하세요." >
            <input type="submit" value="검색" class="button">
          </div>
    </form>
    <hr>
    <form action="https://www.google.com/search?q=" method="GET" target="_blank">
        <div class="input-group">
            <span class="input-text">google</span>
            <input name="query" type="text" class="input" placeholder="검색어를 입력하세요." >
            <input type="submit" value="검색" class="button">
          </div>
    </form>
    
</body>
</html>
```
````

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption><p>간단한 검색창</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/12.gif" alt=""><figcaption><p>네이버 검색창에서 검색한 것과 같이 검색이 가능</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/11.gif" alt=""><figcaption><p>구글 검색창에서 검색한 것과 같이 검색이 가능</p></figcaption></figure>
