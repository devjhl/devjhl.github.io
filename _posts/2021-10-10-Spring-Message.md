---
title:  "Message、Internationalization"
excerpt: "SpringのMessage、Internationalization整理する"

categories:
  - Spring
tags:
  - Message

toc: true
toc_sticky: true
---

Spring mvc講座
<https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2>

## メッセージ

**直接登録**
```java
@Bean
public MessageSource messageSource() {
 ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
 messageSource.setBasenames("messages", "errors");
 messageSource.setDefaultEncoding("utf-8");
 return messageSource
```  
- messages.properties
- /resources/messages.properties 

**SpringBoot**
- SpringBootは自動的にSpringBeanで登録する。

application.properties
spring.messages.basename=messages,config.i18n.messages

**Messageファイルを作る**

- messages.properties:基本値で使用（日本語）
- messages_en.properties :英語国際化使用

**messages.properties**
```html
hello=こんにちは
hello.name=こんにちは {0}
```

**messages_en.properties**
```html
hello=hello
hello.name=hello {0}
```

**Messageファイルを使用**
```java
@SpringBootTest
public class MessageSourceTest {

    @Autowired
     MessageSource ms;
    
    @Test
    void helloMessage() {
      String result = ms.getMessage("hello", null, null);
      assertThat(result).isEqualTo("こんにちは");
   }
}
```


---