---
layout: post
title:  "security_ Secure coding"
author: fennec-fox
tags: Security
subtitle: "   "
category:  security


---

<br>

<br>

# Secure coding

정해진 룰에 따라서 코드를 작성하면 특정 취약점을 제거 할 수 있다.

---> 특정 취약점이 발생하지 않도록 코드를 작성하는 규칙의 모음

- 규칙 모음 사이트 

  -  [https://wiki.sei.cmu.edu/confluence/](https://wiki.sei.cmu.edu/confluence/)

  - 소프트웨어 개발보안 가이드 : 47개의 보안약점 제거를 위한 개발 보안 활동을 분석, 설계 단계까지 확대
  - [http://www.kisa.or.kr/public/laws/laws3.jsp](http://www.kisa.or.kr/public/laws/laws3.jsp)

<br>

- 예를 들자,

  1. `request.getParameter` 사용법

  ```java
  
  request.getParameter(param_name)
  요청 파라미터 목록에서 param_name에 해당하는 파라미터의 값을 문자열로 반환하는 메소드. 단, 요청 파라미터 목록에 해당 파라미터가 존재하지 않으면 null을 반환.
  
  예) request.jsp?first_name=hong&last_name=
  request.getParameter("first_name") ⇒ "hong"
  request.getParameter("last_name")  ⇒ ""
  request.getParameter("age")        ⇒ null
  
  ```

  <br>

  2. 기본코드 : null 값으로 인한 오류발생 여지 있음

  ```java
  
  String input = request.getParameter("age");
  if (input.equals("32")) { … }
  
  // 요청 파라미터 목록에 age라는 파라미터가 존재하지 않을 경우, 널 포인트 역참조가 발생
  
  ```

  <br>

  3. 널 포인트 역참조가 발생하지 않도록 코딩하는 방법은? ⇒ Secure coding

  ```java
  
  // 널이 될 수 있는 변수는 반드시 널 여부를 확인한 후 참조한다.
  if (input != null & input.equals("32")) { … } 
  
  // 상수와 변수를 비교할 경우, 상수를 기준으로 한다. 
  if ("32".equals(input)) { … }
  
  ```

  <br>

  <br>

  # 구현단계 보안약점

  1. 입력데이터 검증 및 표현
  2. 보안 기능
  3. 시간 및 상태
  4. 에러 처리
  5. 코드 오류
  6. 캡슐화
  7. API오용

  <br>

  ### 3. 시간 및 상태

  - 종료되지 않는 반복문 또는 재귀함수

  ```java
  
  public class Factorial {
  	
  	// 10! = 10 * 9 * 8 * ... * 1
  	public static int factorial(int i) {
  		System.out.println(i);
  		if (i <= 1) {         // 귀납조건(base-case)
  			return i;
  		}
  		return i * factorial(i-1);		
  	}
  	
  	public static void main(String[] args) {
  		int result = factorial(10);
  		System.out.println(result);
  	}
  
  }
  
  
  ```

  - 반복을 제어해 주지 않으면 스택오버플로우가 발생함

  <br>

  ### 5. 코드오류

  ```java
  
  // [ 오류 유형 ] 
  
  1. 문법 오류(Syntax Error) = 컴파일 오류
   	String input = null;
  	if (input.equals("32")) { … }
  2. 실행 오류(Runtime Error) = 실행 시점에 결정된 값으로 인해 발생하는 오류
  	String input = request.getParameter("age");
  	if (input.equals("32")) { … }
  
  ```

  

  <br>

  

  <https://www.gpgstudy.com/forum/viewtopic.php?t=1873>

