# JavaScript를 이용해서 Node.js를 제어해 동적으로 HTML 코드를 생성하는 웹애플리케이션

- 프로그래밍 언어 JavaScript 를 이용해서, Node.js의 기능을 제어해, 웹 애플리케이션 구동

---

## JavaScript와 Node.js

JavaScript와 Node.js는 둘 다 JavaScript 프로그래밍 언어와 관련이 있지만, 각각 다른 용도로 사용됩니다.

JavaScript는 주로 웹 프론트엔드 개발에 사용되는 스크립트 언어입니다. 웹 페이지에 상호작용성(interactivity)과 기능성(functionality)을 추가하는 데 사용되며, 웹 브라우저 내에서 실행됩니다. JavaScript 코드는 브라우저에서 해석되어 클라이언트 측에서 실행되므로, 사용자의 장치에서 실행됩니다.

반면, Node.js는 JavaScript 코드를 실행하는 서버 측 플랫폼입니다. 개발자들은 JavaScript를 웹 응용 프로그램의 백엔드에서 사용할 수 있게 되며, 파일 I/O, 네트워크 요청 및 데이터베이스 액세스와 같은 작업을 처리할 수 있습니다. Node.js는 서버에서 실행되며 확장 가능한 웹 응용 프로그램 및 API를 구축하는 데 사용될 수 있습니다. Node.js는 Google의 V8 JavaScript 엔진 위에 구축되어 JavaScript 코드를 기계 코드로 컴파일하여 해석되는 JavaScript 코드보다 더 빠르게 실행됩니다.

요약하면, JavaScript는 주로 웹 프론트엔드 개발에 사용되고, Node.js는 백엔드 개발에 사용되며, 웹 응용 프로그램의 서버 측에서 JavaScript를 사용할 수 있도록 합니다.


---

## Node.js를 웹서버로 구동

Node.js는 웹서버 기능을 가지고 있습니다. 

이런 특성을 이용해서 컨텐츠를 프로그래밍적으로 생산할 수 있게 됩니다. 

여기서는 Node.js를 웹서버로 구동하는 방법을 살펴보겠습니다. 

 

---

```.js
var http = require('http');
var fs = require('fs');
var app = http.createServer(function(request,response){
    var url = request.url;
    if(request.url == '/'){
      url = '/index.html';
    }
    if(request.url == '/favicon.ico'){
      return response.writeHead(404);
    }
    response.writeHead(200);
    
    console.log(__dirname + _url);
    
    response.end(fs.readFileSync(__dirname + url));
 
});
app.listen(3000);

```

---

## URL에 포함된 쿼리 스트링 해석, 처리


- protocol

- host domain name

- port #

- path

- query string


주어진 코드에서 url은 Node.js에서 기본적으로 제공하는 URL을 다루는 모듈을 의미하고, _url은 request.url 속성의 값을 저장하는 변수입니다.

즉, url 모듈은 URL을 다루는데 필요한 함수들을 제공하며, request.url은 현재 요청이 들어온 URL을 나타냅니다. 

request.url 값은 URL의 프로토콜, 호스트 이름, 경로, 쿼리 파라미터 등의 구성 요소를 포함합니다. 

_url 변수는 이러한 요청 URL 값을 저장하여 다른 부분에서 사용할 수 있도록 합니다.

따라서, _url 변수에 저장된 값은 url.parse() 함수를 사용하여 필요한 경우 URL을 구성 요소로 분해하거나, URL 문자열을 비교하는 등의 작업을 수행할 수 있습니다.

```.js

var http = require('http');
var fs = require('fs');
var url = require('url');


var app = http.createServer(function(request,response){
    var _url = request.url;
    
    var queryData = url.parse(_url, true).query;

    if(_url == '/'){
      _url = '/index.html';
    }
    if(_url == '/favicon.ico'){
      return response.writeHead(404);
    }
    response.writeHead(200);
    
    console.log(_url);

    console.log(queryData.id);

    response.end(queryData.id);
 
});
app.listen(3000); 


```

console.log(_url);

- `/?id=CSS`

console.log(queryData.id);
    
- `CSS`


---

## 동적 웹페이지 생성

```.html

var http = require('http');
var fs = require('fs');
var url = require('url');
 
var app = http.createServer(function(request,response){
    var _url = request.url;
    var queryData = url.parse(_url, true).query;

    var title = queryData.id;
    
    if(_url == '/'){
      title = 'Welcome';
    }
    if(_url == '/favicon.ico'){
      return response.writeHead(404);
    }
    response.writeHead(200);
    
    var template = `
    <!doctype html>
    <html>
    <head>
        <title>WEB1 - ${title}</title>
        <meta charset="utf-8">
    </head>
    
      <body>
        
      <h1><a href="/">WEB</a></h1>

        <ul>
          <li><a href="/?id=HTML">HTML</a></li>
          <li><a href="/?id=CSS">CSS</a></li>
          <li><a href="/?id=JavaScript">JavaScript</a></li>
        </ul>

        <h2>${title}</h2>
        <p><a href="https://www.w3.org/TR/html5/" target="_blank" title="html5 speicification">Hypertext Markup Language (HTML)</a> is the standard markup language for <strong>creating <u>web</u> pages</strong> and web applications.Web browsers receive HTML documents from a web server or from local storage and render them into multimedia web pages. HTML describes the structure of a web page semantically and originally included cues for the appearance of the document.
        <img src="coding.jpg" width="100%">
        </p><p style="margin-top:45px;">HTML elements are the building blocks of HTML pages. With HTML constructs, images and other objects, such as interactive forms, may be embedded into the rendered page. It provides a means to create structured documents by denoting structural semantics for text such as headings, paragraphs, lists, links, quotes and other items. HTML elements are delineated by tags, written using angle brackets.
        </p>
      </body>

    </html>
    `;

    response.end(template);
 
});
app.listen(3000);


```

----

## Node.js 파일 읽기


```.js

var fs = require('fs');

fs.readFile('sample.txt', 'utf8', function(err, data){
  console.log(data);
});


```

---

## 파일을 이용해 본문 구현

- 파일에 본문을 저장하고, Node.js의 파일 읽기 기능(fs.readFile)을 이용해서 본문을 생성


```.js


var http = require('http');
var fs = require('fs');
var url = require('url');
 
var app = http.createServer(function(request,response){
    var _url = request.url;
    var queryData = url.parse(_url, true).query;

    var title = queryData.id;
    
    if(_url == '/'){
      title = 'Welcome';
    }
    if(_url == '/favicon.ico'){
      return response.writeHead(404);
    }
    response.writeHead(200);
    fs.readFile(`data/${queryData.id}.txt`, 'utf8', function(err, description){
      var template = `
      
    <!doctype html>
    <html>
    <head>
        <title>WEB1 - ${title}</title>
        <meta charset="utf-8">
    </head>
    
      <body>
        
      <h1><a href="/">WEB</a></h1>

        <ul>
          <li><a href="/?id=HTML">HTML</a></li>
          <li><a href="/?id=CSS">CSS</a></li>
          <li><a href="/?id=JavaScript">JavaScript</a></li>
        </ul>

        <h2>${title}</h2>
        <p>${description}</p>
      </body>

    </html>
    `;

    response.end(template);

    })
    
});

app.listen(3000);



```

---

## Node.js - 콘솔에서의 입력값

- 콘솔 환경에서 앱을 실행할 때 입력 값을 전달하는 방법

```.js
var args = process.argv;
console.log(args);
```


```
(base) daeheehan@DAEHEEui-MacBookPro 0.WEB2-Node.js % node inout.js hello world
[
  '/usr/local/bin/node',
  '/Users/daeheehan/Desktop/0.WEB2-Node.js/inout.js',
  'hello',
  'world`
]
```


---

##  Not found 오류 구현

- pathname

- path

 - pathname: '/',
 
 - path: '/?id=HTML',
  

```.js
var http = require('http');
var fs = require('fs');
var url = require('url');
 
var app = http.createServer(function(request,response){
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    
    var pathname = url.parse(_url, true).pathname;
    
    var title = queryData.id;

    console.log(url.parse(_url, true))

    console.log(pathname)

    if(pathname === '/'){
      fs.readFile(`data/${queryData.id}.txt`, 'utf8', function(err, description){
        
        var template = `
        <!doctype html>
        <html>
        <head>
          <title>WEB1 - ${title}</title>
          <meta charset="utf-8">
        </head>
        <body>
          <h1><a href="/">WEB</a></h1>
          <ul>
            <li><a href="/?id=HTML">HTML</a></li>
            <li><a href="/?id=CSS">CSS</a></li>
            <li><a href="/?id=JavaScript">JavaScript</a></li>
          </ul>

          <h2>${title}</h2>
          <p>${description}</p>

        </body>
        </html>
        `;

        response.writeHead(200);
        response.end(template);

      });
    } else {
      response.writeHead(404);
      response.end('Not found');
    }
 
 
 
});
app.listen(3000);

```

---

## 홈페이지 구현

- 조건문을 활용해서 홈 구현

- `undefined

- pathname 

```.js
var http = require('http');
var fs = require('fs');
var url = require('url');
 
var app = http.createServer(function(request,response){
    
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var pathname = url.parse(_url, true).pathname;
    
    if(pathname === '/'){
      if(queryData.id === undefined){
        fs.readFile(`data/${queryData.id}.txt`, 'utf8', function(err, description){
          
          var title = 'Welcome';
          
          var description = 'Hello, Node.js';
          
          var template = `
          <!doctype html>
          <html>
          <head>
            <title>WEB1 - ${title}</title>
            <meta charset="utf-8">
          </head>
          <body>
            <h1><a href="/">WEB</a></h1>
            <ul>
              <li><a href="/?id=HTML">HTML</a></li>
              <li><a href="/?id=CSS">CSS</a></li>
              <li><a href="/?id=JavaScript">JavaScript</a></li>
            </ul>
            <h2>${title}</h2>
            <p>${description}</p>
          </body>
          </html>
          `;
          response.writeHead(200);
          response.end(template);
        });
      } else {
        fs.readFile(`data/${queryData.id}.txt`, 'utf8', function(err, description){
          
          var title = queryData.id;
          
          var template = `
          <!doctype html>
          <html>
          <head>
            <title>WEB1 - ${title}</title>
            <meta charset="utf-8">
          </head>
          <body>
            <h1><a href="/">WEB</a></h1>
            <ul>
              <li><a href="/?id=HTML">HTML</a></li>
              <li><a href="/?id=CSS">CSS</a></li>
              <li><a href="/?id=JavaScript">JavaScript</a></li>
            </ul>
            <h2>${title}</h2>
            <p>${description}</p>
          </body>
          </html>
          `;
          response.writeHead(200);
          response.end(template);
        });
      }
    } else {
      response.writeHead(404);
      response.end('Not found');
    }
});
app.listen(3000);

```

---

## Node.js - 파일 목록 알아내기

- Node.js에서 특정 디렉토리 하위에 있는 파일과 디렉토리의 목록을 알아내는 방법


```.js

var testFolder = './data';
var fs = require('fs');
 
fs.readdir(testFolder, function(error, filelist){
  console.log(filelist);
})


```

---

## 글목록 출력

- data 디렉토리에 있는 파일들의 이름을 이용해서 글 목록을 생성하는 기능 구현


```.js


var http = require('http');
var fs = require('fs');
var url = require('url');
 
var app = http.createServer(function(request,response){
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var pathname = url.parse(_url, true).pathname;
    
    if(pathname === '/'){
      if(queryData.id === undefined){
 
        fs.readdir('./data', function(error, filelist){
          
          var title = 'Welcome';
          var description = 'Hello, Node.js';
          
          var list = '<ul>';
          var i = 0;
          while(i < filelist.length){
            list = list + `<li><a href="/?id=${filelist[i]}">${filelist[i]}</a></li>`;
            i = i + 1;
          }

          list = list+'</ul>';

          var template = `
          <!doctype html>
          <html>
          <head>
            <title>WEB1 - ${title}</title>
            <meta charset="utf-8">
          </head>
          <body>
            <h1><a href="/">WEB</a></h1>
            ${list}
            <h2>${title}</h2>
            <p>${description}</p>
          </body>
          </html>
          `;
          response.writeHead(200);
          response.end(template);
        })
 
 
 
      } else {
        fs.readdir('./data', function(error, filelist){
          
          var list = '<ul>';
          var i = 0;
          while(i < filelist.length){
            list = list + `<li><a href="/?id=${filelist[i]}">${filelist[i]}</a></li>`;
            i = i + 1;
          }
          
          list = list+'</ul>';
          
          fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description){
            var title = queryData.id;
            var template = `
            <!doctype html>
            <html>
            <head>
              <title>WEB1 - ${title}</title>
              <meta charset="utf-8">
            </head>
            <body>
              <h1><a href="/">WEB</a></h1>
              ${list}
              <h2>${title}</h2>
              <p>${description}</p>
            </body>
            </html>
            `;
            response.writeHead(200);
            response.end(template);
          });
        });
      }
    } else {
      response.writeHead(404);
      response.end('Not found');
    }
 
 
 
});
app.listen(3000);


```

---

## Refactoring - 함수

- 함수를 이용해서 코드를 정리 정돈


```.js

var http = require('http');
var fs = require('fs');
var url = require('url');
 
function templateHTML(title, list, body){
  return `
  <!doctype html>
  <html>
  <head>
    <title>WEB1 - ${title}</title>
    <meta charset="utf-8">
  </head>
  <body>
    <h1><a href="/">WEB</a></h1>
    ${list}
    ${body}
  </body>
  </html>
  `;
}

function templateList(filelist){
  var list = '<ul>';
  var i = 0;
  while(i < filelist.length){
    list = list + `<li><a href="/?id=${filelist[i]}">${filelist[i]}</a></li>`;
    i = i + 1;
  }
  list = list+'</ul>';
  return list;
}
 
var app = http.createServer(function(request,response){
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var pathname = url.parse(_url, true).pathname;
    
    if(pathname === '/'){
      if(queryData.id === undefined){
        fs.readdir('./data', function(error, filelist){
          var title = 'Welcome';
          var description = 'Hello, Node.js';
          var list = templateList(filelist);
          var template = templateHTML(title, list, `<h2>${title}</h2>${description}`);
          
          response.writeHead(200);
          response.end(template);
        })
      } else {
        fs.readdir('./data', function(error, filelist){
          fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description){
            var title = queryData.id;
            var list = templateList(filelist);
            var template = templateHTML(title, list, `<h2>${title}</h2>${description}`);
            
            response.writeHead(200);
            response.end(template);
          });
        });
      }
    } else {
      
      response.writeHead(404);
      response.end('Not found');
    }
 
 
 
});
app.listen(3000);


```

---

## Node.js - 동기와 비동기 그리고 콜백 

- synchronous 

- asynchronus


```.js

// synchronous & asynchronus

var fs = require('fs');
 
// readFileSync

console.log('A');

var result = fs.readFileSync('sample.txt', 'utf8');
console.log(result);

console.log('C');


// readFile

console.log('A');

fs.readFile('sample.txt', 'utf8', function(err, result){
    console.log(result);
});

console.log('C');


```

---

## Node.js - 패키지 매니저와 PM2

-  패키지 매니저 NPM 

- 실행중인 Node.js 애플리케이션을 관리하는 프로세스 매니저 PM2

`sudo npn install pm2 -g`

`pm2 start main.js`

`pm2 monit`

---

## HTML - Form

- 웹브라우저에서 서버로 데이터를 전송할 때 사용하는 기능 : form 

- HTML로 폼을 만드는 방법

HTML 폼(form)은 HTTP 요청을 보내기 위한 수단으로 사용됩니다. 

HTML form은 'get' 또는 'post'라는 두 가지 메서드를 사용하여 데이터를 서버로 보낼 수 있습니다. 두 방식에는 몇 가지 차이점이 있습니다.

1. 전송되는 데이터 양

'get' 방식은 쿼리 문자열(query string)을 사용하여 데이터를 전송합니다. 이는 URL에 포함되는 데이터이며, 제한된 데이터 양만 보낼 수 있습니다. 일반적으로 URL은 2048자까지 지원합니다.
반면, 'post' 방식은 HTTP 요청 본문(request body)을 사용하여 데이터를 전송하며, 이를 통해 더 많은 양의 데이터를 전송할 수 있습니다.


2. 보안

'get' 방식은 URL에 데이터를 노출하기 때문에 보안에 취약합니다. 예를 들어, 로그인 정보와 같은 중요한 데이터를 'get' 방식으로 전송하면, URL에서 데이터가 노출되어 다른 사람이 이를 볼 수 있습니다.
'post' 방식은 HTTP 요청 본문에 데이터를 포함시키기 때문에, 'get' 방식보다는 보안적입니다. 데이터는 URL에 노출되지 않으며, HTTP 요청 본문은 암호화될 수 있습니다.


3. 캐싱

'get' 방식은 브라우저에서 캐싱될 수 있습니다. 이는 같은 URL을 다시 요청할 때, 브라우저가 캐시된 데이터를 사용하여 요청을 처리하는 것을 의미합니다.
'post' 방식은 브라우저에서 캐싱되지 않습니다. 이는 모든 HTTP 요청이 새로운 것으로 처리되어야 하기 때문입니다.


- 따라서, 'get' 방식은 데이터 양이 작고 보안성이 중요하지 않을 때 사용하며, 'post' 방식은 데이터 양이 많거나 보안성이 중요한 경우에 사용합니다.



### GET

```.html
<form action="http://localhost:3000/process_create" method="get">
    <p><input type="text" name="title"></p>
    <p>
      <textarea name="description"></textarea>
    </p>
    <p>
      <input type="submit">
    </p>
</form>

```

> `http://localhost:3000/process_create?title=sample&description=sample+is... `
---


### POST


```.html
<form action="http://localhost:3000/process_create" method="post">
    <p><input type="text" name="title"></p>
    <p>
      <textarea name="description"></textarea>
    </p>
    <p>
      <input type="submit">
    </p>
</form>`
```

> `http://localhost:3000/process_create`

---

## 글생성 UI 만들기



```.js

else if(pathname === '/create'){ fs.readdir('./data', function(error, filelist){
      
      var title = 'WEB - create';
      var list = templateList(filelist);
      
      var template = templateHTML(title, list, `
      <form action="http://localhost:3000/create_process" method="post">
          <p><input type="text" name="title" placeholder="title"></p>
          <p>
            <textarea name="description" placeholder="description"></textarea>
          </p>
          <p>
            <input type="submit">
          </p>
        </form>
      `);

      response.writeHead(200);
      response.end(template);
    });
   }
    
```

- url parse 시 ,글목록 내 pathname 이 존재 한다면 `if(pathname === '/')`

- create : `else if(pathname === '/create')`


---

## POST 방식으로 전송된 데이터 받기

```.js


var qs = require('querystring');

else if(pathname === '/create_process' ){

      var body = '';
      request.on('data', function(data){
          body = body + data;
      });

      request.on('end', function(){
          var post = qs.parse(body);
          console.log(post);
          
          var title = post.title;
          var description = post.description

      });

      response.writeHead(200);
      response.end('success');

    }
    
    else {
      
      response.writeHead(404);
      response.end('Not found');
    }
 
 
 
});
app.listen(3000);


```

---

## 파일 생성과 Redirection (리다이렉션)

```.js

else if(pathname === '/create_process' ){

      var body = '';
      request.on('data', function(data){
          body = body + data;
      });

      request.on('end', function(){
          var post = qs.parse(body);
          console.log(post);

          var title = post.title;
          var description = post.description

          fs.writeFile(`data/${title}`, description, 'utf8', function(err){
            
            response.writeHead(302, {Location: `/?id=${title}`});
            response.end();
            
          })
      });
    }
    
```


---

## 수정 링크 생성

---

## 수정할 정보 전송

---

## 수정된 내용 저장

---

## 삭제 버튼 구현

---

## 글 삭제

---

## 객체를 이용해서 템플릿 기능 정리

---

## 모듈

---

## 보안

---

## API

---
