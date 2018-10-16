---
layout: post
comments: true
title: '[JS] javaScript - 유용한 Snippet 1'
categories: [js]
tags: [javascript]
date: 2018-09-21
---
![headerimg](/assets/img/subcate/javascript.gif)
`javaScript`를 사용하며 만들어본 유요한 함수들을 정리합니다.
{:.center}

## Snippet


<br>
**1. Textarea 줄바꿈 제한 및 자동수정**
~~~javascript
    $('text').keydown(function (){
        var rows = $('text').val().split('\n').length;
        var maxRows = 10;
        if( rows > maxRows) {
            alert('줄바꿈을 조금 줄여주시겠어요?ㅠ');
            modifiedText = $('text').val().split("\n").slice(0, maxRows);
            $('text').val(modifiedText.join("\n"));
        }
    });
}

~~~
Jquery를 사용하였고 maxRows만 지정해 사용한다.

<br>
**2. 배열을 특정 개수로 나누기**
~~~javascript
    let arr = [
    { a: 0 },
    { a: 1 },
    { a: 2 },
    { a: 3 },
    { a: 4 },
    { a: 5 },
    { a: 6 },
    { a: 7 },
    { a: 8 },
    { a: 9 },
    { a: 10 },
    { a: 11 }
    ];

    const sliceArray = (target, each) => {
      const slicedPosts: any[] = [];
      const section = Math.floor(target.length / each) + 1;

      Array(section)
        .fill(null)
        .forEach((_, i) => {
          const result = target.slice(i * each, (i + 1) * each);
          if (result.length !== 0) {
            slicedPosts.push(result);
          }
        });

      return slicedPosts;
    };

    console.log(sliceArray(arr, 2));
~~~
each수에 따라 배열을 쪼개고 나머지는 마지막 인덱스로 삽입한다.


<br>
**3. 글 생략해서 보여주기**
~~~javascript
function ellipsis(text: string, maxLength: number) {
  const textArr = text.split('\n');
  if (textArr.length >= 2) {
    return textArr[0].length > maxLength
      ? `${textArr[0].slice(0, maxLength)}...`
      : textArr[1].length > maxLength
        ? `${textArr[0]}\n${textArr[1].slice(0, maxLength)}...`
        : `${textArr[0]}\n${textArr[1]}`;
  }
  return text.length <= maxLength ? text : `${text.slice(0, maxLength)}...`;
}
~~~
2줄이상 또는 text가 maxLength를 넘으면 ...으로 표현한다.


