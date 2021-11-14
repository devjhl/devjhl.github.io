---
title:  "Spring Interceptor 重複排除"
excerpt: ""

categories:
  - Spring
  - Interceptor
tags:
  - Exception

toc: true
toc_sticky: true
---

Spring mvc講座
<https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2>

```java

@Configuration
public class WebConfig implements WebMvcConfigurer {
	@Override
	public void addInterceptors(InterceptorRegistry registry) {
		registry.addInterceptor(new LogInterceptor())
				.order(1)
				.addPathPatterns("/**")
				.excludePathPatterns(
						"/css/**", "/*.ico"
						, "/error", "/error-page/**" // エラーページ径路
				);
	}

```
---