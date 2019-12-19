---
title:  "super와 this"
date:   2019-05-25
tags: [java]
toc: true 
---

super, this 키워드는 객체와 상속과 관련 있다.

1. this 키워드는 인스턴스화된 자기 자신의 객체를 지칭한다.
2. super 키워드는 인스턴스화된 부모 객체를 지칭한다.
3. this와 super 키워드는 클래스의 메소드나 클래스 변수와 결합해서 사용한다.

즉, this = 자신이 속한 객체 / super = 자신이 속한 객체의 부모 객체



```java
//사용법

this();  			 //자신 객체의 생성자 호출
this.[변수 이름]; 	  //자신 객체의 변수를 호출
this.[메소드 이름](); //자신 객체의 메소드를 호출

this();
this.age;
this.setAge();


super(); 			//부모 객체의 생성자 호출
super.[변수 이름]; 	 //부모 객체의 변수를 호출
super.[메소드 이름]();//부모 객체의 메소드를 호출

super();
super.age;
super.setAge();
```

