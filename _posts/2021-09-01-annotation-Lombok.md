---
title:  "Lombok"
excerpt: "Spring勉強しながら、Lombok便利だと感じて整理しておく"

categories:
  - Lombok
tags:
  - 

toc: true
toc_sticky: true
---

## Lombokは？
アノテーションで基本コード最適化
Springでよく使う

```java
@Getter
@Setter
@ToString
@RequiredArgsConstructor  
- finalが付けているフィールドをあつめて生成者を自動で作ってくる(コードは見えないが、呼び出しが可能)
ex) @Autowired省略
```

---