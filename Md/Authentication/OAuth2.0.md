# OAuth2.0について
# 目次
[1. 概要](#概要)
[2. 認可フロー](#認可フロー)

※参照元：[RFC6749](https://tools.ietf.org/html/rfc6749#section-2.3.1)
# 1. 概要  
[RFC6749](https://tools.ietf.org/html/rfc6749#section-2.3.1)で規定された認可フローのプロトコル。OAuth1.0とは全くの別物。  
認証プロトコルであるOpenId Connect(OAuth2.0+OpenID)のベースになっている。  
OAuth2.0では以下の用語が使われる。  
##### アクセストークン  
- リソースサーバのデータにアクセスする為の許可証。アクセスできる範囲(SCOPE)が制限されている。
##### 認可コード
- アクセストークンを取得する為に使用される一時トークン。アクセストークンの引換券的な役割。  
アクセストークンがWebブラウザに渡らないようにするために使われる。
##### スコープ
- アクセストークンの許可されているアクセス範囲の事
##### リソースオーナー(Resource Owner)
- リソースの持ち主。アカウントの持ち主。エンドユーザー。
##### リソースサーバ(Resource Server)
- リソースをホストするサーバ。アクセストークンを使用して、制限されたリソース要求に応える。
##### クライアント(Client)
- リソース所有者に代わって保護されたリソースへリクエストを行うアプリケーション。  
  特定のエンティティを指すわけではない。(状況によってクライアントとなるエンティティは変わる)
##### 認可サーバ(Authorization Server)
- リソース所有者の承認と認証が成功した後に、クライアントにアクセストークンを発行するサーバ。  
  認可サーバーとリソースサーバーは同じサーバーでも異なるエンティティでもどちらでも良い。  
  (OAuth仕様では定められていない)

 
# 2. 基本仕様
## 2.1.抽象プロトコルフロー
```
     +--------+                               +---------------+
     |        |--(A)- Authorization Request ->|   Resource    |
     |        |                               |     Owner     |
     |        |<-(B)-- Authorization Grant ---|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- Authorization Grant -->| Authorization |
     | Client |                               |     Server    |
     |        |<-(D)----- Access Token -------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)----- Access Token ------>|    Resource   |
     |        |                               |     Server    |
     |        |<-(F)--- Protected Resource ---|               |
     +--------+                               +---------------+
```
- (A)：
  クライアントがリソース所有者へ許可を求める。この許可要求は、リソース所有者に直接行うこともできるし、認可サーバを仲介することもできる。
- (B)：
  リソースオーナーがクライアントの認可(承認)を行い、クライアントは認可コードを受け取る。 リソース所有者に許可された事を表す資格情報であり、本仕様で4タイプ定義されている。  
  認可リクエストのためにクライアントによって使用されるメソッドに依存しており、タイプは認可サーバーでサポートされている。
- (C)：
  クライアントが認可サーバーで認可を受けとった後、アクセストークンを要求する。
  この時、認可権限を渡す。
- (D)：
  認可サーバーはクライアントを認証し、認可権限を検証する。  
  これが問題なければ、アクセストークンを発行する。
- (E)：
  クライアントはリソースサーバーから保護されたリソースを要求する。  
  この時、アクセストークンを渡して認証する。
- (F)：
  リソースサーバーはアクセストークンを検証し、問題なければリクエストに応える。

## 2.2.認可権限
認可権限はアクセストークンを得るためにクライアントが使用する、リソースオーナーに渡される証明書。  
この証明書は、authorization code ／ implicit　／ resource owner password credentials／ client credentialsの4種類 + 拡張された追加タイプがある。  
#### 1).認可コード(Authorization code)  
認可コードは、クライアントとリソースオーナー間で仲介役の認可サーバーを利用した場合に得られる資格証明。  
クライアントは、リソースオーナーから直接認可リクエストを要求する代わりに、UA(リソースオーナー)を認可サーバーにリダイレクトして承認・認証を受けさせる。  
認証を受けた後、認可サーバーから認可コードを受け取り、クライアントにリダイレクトする。  
このように認証を認可サーバーで行うことにより、クライアントに認証情報(ログイン情報)が渡ることがない。  
#### 2).Implicit
Implicitは、javascriptなどのスクリプト言語を使用してブラウザに実装されたクライアント向けに最適化された単純な承認コードフロー。  
Implicitは、クライアントに認証権限を発行する代わりに、アクセストークンを直接発行する。  
(リソース所有者の承認プロセスはあるが、認証コードのような中間情報は発行されない)  
#### 3).パスワードクレデンシャル
リソース所有者のユーザー名およびパスワードをアクセストークンを取得する為の資格情報として直接利用する方法。  
リソースオーナーの情報を直接利用する為、高度な信頼がある(クライアントがOSの場合など)場合や、他の認可権限タイプが使用できない場合に使用する。　　
#### 4).クライアントクレデンシャル
認可スコープがクライアントの制御下で保護されたリソースに制限されているとき、認可権限として利用できる。

## 2.3. アクセストークン
保護されたリソースへのアクセスに使用される資格情報。  
トークンには、特定のスコープとアクセスの期間が入っている。  

## 2.4. リフレッシュトークン
アクセストークンの再取得に使用される資格情報。  
認可サーバによってクライアントに発行され、現在のアクセストークンが無効または期限切れになったときに新しいアクセストークンを取得するために使用する。  
リフレッシュトークンは認可サーバでのみ使われ、リソースサーバに送られることはない。  
```
  +--------+                                           +---------------+
  |        |--(A)------- Authorization Grant --------->|               |
  |        |                                           |               |
  |        |<-(B)----------- Access Token -------------|               |
  |        |               & Refresh Token             |               |
  |        |                                           |               |
  |        |                            +----------+   |               |
  |        |--(C)---- Access Token ---->|          |   |               |
  |        |                            |          |   |               |
  |        |<-(D)- Protected Resource --| Resource |   | Authorization |
  | Client |                            |  Server  |   |     Server    |
  |        |--(E)---- Access Token ---->|          |   |               |
  |        |                            |          |   |               |
  |        |<-(F)- Invalid Token Error -|          |   |               |
  |        |                            +----------+   |               |
  |        |                                           |               |
  |        |--(G)----------- Refresh Token ----------->|               |
  |        |                                           |               |
  |        |<-(H)----------- Access Token -------------|               |
  +--------+           & Optional Refresh Token        +---------------+
```

## 2.5. 他使用技術
- TLS : 通信路は暗号化する。バージョンに特に指定はないが脆弱性に基づいて対応する事。
- HTTPリダイレクト : 本仕様ではリダイレクトを多用する。ステータスコードに特に指定はない。
  
# 3. 事前登録
OAuthプロトコル開始前に、クライアントは認可サーバーを登録する必要がある。  
登録方法は特に定められていないが、通常HTMlフォームでDBなどに登録する。  
登録項目は主に以下の通り。  
- Client_type
- Redirect_uri
- Authorization server's information
## 3.1. Client Types
OAuthでは２つのクライアントタイプが定義されている。  
クライアントタイプの指定は、認可サーバの


#### 1).confidential
機密情報の維持ができる安全なサーバ上に実装されているクライアント。  
または他の何らかの方法で資格情報を維持できるクライアント。
#### 2).public
インストールされているネイティブアプリケーションやWebブラウザベースのアプリケーションなど、リソース所有者が使用するデバイス上で実行されるクライアント。
他の手段を介した安全なクライアント認証が不可能である。  
