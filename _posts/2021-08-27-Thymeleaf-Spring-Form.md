---
title:  "Thymeleaf-Spring統合とフォーム(作成中)"
excerpt: "Thymeleaf-Spring統合とフォーム勉強する"

categories:
  - Thymeleaf
tags:
  - Spring統合とフォーム

toc: true
toc_sticky: true
---

## 入力フォーム処理
> th:object : コマンドオブジェクトを指定する  

> *{...} : 選択変数式という。th:objectで選択したオブジェクトに接近する  

> th:field
    - HTMLタグの id,name,value属性を自動で処理してくれる  


**レンダリング前**
```html
<input type="text" th:field="*{itemName}" />
```
**レンダリング後**
```html
<input type="text" id="itemName" name="itemName" th:value="*{itemName}" />
```

出所 : https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2