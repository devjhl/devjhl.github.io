---
title:  "Java基礎"
excerpt: "Java基礎を整理しておく"

categories:
  - Java
tags:
  - 基礎

toc: true
toc_sticky: true

last_modified_at: 2021-08-22T14:33:00-00:00
---
#### エスケープシーケンス  
    \t   タブ
    \n   改行 

#### 書式指定子
```java
System.out.printf(format,arguments)
```


|指定子|型|
|:---:|:---:|
|%d|整数|
|%c|文字|
|%s|文字列|
|%f|実数|


#### 演算子
    + 加算
    - 減算
    * 乗算
    / 除算
    % 剰余算

#### 関係演算子
    > < >= <= == !=

#### 論理演算子
    && || or

#### 文字列比較
    equals()

#### 条件演算子
    条件式？式１：式２；
    **if文で主に使用**  

#### 入出力
```java
Scanner sc = new Scanner(System.in);
```

#### 分岐処理
**if文、else if文(if文とセット)、else**  

**switch文**
```java
switch(式) {
  case 式1:
    break;
  case 式2:
    break;
  default:
    break; 
}
```
#### 繰り返し文
**for文、拡張for文(主に配列やリストで使用)**
  for文で()中を変更するより、ステートメントで変更しよう　　

**while文**
```java
int a = 1;
while (a<10) {
  System.out.print(a + "");
  a++;
}
```
**do while文**
```java
do {
  必ず一度実行される
}while(継続条件式);
```
#### continue? break?
continue : 特定の条件をスキップする  

break : 自身が含まれ、反復文をはずれる  
#### 配列、2次元配列  
配列は一度生成したら、実行してる中は長さ変更できない。　　
```java
Type[] 変数名 = new Type[長さ];　//1次元配列
Type[][] 変数名 = new Type[長さ][長さ]; //2次元配列
```
index : 0からはじまるから配列の長さ-1まで<br>  
長さ　: 配列名.length<br>
出力　: Arrays.toString(参照変数);<br>    
#### Stringクラスの主要メソッド(機能)


|メソッド|機能|
|:---:|:---:|
|char charAt(int index)|文字列で該当にある文字列を返還する|
|int length|文字列の長さを返還する|
|String substring(int from,int to)|文字列で該当範囲(from~to)の文字列を返還する(toは排除)|
|boolean equals(Object obj)|文字列の内容同じか確認|
|char[] toCharArray()|文字列を文字配列を変換して返還する|

#### Arraysクラスで配列使う

**出力**

- toString() - 1次元配列
- deepToString - 多次元配列

**比較**

- equals  - 1次元配列
- deepEquals - 多次元配列

**コピー**

- copyOf()
- copyOfRange()

**整列**

- sort

#### 定数と列挙型
- 定数　：　値を変更できない変数  

```java
public static final 型 定数名　= 値;
                //大文字、アンダースコア
```

**列挙型(enum)**
```java
public enum 型名 {
  値1,
  値2,
  ....
}
```









