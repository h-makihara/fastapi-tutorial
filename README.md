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
