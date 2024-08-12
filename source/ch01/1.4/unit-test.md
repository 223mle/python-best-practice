# ユニットテスト

## 19. テストにテスト対象と同等の実装を書かない

テストしたい関数と同等の処理をテストの中に書くな、ということ。

```python
# ダメな例
def two_plus_three(x: int = 2, y: int = 3):
    return x + y

def test_two_plus_three():
    ans = 2 + 3
    assert two_plus_three() == ans
```

```python
# 良い例
def two_plus_three(x: int = 2, y: int = 3):
    return x + y

def test_two_plus_three():
    assert two_plus_three() == 5
```

やっていることは大して変わらないが、テスト内で同様の処理を書くと、間違っていてもassertが起きないため、気づかない。
テスト関数内に何らかの処理や計算が含まれている場合に注意。

## 20. 1つのテストメソッドでは1つの項目のみ確認する

1つのテストメソッドでは、1つの項もこうのみを確認するように。
読んでいて、「いやいや、`pytest`使いましょうよ」と思っていたらしっかりpytestでの実装も説明されている。

## 21. テストケースは準備、執行、検証に分割しよう

処理は分割しよう、というテストに限らない話をテスト固有のフレームワークで説明されている。
[Arrange Act Assert](http://wiki.c2.com/?ArrangeActAssert)
以下のような形で、分割しよう、というお話。

```python
class Test:
    def test_x(self, ...):
        # 準備
        expected = ...

        # 実行
        actual = x(...)

        # 検証
        assert expected == actual
```

そもそもこの三つを同じ関数の中で行って、テストを行うのがベストプラクティスなのか？と、思った。
準備に関しては別で行うやり方の方が可読性高そうと個人的には思った。

## 22. 単体テストをする観点から実装の設計を洗練させる

「テストしにくい実装は設計が悪い」
本当にそう思う。一方で、LLM、NLP周りのテストは結構やりにくい。。
「テストをよりよく書こうとするのではなく、そもそも実装の設計を見直そう」←効く。

## 23. テストから外部環境への依存を排除しよう

LMはhugging faceなど、外部環境へアクセスすることが多い。
mockerについて調べる。

```python
def mock_pipeline(*args, **kwargs):
    return ['output']

# これを行うことで、この後呼び出されるPipelineは、['output']を返す処理になる。
mocker.patch('Pipeline', return_value=mock_pipeline)
```
