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
