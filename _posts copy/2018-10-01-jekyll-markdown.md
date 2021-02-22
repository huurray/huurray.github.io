---
layout: post
comments: true
title: '[DEVLOG] 마크다운 문법 총정리'
categories: [devlog]
tags: [jekyll]
date: 2018-10-01
---
![headerimg](/assets/img/subcate/jekyll-head.png)
Markdown 문법을 정리하다.
{:.center}


Github나 Jekyll블로그를 사용하면 **마크다운(Markdown)**을 사용하여 글을 작성하는 경우가 아주 많죠.
그래서 그럴때마다 참고할 수 있는 마크다운 문법을 정리하려고 합니다.

## 제목(Heading)
제목부분은 HTML의 `<h1>`~`<h6>` 태그와 비슷합니다. #의 개수에 따라 글자의 크기가 달라집니다.
<br> `#`은 `<h1>`, `###`은 `<h3>` `######`은 `<h6>`인거죠.

```
# Heading
### Heading
###### Heading
```

# Heading
### Heading
###### Heading


## 본문(paragraph)
그렇다면 HTML의 `<p>`와 같은 본문은 어떻게 작성할까요?<br>
텍스트를 그대로 작성하면 됩니다.

```markdown
Lorem ipsum dolor sit amet, consectetur adipisicing elit
```
Lorem ipsum dolor sit amet, consectetur adipisicing elit


## 인용(Blockquotes)
인용은 `>`를 넣어서 작성합니다.
```markdown
> Lorem ipsum dolor sit amet, consectetur adipisicing elit
>> Lorem ipsum dolor sit amet, consectetur adipisicing elit

```
> Lorem ipsum dolor sit amet, consectetur adipisicing elit
>> Lorem ipsum dolor sit amet, consectetur adipisicing elit


## 리스트
### 순서가 없는 리스트(Unordered List)
`*` 또는 `-`를 사용해서 순서가 없는 리스트를 작성할 수 있습니다. tab 또는 2칸 띄어쓰기를 통해 중첩된 항목을 작성할 수 있습니다.

```markdown
* Frontend
  * HTML
  * CSS
  * JavaScript
    * React.js

- Frondend
  - HTML
  - CSS
  - JavaScript
    - React.js
```

* Frontend
  * HTML
  * CSS
  * JavaScript
    * React.js

- Frondend
  - HTML
  - CSS
  - JavaScript
    - React.js

### 순서가 있는 리스트(Ordered List)

```markdown
1. HTML
2. CSS
3. JavaScript
```

1. HTML
2. CSS
3. JavaScript

```markdown
1. HTML
1. CSS
1. JavaScript
```

1. HTML
1. CSS
1. JavaScript

```markdown
1. Frontend
    1. HTML
    2. CSS
    3. JavaScript
        1. React.js
2. Backend
```

1. Frontend
    1. HTML
    2. CSS
    3. JavaScript
        1. React.js
2. Backend


## 코드블럭(Code blocks)
개발자 블로그에서 가장 중요한 코드블럭!

코드블럭의 시작과 끝을 ```으로 감싸고 내부에 코드를 작성하면 됩니다.

```javascript
function square(n) {
  return n * n;
}
```

## 수평선(Horizontal Rules)
문단과 문단 사이를 나눌 때 등 사용되는 수평선은 HTML의 <hr />과 같이 동작합니다.

```markdown
* * *
***
*****
- - -
---------------------------------------
```
* * *
***
*****
- - -
---------------------------------------


## 링크(Links)
HTML의 하이퍼링크와 같은 링크는 다음과 같이 작성합니다. 
```markdown
[example](http://example.com)

검색엔진은 [구글](https://www.google.com)을 사용합니다.
```
[example](http://example.com)

검색엔진은 [구글](https://www.google.com)을 사용합니다.


## 강조(Emphasis)
HTML의 `<em>`과 같은 동작을 하는 강조는 `*`, `_`가 있고 `<strong>`은 `**`와 `__`를 사용합니다. 취소선은 `~~`을 사용합니다.

```markdown
*강조*한 텍스트
_강조_한 텍스트

```
*강조*한 텍스트

```markdown
**강조**한 텍스트
__강조__한 텍스트
```

**강조**한 텍스트

```markdown
~~취소~~한 텍스트
```

~~취소~~한 텍스트

## 이미지 삽입(Images)
이미지는 역시 HTML의 `<img>`태그와 동일하게 작동합니다. 대체 택스트를 삽입할 수 있으며, 링크 또는 로컬의 이미지파일을 연결할 수 있습니다.

```markdown
![대체 텍스트](/경로/example.jpg)
![대체 텍스트](링크)
```

```markdown
![Github](./public/img/3/github.png)
```
```markdown
![Github](https://assets-cdn.github.com/images/modules/open_graph/github-octocat.png)
```
![Github](https://assets-cdn.github.com/images/modules/open_graph/github-octocat.png)


## 참고문헌
* [존 그루버의 Markdown: Syntax](https://daringfireball.net/projects/markdown/syntax)


