# [Javascript] HTML파일에 script 태그를 사용해서 import 하기

자바스크립트 문법을 공부하다가 마주한 문제였다. 다음과 같이 HTML파일에 script태그를 사용해서 자바스크립트 파일을 import했는데, 문득 코드 실행순서에 대해 호기심이 생겨서 시도해보았다.

```HTML
<!DOCTYPE html>
<html>
  <head>
    <title>Document</title>
  </head>
  <body>
    <h1 id="title">제목</h1>
    <script src="1.js"></script>
  </body>
</html>
```

script 태그 안에 자바스크립트 코드를 입력하면 src="1.js" 를 통해 임포트한 파일의 코드와 script 안에 입력한 코드 중에 무엇이 먼저 실행되는지 궁금해서 시도해봤다.

그래서 다음과 같이 코드를 입력해서 크롬에서 실행시켜보았다.

```html
<!-- html 파일 -->
<!DOCTYPE html>
<html>
  <head>
    <title>Document</title>
  </head>
  <body>
    <h1 id="title">제목</h1>
    <script src="1.js">
      console.log("HTML의 script실행");
      document.getElementById("title").innerText =
      	"HTML파일의 script 태그에 입력한 제목";
    </script>
  </body>
</html>
```

```javascript
// javascript 파일
console.log("1.js의 script 실행");
document.getElementById("title").innerText = "1.js 파일에 입력한 제목";
```

다음은 실행 결과 화면이다.
![](https://velog.velcdn.com/images/mintzzz1009/post/bf28d410-dd29-4457-bc8b-29d3b885e00a/image.png)

코드순서를 확인하려고 했는데, `console.log("HTML의 script실행")` 가 찍히지도 않은 것을 보니 아예 script태그 안에 코드들은 실행이 되지 않은 것 같다. 혹시 몰라서 의도적으로 오류코드가 찍히도록 DOM을 body태그 안에 존재하지 않는 ID를 가리키게 바꿔봤지만 아예 실행되지 않았다.

그래서 스크립트 안 코드를 크롬개발자모드 콘솔에 찍어보니까 실행이 잘 되는 것을 보니 코드의 문제는 아니라고 생각했다.
![](https://velog.velcdn.com/images/mintzzz1009/post/729a00e6-5287-4521-ba75-cc796efaa9f0/image.png)

그래서 script 태그에서 src 속성을 제거해서 자바스크립트 파일을 제외하고 HTML 파일만 브라우저에서 실행시켜봤다. 그랬더니 문제없이 코드가 실행되었다.

결론은 src 속성을 통해 javascript 파일을 참조하게된 script태그는 안에있는 코드를 실행시키지 않는다는 것이다.
확실하게 하기 위해서 이번에는 src속성을 주어서 javascript를 참조하는 script태그와 아무 속성이 없이 안에 코드를 입력한 script태그를 입력한 HTML파일로 실험해보았다

```html
<script>
  console.log("HTML의 script실행");
  document.getElementById("title").innerText =
    "HTML파일의 script 태그에 입력한 제목";
</script>
<script src="1.js"></script>
```

![](https://velog.velcdn.com/images/mintzzz1009/post/682a6b79-4455-48de-a3be-cce8866f9442/image.png)

입력한 순서대로 script가 실행되는 것을 확인할 수 있었다. 순서를 바꾸면 반대로 실행된다.

## 결론

`src`속성을 준 `<script></script>` 태그는 태그 안의 코드를 실행하지 않는다.
