# JavaScript コーディングガイドライン

## はじめに

JavaScriptのコーディングをする上でのガイドラインになります。  
**既にプロジェクトで、コーディング規約がある場合はそちらの方に従ってください。**  
特にプロジェクトで規約が無い場合には、こちらのコーディングガイドラインを参考にしてください。

主に参考にしているガイドラインは以下になります。

1. [MDN](https://developer.mozilla.org/ja/)の[JavaScript style guide](https://developer.mozilla.org/ja/JavaScript_style_guide)
2. [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
3. [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
4. [Felix's Node.js Style Guide](http://nodeguide.com/style.html)
5. [airbnb/javascript](https://github.com/airbnb/javascript)

## ホワイトスペース

* **[MUST]** インデントは **スペース2つ** でインデント。タブは使用しない。
* **[SHOULD]** 1行は、 **120文字以内** にするようにする。横スクロールが出ないように。
    * 折り返す際は関連する行のインデントに合わせる。
* **[AVOID]** 行末に **絶対に** スペースは入れないようにする。
* **[MUST]** キーワードの後ろにはスペースを入れる。 例： `if (a === b)`
* **[MUST]** 二項演算子はスペースで区切る。 例： `a + b`
* **[MUST]** コンマ・セミコロンの**後ろ**にスペースを入れる。
* **[SHOULD]** ブロックとブロックの間は1行以上空ける。

```javascript
var foo = function() {
    // 処理
}

var bar = function() {
     // 処理
}
```

## 記号

* **[SHOULD]** インラインで記述する関数や、オブジェクトではコンマ・セミコロンの前以外では波括弧の前後にスペースを入れる。

```javascript
var hoge = function(value) { return { fuga: value }; }
```

* **[DON'T]**パラメータリストや配列の添え字などの角括弧や丸括弧の中のスペースは必須ではありません。

- **[MUST]** ブロックは、波括弧は省略せずに記述してください。JavaScriptはインタプリタの種類によっては、解釈する際に勝手にセミコロンを自動挿入するので変なエラーが起こり得ます.

```javascript
if (a === b) {
  return a + b;
}  // ブロックの場合は波括弧の省略をせずに記述

if (a === b)
  return a + b;  // NG
```

* **[MUST]** JavaScriptではシングルクォートとダブルクォートで機能は違いませんが、JSON形式で記述する目的以外ではシングルクォートで記述してください。

```javascript
var foo = 'foofoo';  // OK

var bar = "barbar"; // NG

var baz = '<a href="http://www.ameba.jp">Ameba</a>';  // HTMLを文字列として使用する場合にダブルクォートをエスケープしないで済む
```

## 命名規則

* **[MUST]** 命名には基本的には `functionNamesLikeThis` や `variableNamesLikeThis` のように命名してください。
* **[MUST]** 定数や名前空間は `CONST_NAMES_LIKE_THIS` のように命名してください。
* **[MUST]** コンストラクタ呼び出しをする関数は `ClassNamesLikeThis` のように命名してください。
* **[MUST]** jQueryオブジェクトを入れる変数は、 `$foo = $('#bar');` のようにプレフィックスとして `$` を付けてください。
* **[SHOULD]** 関数などは動詞 + 名詞で命名してください。
* **[SHOULD]** 関数以外の変数などは名詞・形容詞 + 名詞で命名してください。
* **[MUST]** 命名は他の人が読んでも分かるように、具体的にしてください。
    * 名前が多少長くても問題ないです。一般性が無いものを略称にするのは可読性が悪くなります。やめましょう。

```javascript
var HOGE = HOGE || {};  // 名前空間の定義はこのように命名

HOGE = {
    checkForm: function() {  // 動詞 + 名詞
        var $textInput = $('#contact > input[type=text]'),  // jQueryオブジェクトはプレフィックスに$を付ける
            inputValue = $textInput.val(),  // jQueryを使用していても、返り値がjQueryオブジェクトでない場合は$を付けない
            a = [], // 意味が分からない変数はNG
            $submitButton = $('#contact > input[type=submit]');  // 関数ではないので、この場合はbuttonSubmitなどにする
    },
    ControlSubmit: function() {  // jQueryを使用してる場合は、あまり無いが、コンストラクタ呼び出しする場合にはこの命名
    }
};
```

## コメント

* **[MUST]** コメントはソースコードの先頭以外では、 `//` を使用してください。コード中で `/* */` を使用するとバグが混入する可能性があります。
* ただし、JSDocを書くのはもちろん上記の限りではありません。
* [JSDoc](http://usejsdoc.org/)の使用については、ここでは触れません。

```javascript
/*
先頭行で複数コメントを記入するのは、OK
*/

/**
 * hoge
 *
 * @name hoge
 * @function
 * @param x
 * @param y
 * @return
 */
var hoge = function(x, y) {  // JSDocはもちろん大丈夫
    this.x = x;
    this.y = y;

    return x + y;
}

// var fuga = function() {
//   var reg = /abc.*/g;  このように複数行でのコメントアウトをする
// }

/*
var bar = function() {
    var reg = /abc.*/g;  ここで正規表現をこのように使用すると、コメントの最終行と判断されてしまうので…
}
*/  // これ以降のコードが動作しなくなるのでNG

```

* **[SHOULD]** 不要なコメントは残さないでください。特に仕様や動作が変更されているのに、コメントが変更されていないと保守がしにくいです。

## ブロック

* **[SHOULD]** `if/else/for/while/try` などは常に、複数行で記述してください。短かいブロックでも1行で記述する事は避けてください。

```javascript
if (a === b) {  // OK
    return;
}

if (a === b) { return; }  // NG。可読性確保の為です。どうせcompressされるし。
```

* **[MUST]** `if/else` や `try/catch` は同じ行に波括弧を書いてください。

> JavaScriptはインタプリタが解釈する際に勝手にセミコロンを自動挿入するので変なエラーが起きえます。

```javascript
if (a === b) {  // OK
    return;
} else {
    return false;
}

if (a === b)  // NG。C言語だとこの形式(バークレースタイルでしたっけ？)多いですが、JavaScriptでは避けた方が無難…
{
    return;
}
else
{
    return false;
}
```

## 文字列の連結

**[MUST]** 長い文字列の連結をする際は `+` を行末に置いてください。セミコロン自動挿入などにより無用なエラーや、圧縮時のトラブルを防ぐためです。

```javascript
var a = 'hogehogehogehogehogehogehogehogehogehogehogehogehogehogehogehoge' +
    'fugafugafugafugafugafugafugafugafugafugafugafugafugafugafugafuga'; // OK

var b = 'hogehogehogehogehogehogehogehogehogehogehogehogehogehogehogehoge'
    + 'fugafugafugafugafugafugafugafugafugafugafugafugafugafugafugafuga'; // NG
```

## オブジェクト・配列の記法

* **[SHOULD]** オブジェクトは複数行にして記述してください。配列の場合は基本1列で記述してください。長くなるようなら、キリ良く複数行対応で。
* **[MUST]** オブジェクトのKeyが複数の単語になる・予約語の場合はシングルクォートで囲ってください。

```javascript
var a = ['foo', 'bar'];  // OK
var b = [ // OK
  'foo', 'bar', 'baz',
  'foo2', 'bar2', 'baz2',
  'foo3', 'bar3', 'baz3',
  'foo4', 'bar4', 'baz4'
];
var c = { // OK
  hoge: 'foo',
  'is huga': 'bar' 
}

var a = [ // NG
  'foo', 'bar'
];
var b= { //NG
  "hoge": 'foo',
  is huga: 'bar'
}
```

## コンストラクタ

* **[AVOID]** `prototype` に`{}`を入れて継承する場合には、 `new` を使用して呼び出す際に、コンストラクタが `Object` になってしまいます。
	* `prototype` で自動で設定されるコンストラクタは上書き可能な為に `Object` が代入されてしまう為です。
	* `Object`が`prototype`になってしまう一番の弊害は、先にインスタンスを呼び出しておいて後から`prototype`を設定された場合にエラーが起きることです。
	* 意図して、上記のような実装をすることはないと思いますが、予期せぬバグになりえます。

```javascript
function Hoge() {
  console.log('new hoge');
}

var hoge = new Hoge();

// NG
Hoge.prototype = {
    fuga: function fuga() {
        console.log('hoge fuga');
    },
    hogera: function hogera() {
        console.log('hoge hogera');
    }
};

// hogeのconstructorがオブジェクトなので、先にインスタンス呼び出ししてるとエラー
hoge.hogera();
//
//Uncaught TypeError: Object [object Object] has no method 'hogera'
```

* **[MUST]** 上記を防ぐ為に、以下の方法で実装してください。

```javascript
Hoge.prototype.fuga = function fuga() {
    console.log('hoge fuga');
};

Hoge.prototype.hogera = function hogera() {
    console.log('hoge hogera');
};

console.log(hoge.constructor) // function Hoge() { console.log('new hoge'); } 
hoge.hogera(); // hoge hogera
```

## 等価演算子

* **[MUST]** JavaScriptでは、2種類の等価演算子が存在しています。( `==` と `===` )
	* 一部の例外を除いて、比較には厳密等価演算子( `===` )を使用してください。等価演算子( `==` )を使用すると暗黙の型変換が起こりバグの原因になってしまいます。
	* 型変換が起きると、実行速度的にも遅くなります。ほぼ迷わず、厳密等価演算子で問題無いです。

> 注意する点としては、既存のソースを等価演算子だからと言って、厳密等価演算子にしてしまうと動いていたコードが動かなくなる場合があります。

* **[SHOULD]** 上記の一部の例外とは `null` と `undefined` を判定する場合です。この時に `null` と `undefined` の両方の値を評価したい場合に、等価演算子( `==` )を使用すると、本来は `null` と `undefined` は別のオブジェクトですが両方とも評価します。

> 詳しくは[Qiita](http://qiita.com/items/7d6763ba2594b8f36163)に書きました。

```javascript
var a = 0;  // 変数aはNumber
if (a === '') {
    console.log('OK'); // このlogは表示されないので、OK
}

var b = 0;  // 変数bはNumber
if (b == '') { // ここで暗黙の型変換が起こり、trueで評価される
    console.log('NG'); // このlogは表示されてしまい、NG
}

var c = undefined;  // 変数cはundefined
if (c == null) {  // 右辺をnullにして、等価演算子で評価すると…。
    console.log('OK');  // このlogが表示されて、OK
}
```

## 三項演算子について

* **[MUST]** 三項演算子を使用する場合は、可読性を良くする為に条件文に丸カッコを付けて `(condition) ? a : b;` のように記述してください。
	* `if` 文を使用した方が可読性は間違いなく高くなるので、三項演算子は主に変数への代入する際に条件によって値を分岐させる場合などに使用してください。乱用は可読性が悪くなるので、禁物です。

```javascript
var foo = (a === 'bar') ? return true : return false;  // OK:  条件部分は丸括弧で括る。
var foo = a === 'bar' ? return true : return false;  // NG: 丸括弧が無いとこのように可読性が低くなる。
```

## 変数定義

* **[SHOULD]** 変数の定義は関数スコープの中の先頭に1つだけ定義してください。ただし、コードサイズや速度的に遅くなるなどの理由がある場合は、もう1つだけ設置できます。(最大2つ)
* **[SHOULD]** `for` 文のカウンターの定義は、こちらのルールに当てはめません。条件文で変数宣言しても良いですし、スコープの先頭で宣言しても構いません。
* **[SHOULD]** `var` の下には必ず1行の空行を入れてください。
* **[SHOULD]** 宣言時に未定義の変数は最後にまとめてください。
* **[SHOULD]** 変数定義の場所がバラバラだと、デバッグが困難になったり、潜在的なバグの原因になります。
* **[AVOID]** 変数は必ず定義してから、呼び出してください。意図しない[**巻き上げ**](http://bonsaiden.github.com/JavaScript-Garden/ja/#function.scopes)でバグになる可能性があります。

```javascript
var testFunc = function() { // OK
    var foo = 'foofoo',  // 変数はスコープの先頭で全て定義しておく。また、 `var` は1つのみ宣言する。各変数で `var` を宣言しない。
        bar = true,
        baz = {
            bazbaz: 123456
        },
        hoge, fuga;  // DOMを取得するなどで、この時点では代入できない場合は、最後に変数の宣言のみしておく

    if (foo === 'foofoo') {
        hoge = foo.length;
        // 処理
    }
}

var testFunc = function() { // NG
    var foo = 'foofoo';
    var baz = {
        bazbaz: 123456
    };  //各行に `var` を宣言しない。
    if (foo === 'foofoo') {  // 空行を入れていない。
        var bar = true;
        // 処理
        var hoge = foo.length;  // 変数定義の場所は先頭にまとめる。
    }
}
```

## Strict Mode セーフなコードにする

* **[MUST]** ECMAScript 5.1のStrict Modeを仮に指定したとしても、動くコードを書いてください
	* 具体的には **行末のセミコロンの省略** 、 **`arguments.callee` の使用** などです。
* **[DON'T]** 下記にあるように `eval()` 、 `with()`や`new Function()` の使用も禁止です。
* **[DON'T]** `arugments.callee` 、 `eval()` 、 `with()`や`new Function()` は特にパフォーマンス面で深刻な問題があります。
* 他にもあるのですが、[JSHint](http://www.jshint.com/)のオプションで `Assume > EcmaScript 5` にチェックを入れても、問題無いようにしてください。
    * JSHintのバージョンが2以降の場合はデフォルトで`EcmaScript 5`のオプションが`true`になっています。

```javascript
var a = 200 * 50  // 行末のセミコロンが抜けているので、NG

function hoge() {
    return arugments.callee; //arguments.calleeは使用禁止
}
```

## JavaScript特有の注意点

* **[DON'T]** 特別な理由が無い限り、 `eval()` 、 `with()`や`new Function()` の使用は禁止です。普通のWebアプリを作るのに際して、この3つを使わなければならない状況は無いはずです。

```javascript
var hoge = function(string) {
    return test.prop[string];  // 普通に関数を使うのでOK
}

var hoge = function(string) {
    var reference;

    eval('reference = test.prop.' + string);  // eval()の使用はNG
    return reference;
}
```

* **[DON'T]** JSONファイルのパースは上述のように `eval()` ではなく、 `JSON.parse()` を使用してください。jQueryは `jQuery.ajax()` など使用すれば問題ありません。

> 特に[最近のブラウザ](http://caniuse.com/#search=JSON)では、 `JSON` がほぼ搭載されてます。どうしてもIE対応などで必要な場合は*json2.js*などを使用するのをオススメ

```javascript
var parseJson = function(json) {
    var json = JSON.parse(json);  // OK
}

var parseJson = function(json) {
    var json = eval(json);  // NG
}
```

* **[DON'T]** `setInterval()` や `setTimeout()` 、の引数に文字列を渡さないようにしてください。引数には必ず関数か、匿名関数を渡すようにしてください。

> 文字列で引数を渡すと、内部で `eval()` を使用して評価されるので大変に遅くなります。

```javascript
setTimeout(function() {  //setTimeout()には必ず関数を引数で渡すようにする。OK
    var target += 3;

    if (target.length > 0) {
        return result;
    }
}, 500);

setTimeout('var target += 3; returnResult(); if (target.length > 0) { return result;}', 500);  // 文字列で関数を渡すのはNG
```

* **[AVOID]** `try-catch-finally` を速度が必要な部分で使用しないでください。特に `catch` 節で複雑な処理をしないでください。

> これも `catch` 部分のコードが内部で `eval()` を呼んでしまうからです。

```javascript
var targetArray = ['1st', '2nd', '3rd'];
for (var i = 0, len = targetArray.length; i < len; i++) { // 大体のコードでは、try-catch-finaly文を使用しないでも同じ動きのコードが書ける
    if (test[targetArray[i]]) {
        test[targetArray[i]].prop = val;
    }
}

var targetArray = ['1st', '2nd', '3rd'];
for (var i = 0, len = targetArray.length; i < len; i++) {
    try {
        test[targetArray[i]].prop = val;
    } catch(e) {  // catch節の中で、ループするなどは速度低下の原因
        for (var j = 0, len = hoge.length; j < len; j++) {
            // 複雑な処理
        }
    }
}
```

* **[AVOID]** グローバル変数を特別な理由が無い限り、使用しないでください。Google Analyticsなどで使用する場合はコメントを残してください。

```javascript
var foo = function() {
    var i, hoge = ''; // 変数がローカルなので、OK
    for (i = 0; i < 20 i++) {
        hoge += i;
    }
}
foo();

var i, hoge = ''; // 変数がグローバルなので、NG
var foo = function() {
    for (i = 0; i < 20 i++) {
        hoge += i;
    }
}
foo();
```

* **[MUST]** 各画面で引き回す値などがある場合は必ず *名前空間* を区切って、その中に値を格納するように心がけてください。

```javascript
// 名前空間(この場合はnamespace)オブジェクトの下にさらにオブジェクトを作る
var window.namespace.hoge = {  // ok
    message: 'hello'
};

// グローバル変数として、オブジェクトを作ると他の同名の変数で上書きされたりする
var hoge = {  // ng
        message: 'hello'
};
```

* **[DON'T]** 暗黙の型変換をしないようにしてください。具体的には等価演算子を使用するようにしてください。
	* 暗黙の型変換するとパフォーマンス的にも遅いです。
		  * 速度の違いはこちらの[JSPerf](http://jsperf.com/super-duper-equality-checks)を参考にしてください

```javascript
var a = 0;  // 変数aはNumber
if (a === '') {
    console.log('OK'); // このlogは表示されないので、OK
}

var b = 0;  // 変数bはNumber
if (b == '') { // ここで暗黙の型変換が起こり、trueで評価される
    console.log('NG'); // このlogは表示されてしまい、NG
}
```

* **[AVOID]** `for-in` 文をむやみに使用しないでください。大概が `for` 文で事足ります。オブジェクトの走査などで使用する場合は必ず `Object.hasOwnProperty()` を併用してください。
	- `for-in` 文をオブジェクトの走査に使用すると、大元のObjectまで走査しにいきます。なので、 `Object.hasOwnProperty` 無しだと無駄なコストがかかってしまいます。
	- `for-in` + `Object.hasOwnProperty` の代わりに、 `Object.keys(obj)` を使用する事もできます。使えるならこちらの方がパフォーマンスは良いはず。(関数呼び出しのコストが無くなる)
* **[DON'T]** `for-in` 文を配列に対して使う事は特に厳禁です。パフォーマンスがむちゃくちゃ落ちます。

```javascript
var array = ['foo', 'bar', 'baz'],
    sum = 0;
for (var i = 0, len = array.length; i < len; i++) { // 配列の場合はfor文でループできる。
    sum += array[i];
}

var array = ['foo', 'bar', 'baz'],
    sum = 0;
for (i in array) {  // この場合にfor-in文を使用すると非常に遅くなる。
    sum += array[i];
}

var object = {
foo: 'hoge', 
     bar: 'huga',
     baz: 'hogehoge'
},
    result = [];

for (name in object) { // オブジェクトの走査の場合はfor-inを使用してもOK
    if (object.hasOwnProperty(name)) { // その場合Object.hasOwnPropertyで判定して、必要以上にチェーンを遡らないようにする。
        result.push(object[name]);
    }
}

// ES5の `Object.keys(obj)` を使用する場合はこんな感じかな
var object = {
foo: 'hoge',
     bar: 'huga',
     baz: 'hogehoge'
},
    result = [],
    keys = Object.keys(object), key;

// そこまで大量なプロパティが無ければ…
keys.forEach(function(name) {
    result.push(object[name]);
});

// 速さ命の場合はfor文で
for (var i = 0, len = keys.length; i < len; i++) {
    key = keys[i];

    result.push(object[key]);
}
```

* **[SHULD]** `for` 文の条件内で、 `length` を使用する場合は1回変数に格納してください。変数にキャッシュする事により、ループの速度が速くなります。

```javascript
var array = ['foo', 'bar', 'baz'],
    html = '';

for(var i = 0, len = array.length; i < len; i++) {  // array.lengthを変数lenに格納する事により、ループ回数に関わらず1回参照するだけ。
    html += array[i];
}

var array = ['foo', 'bar', 'baz'],
    html = '';

for(var i = 0; i < array.length; i++) {  // array.lengthを変数に入れないと、ループ毎にarray.lengthを参照してしまう。
    html += array[i];
}
```

* **[SHOULD]** オブジェクト・配列・正規表現・関数・文字列・数値などは、特別に理由が無い限り、リテラルで生成してください。コンストラクタ呼び出しは速度が遅くなります。

```javascript
var object = {},
    array = [],
    regex = /hoge.*/g,
    func = function() {},
    string = '';  // 特別理由が無ければ、リテラルで生成する。

var object = new Object(),
    array = new Array(),
    regex = new RegExp('hoge.*', g),
    func = new Function(),
    string = new String();  // コンストラクタ呼び出しは、基本しない
```

> なぜ、使用を禁止されるものがあるかなどの詳しい理由は、ここでは字数が足りませんので、説明しません。参考図書として[JavaScript: The Good Parts](http://www.oreilly.co.jp/books/9784873113913/)をお勧めします。また、Webサイトでは[Efficient JavaScript](http://dev.opera.com/articles/view/efficient-javascript/)、[Efficient JavaScript 日本語訳](http://www.hyuki.com/yukiwiki/wiki.cgi?EfficientJavaScript)、[JavaScript Garden](http://bonsaiden.github.com/JavaScript-Garden/ja/)が参考になります。


## クライアントサイドJavaScript高速化の要点

今までは、JavaScript単体での注意点ですが、DOM操作を含めたクライアントの描画の要点をまとめてみます。

### HTMLのレンダリングをブロッキングしないようにscriptタグは下部に置く

`<head>` タグに `<script>` を書くと、どうしてもブラウザのレンダリングを阻害するので(白くなったりとかはこれのせいです)
`</body>` の直前に書くのが良いです。大体YSlowとかGoogle Speedもこれは見てます。 **DOMContentLoaded** のタイミングでJavaScriptの実行がされて、
なおかつ、HTMLの中に *onclick* などがインラインで書かれてたりしなければ、ほぼ不具合は無いはずです。jQueryは `$(function {});` でイベントハンドリング
すれば勝手に **DOMContentLoaded** のタイミングで実行してくれます。

### document.write()をHTMLに書かない

今どき `document.write()` はねーよww とか思いますが、案外入れらる事があったりも。気を付けようねレベルですが、これが入るとその部分でレンダリングが
絶対止まります。そこで他に `<script>` タグとか呼んでたら、事によるとその実行まで入ったりします。ダメ絶対。

### ブラウザの再描画・再フローをなるべくしない

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

### ずっと続くタイマーにsetInterval()を使用しない

代わりに、 `setTimeout()` を再帰で呼び出した方が良いです。理由としてはブラウザはシングルスレッドなので一度に1つの事しか実行できません。が、 `setInterval()` は
呼び出されると延々と将来のスレッドにも呼び出された処理を(止めるまで)挟み込み続けるからです。

これは例えばアニメーションがガクガクするなどの原因になります。既に `setInterval()` が予約されてるスレッドをアニメに挟みこんでしまうからです。

`setTimeout()` を再帰で呼び出すと将来に渡って延々では無く次の処理を予約くらいで済む為に、アニメーションの処理なんかがスレッドにちゃんと入りますので、
そこまではガクつかないです。(処理にもよりますけど)

### `console.log`をソースに残さない

今までのコードサンプルでこれでもかと使っている`console.log`ですが、本番環境では特に消しておくようにしてください。
ログに吐くオブジェクトの内容によってはメモリーリークの原因になりかねないです。色々手段はあると思うので、頑張って消していきましょう。

参考：http://www.ibm.com/developerworks/jp/web/library/wa-jsmemory/#N101BF

### JavaScriptをなるべく使わない

どっちらけな感じですが、これが一番速いです。これだけだと身も蓋も無いのですが、要はHTML・CSSで出来る事はJavaScriptでやらないという事を意識すると良いかと。

```javascript
// user jQuery
$('#hoge').css('color', 'red'); // こっちより
```

```css
.red { color: red; }
```

```javascript
// user jQuery
$('#hoge').addClass('red'); // こっちが良い
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

