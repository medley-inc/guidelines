# JavaScript コーディングガイドライン

## はじめに

株式会社メドレーのJavaScriptのコーディングをする上でのガイドラインになります。  

主に参考にしているガイドラインは以下になります。

1. [MDN](https://developer.mozilla.org/ja/)内[JavaScript style guide](https://developer.mozilla.org/ja/JavaScript_style_guide)
2. [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
3. [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
4. [Felix's Node.js Style Guide](http://nodeguide.com/style.html)
5. [airbnb/javascript](https://github.com/airbnb/javascript)
6. [cookpad/styleguide](https://github.com/cookpad/styleguide)
7. [thoughtbot/guides](https://github.com/thoughtbot/guides)

## ホワイトスペース

この項目は主に可読性の確保が目的です。

* **[MUST]** インデントは **スペース2つ** でインデント。タブは使用しない。
* **[SHOULD]** 1行は、 **120文字以内** にするようにする。横スクロールが出ないように。
    * 折り返す際は関連する行のインデントに合わせる。
* **[DON'T]** 行末に **絶対に** スペースは入れないようにする。
* **[MUST]** キーワードの後ろにはスペースを入れる。 例： `if (a === b)`
* **[MUST]** 二項演算子はスペースで区切る。 例： `a + b`
* **[MUST]** コンマ・セミコロンの**後ろ**にスペースを入れる。
* **[SHOULD]** ブロックとブロックの間は1行以上空ける。
* **[SHOULD]** 無名関数の `function` と `()` の間にスペースはいらない。
* **[SHOULD]** 関数宣言の 関数名と `()` の間にはスペースはいらない。

```javascript
var foo = function() {
    // 処理
} // 次の行を開けておく

var bar = function() {
     // 処理
}

var hoge = function() { // OK: 無名関数では()の前にスペースなし
     // 処理
}

var hoge = function () { // NG: 無名関数では()の前にスペースあり
     // 処理
}

function fuga() { // OK: 関数宣言の名前の後にスペースなし
     // 処理
}

function fuga () { // NG: 関数宣言の名前の後にスペースあり
     // 処理
}
```

## 記号

* **[DON'T]**パラメータリストや配列の添え字などの角括弧や丸括弧の中のスペースは必須ではありません。

```javascript
for (var i = 0; i > 10; i++) {
  hoge[i] = fuga.innerHTML; // OK: []や()内にスペースが入ってない
}

for ( var i = 0; i > 10; i++ ) {
  hoge[ i ] = fuga.innerHTML; // NG: []や()内にスペースが入ってる
}
```

- **[MUST]** ブロックは、波括弧は省略せずに記述してください。JavaScriptはインタプリタの種類によっては、解釈する際に勝手にセミコロンを自動挿入するので変なエラーが起こり得ます。

```javascript
if (a === b) {
  return a + b;
}  // OK: ブロックの場合は波括弧の省略をせずに記述

if (a === b)
  return a + b;  // NG: {}が省略されている
```

* **[MUST]** JavaScriptではシングルクォートとダブルクォートで機能は違いませんが、JSON形式で記述する目的以外ではシングルクォートで記述してください。
	* 理由は文字列にHTMLを入れる際にHTML属性をダブルクォートでそのまま記述できるからです。

```javascript
var foo = 'foofoo';  // OK

var bar = "barbar"; // NG

var baz = '<a href="http://www.ameba.jp">Ameba</a>';  // OK: HTMLを文字列として使用する場合にダブルクォートをエスケープしないで済む

var baz = "<a href=\"http://www.ameba.jp\">Ameba</a>";  // NG: HTMLを文字列として使用する場合にダブルクォートをエスケープする必要がある
```

## 命名規則

* **[MUST]** 関数には基本的には `functionNamesLikeThis` のようにローワーキャメルケースで命名してください。
* **[MUST]** 定数や名前空間は `CONST_NAMES_LIKE_THIS` のように大文字スネークケースで命名してください。
* **[MUST]** コンストラクタ呼び出しをする関数は `ClassNamesLikeThis` のようにアッパーキャメルケース命名してください。
* **[MUST]** jQueryオブジェクトを入れる変数は、 `$foo = $('#bar');` のようにプレフィックスとして `$` を付けてください。
* **[MUST]** 上記以外の命名は `variable_like_this` のように小文字スネークケースで命名してください
* **[MSUT]** 関数などは動詞から始めてください
* **[DON'T]** 関数以外の変数などは動詞から始めないでください。
* **[MUST]** 命名は他の人が読んでも分かるように、具体的にしてください。
    * 名前が多少長くても問題ないです。一般性が無いものを略称にするのは可読性が悪くなります。やめましょう。
* **[SHOULD]** `this` をローカルにバインドする場合は `var self = this;` にしてください。
	* またjQueryオブジェクトであれば `var $self = $(this)` にしてください。

```javascript
var HOGE = HOGE || {};  // 名前空間の定義はこのように命名

HOGE = {
    checkForm: function() {  // 動詞 + 名詞
        var $text_input = $('#contact > input[type=text]'),  // jQueryオブジェクトはプレフィックスに$を付ける
            input_value = $text_input.val(),  // jQueryを使用していても、返り値がjQueryオブジェクトでない場合は$を付けない
            a = [], // 意味が分からない変数はNG
            $button_submit = $('#contact > input[type=submit]');  // 関数ではないので名詞から始める
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

if (a === b) { return; }  // NG: 可読性確保の為です。
```

* **[MUST]** `if/else` や `try/catch` は同じ行に波括弧を書いてください。

> JavaScriptはインタプリタが解釈する際に勝手にセミコロンを自動挿入するので変なエラーが起きえます。

```javascript
if (a === b) {  // OK
    return;
} else {
    return false;
}

if (a === b)  // NG
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
var a = { // OK
  hoge: 'foo',
  'is huga': 'bar' // OK: reserved wordsを''で囲む
}
a['is huga']; // 呼び出しのときはドットノテーションではダメです
var b = ['foo', 'bar'];  // OK
var c = [ // OK
  'foo', 'bar', 'baz',
  'foo2', 'bar2', 'baz2',
  'foo3', 'bar3', 'baz3',
  'foo4', 'bar4', 'baz4'
];

var d= { //NG
  "hoge": 'foo',
  is huga: 'bar' // NG: エラーになります
}
var e = [ // NG
  'foo', 'bar'
];
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
	* 型変換が起きると、実行速度的にも遅くなります。(現在ではこれが問題になることもないですが)ほぼ迷わず、厳密等価演算子で問題無いです。

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
