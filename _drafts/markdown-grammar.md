---
layout: post
title:  "마크다운 문법"
date:   2016-07-18 00:00:00 +0900
categories: dev
tags: 
- markdown
---

# Markdown

---

## 마크다운이란?
**마크다운**(markdown)은 일반 텍스트 문서의 양식을 편집하는 문법이다.
README 파일이나 온라인 문서, 혹은 일반 텍스트 편집기로 문서 양식을 편집할 때 쓰인다.

> 핵심 : Text만으로 구조적인 글을 작성하는 문법이다.

## 예제
- 제목   
`# 큰제목` 또는 `큰제목+엔터키+=` (HTML의 H1태그)  
`## 중간제목` 또는 `중간제목+엔터키+-` (HTML의 H2태그)  
`### 작은제목` (HTML의 H3태그를 의미)

- 이탤릭체  
`*이탤릭체*` 또는 `_이탤릭체_` (HTML의 em태그)

- 볼드체  
`**볼드체**` 또는 `__볼드체__` (HTML의 strong태그)

- 인라인 하이퍼링크 
`[텍스트](http://lattekafe.com "(optional)사이트제목"`

- 참조 하이퍼링크  
여러 위치에 동일 참조명으로 하이퍼링크를 달 수 있다.  
`[텍스트][참조명]`  
문서 마지막에 다음을 추가  
`[참조명]: http://lattekafe.com`

- 이미지  
`![텍스트](http://lattekafe.com/xxx.png)`

- 블럭 인용  
`>문단을 입력합니다.`

- 순서가 부여된 리스트  
`1. 첫번째 2. 두번째 3. 세번째`  
`1. 첫번째 1. 첫번째 1. 첫번째` 처럼 입력해도 순서는 제대로 부여된다.

- 순서 없는 리스트  
`- 첫번째 - 두번째 - 세번째`  
`- / + / *` 기호 중 하나를 사용하면 된다.

- 코드  
\`코드 내용 입력\`

- 코드블럭  
    스페이스 4개 이상 삽입하면 코드블럭을 입력할 수 있다.


## 문장쓰기 예제

## 문법

### 문장
마크다운에서 엔터키 한 번은 같은 문장이 된다. 
`엔터키`
엔터를 한번 입력했지만 줄바꿈은 안된다.
`엔터키``엔터키`

줄바꿈이 된 것을 확인할 수 있다. 

줄바꿈은 빈 칸 두개로 구분한다.
`스페이스``스페이스`.    줄바꿈이 되었다.

### 제목
큰제목`=`
=
중간제목`-`
-

*이탤릭체*
_이탤릭체_

**볼드체**
__볼드체__

[하이퍼링크](http://lattekafe.com)
[하이퍼링크](http://lattekafe.com "라떼카페")

>인용문


- ㅅㅅㅅ
- ㅊㅊㅊ

그냥아무글이나...

1. ㄴㅇㅁㄹ



[http://nolboo.kim/blog/2014/04/15/how-to-use-markdown/](http://nolboo.kim/blog/2014/04/15/how-to-use-markdown/)

[http://blog.kalkin7.com/2014/02/10/lets-write-using-markdown/](http://blog.kalkin7.com/2014/02/10/lets-write-using-markdown/)

[https://ko.wikipedia.org/wiki/%EB%A7%88%ED%81%AC%EB%8B%A4%EC%9A%B4](https://ko.wikipedia.org/wiki/%EB%A7%88%ED%81%AC%EB%8B%A4%EC%9A%B4)
