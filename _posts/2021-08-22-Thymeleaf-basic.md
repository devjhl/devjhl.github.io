---
title:  "Thymeleaf"
excerpt: "Thymeleafを勉強する"

categories:
  - Thymeleaf
tags:
  - 基礎

toc: true
toc_sticky: true

last_modified_at: 2021-08-22T17:40:00-00:00
---

**Thymeleaf**
- サーバサイドHTMLレンダリング (SSR)
- ナチュラルテンプレート
- Springの統合を提供

**使用宣言**
```html
<html xmlns:th="http://www.thymeleaf.org">
```

**text**
```html
HTMLタグの属性
<th:text="${data}">

コンテンツ中から直接出力する
[[$data]]
```

#### 変数-SpringEL ####
    ${...}<br>
    変数表現式ではSpringELというSpringが提供する表現式使用

<h1>SpringEL</h1>
<ul>Object
    <li>${user.username} =    <span th:text="${user.username}"></span></li>
    <li>${user['username']} = <span th:text="${user['username']}"></span></li>
    <li>${user.getUsername()} = <span th:text="${user.getUsername()}"></span></li>
</ul>
<ul>List
    <li>${users[0].username}    = <span th:text="${users[0].username}"></span></li>
    <li>${users[0]['username']} = <span th:text="${users[0]['username']}"></span></li>
    <li>${users[0].getUsername()} = <span th:text="${users[0].getUsername()}"></span></li>
</ul>
<ul>Map
    <li>${userMap['userA'].username} =  <span th:text="${userMap['userA'].username}"></span></li>
    <li>${userMap['userA']['username']} = <span th:text="${userMap['userA']['username']}"></span></li>
    <li>${userMap['userA'].getUsername()} = <span th:text="${userMap['userA'].getUsername()}"></span></li>
</ul>
<h1>ローカル変数 - (th:with)</h1>
<div th:with="first=${users[0]}">
    <p>初めの人の名前は <span th:text="${first.username}"></span></p>
</div>
</body>
</html>

#### 基本オブジェクト ####
- ${#request}<br>
- ${#response}<br>
- ${#session}<br>
- ${#servletContext}<br>
- ${#locale}<br>

#### 便宜オブジェクト ####
- HTTP要請パラメータ接近　： param<br>
例) ${param.paramData}<br>
- HTTPセッション接近: session<br>
例) ${session.sessionData}<br>
- SpringBean接近: @<br>
例) ${@helloBean.hello('Spring!')}

**utilityオブジェクトと日付**
- #message : メッセージ、国際化
- #uris : URI escape
- #dates : java.util.Date 
- #calendars : java.util.Calendar 
- #temporals : Java8の天気関連書式
- #numbers : 数字
- #strings : 文字
- #objects : オブジェクト
- #bools : boolean 
- #arrays : 配列
- #lists , #sets , #maps :　コレクション
- #ids : id処理

#### #temporals(天気)
```java
@GetMapping("/date")
public String date(Model model) {
 model.addAttribute("localDateTime", LocalDateTime.now());
 return "basic/date";
}
```

```html
<h1>LocalDateTime</h1>
<ul>
 <li>default = <span th:text="${localDateTime}"></span></li>
 <li>yyyy-MM-dd HH:mm:ss = <span th:text="${#temporals.format(localDateTime, 
'yyyy-MM-dd HH:mm:ss')}"></span></li>
</ul>
<h1>LocalDateTime - Utils</h1>
<ul>
 <li>${#temporals.day(localDateTime)} = <span th:text="$
{#temporals.day(localDateTime)}"></span></li>
 <li>${#temporals.month(localDateTime)} = <span th:text="$
{#temporals.month(localDateTime)}"></span></li>
 <li>${#temporals.monthName(localDateTime)} = <span th:text="$
{#temporals.monthName(localDateTime)}"></span></li>
 <li>${#temporals.monthNameShort(localDateTime)} = <span th:text="$
{#temporals.monthNameShort(localDateTime)}"></span></li>
 <li>${#temporals.year(localDateTime)} = <span th:text="$
{#temporals.year(localDateTime)}"></span></li>
 <li>${#temporals.dayOfWeek(localDateTime)} = <span th:text="$
{#temporals.dayOfWeek(localDateTime)}"></span></li>
 <li>${#temporals.dayOfWeekName(localDateTime)} = <span th:text="$
{#temporals.dayOfWeekName(localDateTime)}"></span></li>
 <li>${#temporals.dayOfWeekNameShort(localDateTime)} = <span th:text="$
{#temporals.dayOfWeekNameShort(localDateTime)}"></span></li>
 <li>${#temporals.hour(localDateTime)} = <span th:text="$
{#temporals.hour(localDateTime)}"></span></li>
 <li>${#temporals.minute(localDateTime)} = <span th:text="$
{#temporals.minute(localDateTime)}"></span></li>
 <li>${#temporals.second(localDateTime)} = <span th:text="$
{#temporals.second(localDateTime)}"></span></li>
 <li>${#temporals.nanosecond(localDateTime)} = <span th:text="$
{#temporals.nanosecond(localDateTime)}"></span></li>
</ul>
```

#### URLリンク
    @{...}

```java
@GetMapping("/link")
public String link(Model model) {
 model.addAttribute("param1", "data1");
 model.addAttribute("param2", "data2");
 return "basic/link";
}
```

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
 <meta charset="UTF-8">
 <title>Title</title>
</head>
<body>
<h1>URLリンク</h1>
<ul>
    <li><a th:href="@{/hello}">basic url</li>
    <li><a th:href="@{/hello(param1=${param1}, param2=${param2})}">hello query 
param</a></li>
    )">
    <li><a th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=$
{param2})}">path variable</a></li>
    <li><a th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=$
{param2})}">path variable</a></li>

```
####　Literals
- 文字は' 'でしなければならないが、こんな方法でもできる

```html
<span th:text="'hello'">

<li>'hello' + ' world!' = <span th:text="'hello' + ' world!'"></span></li>
<li>'hello world!' = <span th:text="'hello world!'"></span></li>
<li>'hello ' + ${data} = <span th:text="'hello ' + ${data}"></span></li>
<li>Literals代替 |hello ${data}| = <span th:text="|hello ${data}|"></span></li>

```
#### 演算子

- 比較演算子
>     > (gt), < (lt), >= (ge), <= (le), 
      ! (not), == (eq), != (neq, ne)

```html
// Elvis演算子
   <li>${data}?: 'データがないです。' = <span th:text="${data}?: 'データがないです。'"></span></li>
// No-Operation
    <li>${nullData}?: _ = <span th:text="${nullData}?: _">データがないです</span></li>
```

---
韓国語になっている講座を見てから、勉強しましたものを日本語で作成しました。<br>

出所 : https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2

