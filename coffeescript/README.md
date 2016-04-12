# JavaScript コーディングガイドライン

## はじめに

株式会社メドレーのCoffeeScriptをコーディングする上でのガイドラインです。

主に参考にしているガイドラインは以下になります。

1. [Medley JavaScript Style Guide](https://github.com/medley-inc/guidelines/tree/master/javascript)
2. [polarmobile/coffeescript-style-guide](https://github.com/polarmobile/coffeescript-style-guide)
3. [cookpad/styleguide](https://github.com/cookpad/styleguide/blob/master/coffeescript.ja.md)

## ホワイトスペース

この項目は主に可読性の確保が目的です。

* **[MUST]** インデントは **スペース2つ** でインデント。タブは使用しない。
* **[SHOULD]** 1行は、 **80文字以内** にするようにする。
    * 折り返す際は関連する行のインデントに合わせる。
* **[DON'T]** 行末に **絶対に** スペースは入れないようにする。
* **[MUST]** キーワードの後ろにはスペースを入れる。 例： `if (a == b)`
* **[MUST]** 二項演算子はスペースで区切る。 例： `a + b`
* **[MUST]** コンマ・セミコロンの**後ろ**にスペースを入れる。
* **[SHOULD]** ブロックとブロックの間は1行以上空ける。
* **[MUST]** `->` の前後にはスペースをあける

```coffeescript
foo = ->
  # 処理
  return # 次の行を開けておく

bar = ->
  # 処理
  return

hoge = ->
  # OK: 関数宣言する際、=の前後にスペースあり
  # 処理
  return

hoge=->
  # NG: 関数宣言する際、=の前後にスペースなし
  # 処理
  return
```
