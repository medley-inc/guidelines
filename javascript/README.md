# JavaScript コーディングガイドライン

**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

- [JavaScript コーディングガイドライン](#javascript-コーディングガイドライン)
    - [はじめに](#はじめに)
    - [ホワイトスペース](#ホワイトスペース)
    - [記号](#記号)
    - [命名規則](#命名規則)
    - [コメント](#コメント)
    - [ブロック](#ブロック)
    - [文字列の連結](#文字列の連結)
    - [オブジェクト・配列の記法](#オブジェクト・配列の記法)
    - [コンストラクタ](#コンストラクタ)
    - [等価演算子](#等価演算子)
    - [三項演算子について](#三項演算子について)
    - [変数定義](#変数定義)
    - [Strict Mode セーフなコードにする](#strict-mode-セーフなコードにする)
    - [JavaScript特有の注意点](#javascript特有の注意点)
    - [JSHint](#jshint)
        - [Webサービス](#webサービス)
        - [Node.jsのnpmを使用して，JSHintをインストールする](#nodejsのnpmを使用して，jshintをインストールする)
        - [WebStorm & PHPStorm](#webstorm-phpstorm)
        - [Eclipse](#eclipse)
        - [Sublime Text2](#sublime-text2)
        - [Vim](#vim)
        - [Emacs](#emacs)
        - [その他](#その他)
        - [JSHintの設定について](#jshintの設定について)
            - [JSHint 2.0での変更点](#jshint-20での変更点)
    - [JavaScriptフレームワーク・ライブラリ](#javascriptフレームワーク・ライブラリ)
        - [DOM操作](#dom操作)
        - [Async/Control Flow](#asynccontrol-flow)
        - [注意点](#注意点)
    - [クライアントサイドJavaScript高速化の要点](#クライアントサイドjavascript高速化の要点)
        - [HTMLのレンダリングをブロッキングしないようにscriptタグは下部に置く](#htmlのレンダリングをブロッキングしないようにscriptタグは下部に置く)
        - [document.write()をHTMLに書かない](#documentwriteをhtmlに書かない)
        - [ブラウザの再描画・再フローをなるべくしない](#ブラウザの再描画・再フローをなるべくしない)
        - [ずっと続くタイマーにsetInterval()を使用しない](#ずっと続くタイマーにsetintervalを使用しない)
        - [`console.log`をソースに残さない](#consolelogをソースに残さない)
        - [JavaScriptをなるべく使わない](#javascriptをなるべく使わない)
    - [jQueryを使用したクライアントサイドJavaScript特有の注意点](#jqueryを使用したクライアントサイドjavascript特有の注意点)
    - [1ソースアプリ特有の注意点](#1ソースアプリ特有の注意点)
        - [参照URL](#参照url)
        - [具体例](#具体例)

## <a name="はじめに">はじめに</a>

JavaScriptのコーディングをする上でのガイドラインになります．  
**既にプロジェクトで，コーディング規約がある場合はそちらの方に従ってください．**  
特にプロジェクトで規約が無い場合には，こちらのコーディングガイドラインを参考にしてください．

主に参考にしているガイドラインは以下になります．

1. [MDN](https://developer.mozilla.org/ja/)の[JavaScript style guide](https://developer.mozilla.org/ja/JavaScript_style_guide)
2. [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
3. [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
4. [Felix's Node.js Style Guide](http://nodeguide.com/style.html)
5. [airbnb/javascript](https://github.com/airbnb/javascript)

## <a name="ホワイトスペース">ホワイトスペース</a>

* インデントは **スペース2つ** でインデント．タブは使用しない．
* 1行は， **120文字以内** にするようにする．横スクロールが出ないように．
    * 折り返す際は関連する行のインデントに合わせる．
* 行末に **絶対に** スペースは入れないようにする．
* キーワードの後ろにはスペースを入れる． 例： `if (a === b)`
* 二項演算子はスペースで区切る． 例： `a + b`
* コンマ・セミコロンの後ろにスペースを入れる．
* ブロックの間は1行以上空ける．

```javascript
var foo = function() {
    // 処理
}

var bar = function() {
     // 処理
}
```

## <a name="記号">記号</a>

* インラインで記述する関数や，オブジェクトではコンマ・セミコロンの前以外では波括弧の前後にスペースを入れる．

```javascript
var hoge = function(value) { return { fuga: value }; }
```

* パラメータリストや配列の添え字などの角括弧や丸括弧の中のスペースは必須ではありません．

> ブロックでは，波括弧は省略せずに記述してください．JavaScriptはインタプリタが解釈する際に勝手にセミコロンを自動挿入するので変なエラーが起こり得ます．

```javascript
if (a === b) {
  return a + b;
}  // ブロックの場合は波括弧の省略をせずに記述

if (a === b)
  return a + b;  // NG
```

* JavaScriptではシングルクォートとダブルクォートで機能は違いませんが，JSON形式で記述する目的以外ではシングルクォートで記述してください．

```javascript
var foo = 'foofoo';  // OK

var bar = "barbar"; // NG

var baz = '<a href="http://www.ameba.jp">Ameba</a>';  // HTMLを文字列として使用する場合にダブルクォートをエスケープしないで済む
```

## <a name="命名規則">命名規則</a>

* 命名には基本的には `functionNamesLikeThis` や `variableNamesLikeThis` のように命名してください．
* 定数や名前空間は `CONST_NAMES_LIKE_THIS` のように命名してください．
* コンストラクタ呼び出しをする関数は `ClassNamesLikeThis` のように命名してください．
* jQueryオブジェクトを入れる変数は， `$foo = $('#bar');` のようにプレフィックスとして `$` を付けてください．
* 関数などは動詞 + 名詞で命名してください．
* 関数以外の変数などは名詞・形容詞 + 名詞で命名してください．
* 命名は他の人が読んでも分かるように，具体的にしてください．
    * 名前が多少長くても問題ないです．一般性が無いものを略称にするのは可読性が悪くなります．やめましょう．

```javascript
var HOGE = HOGE || {};  // 名前空間の定義はこのように命名

HOGE = {
    checkForm: function() {  // 動詞 + 名詞
        var $textInput = $('#contact > input[type=text]'),  // jQueryオブジェクトはプレフィックスに$を付ける
            inputValue = $textInput.val(),  // jQueryを使用していても，返り値がjQueryオブジェクトでない場合は$を付けない
            a = [], // 意味が分からない変数はNG
            $submitButton = $('#contact > input[type=submit]');  // 関数ではないので，この場合はbuttonSubmitなどにする
    },
    ControlSubmit: function() {  // jQueryを使用してる場合は，あまり無いが，コンストラクタ呼び出しする場合にはこの命名
    }
};
```

## <a name="コメント">コメント</a>

* コメントはソースコードの先頭以外では， `//` を使用してください．コード中で `/* */` を使用するとバグが混入する可能性があります．
* ただし，JSDocを書くのはもちろん上記の限りではありません．
* [JSDoc](http://usejsdoc.org/)の使用については，ここでは触れません．

```javascript
/*
先頭行で複数コメントを記入するのは，OK
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
    var reg = /abc.*/g;  ここで正規表現をこのように使用すると，コメントの最終行と判断されてしまうので…
}
*/  // これ以降のコードが動作しなくなるのでNG

```

* 不要なコメントは残さないでください．特に仕様や動作が変更されているのに，コメントが変更されていないと保守がしにくいです．

## <a name="ブロック">ブロック</a>

* `if/else/for/while/try` などは常に，複数行で記述してください．短かいブロックでも1行で記述する事は避けてください．

```javascript
if (a === b) {  // OK
    return;
}

if (a === b) { return; }  // NG．可読性確保の為です．どうせcompressされるし．
```

* `if/else` や `try/catch` は同じ行に波括弧を書いてください．

> JavaScriptはインタプリタが解釈する際に勝手にセミコロンを自動挿入するので変なエラーが起きえます．

```javascript
if (a === b) {  // OK
    return;
} else {
    return false;
}

if (a === b)  // NG．C言語だとこの形式(バークレースタイルでしたっけ？)多いですが，JavaScriptでは避けた方が無難…
{
    return;
}
else
{
    return false;
}
```

## <a name="文字列の連結">文字列の連結</a>

長い文字列の連結をする際は `+` を行末に置いてください．セミコロン自動挿入などにより無用なエラーや，圧縮時のトラブルを防ぐためです．

```javascript
var a = 'hogehogehogehogehogehogehogehogehogehogehogehogehogehogehogehoge' +
    'fugafugafugafugafugafugafugafugafugafugafugafugafugafugafugafuga'; // OK

var b = 'hogehogehogehogehogehogehogehogehogehogehogehogehogehogehogehoge'
    + 'fugafugafugafugafugafugafugafugafugafugafugafugafugafugafugafuga'; // NG
```

## <a name="オブジェクト・配列の記法">オブジェクト・配列の記法</a>

* オブジェクトは複数行にして記述してください．配列の場合は基本1列で記述してください．長くなるようなら，キリ良く複数行対応で．
* オブジェクトのKeyが複数の単語になる・予約語の場合はシングルクォートで囲ってください．

```javascript
var a = ['foo', 'bar'];  // OK
var b = [
    'foo', 'bar', 'baz',
    'foo2', 'bar2', 'baz2',
    'foo3', 'bar3', 'baz3',
    'foo4', 'bar4', 'baz4'
];
var c = {
    hoge: 'good',
    'is huga': 'bad'
}

var a = [ // NG
    'foo', 'bar'
];
var b= {
    "hoge": 'good',
    is huga: 'bad'
}
```

## <a name="コンストラクタ">コンストラクタ</a>

* `prototype` に`{}`を入れて継承する場合には， `new` を使用して呼び出す際に，コンストラクタが `Object` になってしまいます．
* `prototype` で自動で設定されるコンストラクタは上書き可能な為に `Object` が代入されてしまう為です．
* `Object`が`prototype`になってしまう一番の弊害は，先にインスタンスを呼び出しておいて後から`prototype`を設定された場合にエラーが起きることです．
* 意図して，上記のような実装をすることはないと思いますが，予期せぬバグになりえます．

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

// hogeのconstructorがオブジェクトなので，先にインスタンス呼び出ししてるとエラー
hoge.hogera();
//
//Uncaught TypeError: Object [object Object] has no method 'hogera'
```

* 上記を防ぐ為に，以下の方法で実装してください．

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

## <a name="等価演算子">等価演算子</a>

* JavaScriptでは，2種類の等価演算子が存在しています．( `==` と `===` )
* 一部の例外を除いて，比較には厳密等価演算子( `===` )を使用してください．等価演算子( `==` )を使用すると暗黙の型変換が起こりバグの原因になってしまいます．
* 型変換が起きると，実行速度的にも遅くなります．ほぼ迷わず，厳密等価演算子で問題無いです．

> 注意する点としては，既存のソースを等価演算子だからと言って，厳密等価演算子にしてしまうと動いていたコードが動かなくなる場合があります．

* 上記の一部の例外とは `null` と `undefined` を判定する場合です．この時に `null` と `undefined` の両方の値を評価したい場合に，
等価演算子( `==` )を使用すると，本来は `null` と `undefined` は別のオブジェクトですが両方とも評価します．

> 詳しくは[Qiita](http://qiita.com/items/7d6763ba2594b8f36163)に書きました．

```javascript
var a = 0;  // 変数aはNumber
if (a === '') {
    console.log('OK'); // このlogは表示されないので，OK
}

var b = 0;  // 変数bはNumber
if (b == '') { // ここで暗黙の型変換が起こり，trueで評価される
    console.log('NG'); // このlogは表示されてしまい，NG
}

var c = undefined;  // 変数cはundefined
if (c == null) {  // 右辺をnullにして，等価演算子で評価すると…．
    console.log('OK');  // このlogが表示されて，OK
}
```

## <a name="三項演算子について">三項演算子について</a>

* 三項演算子を使用する場合は，可読性を良くする為に条件文に丸カッコを付けて `(condition) ? a : b;` のように記述してください．
* `if` 文を使用した方が可読性は間違いなく高くなるので，三項演算子は主に変数への代入する際に条件によって値を分岐させる場合などに
使用してください．乱用は可読性が悪くなるので，禁物です．

```javascript
var foo = (a === 'bar') ? return true : return false;  // 条件部分は丸括弧で括る．
var foo = a === 'bar' ? return true : return false;  // 丸括弧が無いとこのように可読性が低くなる．
```

## <a name="変数定義">変数定義</a>

* 変数の定義は関数スコープの中の先頭に1つだけ定義してください．ただし，コードサイズや速度的に遅くなるなどの理由がある場合は，もう1つだけ設置できます．(最大2つ)
* `for` 文のカウンターの定義は，こちらのルールに当てはめません．条件文で変数宣言しても良いですし，スコープの先頭で宣言しても構いません．
* `var` の下には必ず1行の空行を入れてください．
* 宣言時に未定義の変数は最後にまとめてください．
* 変数定義の場所がバラバラだと，デバッグが困難になったり，潜在的なバグの原因になります．
* 変数は必ず定義してから，呼び出してください．意図しない[**巻き上げ**](http://bonsaiden.github.com/JavaScript-Garden/ja/#function.scopes)でバグになる可能性があります．

```javascript
var testFunc = function() { // OK
    var foo = 'foofoo',  // 変数はスコープの先頭で全て定義しておく．また， `var` は1つのみ宣言する．各変数で `var` を宣言しない．
        bar = true,
        baz = {
            bazbaz: 123456
        },
        hoge, fuga;  // DOMを取得するなどで，この時点では代入できない場合は，最後に変数の宣言のみしておく

    if (foo === 'foofoo') {
        hoge = foo.length;
        // 処理
    }
}

var testFunc = function() { // NG
    var foo = 'foofoo';
    var baz = {
        bazbaz: 123456
    };  //各行に `var` を宣言しない．
    if (foo === 'foofoo') {  // 空行を入れていない．
        var bar = true;
        // 処理
        var hoge = foo.length;  // 変数定義の場所は先頭にまとめる．
    }
}
```

## <a name="strict-mode-セーフなコードにする">Strict Mode セーフなコードにする</a>

* ECMAScript 5.1のStrict Modeを仮に指定したとしても，動くコードを書いてください
* 具体的には **行末のセミコロンの省略** ， **`arguments.callee` の使用** などです．
* また下記にあるように `eval()` ， `with()`や`new Function()` の使用も禁止です．
* `arugments.callee` ， `eval()` ， `with()`や`new Function()` は特にパフォーマンス面で深刻な問題があります．
* 他にもあるのですが，[JSHint](http://www.jshint.com/)のオプションで `Assume > EcmaScript 5` にチェックを入れても，問題無いようにしてください．
    * JSHintのバージョンが2以降の場合はデフォルトで`EcmaScript 5`のオプションが`true`になっています．

```javascript
var a = 200 * 50  // 行末のセミコロンが抜けているので，NG

function hoge() {
    return arugments.callee; //arguments.calleeは使用禁止
}
```

## <a name="javascript特有の注意点">JavaScript特有の注意点</a>

* 特別な理由が無い限り， `eval()` ， `with()`や`new Function()` の使用は禁止です．普通のWebアプリを作るのに際して，この3つを使わなければならない状況は無いはずです．

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

* JSONファイルのパースは上述のように `eval()` ではなく， `JSON.parse()` を使用してください．jQueryは `jQuery.ajax()` など使用すれば問題ありません．

> 特に[最近のブラウザ](http://caniuse.com/#search=JSON)では， `JSON` がほぼ搭載されてます．どうしてもIE対応などで必要な場合は*json2.js*などを使用するのをオススメ

```javascript
var parseJson = function(json) {
    var json = JSON.parse(json);  // OK
}

var parseJson = function(json) {
    var json = eval(json);  // NG
}
```

* `setInterval()` や `setTimeout()` ，の引数に文字列を渡さないようにしてください．引数には必ず関数か，匿名関数を渡すようにしてください．

> 文字列で引数を渡すと，内部で `eval()` を使用して評価されるので大変に遅くなります．

```javascript
setTimeout(function() {  //setTimeout()には必ず関数を引数で渡すようにする．OK
    var target += 3;

    if (target.length > 0) {
        return result;
    }
}, 500);

setTimeout('var target += 3; returnResult(); if (target.length > 0) { return result;}', 500);  // 文字列で関数を渡すのはNG
```

* `try-catch-finally` を速度が必要な部分で使用しないでください．特に `catch` 節で複雑な処理をしないでください．

> これも `catch` 部分のコードが内部で `eval()` を呼んでしまうからです．

```javascript
var targetArray = ['1st', '2nd', '3rd'];
for (var i = 0, len = targetArray.length; i < len; i++) { // 大体のコードでは，try-catch-finaly文を使用しないでも同じ動きのコードが書ける
    if (test[targetArray[i]]) {
        test[targetArray[i]].prop = val;
    }
}

var targetArray = ['1st', '2nd', '3rd'];
for (var i = 0, len = targetArray.length; i < len; i++) {
    try {
        test[targetArray[i]].prop = val;
    } catch(e) {  // catch節の中で，ループするなどは速度低下の原因
        for (var j = 0, len = hoge.length; j < len; j++) {
            // 複雑な処理
        }
    }
}
```

* グローバル変数を特別な理由が無い限り，使用しないでください．Google Analyticsなどで使用する場合はコメントを残してください．

```javascript
var foo = function() {
    var i, hoge = ''; // 変数がローカルなので，OK
    for (i = 0; i < 20 i++) {
        hoge += i;
    }
}
foo();

var i, hoge = ''; // 変数がグローバルなので，NG
var foo = function() {
    for (i = 0; i < 20 i++) {
        hoge += i;
    }
}
foo();
```
* 各画面で引き回す値などがある場合は必ず *名前空間* を区切って，その中に値を格納するように心がけてください．

```javascript
// 名前空間(この場合はnamespace)オブジェクトの下にさらにオブジェクトを作る
var window.namespace.hoge = {  // ok
    message: 'hello'
};

// グローバル変数として，オブジェクトを作ると他の同名の変数で上書きされたりする
var hoge = {  // ng
        message: 'hello'
};
```

* 暗黙の型変換をしないようにしてください．具体的には等価演算子を使用するようにしてください．
* 暗黙の型変換するとパフォーマンス的にも遅いです．
    * 速度の違いはこちらの[JSPerf](http://jsperf.com/super-duper-equality-checks)を参考にしてください

```javascript
var a = 0;  // 変数aはNumber
if (a === '') {
    console.log('OK'); // このlogは表示されないので，OK
}

var b = 0;  // 変数bはNumber
if (b == '') { // ここで暗黙の型変換が起こり，trueで評価される
    console.log('NG'); // このlogは表示されてしまい，NG
}
```

* [こちら](http://jsperf.com/cast-int/3)で確認できますが，以前は`parseInt()`が遅かったのですが，
現在のWebKitの実装では，むしろ`>> 0`の方が遅いです．
* <del>整数が必要なので型変換するという場合は速度的には `parseInt()` は一番遅いです．</del>
* 複雑な計算以外では以下のようにすると速度は出ます．(適材適所です)

```javascript
var hoge = '05';

parseInt(hoge, 10); // WebKit系はこれが一番速い
parseInt(hoge); // WebKit系は次点で速いですが，実装によっては8進数に変換される可能性あり．
hoge >> 0; // Firefoxではこれが一番速い
Math.floor(hoge); // ここからは他の数値変換の方法
Number(hoge);
hoge * 1;
~~hoge;
hoge | 0;
hoge + 0; // NG．元がStringなので結果は"050"になってしまいます…
```

* `for-in` 文をむやみに使用しないでください．大概が `for` 文で事足ります．オブジェクトの走査などで使用する場合は必ず `Object.hasOwnProperty()` を併用してください．

> `for-in` 文をオブジェクトの走査に使用すると，大元のObjectまで走査しにいきます．なので， `Object.hasOwnProperty` 無しだと無駄なコストがかかってしまいます．

> `for-in` + `Object.hasOwnProperty` の代わりに， `Object.keys(obj)` を使用する事もできます．使えるならこちらの方がパフォーマンスは良いはず．(関数呼び出しのコストが無くなる)

* `for-in` 文を配列に対して使う事は特に厳禁です．パフォーマンスがむちゃくちゃ落ちます．

```javascript
var array = ['foo', 'bar', 'baz'],
    sum = 0;
for (var i = 0, len = array.length; i < len; i++) { // 配列の場合はfor文でループできる．
    sum += array[i];
}

var array = ['foo', 'bar', 'baz'],
    sum = 0;
for (i in array) {  // この場合にfor-in文を使用すると非常に遅くなる．
    sum += array[i];
}

var object = {
foo: 'hoge', 
     bar: 'huga',
     baz: 'hogehoge'
},
    result = [];

for (name in object) { // オブジェクトの走査の場合はfor-inを使用してもOK
    if (object.hasOwnProperty(name)) { // その場合Object.hasOwnPropertyで判定して，必要以上にチェーンを遡らないようにする．
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

* `for` 文の条件内で， `length` を使用する場合は1回変数に格納してください．変数にキャッシュする事により，ループの速度が速くなります．

```javascript
var array = ['foo', 'bar', 'baz'],
    html = '';

for(var i = 0, len = array.length; i < len; i++) {  // array.lengthを変数lenに格納する事により，ループ回数に関わらず1回参照するだけ．
    html += array[i];
}

var array = ['foo', 'bar', 'baz'],
    html = '';

for(var i = 0; i < array.length; i++) {  // array.lengthを変数に入れないと，ループ毎にarray.lengthを参照してしまう．
    html += array[i];
}
```

* オブジェクト・配列・正規表現・関数・文字列・数値などは，特別に理由が無い限り，リテラルで生成してください．コンストラクタ呼び出しは速度が遅くなります．

```javascript
var object = {},
    array = [],
    regex = /hoge.*/g,
    func = function() {},
    string = '';  // 特別理由が無ければ，リテラルで生成する．

var object = new Object(),
    array = new Array(),
    regex = new RegExp('hoge.*', g),
    func = new Function(),
    string = new String();  // コンストラクタ呼び出しは，基本しない
```

なぜ，使用を禁止されるものがあるかなどの詳しい理由は，ここでは字数が足りませんので，説明しません．参考図書として[JavaScript: The Good Parts](http://www.oreilly.co.jp/books/9784873113913/)をお勧めします．また，Webサイトでは[Efficient JavaScript](http://dev.opera.com/articles/view/efficient-javascript/)，[Efficient JavaScript 日本語訳](http://www.hyuki.com/yukiwiki/wiki.cgi?EfficientJavaScript)，[JavaScript Garden](http://bonsaiden.github.com/JavaScript-Garden/ja/)が参考になります．

## <a name="jshint">JSHint</a>

[JSLint](http://www.jslint.com/)や，[JSHint](http://www.jshint.com/)といったツールを使用する事によって，コードの品質チェックが可能になります．
ここまで上げた注意点も上記のツールを使用する事により大部分で警告が出るようになるので，大変便利です．

`JSLint` は厳しい判定が出る場合が多い為に `JSHint` を使用していく事を推奨します．
導入例は下記にいくつかの例を載せておきますが，一番効果的なのは使用してるエディタやIDEでの自動チェックになります．

また，おすすめの設定はこの章の最後の方で紹介します．

### <a name="webサービス">Webサービス</a>

[JSHint](http://www.jshint.com/)

ここにチェックしたいコードを貼りつけて，下部のチェックボックスからチェックしたいものにチェックを入れ， **Lint** ボタンを押すだけです．

### <a name="nodejsのnpmを使用して，jshintをインストールする">Node.jsのnpmを使用して，JSHintをインストールする</a>

ここからのエディタにはコマンドラインで， `JSHint` が必要になります．
必要なものは下記になります．

- Homebrew(Mac OS Xのパッケージ管理システム)
- node
- npm

`Homebrew`でインストールする場合は下記のようになります．

```shell
$ brew install node
$ npm install -g jshint
```

また，WindowsやMac OS Xでもインストーラが[配布](http://nodejs.org/download/)されているので，  
こちらからインストールする方法も取れますが，Macであれば前述の`Homebrew`のインストールがオススメです．

詳しい導入方法は，[こちら](http://blog.craftgear.net/50832ff38cdc8fb415000001/title)を参考に導入してください．

### <a name="webstorm-phpstorm">WebStorm & PHPStorm</a>

標準で，使用できます． `Settings` を開き `JSHint` で検索して `enable` のチェックを入れるだけです．

参考URL(英文)

* [Web Coding Made Smarter](http://www.jetbrains.com/webstorm/features/index.html#JSLintJSHint)
* [How to lint your JavaScript with JSLint in real time](http://blog.jetbrains.com/webide/2011/12/how-to-lint-your-javascript-with-jslint-in-real-time/)

### <a name="eclipse">Eclipse</a>

[jshint-eclipse](http://github.eclipsesource.com/jshint-eclipse/)プラグインを導入してください．

### <a name="sublime-text2">Sublime Text2</a>

[SublimeLinter](https://github.com/SublimeLinter/SublimeLinter)がおすすめです．


### <a name="vim">Vim</a>

[Syntastic](https://github.com/scrooloose/syntastic)がおすすめです．

### <a name="emacs">Emacs</a>

Flymakeでの使用か[jshint-mode](https://github.com/daleharvey/jshint-mode)がおすすめです．

### <a name="その他">その他</a>

公式サイトに他のエディタで使用する場合の[リンク](http://www.jshint.com/platforms/)があります．

### <a name="jshintの設定について">JSHintの設定について</a>

`JSHint` ではオプションの設定により，警告の有無を選ぶ事ができます． `npm` でインストールする場合は， `.jshintrc` ファイルをホームディレクトリや
プロジェクトに作る事により，設定が反映されます．(プロジェクトの場合は `--config /path/to/.jshintrc` オプションを渡す必要あり)
`.jshint` 自体はJSON形式で記述します．

[こちら](http://www.jshint.com/docs/)が使用できるオプションになりますが，日本語での[説明](http://blog.craftgear.net/50832ff38cdc8fb415000001/title)も
あります．

`.jshintrc` 自体の書き方は[こちら](https://github.com/jshint/node-jshint/blob/master/.jshintrc)を参考にしてください．

`.jshintrc` の[サンプル](http://ghe.amb.ca.local/hiraki-satoru/js-coding-guideline/blob/master/misc/.jshintrc)．一応，上記のガイドラインに合わせるようにしました．他はプロジェクト毎に変更していく・ファイル毎にオプションを書いていくなどしてください．

#### <a name="jshint-20での変更点">JSHint 2.0での変更点</a>

**2013-05-17**

先日，JSHintが2.0にバージョンアップしまして，そちらの[変更点](http://www.jshint.com/blog/2013-05-07/2-0-0/)が発表されています．
主な変更は以下になります．

* `es5`オプションがデフォルトになり，代わりに古いブラウザ用には`es3: true`を設定するようになった
* *ES6(ES.next)* とMozilla Firefox限定の機能が部分的にサポートされている
    * Destructuring assignment
    * const
    * let blocks and expressions
    * Generators and iterators
    * List comprehension
    * Try/catch filters and multiple catch blocks
    * Concise method declaration
    * for ... of loops
    * Fat arrows
* CUIで使用する場合にディレクトリ内に`.jshintrc`ファイルがあるとそちらの設定を優先で読みに行くようになった
* Node.jsのシステムでない場合にも[Browserify](http://browserify.org/)を使用するようになり，Rhinoでのパフォーマンスの問題が解決
* SVGがグローバルオプションに登録できる
* `smarttabs`オプションの挙動が変更になり，コメントの中のタブ・スペース混在は無視するようになる．
* 推奨されないJSLintのオプションが無効に

あとは，このバージョンからバージョンアップのスピードアップがされるそうです．

## <a name="javascriptフレームワーク・ライブラリ">JavaScriptフレームワーク・ライブラリ</a>

使用するフレームワークについては，各プロジェクトでチームビルドにより違うので一概に「これが良い」というものはありません．  
しかし，使用を推奨するフレームワーク・ライブラリは以下のようなものになるかと思います．

### <a name="dom操作">DOM操作</a>

* [jQuery](http://jquery.com/)
    * 2.0で使用するのが良いです．
* [zepto.js](http://zeptojs.com/)
    * jQueryよりもイニシャルの転送量は少なくて済みます．が，セレクタの使い方を間違えるとDOM操作が遅くなるので注意．

### <a name="asynccontrol-flow">Async/Control Flow</a>

* [Q](http://documentup.com/kriskowal/q/)
* [When.js](https://github.com/cujojs/when)

これらはこちらの[リポジトリ](http://ghe.amb.ca.local/am-game-coredev/async-controlFlow-compare)で検証していますが，
jQuery代替として使用するなら上の2つがオススメです．

### <a name="注意点">注意点</a>

またテンプレートエンジンとして[Handlebars.js](http://handlebarsjs.com/)や[jsrender](http://borismoore.github.io/jsrender/demos/index.html)などが
使用されています．(詳しい比較検討はしていません)

プロジェクトによっては，もちろん他の選択もあると思いますが，どのライブラリもファイル容量やイニシャルのコストは絶対に増えるため  
導入に際しては以下のような視点で選択していただきたいと思います．

* そもそも本当に導入が必要なのか
* 導入によるメリット・デメリット
* ライブラリ(フレームワーク)自体のコードがきちんとしているか
* 導入コストが低いか
* ライブラリ(フレームワーク)が活発に更新されているか
* ドキュメントなどが充実しているか

など複数の視点で選択してください．もし迷った場合は，相談いただいても問題ありません．

## <a name="クライアントサイドjavascript高速化の要点">クライアントサイドJavaScript高速化の要点</a>

ここまで，色々と書いてきましたが，以上を踏まえてのクライアントの描画の要点をまとめてみます．

### <a name="htmlのレンダリングをブロッキングしないようにscriptタグは下部に置く">HTMLのレンダリングをブロッキングしないようにscriptタグは下部に置く</a>

`<head>` タグに `<script>` を書くと，どうしてもブラウザのレンダリングを阻害するので(白くなったりとかはこれのせいです)
`</body>` の直前に書くのが良いです．大体YSlowとかGoogle Speedもこれは見てます． **DOMContentLoaded** のタイミングでJavaScriptの実行がされて，
なおかつ，HTMLの中に *onclick* などがインラインで書かれてたりしなければ，ほぼ不具合は無いはずです．jQueryは `$(function {});` でイベントハンドリング
すれば勝手に **DOMContentLoaded** のタイミングで実行してくれます．

### <a name="documentwriteをhtmlに書かない">document.write()をHTMLに書かない</a>

今どき `document.write()` はねーよww とか思いますが，案外入れらる事があったりも．気を付けようねレベルですが，これが入るとその部分でレンダリングが
絶対止まります．そこで他に `<script>` タグとか呼んでたら，事によるとその実行まで入ったりします．ダメ絶対．

### <a name="ブラウザの再描画・再フローをなるべくしない">ブラウザの再描画・再フローをなるべくしない</a>

再描画はDOM要素の色や見た目，再フローは位置・サイズ変更・追加などでブラウザがレンダリングしなおすことです．
ブラウザの動作の中で非常にコストが高い上，回線速度によっては真っ白になったりするなどユーザーの体感速度にもかなり影響してきます．
速度的には **再フロー &gt; 再描画** という順番になります．

とは言え，JavaScriptでは上記の操作は避けて通れないのですが，以下のような施策で再描画の回数を減らすようにすると良いです．

* 複数のDOM要素の追加などは，1つずつ追加するのではなく一気に追加する

> 例えば `ul` 内に `li` を追加していく場合だとかが簡単な例なのですが，以下のコードだと再描画が `li` 追加毎に走るので体感速度が遅いです

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

> 余談ですが，最近のブラウザであれば，文字列を連結する方が `Array.push()` と `Array.join()` よりも高速なものが多いです．(古いIEを除く)

* 再描画の範囲はなるべく小さくする

> 子要素で再フローが起きると結局の所，親の要素に次々と影響します．ベストなものは `position: absolute` などして，ブラウザの描画フローから切り離していくと最小限の影響になります．

* 既存のDOM要素の再描画・再フローが避けられない場合は `display:none` を設定してCSSなどを変更してから `display:block` にして表示させる．

> 特に複数のプロパティを設定する場合などはその度に再描画・再フローを実行しようとするので，影響が最小限になるように表示させないようにすると効果的です．

* 裏技として， `document.createDocumentFragment()` を使う方法があります． 

> DOMツリーには挿入されるけど，描画の範囲外というのが `documentFragment` なのでここに `appendChild()` するという手です．[http://ejohn.org/apps/workshop/adv-talk/#6](http://ejohn.org/apps/workshop/adv-talk/#6)←これが分かりやすいかも 

### <a name="ずっと続くタイマーにsetintervalを使用しない">ずっと続くタイマーにsetInterval()を使用しない</a>

代わりに， `setTimeout()` を再帰で呼び出した方が良いです．理由としてはブラウザはシングルスレッドなので一度に1つの事しか実行できません．が， `setInterval()` は
呼び出されると延々と将来のスレッドにも呼び出された処理を(止めるまで)挟み込み続けるからです．

これは例えばアニメーションがガクガクするなどの原因になります．既に `setInterval()` が予約されてるスレッドをアニメに挟みこんでしまうからです．

`setTimeout()` を再帰で呼び出すと将来に渡って延々では無く次の処理を予約くらいで済む為に，アニメーションの処理なんかがスレッドにちゃんと入りますので，
そこまではガクつかないです．(処理にもよりますけど)

### <a name="consolelogをソースに残さない">`console.log`をソースに残さない</a>

今までのコードサンプルでこれでもかと使っている`console.log`ですが，本番環境では特に消しておくようにしてください．
ログに吐くオブジェクトの内容によってはメモリーリークの原因になりかねないです．色々手段はあると思うので，頑張って消していきましょう．

参考：http://www.ibm.com/developerworks/jp/web/library/wa-jsmemory/#N101BF

### <a name="javascriptをなるべく使わない">JavaScriptをなるべく使わない</a>

どっちらけな感じですが，これが一番速いです．これだけだと身も蓋も無いのですが，要はHTML・CSSで出来る事はJavaScriptでやらないという事を意識すると良いかと．

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

## <a name="jqueryを使用したクライアントサイドjavascript特有の注意点">jQueryを使用したクライアントサイドJavaScript特有の注意点</a>

* ライブラリを使用する場合は出来る限り最新のものを使用する．バージョンが上がるとフレームワーク自体のパフォーマンスが向上している事が多いです．

> ただし，これは新規で作る場合．公開されているものは安易にバージョンを上げると後方互換が取れずに不具合が出る可能性があるので，慎重にして下さい．

* DOMの取得が速い順番で `$('#id')` >= `$('element')` > `$('.class')` > `$('[attribute=value]')` >= `$(':pseudo')` となる．

> DOMの取得は疑似セレクタが凄く遅いです．なぜかというと，疑似セレクタに関してはjQueryは一回Sizzleエンジン(CSSセレクタエンジン)を通して処理するからです．
> DOMの取得の速度順というのは，Pure JavaScriptでメソッドが用意されているもの→Sizzle使うものという順番です．
> ただ上記は一般的に…というもので，ブラウザの実装状況には左右されます． `document.querySelectorAll()` が実装されてるかどうかなど．

* jQuery オブジェクトを取得する場合には出来る限り，idをHTMLに指定してください．
* 取得したい要素にidが振れない場合には，その親要素などにidを振って下さい．
* どうしてもid以外の指定をしなくてはいけないという場合もあるかと思いますが，その場合は出来る限りの直近の親などから目的の要素を見つけるようにすると良いです．
その際に，なるべくjQueryセレクタの中で，疑似要素なんかを使わない方が高速になります．

> 擬似セレクタに加えて，jQueryのカスタムセレクタを使用しないでください．

* `querySelectorAll()`で解釈できる疑似セレクタの他，Sizzleエンジンには独自の疑似セレクタが存在します．
* これらは前述の通りの低速であることに加えて，[w3c](http://www.w3.org/TR/css3-selectors/)で提唱されてるわけではないため，使わないほうがいいです．

`:animated`, `[name!="value"]` `:button`, `:checkbox`, `:contains()`, `:eq()`, `:even`,  
`:file`, `:first`, `:gt()`, `:has()`, `:header`, `:hidden`, `:image`, `:input`, `:last`, `:lt()`,  
`:odd`, `:parent`, `:password`, `:radio`, `:reset`, `:selected`, `:submit`, `:text`, `:visible`  

> *ex* `$('#hoge').find('li').eq(3).text();` の方が `$('#hoge li:eq(3)').text();` よりも高速なはずです．

* イベントハンドラを付ける場合は基本的に該当部分の要素に付けていきますが，例えば `ul` 内の複数の `li` 要素それぞれをイベントハンドリングする場合は効率が悪いので， `delegate()` メソッドを使ってイベントのバブリングを `ul` 要素でキャッチする方が効率良いです．
* 基本として，HTML/CSSでまかなえる事をJavaScriptでは行わないようにしてください．

> 例えば，css()で色々要素の装飾をするよりは，id/classを振って，cssで設定した方がパフォーマンスが良いです．

* HTMLにid/classを付ける場合はid→jsi，class→jscのプレフィックスを入れる．こうする事によりデザイン部分に必要なid/classか，JavaScriptでのターゲッティングに必要なだけのid/classなのかを区別できるので，後々のリファクタリングがやり易くなります．(現状はそこまでJavaScriptだけでしか使わないもの無いので無視で良いかも…)

```html
<div id="listWrapper" class="listDivider"></div> // NG

<div id="jsiListWrapper" class="jscListDivider"></div> // OK
```

## <a name="1ソースアプリ特有の注意点">1ソースアプリ特有の注意点</a>

`Backbone.js`などを使用してのアプリケーション開発では気をつけないと容易にメモリリークを起こしてしまいます．

特に画面が遷移した後に，以前のイベントを取り除かないなど

### <a name="参照url">参照URL</a>

メモリリークについては[こちら](http://www.ibm.com/developerworks/jp/web/library/wa-jsmemory/?cmp=dw&cpb=dwwdv&ct=dwrss&cr=dwrss&ccy=jp&csr=120712)が分かりやすい解説です．
Backbone.jsに焦点を合わせたメモリーリーク対策は下記の記事が参考になると思います．

- [Avoiding memory leaks in backbone.js](http://unspace.ca/blog/avoiding-memory-leaks-in-backbone/)
- [Avoiding Common Backbone.js Pitfalls](http://ozkatz.github.io/avoiding-common-backbonejs-pitfalls.html)
- [Backbone.js And JavaScript Garbage Collection](http://lostechies.com/derickbailey/2012/03/19/backbone-js-and-javascript-garbage-collection/)
    - 若干古いです

Gmailでのメモリリークの対処を書いた[こちら](http://www.html5rocks.com/en/tutorials/memory/effectivemanagement/)の記事は，Devツールを使っての原因特定の  
仕方などもあり大変に参考になります．

### <a name="具体例">具体例</a>

- イベントハンドラを削除
- 参照がないDOMの削除
- 循環参照をなくしていく
- 必要がなくなった変数の初期化

`View.remove()`や`View.stopListening()`，`Event.off()`，`Event.unbind()`，`Event.stopListening()`などを忘れずに活用していきましょう．

