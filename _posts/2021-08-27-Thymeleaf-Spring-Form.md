---
title:  "Thymeleaf-Spring統合とフォーム"
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

## チェックボックス
- Thymeleaf作用前
  > HTML checkbox は選択ができなかったらクライアントからサーバへ値を送らない。
    しかし、ヒドゥンフィールドを一つ作って、_openのように既存のチェックボックス名の前にアンダースコアを付けて転送するとチェックを解除したと認識できる。
    ヒドゥンフィールドはいつも転送される。

```html
<!-- single checkbox -->
<div>
    <div class="form-check">
      <input type="checkbox" id="open" name="open" class="form-check-input">
      <input type="hidden" name="_open" value="on"/> <!-- ヒドゥンフィールド追加 -->
    <label for="open" class="form-check-label">販売オープン</label>
 </div>
```

 - Thymeleaf作用後
   > Thymeleafがヒドゥンフィールドを自動で生成してくれる
```html
<div>
    <div class="form-check">
      <input type="checkbox" id="open" th:field="*{open}" class="form-check-inout">
      <label for="open" class="form-check-lable">販売オープン</label>
    </div>
</div>
```

## マルチチェックボックス

```java
@ModelAttribute("regions")
public Map<String,String> regions() {
    Mam<String,String> regions = new LinkedHashMap<>();
    regions.put("TOKYO","東京");
    regions.put("OSAKA","大阪");
    return regions;
}
```
> **@ModelAttributeの特別使用方法**
**コントロールにこんな風に別のメソッドで適用できる**


```html
 <!-- multi checkbox -->
<div>
   <div>登録地域</div>
   <div th:each="region : ${regions}" class="form-check form-check-inline">
      <input type="checkbox" th:field="*{regions}" th:value="${region.key}"
class="form-check-input">
      <label th:for="${#ids.prev('regions')}"
             th:text="${region.value}" class="form-check-label">東京</label>
  </div>
</div>
```
- **th:for="${#ids.prev('regions')}"**
> 生成されたHTMLタグ属性でname同じでもいいが、idは全部違わなければならない。
Thymeleafはチェックボックスをeachループの中で繰り返し作る時、任意で1、2、3の数字を後ろに付ける。

```html
<input type="checkbox" value="東京" class="form-check-input" id="regions1" 
name="regions">
<input type="checkbox" value="OSAKA" class="form-check-input" id="regions2" 
name="regions">
```

Thymeleafは自動でchecked属性追加する


## ラジオボタン

```html
<!-- radio button -->
<div>
 <div>商品種類</div>
 <div th:each="type : ${itemTypes}" class="form-check form-check-inline">
 <input type="radio" th:field="${item.itemType}" th:value="$
{type.name()}" class="form-check-input" disabled>
 <label th:for="${#ids.prev('itemType')}" th:text="${type.description}"
class="form-check-label">
 BOOK
 </label>
 </div>
</div>
```
## セレクトボックス

**Controller**
```java
    @ModelAttribute("deliveryCodes")
    public List<DeliveryCode> deliveryCodes() {
        List<DeliveryCode> deliveryCodes = new ArrayList<>();
        deliveryCodes.add(new DeliveryCode("FAST", "早い配送"));
        deliveryCodes.add(new DeliveryCode("NORMAL", "普通配送"));
        deliveryCodes.add(new DeliveryCode("SLOW", "遅い配送"));
        return deliveryCodes;
    }
```

**thymeleaf**
```html
<!-- SELECT -->
<div>
 <div>配送方法</div>
 <select th:field="*{deliveryCode}" class="form-select">
 <option value="">==配送方法選択==</option>
 <option th:each="deliveryCode : ${deliveryCodes}" th:value="$
{deliveryCode.code}"
 th:text="${deliveryCode.displayName}">FAST</option>
 </select>
</div>
<hr class="my-4">
```
<div>
 <div>配送方法</div>
 <select th:field="*{deliveryCode}" class="form-select">
 <option value="">==配送方法選択==</option>
 <option th:each="deliveryCode : ${deliveryCodes}" th:value="$
{deliveryCode.code}"
 th:text="${deliveryCode.displayName}">FAST</option>
 </select>
</div>
<hr class="my-4">



出所 : 
<https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2>