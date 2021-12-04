---
title:  "MySQL演算子と関数"
excerpt: ""

categories:
  - MySQL
tags:
  - 演算子
  - 関数

toc: true
toc_sticky: true
---

#### 演算子
+, -, *, /  
%, MOD  
IS  
IS NOT  
AND, &&  
OR, ||  
=  
!=, <>  
, > <  
, >=, <=  
BETWEEN {MIN} AND {MAX}  
NOT BETWEEN {MIN} AND {MAX}  
IN (...)  
NOT IN (...)  
LIKE '... % ...'  
LIKE '... _ ...'	

### 数字関数
ROUND 四捨五入   <br>
CEIL  あげ  <br>
FLOOR  さげ  <br>
ABS 絶対値  <br>
GREATEST	() 一番大きい数  <br>
LEAST () 一番小さい数  <br>
MAX  <br>
MIN  <br>
COUNT (NULL数は排除)  <br>
SUM  <br>
AVG  <br>
POW(A,B) , POWER(A,B) 二乗  <br>
SQRT 平方根  <br>
TRUNCATE(N,n) 小数点  <br>

#### 文字関数
UCASE , UPPER 大文字 <br>
LCASE , LOWER 小文字 <br>
CONCAT(...) 内容連結 <br>
SUBSTR,SUBSTRING 文字列切る <br>
LEFT (左から) <br>
RIGTH　(右から) <br>
LENGTH (文字列バイト長さ) <br>
CHAR_LENGTH,CHARACTER_LENGTH 文字列の文字長さ <br>
TRIM 両側空白除去 <br>
LTRIM 左空白を削除 <br>
RTRIM 右空白を除去 <br>
LPAD(S,N,P) SがN文字になるまでPを付ける <br>
RPAD(S,N,P) SがN文字になるまでPを付ける <br>
REPLACE(S,A,B) AをBに変更 <br>
INSTR(S,s) Sの中sの最初位置変換なかったら０ <br>
CAST(A,T) <br>

#### 時間・日付関数
CURRENT_DATE, CURDATE	<br>
CURRENT_TIME, CURTIME	<br>
CURRENT_TIMESTAMP, NOW <br>
DATE 文字列によって日付生成<br>
TIME　文字列によって時間生成<br>

YEAR	
MONTHNAME
MONTH	
WEEKDAY	
DAYNAME	
DAYOFMONTH, DAY	

HOUR
MINUTE	
SECOND

ADDDATE, DATE_ADD	
SUBDATE, DATE_SUB	

DATE_DIFF	
TIME_DIFF

LAST_DAY

DATE_FORMAT <br>
%Y	XXXX <br>
%y  XX <br>
%M	月 英語 <br>
%m	月 数字 <br>
%D	(1st, 2nd, 3rd...) <br>
%d, %e	(01 ~ 31) <br>
%T	hh:mm:ss <br>
%r	hh:mm:ss AM/PM <br>
%H, %k	(~23) <br>
%h, %l	(~12) <br>
%i	文 <br>
%S, %s	秒 <br>
%p	AM/PM <br>

#### 以外

IF(条件,T,F)
もっとも複雑な条件はCASE文を使う

IFNULL(A,B) AがNULLならＢ出力

---