---
layout: post
title:  "자바 기초 상식 정리"
date:   2016-07-18 00:00:00 +0900
categories: dev
tags: 
- java
---

# Java

## 제어문

### 조건문
- if ~ else if ~ else 문

~~~
if(조건식1){

}else if(조건식2){

}else{

}
~~~
{: .language-java}


- switch ~ case 문

~~~
switch(조건식){  
	case 상수1:  
	break;  

	case 상수2:  
	break;  

	default:  
	break;  
}
~~~
{: .language-java}

### 반복문
- for문  

~~~
for(서식값 ; 조건식 ; 증감식){}
~~~
{: .language-java}

- while문  

~~~
while(조건식){}
~~~
{: .language-java}

- do-while문  

~~~
do{

}while(조건식);
~~~
{: .language-java}

- break문  
loop를 중단하고 빠져나갈 때 사용

- continue문  
반복문 수행 후 continue 키워드를 만나면, 해당 회차의 반복을 그만하고 다음회차로 넘어간다.


## 자료형

### 기본자료형 및 형변환
- 기본 데이터 타입  
boolean / byte / short / int / long / float / double / char
- 자동형변환  
작은범위의 데이터 타입에 저장된 값은 큰 범위의 데이터 타입에도 저장된다.
- 강제형변환(Casting)  
큰 범위의 데이터 타입에 저장된 값은 작은 범위의 데이터 타입에 저장하려고 할 때, 값이 잘려서 소실될 가능성이 있다.


### 변수의 Scope

### Primitive Type & Reference Type
- Primitive Type  
boolean / byte / short / int / long / float / double / char
- Wrapper Class  
반드시 객체를 써야할 때 사용하도록 함(예. collection의 generics)
- Boxing : Primitive 타입 -> Wrapper 객체
- Unboxing : Wrapper 객체 -> Primitive 타입

### 배열
- 배열 선언 및 생성

~~~
DataType[] 변수명;  
변수명 = new DataType[저장공간 개수];
~~~
{: .language-java}

- 배열 선언 시 값 대입  

~~~
DataType[] 변수명 = {값1, 값2, 값3, ...};
~~~
{: .language-java}


## 객체와 클래스
- 캡슐화(Encapsulation)    
접근제한자 이용한 캡슐화 구현
- 상속성(Enheritance)    
클래스 간 부모-자식관계, 재사용성
- 다형성(Polymorphism)  
동일한 형태로 여러가지 기능을 사용할 수 있음
- 추상화(Abstraction)  
복잡한 현실세계를 시스템에 중요한 내용 위주로 단순화 시킴

  
### this와 super
- this
	- 자기 자신, 내 객체 라는 의미
	- 내 객체가 가지고 있는 멤버에 접근할 수 있는 키워드
- super
	- 부모객체 라는 의미
	- 부모의 멤버에 접근할 수 있는 키워드
	- 메소드 오버라이딩을 할 경우, 부모 메소드 내용도 실행하고자 할 때, super키워드를 이용할 수 있다.
- super 키워드 생성자
	- 자식 클래스에서 부모 멤버를 사용하기 때문에 부모생성자를 먼저 호출
	- 자식클래스에서 부모의 생성자를 호출하기 위해 super()가 필요함
	- 명시적으로 부모 생성자 호출하지 않는 경우, 컴파일러가 자동으로 super()추가
	- super키워드 사용해서 생성자 호출 할 경우, 생성자 첫번째 라인에 기술
	- super()와 this() 같이 호출 안됨 

### 객체의 생성과 초기화
- 생성자
	- 클래스를 이용하여 객체를 만드는 역할
	- new + 클래스명() : 생성자 호출, 객체 생성
	- 생성자 오버로딩으로 여러개 생성자 만들 수 있음
- 기본 생성자
	- 매개변수가 없고 내용이 없는 생성자
	- 클래스에 생성자가 하나도 없으면 자동으로 기본생성자 추가
	- 파라미터 있는 생성자가 하나라도 있다면, 자동으로 기본생성자 만들지 않음


### 접근제한자 및 기타제한자
- package : 관련있는 클래스들의 집합
	- 클래스파일 맨 위에 한번만 작성
	- 패키지 이름으로 폴더가 만들어지고 그 위치에 저장
- import : 다른 패키지에 있는 클래스/멤버가 필요할 때 사용
	- package선언문 / class 선언문 앞에 작성, 여러번 import 가능
- public
	- 누구나 접근가능한 필드, 멤버 메소드 선언
	- public으로 선언된 멤버는 다른 클래스, 다른 package에서 사용가능
- protected
	- 같은 package 내 클래스에서 접근 가능
	- 다른패키지의 상속관계의 자식 클래스에서도 접근 가능
- default
	- 접근제한자를 쓰지 않는 경우
	- 같은 package 내의 클래스에서 접근 가능
- private
	- 다른 클래스에서 접근할수 없는 멤버에 사용
	- 상속관계의 자식 클래스도 사용불가

### Call by value & Call by reference

### 메소드 오버로딩 / 오버라이딩
- 메소드 오버로딩 (다중정의)  
	- 한 클래스 내에 비슷한 기능의 메소드를 같은 이름으로 여러 개 정의하는 것
	- 메소드 이름 같아야 함
	- 파라미터 개수, 데이터 타입, 순서가 달라야 함
	- 메소드 리턴타입과는 상관 없음
- 메소드 오버라이딩 (재정의)
	- 부모 클래스의 메소드 내용을 자식 클래스에서 다시 정의하는 것
	- 메소드 이름 같아야 함
	- 파라미터 개수, 데이터 타입 같아야 함
	- 메소드 리턴타입 같아야 함
	- 접근제어자는 부모의 것과 같거나 더 넓은 범위여야 함 
	- 부모클래스 메소드가 private, final일 경우 오버라이딩 불가


## 상속과 다형성

### 상속
- 부모 클래스의 멤버를 자식 클래스에서 사용할 수 있는 것
- 각 클래스에서 공통적으로 반복되는 부분을 묶어서 독립된 클래스를 만든다. 그리고 그 클래스를 상속받는다.
- is-a 관계가 성립될 때에만 상속관계를 맺는다.

> Student is a Person (O)  
> Teacher is a Person (O)  
> Staff is a Person (O)  
> Dog is a Person (X)

- 자식 클래스 선언 시, `extends` 키워드 이용하여 부모클래스 상속받음

~~~
public class 자식클래스명 extends 부모클래스명 {}
~~~
{: .language-java}

### 다형성
- 동일한 형태로 여러가지 기능을 사용할 수 있음
- 보모의 객체 타입으로 다양한 형태의 자식 객체를 생성할 수 있는 것

~~~
부모클래스 변수명 = new 자식클래스();
~~~
{: .language-java}

- 동일한 작업을 반복할 때 유용하다.

~~~
Person[] people = {new Person(), new Student(), new Teacher()};
for(Person p : people){
	p.eat();
	p.sleep();
}
~~~
{: .language-java}

### 인터페이스
- 상수와 추상메소드로 구성
- 인터페이스 자체로는 객체 생성 불가, 반드시 인터페이스 구현하는 클래스를 통해 객체 생성해야 함

~~~
public interface 인터페이스명{  
	public final static DataType 상수명 = 상수값;  
	public ReturnType 메소드명 (인자1, 인자2, ...);  
}
~~~
{: .language-java}

- 인터페이스 구현 시 `implements`키워드 이용

~~~
public class 클래스명 implements 인터페이스명{}
~~~
{: .language-java}

- 인터페이스는 여러 개를 구현할 수 있다.

### 추상메소드
- 메소드의 구현체가 없는 것, 메소드의 선언부만 있다.
- 추상메소드는 반드시 하위 클래스에서 구현해야 한다.

### 추상클래스
- 클래스 선언 시, abstract 키워드를 이용하는 클래스

~~~
public abstract class 클래스명{}
~~~
{: .language-java}

- 추상클래스로 객체 생성 불가


## Java API

### 컬랙션의 개념 및 활용
- 배열의 장단점
	- 장점 : 모든데이터를 저장가능, 사용이 편리
	- 단점 : 처음 지정한 크기에서 공간의 크기 변경할 수 없음
- 데이터를 유동적으로 다루기 위해 컬랙션 사용
	- 장점 : 자동으로 크기를 조절할 수 있으며, 명시적 이름의 메소드 사용
	- 단점 : 배열에 비해 사용법이 복잡한 편    
- 주요 Collection
	- List계열 : ArrayList, LinkedList, Vector
		- 저장되는 순서가 유지
	- Map계열 : HashMap, LinkedHashMap, HashTable
		- 저장되는 순서가 유지되지 않음.
		- Key-value 구조

### 제너릭스(Generics)
컬랙션에 저장할 객체 타입을 지정할 수 있다.

~~~
ArrayList<데이터타입> 변수명 = new ArrayList<데이터타입>();
~~~
{: .language-java}

- 클래스에 사용할 타입을 설계 시에 지정하는 것이 아니라, 객체 생성시 지정하는 방법
- 컴파일러에게 데이터 타입을 전달
- 캐스팅 필요 없고, 안전한 코드 작성 가능
- 객체만 사용 가능

### 문자열 (String, StringBuffer)

### 표준 입출력 / 파일 입출력
- System.out : 표준 출력(모니터) 스트림

~~~
System.out.println("정보 메시지");
~~~
{: .language-java}

- System.in : 표준 입력(키보드) 스트림

~~~
int i = System.in.read();    //1byte 입력
~~~
{: .language-java}
 
- System.err : 표준 에러출력(모니터) 스트림. 콘솔창에만 출력

~~~
System.err.println("에러 메시지");
~~~
{: .language-java}
 
- FileInputStream / FileOutputStream : 바이트 단위로 파일 읽고 쓸 때 사용
- FileReader / FileWriter : 문자단위로 파일 읽고 쓸 때 사용  


## 예외처리

### Exception 클래스
- Unchecked Exception : 컴파일 시 문제 없으나, 프로그램 실행 중 발생하는 Exception  
(ex. NullPointerException, IndexOutOfBoundsException..)
- Checked Exception : 코드 작성 후, 컴파일 시 예외처리 해주었는지 체크하는 Exception  
(ex. EOFException, FileNotFoundException..)

### 예외처리 문법
- try-catch-finally 구문

~~~
try{  
	//Exception 발생할 가능성있는 구문
}catch(하위 Exception클래스타입1번 변수명1){
	//Exception 발생했을 경우 수행하는 문장
}catch(상위 Exception클래스타입2번 변수명2){
	//Exception 발생했을 경우 수행하는 문장
}finally{
	//Exception발생 유무와 상관없이 수행해야 하는 문장  
}
~~~
{: .language-java}

- throws 구문 : Exception이 발생한 곳에서 예외처리 하지 않고, 상위 메소드로 위임 

~~~
public ReturnType 메소드명(인자1,...) throws ExceptionType1, ExceptionType2, ... {} 
~~~
{: .language-java}

- throw new Exception() : 강제로 Exception 발생시켜서 예외상황 발생시킴

~~~
throw new Exception("강제 예외 발생");
~~~
{: .language-java}


## 자료추출

### Parsing

- 어떤 문장을 분석하거나 문법적 관계를 해석하는 행위

### XML Parser
- DOM Parser : 문서를 객체 형태로 접근하는 트리기반 방식
	- 장점 : 검색/수정/삭제 용이
	- 단점 : 문서를 다 읽어야 처리할 수 있어서 메모리 많이 사용, 속도가 느림
	
- SAX Parser : 문서를 순차적으로 처리하는 이벤트 기반의 방식
	- 장점 : 적은 메모리로 빠르게 처리, 대용량/대규모 문서처리에서 장점
	- 단점 : 문서구조 파악 불가, 파싱결과를 저장하지 않으므로 매번 다시 파싱
	
- PULL Parser : 문서를 순차적으로 원하는 부분까지 파싱할 수 있는 이벤트 기반 방식
	- 장점 : 원하는 부분까지만 파싱 가능, 낮은 메모리 사용
	- 단점 : SAX보다 느림, 문서구조 파악불가, 매번 다시 파싱


## 병렬처리

### 병렬처리

### 일관성 유지


## 기타

### Annotation
- @으로 시작하며, 컴파일러가 검증해야 하는 부가정보 추가하는 일종의 주석
- 필요한 데이터를 쉽게 활용할 수 있도록 소스코드에 추가로 작성하는 정보

> @overide : 해당 메소드가 오버라이딩 메소드가 아닌경우, 컴파일러가 에러를 발생한다.

### final
- final클래스 : 자식클래스를 만들 수 없음
- final메소드 : 자식클래스에서 오버라이딩 할 수 없음
- final변수 : 값을 변경할 수 없음(상수)


# Framework

## Framework

### Controller / Biz / Dao

### Transaction


# UI개발

## Javascript

### Javascript기본

### DOM처리
