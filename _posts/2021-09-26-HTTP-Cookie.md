---
title:  "HTTPクッキー"
excerpt: "HTTPクッキーを整理する"

categories:
  - HTTP
tags:
  - クッキー

toc: true
toc_sticky: true
---

HTTP講座
<https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC>

## クッキー
- Set-Cookie : サーバーからクライアントにクッキー伝達(リスポンス)  
- Cookie : クライアントがサーバーからもらったクッキーをセーブし、HTTPリクエストしサーバーに伝達　　
ex)  set-cookie: sessionId=abcde1234; expires=Sat, 26-Dec-2020 00:00:00 GMT; path=/; domain=.google.com; Secure　　

- 使用
  - ユーザログインセッション管理
  - 広告情報トラッキング　　
- クッキーの情報はいつもサーバー転送される
- 最小限の情報のみ使用(セッションid、認証トークン)
- サーバーに転送しないでウェブブラウザ内部にデータをセーブしたいとウェブストレージ(localStorage, sessionStorage)参考
- セキュリティに敏感なデータは保存しないようにしよう。

## クッキー　domain
- ex) domain=example.org
- 明示:明示した文書基準ドメイン+サブドメインを含む。
  - domain=example.orgを指定してクッキーを作成
    - example.orgはもちろん、dev.example.orgもクッキーにアクセス
- 省略:現在の文書基準ドメインのみ適用
  - [example.org]でクッキーを作成し、domain指定を省略
    - [example.org]でのみクッキーアクセスだが、dev.example.orgはクッキーにアクセスしない


## クッキー　Path
-  dev.example.orgはクッキーにアクセスしない
- このパスを含む下位パスページのみクッキーアクセス
- 一般的にpath=/ルートで指定

## クッキー　セキュリティ
**Secure, HttpOnly, SameSite**

- Secure
  - クッキーはhttp、httpsを区別せずに送信
  - Secureを適用するとhttpsの場合のみ送信
- HttpOnly
  - XSS攻撃防止
  - JavaScriptからアクセス不可(document.cookie)
  - HTTP 伝送のみに使用
- SameSite
  - CSRF攻撃防止
  - リクエストドメインとクッキーに設定されたドメインが同じ場合のみクッキー送信

## クッキー　Expires, max-age
- Set-Cookie: expires=Sat, 26-Dec-2020 04:39:21 GMT
  - 満了日になるとクッキーを削除
  - Set-Cookie: max-age=3600
    - 0や負数を指定するとクッキーを削除
  - セッションクッキー : 満了日を省略するとブラウザ終了時まで維持  
  - パーシステントクッキー : 満了日を入力すると、その日まで維持



---