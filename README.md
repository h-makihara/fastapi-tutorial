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

## サンプル

Python において`@decorator`シンタックスはデコレータと呼ぶ。  
デコレータは、直下の関数を受け取り、それを使って何かを行う。

- パスオペレーションデコレータ
  ```
  @app.get("/")
  ```
  上記は、直下の関数が下記リクエスト処理を担当することを FastAPI に伝える
  - パスが`/`である
  - `get`オペレーションである

# パスオペレーション

上記デコレータを含め、以下のコードで考える。

- パスが`/`である
- `get`オペレーションである
- 関数はパスオペレーションデコレータの直下にある関数

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```

`GET`オペレーションを使った URL でパス「`/`」へのリクエストを受け取るたびに FastAPI によって呼び出される。

# コンテンツの返信

`dict`, `list`, `str`, `int` などを返すことが可能。  
Pydantic モデルを返すことも可能。  
JSON に自動変換されるオブジェクトやモデルも多数ある（ORM 等）。

# パスパラメータと型

標準の Python の型アノテーションを使用し、関数内のパスパラメータの型を宣言できる。  
下記の例では`item_id`は、`int`として宣言されている。  
例：

```python
async def read_item(item_id: int):
```

# 順序の問題

path operations を作成する際、固定パスを持つ状況があり得る。  
`/users/me`から、現在のユーザに関する情報を取得するとし、ユーザ ID によって特定ユーザに関する情報を取得するパス`/users/{user_id}`も持つことができる。

path operations は順に評価されるので、`/users/me`が、`/users/{user_id}`よりも先に宣言されている必要がある。

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/users/me")
async def read_user_me():
    return {"user_id": "the current user"}


@app.get("/users/{user_id}")
async def read_user(user_id: str):
    return {"user_id": user_id}
```

順序が逆の場合、`/users/{user_id}`に、`/users/me`がマッチしてしまう。  
値が「"me"」であるパラメータ`user_id`を受け取ると想定する。

# クエリパラメータ

`?`に続くキーとバリューの組で、`&`で区切られる。  
例：

```
http://127.0.0.1:8000/items/?skip=0&limit=10
```

この URL のクエリパラメータは以下の通り。

- `skip`: 値は`0`
- `limit`: 値は`10`

## オプショナルなパラメータ

デフォルト値を`None`とすることで、オプショナルなパラメータを宣言できる。  
例：

```python
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id: str, q: Union[str, None] = None):
    if q:
        return {"item_id": item_id, "q": q}
    return {"item_id": item_id}
```

この場合、関数パラメータ`q`はオプショナルとなり、デフォルトでは`None`となる。

# リクエストボディ

1. リクエストボディを JSON として読み取る
2. 必要に応じて型変換を行う
3. データ検証を行う
   - データが無効な場合はエラー応答し、どこが不正だったかを示す
4. 受け取ったデータをパラメータに変換する
   - 関数内で宣言したものと、すべての属性とその型に対するエディタサポートを利用可能
5. モデルの JSON スキーマ定義を生成し、好きな場所で使用可能
6. これらのスキーマを生成された OpenAPI スキーマの一部として自動ドキュメントの UI に使用される
