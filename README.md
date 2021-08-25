# 스프링 MVC 2편 - 백엔드 웹 개발 활용 기술

## 1. 타임리프 - 기본 기능

### 1-1. 프로젝트 생성

#### build.gradle

```groovy
plugins {
    id 'org.springframework.boot' version '2.5.4'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id 'java'
}

group = 'hello'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'

configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

test {
    useJUnitPlatform()
}

```

#### index.html

* `src/main/resources/static/index.html`

```html

<html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<ul>
    <li>텍스트
        <ul>
            <li><a href="/basic/text-basic">텍스트 출력 기본</a></li>
            <li><a href="/basic/text-unescaped">텍스트 text, utext</a></li>
        </ul>
    </li>
    <li>표준 표현식 구문
        <ul>
            <li><a href="/basic/variable">변수 - SpringEL</a></li>
            <li><a href="/basic/basic-objects?paramData=HelloParam">기본 객체들</
                a>
            </li>
            <li><a href="/basic/date">유틸리티 객체와 날짜</a></li>
            <li><a href="/basic/link">링크 URL</a></li>
            <li><a href="/basic/literal">리터럴</a></li>
            <li><a href="/basic/operation">연산</a></li>
        </ul>
    </li>
    <li>속성 값 설정
        <ul>
            <li><a href="/basic/attribute">속성 값 설정</a></li>
        </ul>
    </li>
    <li>반복
        <ul>
            <li><a href="/basic/each">반복</a></li>
        </ul>
    </li>
    <li>조건부 평가
        <ul>
            <li><a href="/basic/condition">조건부 평가</a></li>
        </ul>
    </li>
    <li>주석 및 블록
        <ul>
            <li><a href="/basic/comments">주석</a></li>
            <li><a href="/basic/block">블록</a></li>
        </ul>
    </li>
    <li>자바스크립트 인라인
        <ul>
            <li><a href="/basic/javascript">자바스크립트 인라인</a></li>
        </ul>
    </li>
    <li>템플릿 레이아웃
        <ul>
            <li><a href="/template/fragment">템플릿 조각</a></li>
            <li><a href="/template/layout">유연한 레이아웃</a></li>
            <li><a href="/template/layoutExtend">레이아웃 상속</a></li>
        </ul>
    </li>
</ul>
</body>
</html>
```

### 1-2. 타임리프 소개

#### 타임리프 특징

* 서버 사이드 HTML 렌더링(SSR)
* 네츄럴 템플릿
* 스프링 통합 지원

##### 서버 사이드 HTML 렌더링(SSR)

타임리프는 백엔드 서버에서 HTML을 동적으로 렌더링 하는 용도로 사용된다.

##### 네츄럴 템플릿

타임리프는 순수 HTML을 최대한 유지하는 특징이 있다.     
타임리프로 작성한 파일은 HTML을 유지하기 때문에 웹 브라우저에서 파일을 직접 열어도 내용을 확인할 수 있고,      
서버를 통해 뷰 템플릿을 거치면 동적으로 변경된 결과를 확인할 수 있다.        
JSP를 포함한 다른 뷰 템플릿들은 해당 파일을 열면, 예를 들어 JSP 파일 자체를 그대로 웹 브라우저에서 열어보면 JSP 소스코드와 HTML이 뒤죽박죽 섞여서 웹 브라우저에서 정상적인 HTML 결과를 확인할 수
없다.     
오직 서버에를 통해 JSP가 렌더링 되고 HTML 응답 결과를 받아야 화면을 확인할 수 있다.

반면에 타임리프로 작성된 해당 파일을 그대로 웹 브라우저에서 열어도 정상적인 HTML 결과를 확인할 수 있다.       
물론 이 경우 동적으로 결과가 렌더링 되지는 않는다. 하지만 HTML 마크업 결과가 어떻게 되는지 파일만 열어도 바로 확인할 수 있다.         
이렇게 **순수 HTML을 그대로 유지하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징을 네츄럴 템플릿(natural templates)** 이라 한다.

##### 스프링 통합 지원

타임리프는 스프링과 자연스럽게 통합되고, 스프링의 다양한 기능을 편리하게 사용할 수 있게 지원한다.

#### 타임리프 기본 기능

##### 타임리프 사용 선언

```
<html xmlns:th="http://www.thymeleaf.org">
```

##### 기본 표현식

타임 리프는 다음과 같은 기본 표현식들을 제공한다.

* 간단한 표현
    * 변수 표현식: `${...}`
    * 선택 변수 표현식: `*{...}`
    * 메시지 표현식: `#{...}`
    * 링크 URL 표현식: `@{...}`
    * 조각 표현식: `~{...}`
* 리터럴
    * 텍스트: `'one text'`, `'Another one!'`
    * 숫자: `0, 34, 3.0, 12.3, ...`
    * 불린: `true`, `false`
    * 널: `null`
    * 리터럴 토큰: `one`, `sometext`, `main`, ...
* 문자 연산
    * 문자 합치기: `+`
    * 리터럴 대체: `|The name is ${name}|`
* 산술 연산
    * Binary operators: `+, -, *, /, %`
    * Minus sign (unary operator): `-`
* 불린 연산
    * Binary operators: `and`, `or`
    * Boolean negation (unary operator): `!`, `not)
* 비교와 동등
    * 비교: `>`, `<`, `>=`, `<=`(gt, lt, ge, le)
    * 동등 연산: `==`, `!=` (eq, ne)
* 조건 연산
    * If-then: `(if) ? (then)`
    * If-then-else: `(if) ? (then) : (else)`
    * Default: `(value) ?: (defalutValue)`
* 특별한 토큰
    * No-Operations: `_`

## Note