---
title:  "MySQL SELECT文"
excerpt: ""

categories:
  - MySQL
tags:
  - SELECT
  - グループ化
  

toc: true
toc_sticky: true
---

#### LIMIT
- ほしいだけデータ持ってくる
- LIMIT {持ってくる個数} または、LIMIT {スキップする個数} {持ってくる個数} 

#### AS
- 別名でデータを持ってくる

#### GROUP BY 
- 条件によって集計された値を取得する。

#### WITH ROLLUP
- 全体の集計値
- !ORDER BYは一緒に使用できない

#### HAVING
- グループされたデータ分ける
- ！WHEREはグループする前のデータ、HAVINGはグループ後に集計に使用する

#### DISTINCT 
- 重複排除

```SQL
SELECT DISTINCT CategoryID
FROM Products;
```



---