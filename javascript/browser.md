# [WIP] クライアントサイドJavaScript高速化の要点

DOM操作を含めたクライアントの描画の要点をまとめます。

**[CAUTION] こちらは大分昔の情報になるので、随時アップデートします。参考程度にしておいてください**

## HTMLのレンダリングをブロッキングしないようにscriptタグは下部に置く

`<head>` タグに `<script>` を書くと、どうしてもブラウザのレンダリングを阻害するので(白くなったりとかはこれのせいです) `</body>` の直前に書くのが良いです。

大体YSlowとかGoogle Speedもこれは見てます。 **DOMContentLoaded** のタイミングでJavaScriptの実行がされて、なおかつ、HTMLの中に *onclick* などがインラインで書かれてたりしなければ、ほぼ不具合は無いはずです。

jQueryは `$(function {});` でイベントハンドリングすれば勝手に **DOMContentLoaded** のタイミングで実行してくれます。

## document.write()をHTMLに書かない

これが入るとその部分でレンダリングが絶対止まります。そこで他に `<script>` タグとか呼んでたら、事によるとその実行まで入ったりします。

## ブラウザの再描画・再フローをなるべくしない

再描画はDOM要素の色や見た目、再フローは位置・サイズ変更・追加などでブラウザがレンダリングしなおすことです。

ブラウザの動作の中で非常にコストが高い上、回線速度によっては真っ白になったりするなどユーザーの体感速度にもかなり影響してきます。

速度的には **再フロー &gt; 再描画** という順番になります。

とは言え、JavaScriptでは上記の操作は避けて通れないのですが、以下のような施策で再描画の回数を減らすようにすると良いです。

* 複数のDOM要素の追加などは、1つずつ追加するのではなく一気に追加する

> 例えば `ul` 内に `li` を追加していく場合だとかが簡単な例なのですが、以下のコードだと再描画が `li` 追加毎に走るので体感速度が遅いです

```javascript
// use jQuery
var list = '';

for(var i = 0, l = json.length; i < l; i++) {
    list += json[i];
    $('ul').append(list);
}
```

> この場合は `for` 文の中で `li` を作成してそれをループの外で一気に `ul` に挿入すると速度がアップします

```javascript
// use jQuery
var list = '';

for(var i = 0, l = json.length; i < l; i++) {
    list += json[i];
}

$('ul').append(list);
```

> 余談ですが、最近のブラウザであれば、文字列を連結する方が `Array.push()` と `Array.join()` よりも高速なものが多いです。(古いIEを除く)

* 再描画の範囲はなるべく小さくする

> 子要素で再フローが起きると結局の所、親の要素に次々と影響します。ベストなものは `position: absolute` などして、ブラウザの描画フローから切り離していくと最小限の影響になります。

* 既存のDOM要素の再描画・再フローが避けられない場合は `display:none` を設定してCSSなどを変更してから `display:block` にして表示させる。

> 特に複数のプロパティを設定する場合などはその度に再描画・再フローを実行しようとするので、影響が最小限になるように表示させないようにすると効果的です。

* 裏技として、 `document.createDocumentFragment()` を使う方法があります。 

> DOMツリーには挿入されるけど、描画の範囲外というのが `documentFragment` なのでここに `appendChild()` するという手です。[http://ejohn.org/apps/workshop/adv-talk/#6](http://ejohn.org/apps/workshop/adv-talk/#6)←これが分かりやすいかも 

## ずっと続くタイマーにsetInterval()を使用しない

代わりに、 `setTimeout()` を再帰で呼び出した方が良いです。理由としてはブラウザはシングルスレッドなので一度に1つの事しか実行できません。が、 `setInterval()` は
呼び出されると延々と将来のスレッドにも呼び出された処理を(止めるまで)挟み込み続けるからです。

これは例えばアニメーションがガクガクするなどの原因になります。既に `setInterval()` が予約されてるスレッドをアニメに挟みこんでしまうからです。

`setTimeout()` を再帰で呼び出すと将来に渡って延々では無く次の処理を予約くらいで済む為に、アニメーションの処理なんかがスレッドにちゃんと入りますので、
そこまではガクつかないです。

## `console.log`をソースに残さない

今までのコードサンプルでこれでもかと使っている`console.log`ですが、本番環境では特に消しておくようにしてください。
ログに吐くオブジェクトの内容によってはメモリーリークの原因になりかねないです。色々手段はあると思うので、頑張って消していきましょう。

参考：http://www.ibm.com/developerworks/jp/web/library/wa-jsmemory/#N101BF

## JavaScriptをなるべく使わない

これが一番速いです。要はHTML・CSSで出来る事はJavaScriptでやらないという事を意識すると良いです。

```javascript
// user jQuery
$('#hoge').css('color', 'red'); // NG: JavaScriptでcssの操作してる
```

```css
.red { color: red; }
```

```javascript
// user jQuery
$('#hoge').addClass('red'); // OK: あらかじめスタイリングされたclassを付けるのみなので、こちらの方が高速
```

## jQueryを使用したクライアントサイドJavaScript特有の注意点

* ライブラリを使用する場合は出来る限り最新のものを使用する。バージョンが上がるとフレームワーク自体のパフォーマンスが向上している事が多いです。

> ただし、これは新規で作る場合。公開されているものは安易にバージョンを上げると後方互換が取れずに不具合が出る可能性があるので、慎重にして下さい。

* DOMの取得が速い順番で `$('#id')` >= `$('element')` > `$('.class')` > `$('[attribute=value]')` >= `$(':pseudo')` となる。

> DOMの取得は疑似セレクタが凄く遅いです。なぜかというと、疑似セレクタに関してはjQueryは一回Sizzleエンジン(CSSセレクタエンジン)を通して処理するからです。
> DOMの取得の速度順というのは、Pure JavaScriptでメソッドが用意されているもの→Sizzle使うものという順番です。
> ただ上記は一般的に…というもので、ブラウザの実装状況には左右されます。 `document.querySelectorAll()` が実装されてるかどうかなど。

* jQuery オブジェクトを取得する場合には出来る限り、idをHTMLに指定してください。
* 取得したい要素にidが振れない場合には、その親要素などにidを振って下さい。
* どうしてもid以外の指定をしなくてはいけないという場合もあるかと思いますが、その場合は出来る限りの直近の親などから目的の要素を見つけるようにすると良いです。
その際に、なるべくjQueryセレクタの中で、疑似要素なんかを使わない方が高速になります。

> 擬似セレクタに加えて、jQueryのカスタムセレクタを使用しないでください。

* `querySelectorAll()`で解釈できる疑似セレクタの他、Sizzleエンジンには独自の疑似セレクタが存在します。
* これらは前述の通りの低速であることに加えて、[w3c](http://www.w3.org/TR/css3-selectors/)で提唱されてるわけではないため、使わないほうがいいです。

`:animated`, `[name!="value"]` `:button`, `:checkbox`, `:contains()`, `:eq()`, `:even`,  
`:file`, `:first`, `:gt()`, `:has()`, `:header`, `:hidden`, `:image`, `:input`, `:last`, `:lt()`,  
`:odd`, `:parent`, `:password`, `:radio`, `:reset`, `:selected`, `:submit`, `:text`, `:visible`  

> *ex* `$('#hoge').find('li').eq(3).text();` の方が `$('#hoge li:eq(3)').text();` よりも高速なはずです。

* イベントハンドラを付ける場合は基本的に該当部分の要素に付けていきますが、例えば `ul` 内の複数の `li` 要素それぞれをイベントハンドリングする場合は効率が悪いので、 `delegate()` メソッドを使ってイベントのバブリングを `ul` 要素でキャッチする方が効率良いです。
* 基本として、HTML/CSSでまかなえる事をJavaScriptでは行わないようにしてください。

> 例えば、css()で色々要素の装飾をするよりは、id/classを振って、cssで設定した方がパフォーマンスが良いです。

* HTMLにid/classを付ける場合は `js-` のプレフィックスを入れる。こうする事によりデザイン部分に必要なid/classか、JavaScriptでのターゲッティングに必要なだけのid/classなのかを区別できるので、後々のリファクタリングがやり易くなります

```html
<div id="list-wrapper" class="list-divider"></div> <!-- NG -->

<div id="js-list-wrapper" class="js-list-divider"></div> <!-- OK -->
```

## SPA特有の注意点

`Backbone.js` や、 `Angular.js` などを使用してのアプリケーション開発では気をつけないと容易にメモリリークを起こしてしまいます。

特に画面が遷移した後に、以前のイベントを取り除かないなど

### 参照URL

メモリリークについては[こちら](http://www.ibm.com/developerworks/jp/web/library/wa-jsmemory/?cmp=dw&cpb=dwwdv&ct=dwrss&cr=dwrss&ccy=jp&csr=120712)が分かりやすい解説です。
Backbone.jsに焦点を合わせたメモリーリーク対策は下記の記事が参考になると思います。

- [Avoiding memory leaks in backbone.js](http://unspace.ca/blog/avoiding-memory-leaks-in-backbone/)
- [Avoiding Common Backbone.js Pitfalls](http://ozkatz.github.io/avoiding-common-backbonejs-pitfalls.html)
- [Backbone.js And JavaScript Garbage Collection](http://lostechies.com/derickbailey/2012/03/19/backbone-js-and-javascript-garbage-collection/)
    - 若干古いです

Gmailでのメモリリークの対処を書いた[こちら](http://www.html5rocks.com/en/tutorials/memory/effectivemanagement/)の記事は、Devツールを使っての原因特定の  
仕方などもあり大変に参考になります。

### 具体例

- イベントハンドラを削除
- 参照がないDOMの削除
- 循環参照をなくしていく
- 必要がなくなった変数の初期化

`View.remove()`や`View.stopListening()`、`Event.off()`、`Event.unbind()`、`Event.stopListening()`などを忘れずに活用していきましょう。

