# fastapi-tutorial

## async await

関数宣言前に`async`を書けば、非同期処理ができる。  
ex.

```python
@app.get('/')
async def read_results():
    results = await some_library()
    rerturn results
```

`await`の使用をサポートしていないサードパーティライブラリ（現在の殆どのデータベースライブラリが該当）を使用している場合は、通常の`def`を使う。
※よくわからないなら通常の`def`を使う

`def`と`async def`を混在させて定義することは可能。  
FastAPI はそれに応じて正しい処理を行う。  
きちんと書けば非同期で動作し、高速になり、パフォーマンスの最適化ができる。

# OpenAPI

FastAPI は API を定義するための OpenAPI 標準規格ですべての API「スキーマ」を生成する。

## スキーマ

スキーマとは、定義または説明。  
実装コードではなく、単なる抽象的な説明。

## API スキーマ

OpenAPI は API のスキーマ定義の方法を規定する仕様。  
API パス、受取可能なパラメータなどを含む。

## データスキーマ

JSON コンテンツなどの一分のデータ形状を指す場合もある。  
そのような場合、スキーマは JSON 属性とそれらが持つデータ型などを意味する。

# path operation

## パス

最初の`/`から始まる URL の最後の部分を「パス」と呼ぶ。  
一般的に「エンドポイント」や「ルート」とも呼ばれる。  
**API を構築する際、「パス」は「関心事」と「リソース」を分離するための主要な方法。**

例えば、

```
https://example.com/items/foo
```

といった URL では、パスは次のようになる。

```
/items/foo
```

## Operation

オペレーションは、HTTP の「メソッド」の 1 つを指す。  
OpenAPI では、各 HTTP メソッドは「オペレーション」と呼ばれる。  
HTTP メソッドの例：

- `POST` : データの作成
- `GET` : データの読み取り
- `PUT` : データの更新
- `DELETE` : データの削除

更に変わったものの例：

- `OPTIONS`
- `HEAD`
- `PATCH`
- `TRACE`

HTTP プロトコルでは、これらの「メソッド」の 1 つ（または複数）を使用して各パスにアクセスできる。
