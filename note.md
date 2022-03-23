# Note Advanced css and sass

## 目次

-   [basic-tips](#basic-tips)
-   [CSS BEHIND THE SCENES](#CSS BEHIND THE SCENES)

## basic-tips

## `box-sizing`

> box-sizing は CSS のプロパティで、要素の全体の幅と高さをどのように計算するのかを設定します。

CSS ボックスモデルの既定によると、

要素に割り当てられた`width`, `height`は要素のコンテンツ領域のみに適用される

もし要素に`border`, `padding`がある場合、

画面に表示される矩形の大きさは、

`width` + `padding` + `border`であり、

矩形の幅を`width: 300px`で指定しようとする場合、

`padding: 10px`, `border: 2px`だったとき

最終的に矩形の全体の幅は 324px になる

つまり、

矩形に` width``height `を指定する場合、`padding`, `border`などを
勘定に入れてその値を決めなくてはならない

これは面倒ですね

> box-sizing プロパティは上記の振る舞いを調整するために使用できます。

`border-box`:

> border-box は、境界およびパディングを、要素に指定した width および height の中で取るようブラウザーに指示します。要素の width を 100 ピクセルに設定した場合、100 ピクセルの中に追加した境界やパディングが含まれ、コンテンツ領域はそれらの幅を吸収するために縮小されます。

## `font-family`

```html
<link
    href="https://fonts.googleapis.com/css?family=Lato:100,300,400,700,900"
    rel="stylesheet"
/>
```

Google フォントから上記のように取得できるなら

`font-family`は`Lato`
`font-weight`は 100,300,400,700,900 から選べる

という意味

## `line-height`

> line-height は CSS のプロパティで、**行ボックスの高さを設定します。**
> これは主にテキストの行間を設定するために使用します。
> ブロックレベル要素では、要素に含まれる行ボックスの最小の高さを指定します。非置換インライン要素では、行ボックスの高さの計算に使われる高さを指定します。

例

```css
/* 単位のない値: この値を要素のフォントサイズに
掛けたものを使用する */
line-height: 3.5;
```

## `background-cover`, `background-position`

画像を背景いっぱいに配置したいときとか

```CSS
.header {
    height: 95vh;
    background-image: url(../img/hero.jpg);
    background-size: cover;
    /* ブラウザのウィンドウをリサイズしたときにどこを基準に拡縮するのか指定する */
    background-position: top;
}

```

グラデーションを、さきほどの背景画像の上に施す

```CSS
{

 background-image: linear-gradient(to right bottom, #7ed56f, #28b485), url(../img/hero.jpg);
}
```

## `clip-path: polygon()`で shape を生成する

４つの頂点の座標を指定することで
このプロパティを指定している要素の矩形の頂点座標を操作できる

順番が決まっていて、

左上、右上、右下、左下

```CSS

.header {
    height: 95vh;
    background-image: linear-gradient(to right bottom, #7ed56f, #28b485), url(../img/hero.jpg);
    background-size: cover;
    /* ブラウザのウィンドウをリサイズしたときにどこを基準に拡縮するのか指定する */
    background-position: top;

    /* shapeを変形させるﾌﾟﾛﾊﾟﾃｨ */
    /* ４頂点：左上、右上、右下、左下 */
    clip-path: polygon(0 0, 100% 0, 100% 200px, 100% 100%);
}
```

## `letter-spacing`で文字の字間を操作する

> letter-spacing は CSS のプロパティで、テキストの水平方向の字間のスペースに関する挙動を設定します。
> この値はテキストを描画する際に文字間の自然な空間に追加されます。
> letter-spacing が正の値であった場合は、文字と文字の間が開き、 letter-spacing が負の値であった場合は、文字と文字が互いに近づきます。

## コンテンツをコンテナの中のど真ん中に配置する方法

`translate()`を使う

> translate() は CSS の関数で、要素を水平方向や垂直方向で再配置します。結果は <transform-function> データ型になります。

コンテンツの要素を`position: absolute`して
`top`, `left`を 50％にしても
コンテンツの左上の頂点が基準になるので
コンテンツはど真ん中にやってこない

これをうまくやる方法

```CSS
/* before */
.text-box {
    position: absolute;
    top: 50%;
    left: 50%;
    background-color: red;
}

/* after */
.text-box {
    position: absolute;
    top: 50%;
    left: 50%;
    background-color: red;

    transform: translate(-50%, -50%);
}

```

つまり、top, left の位置からそれぞれ 50％「後退」するのである

こうすればどまんなかンい配置できる

## アニメーション：fade in from aside.

アニメーションさせるプロパティ：

opacity 0 -> 1
translateX -100px -> 0

```CSS
@keyframes moveInLeft {
    0% {
        opacity: 0;
        transform: translateX(-100px);
    }

    80% {

    }

    100% {
        opacity: 1;
        transform: translate(0);
    }
}

.header-primary {
    color: #fff;
    text-transform: uppercase;
    /* 必須 */
    animation-name: moveInLeft;
    /* アニメーションに書ける時間 */
    animation-duration: 1s;
    /* アニメーションが起こるまでの時間 */
    animation-delay: 3s;
}

```

アニメーションを適用するには、
アニメーションを適用したい要素のプロパティに
`animation-name`を付ける（任意の名前）
`@keyframes`に`animation-name`を渡し、
その中でｱﾆﾒｰｼｮﾝさせたいプロパティを定義する
（上記の通り）

アニメーションがなぜだか震えたりするときは、

`backface-vibibility`

> backface-visibility は CSS のプロパティで、要素がユーザーに対して裏側を向いたときに、裏面を可視にするかどうかを設定します。

## anchor タグを操作する

デフォを制御：

-   リンクが踏まれる前は青色であること
-   リンクが踏まれたら紫色になること

```CSS

.btn:link,
 .btn:visited{
    text-decoration: none;
    padding: 15px 40px;
    border-radius: 100px;
    display: inline-block;
}

.btn-white {
    color: #777;
}
```

`display: inline-block`:

> この要素はブロック要素ボックスを生成しますが、周囲のコンテンツに対しては単一のインラインボックスであるかのように流れるようになります (置換要素の場合と似ています)。

## リッチなボタンホバー&クリック体験

```CSS

.btn:link,
 .btn:visited{
    text-transform: uppercase;
    text-decoration: none;
    padding: 15px 40px;
    border-radius: 100px;
    display: inline-block;
    transition: all .2s;
}

.btn:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 20px rgba(0,0,0,.2);
}

.btn:active {
    transform: translateY(-1px);
    box-shadow: 0 5px 10px rgba(0,0,0,.2);
}

.btn-white {
    background-color: #fff;
    color: #777;
}
```

## `::after`疑似要素

> CSS において ::after は、選択した要素の最後の子要素として擬似要素を生成します。よく content プロパティを使用して、要素に装飾的な内容を追加するために用いられます。この要素は既定でインラインです。

ということで、つまり CSS で指定できる（生成できる）要素である

## さらにリッチなボタンホバー&クリック体験

```CSS

.btn:link,
 .btn:visited{
    text-transform: uppercase;
    text-decoration: none;
    padding: 15px 40px;
    border-radius: 100px;
    display: inline-block;
    transition: all .2s;
}

.btn:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 20px rgba(0,0,0,.2);
}

.btn:active {
    transform: translateY(-1px);
    box-shadow: 0 5px 10px rgba(0,0,0,.2);
}

.btn-white {
    background-color: #fff;
    color: #777;
}

.bnt::after {
    content: "";
    display: inline-block;
    height: 100%;
    width: 100%;
    border-radius: 100px;
    position: absolute;
    top: 0;
    left: 0;
    z-index: -1;
    transition: all .4s;
}

.btn-white::after {
    background-color: #fff;
}

.btn:hover::after {
    transform: scale(1.5);
    opacity: 0;
}
```

## CSS BEHIND THE SCENES

## web サイトを構築する際の 3 つの重要な柱

-   レスポンシブデザイン
-   メンテナブル/スケーラブル
-   web パフォーマンス

#### レスポンシブデザイン

-   fluid layout
-   Media queries
-   responsive images
-   correct utils
-   Desktop-first vs modile-first

#### メンテナブル/スケーラブル

-   クリーンで再利用性が高いとか命名方法とか普通のこと

#### web パフォーマンス

より少ないコード
より少ない image
コード、image 圧縮

## WHEN WE LOADED UP A WEBPAGE

Load HTML
Parse HTML: HTML ファイルを 1 行ずつデコードする
この際ブラウザは DOM ツリーを生成する
HTML ファイルは最終的にこの DOM ツリーで取得される
stylesheets があった場合これらをロードする
Load CSS
Parse CSS: 2 つの主なプロセスがある 1. cascade(CSS のあらゆる宣言を解決する) 2. 最終的な CSS の値を決定する
CSSObjectModel というものに変換される

Render tree: 各 DOM を描写する

## Parse CSS: Cascade

参考：

https://developer.mozilla.org/ja/docs/Web/CSS/Cascade

> ある要素に複数のルールが適用される場合に、異なるスタイルシートを組み合わせ、異なる CSS ルールや宣言の間の衝突を解決する処理。

複数の stylesheet や複数の CSS ルールが同じ要素に適用されるよう定義されていたとして
それらの組み合わせから優先度を解決する処理のこと

stylesheet にはいくつか種類があり、

-   ユーザエージェントスタイルシート
    ブラウザの既存のスタイルを提供するスタイルシート

-   作成者スタイルシート
    特定の web ページのスタイルを定義するスタイルシートである

-   ユーザスタイルシート
    web サイトを利用するユーザが使い勝手などを調整できるスタイルシート

cascade 順 (定義の適用優先度):

IMPORTANCE > SPECIFICTY > SOURCE ORDER

#### IMPORTANCE の優先度順位：

1. ユーザ `important` 宣言
2. 作成者 `important` 宣言
3. 作成者宣言
4. ユーザ宣言
5. ブラウザのデフォルトの宣言

上記の IMPORTANCE が同党の場合は、次に詳細度の比較でもって優先度を決める

#### SPECIFICITY の優先度順位：

参考

https://developer.mozilla.org/ja/docs/Web/CSS/Specificity

> 詳細度は CSS 宣言が適用される際の重みであり、一致するセレクターそれぞれの種類の数によって特定されます。
> 複数の宣言が同じ詳細度であれば、 CSS の中で最後に宣言されたものが要素に適用されます。
> 詳細度は、同じ要素に対して複数の宣言が行われている場合のみ適用されます。 CSS のルールでは、直接対象となった要素は要素が祖先から継承したルールよりも常に優先されます。

ざっくりいうと、どの要素なのかより詳しく、より特定しやすい情報をふかしている CSS 定義が優先される

詳細度優先順:

ID > class > element & pseudo

`!important`の例外：

**この宣言は悪い習慣です。**

`!important`で宣言された CSS 宣言は複数の宣言が競合するときに最も優先度が高く扱われる

また、

> 同じ要素に二つの競合する宣言が !important ルール付きで適用された場合、より高い詳細度の宣言が適用されます。

これまた詳細度が同等だったら？が次

#### SOURCE ORDER

コードの最後の方の宣言は他のすべての宣言をオーバーライドして適用される

#### cascade で覚えておくこと

-   `!important`はもっとも優先度が高く扱われる
-   しかし`!important`はどうしても競合が解決しないときに最後の手段として用いるべき
    バグの温床になりがち

-   HTML 内で作成されるインラインスタイルは外部スタイルよりも常に優先される
    インラインスタイルは悪い習慣です

-   詳細度をあげるには id か class で CSS 宣言しましょう
-   全称セレクタは詳細度を持たない（優先度がない）
-   セレクタの順番よりも詳細度を信頼しましょう
-   サードパーティのスタイルシートを利用するときは、自身のスタイルシートを一番最後に追加しましょう
    この辺は順番の話で、SOURCEORDER のところで書いた通り

## [Might Conflict] Good Practice: 常にブラウザのデフォルト値を継承などするために

1. rem を使ってあらゆる部分を root font-size と相対的にする

こんな風にしておけば、
ユーザがウェブページのフォントサイズを変更できたりして
拡張性が広がりますね

ブラウザのスタイルシート（ブラウザのデフォルト CSS）のフォントサイズに対して
ルートのフォントサイズを固定値にするとどのブラウザでも固定のサイズになってしまう

これは悪習慣らしい
デフォのフォントサイズに対してルートのフォントサイズを比率にしておくと
ブラウザに応じて適切にリサイズしてくれる

```CSS
/* before */
html {
    /* 一般的なデフォのフォントサイズ */
    font-size: 16px;
}

/* after */
html {
    /* ブラウザのデフォのフォントサイズに常に合わせられる */
    font-size: 100%;
    /*
    また、アプリケーション内部で基準のサイズを常に10pxにしておきたいならば
    specified 10px / default 16px  = 0.625
     */
    font-size: 62.5%
}


```

2. 同様に、ブラウザのデフォルトの仕様を常に適用するために
   inherit をつかって継承させる場合

例：box-sizing

```CSS
/* before */

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Lato', sans-serif;
    font-weight: 400;
    font-size: 16px;
    line-height: 1.7;
    color: #777;
    padding: 30px;
}

/* after */

* {
    margin: 0;
    padding: 0;
    /* ブラウザのデフォルトを常に継承する */
    box-sizing: inherit;
}

body {
    font-family: 'Lato', sans-serif;
    font-weight: 400;
    font-size: 16px;
    line-height: 1.7;
    color: #777;
    padding: 30px;
    /* アプリケーション内で利用したいbox-sizing */
    box-sizing: border-box;
}
```

3. 疑似要素への全称セレクタの適用もできる

```CSS
/* before */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

/* after */
*,
*::after,
*::before,
*:hover {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

```

#### [Might Conflict] BOX MODEL

1. box-sizing のデフォルト`content-box`

width = right border + right padding + specified width + left padding + left border;

height = 同様

2. `border-box`

width = specified width;

つまり、border, padding が width 指定値に収まるようにコンテンツ領域が縮小される

3. box types

-   `display: block`

要素の見た目はブロックのようにフォーマットされる
100% of parents width
垂直方向に並べられる

-   `display: inline`

コンテンツは水平方向に並べられる
コンテンツ領域だけを占領する
改行を起こさない
高さと幅を指定できない
水平方向の padding, margin しか指定できない

-   `display: inline-block`

block と inline のミックスイン

周囲のコンテンツに対して：inline 要素のようにふるまう
コンテンツ領域のみ占領する
改行を起こさない

内部的にはブロックレベルのボックスとして振る舞う

#### [Might Conflict] POSITIONING SCHEME

1. Normal flow

デフォルトの振る舞い。

`position: relative`でもある

2. Floats

要素は通常のフローから外れるが、フローの一部であり続ける

周囲の要素のレイアウトに影響を与える

> 要素を包含ブロックの左右どちらかの側に沿うように設置し、テキストやインライン要素がその周りを回りこめるように定義します。

3. Absolute Positioning

要素は通常のフローから外れるが、フローの一部であり続ける

**周囲の要素のレイアウトに影響を与えない**

#### [Might Conflict] stacking context

主に`z-index`で指定することで生成されるコンテキスト

コンテキストはスタックを形成するレイヤーのようなもの
