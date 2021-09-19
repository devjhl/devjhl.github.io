---
title:  "Spring MVC基本機能"
excerpt: "Spring MVC基本機能を整理しておく"

categories:
  - Spring
tags:
  - フレームワーク

toc: true
toc_sticky: true
---

MVC講座
<https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1#>

<code>@RestController</code>
**ビューではなく、HTTPメッセージボディにすぐ入力する**

## HTTPメソッドマッピング
```java
/**
 * @GetMapping
 * @PostMapping
 * @PutMapping
 * @DeleteMapping
 * @PatchMapping
 */
```

## PathVariable(パス変数)
```java
/** * PathVariable 
 * 変数名が同じなら省略可能
 * @PathVariable("userId") String userId -> @PathVariable userId
 */
@GetMapping("/mapping/{userId}")
public String mappingPath(@PathVariable("userId") String data) {
	 log.info("mappingPath userId={}", data);
	 return "ok";
}
```
## PathVariable(パス変数)　- 複数

```java
/**
 * PathVariable 複数
 */
@GetMapping("/mapping/users/{userId}/orders/{orderId}")
public String mappingPath(@PathVariable String userId, @PathVariable Long orderId) {
	 log.info("mappingPath userId={}, orderId={}", userId, orderId);
	 return "ok";
}
```
## リクエストマッピング　ー　API
```java
@RestController
@RequestMapping("/mapping/users")
public class MappingClassController {
    /**
     * GET /mapping/users
     */
    @GetMapping
    public String users() {
        return "get users";
    }

    /**
     * POST /mapping/users
     */
    @PostMapping
    public String addUser() {
        return "post user";
    }

    /**
     * GET /mapping/users/{userId}
     */
    @GetMapping("/{userId}")
    public String findUser(@PathVariable String userId) {
        return "get userId=" + userId;
    }

    /**
     * PATCH /mapping/users/{userId}
     */
    @PatchMapping("/{userId}")
    public String updateUser(@PathVariable String userId) {
        return "update userId=" + userId;
    }

    /**
     * DELETE /mapping/users/{userId}
     */
    @DeleteMapping("/{userId}")
    public String deleteUser(@PathVariable String userId) {
        return "delete userId=" + userId;
    }
}

```

## HTTPリクエスト　ー　基本、ヘッダー照会
```java
@Slf4j
@RestController
public class RequestHeaderController {

	 @RequestMapping("/headers")
	 public String headers(HttpServletRequest request,
		HttpServletResponse response,
		HttpMethod httpMethod,
		Locale locale,
		@RequestHeader MultiValueMap<String, String>　headerMap,
		@RequestHeader("host") String host,
		@CookieValue(value = "myCookie", required = false)　String cookie
												 ) 
{
				 log.info("request={}", request);
				 log.info("response={}", response);
				 log.info("httpMethod={}", httpMethod);
				 log.info("locale={}", locale);
				 log.info("headerMap={}", headerMap);
				 log.info("header host={}", host);
				 log.info("myCookie={}", cookie);
				 return "ok";
 }
}
```
- HttpMethod : HTTPメソッドを照会　　
- Locale : Locale情報を照会　　
- @RequestHeader MultiValueMap<String, String> : HTTPヘッダーをMultiValueMap形式で照会する　　
- @RequestHeader("host") String host : 特定HTTPヘッダーを照会する
	- 属性
		- 必須　：　required
		- 基本値　: defaultValue
- @CookieValue(value = "myCookie", required = false) String cookie : 特定Cookieを照会する
	- 属性
		- 必須　：　required
		- 基本値　: defaultValue
- MultiValueMap
	- 一つのキーにいくつ値をもらえる
		- **keyA=value1&keyA=value2**
		```java
		MultiValueMap<String, String> map = new LinkedMultiValueMap();
		map.add("keyA", "value1");
		map.add("keyA", "value2");

		//[value1,value2]
		List<String> values = map.get("keyA")
		```

@Slf4
- **log**


---