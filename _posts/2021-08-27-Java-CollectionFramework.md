---
title:  "Java Collections Framework"
excerpt: "Collections Frameworkを整理しておく"

categories:
  - Java
tags:
  - Collections Framework

toc: true
toc_sticky: true
---

## Collections Framework
- 多数のデータを易しく効果的に処理できるクラス集合

#### List　(ArrayList)
- オブジェクトを一列に並べた構造
- セーブしたら、自動でインデックス付与されインデックスでオブジェクトを検索、削除機能があり
- オブジェクトの住所を参照する
- 重複セーブ、nullも可能


|メソッド|説明|
|:---:|:---:|
|boolean add(E e) |最後にオブジェクト追加|
|void add(int index,E element)|インデックスにオブジェクト追加|
|boolean contains(Object o)|オブジェクト検索|
|E get(int index)|オブジェクトReturnする|
|isEmpty()|Collections空いているか確認|
|int size()|オブジェクト数Return|
|E remove(int index) |セーブされたオブジェクト削除|
|void clear()|セーブされたオブジェクト全部削除|
|boolean remove(Object o)|オブジェクト削除|


- ##### ArrayList
  - Generics概念使用(オブジェクトタイプ)　
    >  Wrapper Class作用 - 基本タイプをオブジェクトに扱うため作用するクラス
    >  java.langパッケージ
      
```java
ArrayList<オブジェクトタイプ>　list = new ArrayList<オブジェクトタイプ>();
```

#### Wrapper Class

> byte - Byte
> char - Character
> int - Integer
> float - Float
> double - Double
> boolean - Boolean
> long - Long
> short - Short

**Boxing**
```java
String String.valueOf(基本タイプ);
```

**UNBoxing**
```java
int int = Integer.valueOf("100");
```

#### Set　(HashSet)
- 順序がないが、代わりに全体オブジェクトに一つずつ反復して持ってくる"iterator(イテレータ)"提供
- オブジェクト重複X 、　NULL値も一つだけセーブできる


|メソッド|説明|
|:---:|:---:|
|boolean add(E e) |成功ならtrue重複オブジェクトならfalseをreturnする|
|boolean contains(Object o)|オブジェクトセーブされているかreturnする|
|Iterator<E> iterator() |iteratorをreturnする|
isEmpty()|Collections空いているか確認|
|int size()|オブジェクト数Return|
|void clear()|セーブされたオブジェクト全部削除|
|boolean remove(Object o)|オブジェクト削除|

- ##### HashSet
- 重複排除

```java
HashSet<オブジェクトタイプ> set1 = new HashSet<オブジェクトタイプ>();
HashSet<オブジェクトタイプ> set4 = new HashSet<オブジェクトタイプ>(数字);　//容量
```

**値追加**
```java
HashSet<Integer> hs = new HashSet<Integer>();
hs.add(1);
hs.add(2);
```

**値削除**
```java
HashSet<Integer> hs = new HashSet<Integer>(Arrays.asList(6,5,4));
hs.remove(5);
hs.clear();
```

**大きさ**
```java
HashSet<Integer> hs = new HashSet<Integer>(Arrays.asList(6,5,4));
System.out.println(hs.size());
```

**値出力**
```java
HashSet<Integer> hs = new HashSet<Integer>(Arrays.asList(6,5,4));

Iterator ir = hs.iterator();
while(ir.hasNext()) { // true / false
    System.out.println(ir.next());
}

```

**値検索**
```java
HashSet<Integer> hs = new HashSet<Integer>(Arrays.asList(6,5,4));
System.out.println(hs.contains(5));
```

#### Map　(HashMap)
- Key , value 
- keyは重複X(もし、入ったら新しい値に変更される) value重複O
- ロッカーと考えよう

```java
HashMap<String,String> map1 = new HashMap<String,String>();
HashMap<String,String> map2 = new HashMap<String,String>(){{//初期値
    put("a","b");
}};
```

- ##### HashMap

**値追加**
```java
ashMap<Integer,String> map = new HashMap<>();
map.put("Java");
map.put("C言語");
```

**値削除**
```java
HashMap<Integer,String> map = new HashMap<Integer,String>(){{//初期値
    put(1,"Java");
    put(2,"C言語");
}};
map.remove(1); 
map.clear(); 
```

> Set、Putは保存容量を約2倍に増やすので初期容量を指定した方がいい。
