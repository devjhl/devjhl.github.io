---
title:  "Java8"
excerpt: "Java8を整理する"

categories:
  - Java
tags:
  - ラムダ式

toc: true
toc_sticky: true
---

## ラムダ式

- **(引数) -> {処理};**

```java
	Item item = (i) -> {
	System.out.println(i);
	};
```
## ラムダ式でメソッド簡潔に表現
- **クラス名::メソッド名**  

```java
System.out::println
```

## Stream

**以前と比較**

```java
ArrayList<Integer> list = new ArrayList<Integer>(Arrays.asList(1,2,3));
Iterator<Integer> iter = list.iterator();
while(iter.hasNext()) {
    int num = iter.next();
    System.out.println("値 : "+num);
}
```

**コレクションインスタンス**

```java
ArrayList<Integer> list = new ArrayList<Integer>(Arrays.asList(1,2,3));
Stream<Integer> stream = list.stream();
stream.forEach(num -> System.out.println("値 : "+num));
```

**クラス**

```java
public class FromCollectionExample {
    public static void main(String[] args) {
        List<Student> studentList = Arrays.asList(
            new Student("田中"),
            new Student("高橋"),
            new Student("上野")
        );
		
        Stream<Student> stream = studentList.stream();
        stream.forEach(s -> System.out.println("名前 : "+ s.getName()));
    }
}
```

## Stream 

- **filter**
	- **フィルター処理とか特定の値を取得する**

```java
findAll().stream()
.filter(m -> m.getLoginId().equals(loginId))
.findFirst();
```
## Optional
- 値をラップし、 その値がnullかもしれない ことを表現する
- リターン値で使う推奨

**作用**
- **Optional.of()**
- **Optional.ofNullable()**
- **Optional.empty()**

**値があるかどうか確認**  
- **isPresent()**
- **isEmpty()** //Java 11

**値持ってくる**　　
- **get**

**Optionalに値がある場合その値を持って～～をする**
- **ifPresent(Counsumer)**

**Optionalに値がある場合持ってきてない場合には～～をリターンする**
- **orElse(T)**

**Optionalに値がある場合持ってきてない場合には～～をする**
- **orElseGet**

**Optionalに値がある場合持ってきてない場合はエラーを投げる**
- **orElseThrow()**

- **filter**
	- **フィルター処理とか特定の値を取得する**

  **Optionalに中のある値を変換する**
  - **Optional map(Function)**
  - **Optional flatMap(Function)**

---