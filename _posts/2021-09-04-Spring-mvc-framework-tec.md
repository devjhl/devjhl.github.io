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
## HTTPリクエスト　ー　基本

<code>@CookieValue(value = "myCookie", required = false) String cookie</code>  

**- **keyA=value1&keyA=value2****
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