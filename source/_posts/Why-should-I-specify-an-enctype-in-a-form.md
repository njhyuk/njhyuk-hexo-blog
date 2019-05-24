---
title: Form 파일전송을 위해 enctype을 왜 지정해야 할까?
date: 2019-05-24 13:57:02
tags:
---

브라우저에서 서버와 통신하기 위해 폼을 사용합니다.
사용자가 입력한 정보를 폼에 입력하여 전송하면 입력한 데이터가 웹서버로 전달되어 처리됩니다.

```html
<form action="/api/save-file" method="post">
  <input type="text" name="text" value="Hello 안녕하세요">
  <button>전송</button>
</form>
```

단순 정보뿐만 아니라 파일도 추가로 첨부하려면 익히 알고 있듯이, form 태그에 `enctype="multipart/form-data"` 속성을 추가하면 파일을 웹서버에 전송할 수 있었습니다.
하지만 해당 속성은 왜 기본으로 지정되어 있지 않고 별도로 지정해야 할까요?
이번 시간에는 당연 하게만 알고 있던 enctype을 폼에 왜 추가해야 하는지 알아보도록 하겠습니다.

## HTTP Header

HTTP 헤더는 사용자의 브라우저와 웹 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 해줍니다.
폼에 입력한 enctype은 HTTP 헤더의 내용 중 Content-type 을 수정하는 것이죠.


    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
    Accept-Encoding: gzip, deflate, br
    Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
    Cache-Control: no-cache
    Connection: keep-alive
    Content-Length: 159906
    Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryJlRg3UkmyPL56XOo


그렇다면 Content-type은 어떤 것들이 있을까요?
다양한 타입들이 있지만, 브라우저가 POST를 전송할 때 지원해야 하는 타입은 다음과 같습니다.

- application/x-www-form-urlencoded
- multipart/form-data

## application/x-www-form-urlencoded

일반적인 브라우저에서 폼에 enctype을 별도로 지정하지 않을 때 이 타입으로 전송됩니다.
다음의 예제는 text 필드에 "Hello 안녕하세요"를 입력하여 전송되는 내용입니다.

    POST /api/save-file HTTP/1.1
    Host: localhost
    Content-Type: application/x-www-form-urlencoded
    cache-control: no-cache
    text=Hello+%EC%95%88%EB%85%95%ED%95%98%EC%84%B8%EC%9A%94

key=value&key=value 형태로 전송되며, 영숫자가 아닌 문자는 퍼센트 기호 및 문자의 ASCII 코드를 나타내는 두 개의 16 진수로 변환됩니다.
즉, 우리가 전송하려는 데이터가 영숫자가 아닌 경우 3바이트로 표현하기 때문에 바이너리 파일을 전송할 경우 페이로드를 3배로 만들기에 무척 비효율적입니다.
그렇기 때문에 브라우저에서 파일을 전송할 때 application/x-www-form-urlencoded 은 적합하지 않습니다.
그렇다면 왜 항상 multipart/form-data 데이터로 전송하지 않을까요?
이어서 알아보겠습니다.

## multipart/form-data

같은내용을  multipart/form-data 로 전송한 값입니다.

    POST /api/save-file HTTP/1.1
    Host: localhost
    Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
    cache-control: no-cache
    
    Content-Disposition: form-data; name="text"
    
    Hello 안녕하세요
    ------WebKitFormBoundary7MA4YWxkTrZu0gW--

application/x-www-form-urlencoded 형태에 비해 복잡해 보입니다.
영숫자가 아닌 문자는 3바이트로 표현해야 하는 application/x-www-form-urlencoded에 비해 페이로드에 많은 옵션을 제공하기 때문입니다.
덕분에 바이너리 데이터를 효율적으로 전송할 수 있으나 웹에서 많이 사용되는 텍스트로만 이루어진 POST 전송은 오히려 MIME 헤더가 추가되기 때문에 오버 헤드가 발생됩니다.

## 정리

application/x-www-form-urlencoded 은 영숫자가 아닌 데이터를 3바이트로 표현하기 때문에 바이너리 파일을 전송하면 3배로 전송하여 낭비가 발생됩니다.
그렇기 때문에 파일을 전송할 때 multipart/form-data를 사용하지만, MIME 헤더가 추가되는 오버헤드가 발생되기 때문에 텍스트로만 이루어진 데이터 전송은 오히려 비효율적인 것입니다.
따라서 전송되는 데이터의 유형과 양에 따라 효율적인 Content-type을 선택해야 합니다.