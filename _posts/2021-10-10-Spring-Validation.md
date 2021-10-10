---
title:  "バリデーション"
excerpt: "Springのバリデーション整理する"

categories:
  - Spring
tags:
  - Validation

toc: true
toc_sticky: true
---

Spring mvc講座
<https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2>

**Controllerの重要な役割中一つはHTTPリクエストが正常か検証すること**

**参考：クライアント検証、サーバー検証**

## BindingResult
- Springが提供する検証エラー
- BindingResult はインタフェースであり、Errors インタフェースを継承している。
- Errorsインタフェースは単純なエラー保存と照会機能を提供するが、Binding Resultはこれに加えて追加的な機能を提供する。
- Binding ResultはModelに自動に含まれる。　　

❕注意　Binding Result binding Resultパラメータの位置は、@Model Attribut Itemitemの次に来なければならない。

**FieldError**
```java
public FieldError(String objectName, String field, String defaultMessage);
public FieldError(String objectName, String field, @Nullable Object 
rejectedValue, boolean bindingFailure, @Nullable String[] codes, @Nullable
Object[] arguments, @Nullable String defaultMessage)
```

**ObjectError**
- 特定Field例外ではなく全体例外

**検証失敗**
```java
//検証に失敗したらもう入力フォーム 
//ちなみに否定の否定はコーディング時に読みにくい
if(bindingResult.hasErrors()) {
log.info("errors = {} " , bindingResult);
    return "validation/v2/addForm";
}
```

**エラー処理**
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="utf-8">
    <link th:href="@{/css/bootstrap.min.css}"
          href="../css/bootstrap.min.css" rel="stylesheet">
    <style>
        .container {
            max-width: 560px;
        }

        .field-error {
            border-color: #dc3545;
            color: #dc3545;
        }
    </style>
</head>
<body>

<form action="item.html" th:action th:object="${item}" method="post">
    <div th:if="${#fields.hasGlobalErrors()}">
        <p class="field-error" th:each="err : ${#fields.globalErrors()}"
           th:text="${err}">globalErrorsMessage</p>
    </div>
    <div>
        <label for="itemName" th:text="#{label.item.itemName}">商品名</label>
        <input type="text" id="itemName" th:field="*{itemName}"
               th:errorclass="field-error" class="form-control"
               placeholder="商品名を入力してください。">
        <div class="field-error" th:errors="*{itemName}">
            商品名エラー
        </div>
    </div>
    <div>
        <label for="price" th:text="#{label.item.price}">価格</label>
        <input type="text" id="price" th:field="*{price}"
               th:errorclass="field-error" class="form-control"
               placeholder="価格を入力してください。">
        <div class="field-error" th:errors="*{price}">
            価格エラー
        </div>
    </div>
    <div>
        <label for="quantity" th:text="#{label.item.quantity}">数量</label>
        <input type="text" id="quantity" th:field="*{quantity}"
               th:errorclass="field-error" class="form-control"
               placeholder="数量を入力してください。">
        <div class="field-error" th:errors="*{quantity}">
            数量エラー
        </div>
    </div>
```

**rejectValue()**
- BindingResultはもう本人が検証するオブジェクトターゲットを知っている
- rejectValue(),reject()を使用するとFieldError,ObjectErrorを生成しなくて検証エラーを使える
```java
bindingResult.rejectValue("price", "range", new Object[]{1000, 1000000}, null)
```

**どのようにエラーコードを設計することか?**
- 最も良い方法は汎用性で使いながら、細かく作成しなければならない場合には細かい内容が適用されるようメッセージに段階を置く方法である。
```html
#Level1
required.item.itemName: 商品名は必須です。

#Level2
required: 必須値です。
```

```java
ValidationUtils.rejectIfEmptyOrWhitespace(bindingResult, "itemName",
"required");
```

```java
@InitBinder
public void init(WebDataBinder dataBinder) {
	 log.info("init binder {}", dataBinder);
	 dataBinder.addValidators(itemValidator);
}

@PostMapping("/add")
public String addItemV6(@Validated @ModelAttribute Item item, BindingResult 
bindingResult, RedirectAttributes redirectAttributes) {
```

## Bean Validation
- 依存関係追加
<code>build.gradle</code>
<code>implementation 'org.springframework.boot:spring-boot-starter-validation'</code>

**検証アノテーション**
@NotBlank
@NotNull
@Range(min = 1000, max = 1000000)
@Max(9999)

**message属性**
(message = "")

**groups(@Validated)**
- 同一モデルオブジェクトを登録と修正時別々に検証する方法

---