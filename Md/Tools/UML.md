# UML
# 目次
[概要](#概要)
1. 構造図
[クラス図](#クラス図)
[コンポーネント図](#コンポーネント図)
[オブジェクト図](#オブジェクト図)
[パッケージ図](#パッケージ図)  
  
2. 振る舞い図
[ユースケース図](#ユースケース図)
[アクティビティ図](アクティビティ図)
[ステートマシン図](#ステートマシン図)  

3. 相互作用図
[シーケンス図](#シーケンス図)
[コミュニケーション図](#コミュニケーションズ)
[タイミング図](#タイミング図)


# 概要
統一モデリング言語(以降UML)は、ビジネスプロセス・構造・動画等を記述、指定、設計、文書化する為のモデリング言語である。  
ドメインエキスパート、アーキテクト、開発者における共通言語でとなる。  
なお、UMLは完全ではない。複数のUMLダイアグラム及びドキュメントを組み合わせることで補完し合うべき。  
UMLにはバージョンがあり、2020年時点で最新はver.2.5.1。  

### UMLのダイアグラム
UML図は、大きく以下の図で分類される。
- システムの静的な構造を示す`構造図`
- システムの振る舞いを示す`振る舞い図`
- 振る舞い図の内、Obj間のメッセージングに着目したものを`相互作用図`  

# 1. 構造図
## クラス図  
### 1). クラス名  

|クラス名|
|:--:|
|属性|
|操作|

### 2). 属性  

|可視性|内容|
|:--|:--|
|+|public|
|-|private|
|#|protected|
|~|package|

### 3). 操作  
`可視性 操作名(引数名 :引数の型) :戻り値型`

### 4). クラス間の関係表現  
  
|関係|表現|備考|
|:--|:--:|:--|
|関連(association)|――――|関係表現の基本。has-a関係や、後述の集約、依存も含む。|
|集約(aggregation)|◇―――|関連の内、「全体-部分」でかつ関係が緩いもの。◇側が全体となる。|
|コンポジション(composition)|◆―――|集約の内、関係が強いもの。◆側が全体となる。|
|依存(dependency)|<---------|関連の内、依存元が変更されると依存先にも変更が必要になるもの。矢印の先が依存先|
|汎化(generalizatioon)|◁―――|クラスにおける継承の関係。矢印側がスーパークラス|
|実現(realization)|◁---------|クラスにおけるインターフェース実装の関係。矢印側がインターフェース|  

※「―――――」は実線。◁――の向きは、クラスを特定出来る向きと考えると分かり易い。  
※ex. スーパークラスは、継承クラスを特定することはできないが逆は可能。





## コンポーネント図

## オブジェクト図  
オブジェクト(インスタンス)とオブジェクト同士の関連で表現する。  
クラス図は、構造と構造の関連表現であるのに対し、オブジェクト図はインスタンスとインスタンス操作による関連表現。  

|インスタンス名 :クラス名|
|:--:|
|属性名=値|  


## パッケージ図

# 2. 振る舞い図
## ユースケース図  
システムとユーザ・外部システムとのやり取りや、機能を把握する為のダイアグラム。  
主に要求分析などの、システム全体の把握に利用される。  

|表記名|内容|表現|
|:--|:--|:--|
|アクター|システムを利用するユーザ、外部システム。|人型マーク等|
|ユースケース|システムに対する具体的な操作や命令。|楕円〇で表現。|
|システム境界|対象となるシステムの内部とアクターの境界線|システム境界の中にユースケースが書かれる。|  



## アクティビティ図

## ステートマシン図


# 相互作用図
## シーケンス図
一般的に、ユーザ要求機能(ユースケース)を実現する為に、オブジェクト間の相互作用(メッセージング)を時系列で表現したもの。
システム内のオブジェクト同士がどのように振舞うかを表現。  

## コミュニケーション図
オブジェクト間のメッセージングを示す。シーケンス図が時系列を重視するのに対し、コミュニケーション図はオブジェクト同士の関連を重視する。  
メッセージには必ず、実行順序を示すシーケンス番号を付ける。
見た目的には、ユースケース図におけるアクターとユースケースを線で結んで一連処理に
メッセージングを記述したもの。  


## タイミング図  
