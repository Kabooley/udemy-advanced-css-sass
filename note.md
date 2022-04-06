# Note Advanced css and sass

## 目次

- [basic-tips](#basic-tips)
- [SASS-basic](#SASS-basic)
- [CSS BEHIND THE SCENES](#CSS BEHIND THE SCENES)
- [EMMET-Tips](#EMMET-Tips)
- [お役立ち](#お役立ち)

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

- リンクが踏まれる前は青色であること
- リンクが踏まれたら紫色になること

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

## SASS-basic

#### ネスト

たとえば次のような class 名を以下のようにネストで固まらせることができる

`.composition`
`composition__photo`
`composition__photo--p1`
という、
`composition`で始まる class 名を同じネスト内で定義するならば
`composition`の部分は`&`で省略できる

```html

<div class="composition">
    <img src="img/nat-1.jpg" alt="photo 1" class="composition__photo composition__photo--p1">
    <img src="img/nat-2.jpg" alt="photo 2" class="composition__photo composition__photo--p2">
    <img src="img/nat-3.jpg" alt="photo 3" class="composition__photo composition__photo--p3"></div>
</div>
```

```SCSS
.composition {
    position: relative;

    &__photo {
        width: 55%;
        box-shadow: 0 1.5rem 4rem rgba($color-black, .4);
        border-radius: 2px;
        position: absolute;

        &--p1 {
            left: 0;
            top: -2rem;
        }

        &--p2 {
            right: 0;
            top: 2rem;
        }

        &--p3 {
            left: 20%;
            top: 10rem;
        }
    }
}
```

## CSS BEHIND THE SCENES

## web サイトを構築する際の 3 つの重要な柱

- レスポンシブデザイン
- メンテナブル/スケーラブル
- web パフォーマンス

#### レスポンシブデザイン

- fluid layout
- Media queries
- responsive images
- correct utils
- Desktop-first vs modile-first

#### メンテナブル/スケーラブル

- クリーンで再利用性が高いとか命名方法とか普通のこと

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

- ユーザエージェントスタイルシート
  ブラウザの既存のスタイルを提供するスタイルシート

- 作成者スタイルシート
  特定の web ページのスタイルを定義するスタイルシートである

- ユーザスタイルシート
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

- `!important`はもっとも優先度が高く扱われる
- しかし`!important`はどうしても競合が解決しないときに最後の手段として用いるべき
  バグの温床になりがち

- HTML 内で作成されるインラインスタイルは外部スタイルよりも常に優先される
  インラインスタイルは悪い習慣です

- 詳細度をあげるには id か class で CSS 宣言しましょう
- 全称セレクタは詳細度を持たない（優先度がない）
- セレクタの順番よりも詳細度を信頼しましょう
- サードパーティのスタイルシートを利用するときは、自身のスタイルシートを一番最後に追加しましょう
  この辺は順番の話で、SOURCEORDER のところで書いた通り

#### 最終的な CSS の値の決定フロー

次のような指定値は最終的にどんな値に決定されるのか？

サンプル：

```html
<div class="section">
  <p class="amazing">AMAZING</p>
</div>
```

```CSS
.section {
    font-size: 1.5rem;
    width: 280px;
    background-color: orange;
}

p {
    width: 140px;
    background-color: green;
}

.amazing {
    width: 66%;
}
```

最終的な値の決定手順：

1. 宣言値

   作成者の指定した値

2. cascade 値

   cascade 後にその宣言にたいしてもっとも優先度が高い値

3. 特定値

   cascade しても優先度が高い値がなかった場合に採用される値

4. 計算値

   相対値を絶対値に変換した値

5. 使用済の値

   レイアウトに基づいた計算値

6. 実際の値

   1~5 を経て決定された値

これを適用すると

```CSS
* {
    /* browser default */
    font-size: 16px;
}

.section {
    /*
    1. 1.5rem
    2. 1.5rem
    3. 1.5rem
    4. 24px
        1.5 * 16px
    5. 24px
    6. 24px
    */
    font-size: 1.5rem;
    width: 280px;
    background-color: orange;
}

p {
    /* .sectionからfont-sizeを継承する */
    /* font-size: 24px; */
    width: 140px;
    background-color: green;
}

.amazing {
    /*
    1. 66%
    2. 66%
    3. 66%
    4. 66%
    5. .sectionの子要素なので.sectionのwidth280pxに対して66%である
    つまり184.8px
    6. 185px
    */
    width: 66%;
}
```

相対値から絶対値へどうやって変換されるのか？

サンプル

```CSS
html, body {
    font-size: 16px;
    width: 80vw;    /* 現在のviewportの幅の80% */
}

header {
    font-size: 150%; /* 親要素の計算値 16px * 1.5 = 24px */
    padding: 2em; /* emは親要素のフォントサイズを基準 16 * 2 = 32px */
    margin-bottom: 10rem; /* remはroot要素のフォントサイズを基準 16 * 10 = 160px */
    height: 90vh;   /* 現在のviewportの高さの90% */
    width: 1000px;
}

.header-child {
    font-size: 3rem;
    padding: 10%;  /*親要素に対する相対値 1000px * 0.1 = 100px */
}
```

知っておくこと：

- 宣言されていない&&継承がない場合に採用される初期値がどのプロパティにもある
- ブラウザは root にデフォルトの font-size がある（だいたい 16px）
- %と相対値は常に px へ変換される
- 親要素に font-size が指定されていれば、%は親要素の font-size に対してになる
- 親要素に長さが指定されていれば、%は親要素の width に対してになる
- em は親要素の font-size に対しての相対値
- em は現在の font-size に対しての相対値
- rem は常に root 要素の font-size に対しての相対値

#### 継承

知っておくこと

- 継承は特定のプロパティに対して親要素から子要素へ行われる
- テキストに関するプロパティは継承が行われる
  font-family, font-size とか
- プロパティの計算値は、宣言された値ではなく、継承されるものです
- プロパティの継承は、そのプロパティが宣言されていなかったら継承される
- `inherit`キーワードは特定のプロパティに継承を強制する
- `inherit`キーワードは特定のプロパティの初期値をリセットする

## Good Practice: 常にブラウザのデフォルト値を継承などするために

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

#### BOX MODEL

1. box-sizing のデフォルト`content-box`

width = right border + right padding + specified width + left padding + left border;

height = 同様

2. `border-box`

width = specified width;

つまり、border, padding が width 指定値に収まるようにコンテンツ領域が縮小される

3. box types

- `display: block`

要素の見た目はブロックのようにフォーマットされる
100% of parents width
垂直方向に並べられる

- `display: inline`

コンテンツは水平方向に並べられる
コンテンツ領域だけを占領する
改行を起こさない
高さと幅を指定できない
水平方向の padding, margin しか指定できない

- `display: inline-block`

block と inline のミックスイン

周囲のコンテンツに対して：inline 要素のようにふるまう
コンテンツ領域のみ占領する
改行を起こさない

内部的にはブロックレベルのボックスとして振る舞う

#### POSITIONING SCHEME

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

#### stacking context

主に`z-index`で指定することで生成されるコンテキスト

## CSS Architecture Components and BEM

優れた、再利用性と拡張性の高い CSS 設計のために

THINK, BUILD, ARCHITECT の流れで設計しよう

1. THINK: component driven design CSS (CDCSS)

atomic design から影響された設計哲学

コンポーネント駆動設計の CSS では、

ページをモジュラーコンポーネントに分割する

コンポーネントはインターフェイスを構築するために使用される構成要素である

したがって、
インターフェイスはページ全体のレイアウトによってまとめられたコンポーネントのコレクションである

コンポーネントは再利用性があるべきである

コンポーネントのライブラリがあるとプロジェクト全体で再利用できる

コンポーネントはページのどこにいても完全に独立していないといけない

つまりコンポーネントは親要素に依存してはならない

2. BUILD: Block Element Modifier

https://en.bem.info/

> BEM（Block、Element、Modifier）は、Web 開発へのコンポーネントベースのアプローチです。
> その背後にある考え方は、ユーザーインターフェイスを独立したブロックに分割することです。
> これにより、複雑な UI を使用した場合でも、インターフェイスの開発が簡単かつ迅速になり、コピーして貼り付けることなく既存のコードを再利用できます。

- BLOCK

> 再利用できる機能的に独立したページコンポーネント。 HTML では、ブロックはクラス属性で表されます。 特徴： ブロック名は、その状態（「どのように見えるか」-赤または大きい）ではなく、その目的（「それは何ですか？」-メニューまたはボタン）を説明します。

- ELEMENT

> ブロックとは別に使用できない複合パーツ。 特徴： 要素名は、その状態（「どのタイプ、またはどのように見えるか」-赤、大きいなど）ではなく、その目的（「これは何ですか？」-アイテム、テキストなど）を説明します。 要素のフルネームの構造は block-name**element-name です。要素名は、二重下線（**）でブロック名と区切られます。

```HTML
<!-- `search-form` block -->
<form class="search-form">
    <!-- `input` element in the `search-form` block -->
    <input class="search-form__input">

    <!-- `button` element in the `search-form` block -->
    <button class="search-form__button">Search</button>
</form>
```

- MODIFIER

> ブロックまたは要素の外観、状態、または動作を定義するエンティティ。

> 特徴： 修飾子の名前は、その外観（ "What size？"または "Which theme？"など-size*s または theme_islands）、状態（ "他との違いは？"-disabled、focused など）およびその状態を表します。動作（「どのように動作しますか？」または「ユーザーにどのように応答しますか？」-directions_left-top など）。 修飾子名は、単一の下線（*）でブロック名または要素名から区切られます。

```HTML
<!-- The `search-form` block has the `focused` Boolean modifier -->
<form class="search-form search-form_focused">
    <input class="search-form__input">

    <!-- The `button` element has the `disabled` Boolean modifier -->
    <button class="search-form__button search-form__button_disabled">Search</button>
</form>
```

```CSS
/* ざつにまとめるとこういうこと */
.block {}
.block__element {}
.block__element--modifier {}
```

3. ARCHITECT: 7-1 Pattern

部分的な Sass ファイルを配置する 7 つの異なるフォルダーと、
すべての部分的なファイルを 1 つの最終的にコンパイルされた CSS スタイルシートにインポートする
1​​ つのメイン Sass ファイルがあります。

このメソッドで使用される 7 つのフォルダーは、
基本的な製品定義を配置するベースフォルダー、
コンポーネントごとに 1 つのファイルがあるコンポーネントフォルダー、
レイアウトフォルダー、
プロジェクト、プロジェクトの特定のページのスタイルがある pages フォルダー、
さまざまなビジュアルテーマを実装する場合は themes フォルダー、
変数やミックスインなどの CSS を出力しないコードを配置する abstracts フォルダー、
そして最後に、すべてのサードパーティ CSS が配置される vendors フォルダー。

#### 実践：BEM を NATOURS プロジェクトに適用してみる

```HTML
<!-- before -->
    <body>
        <div class="header">
            <div class="logo-box">
                <img src="img/logo-white.png" alt="Logo" class="logo"/>
            </div>

            <div class="text-box">
                <h1 class="heading-primary">
                    <span class="heading-primary-main">Outdoors</span>
                    <span class="heading-primary-sub">Is where life happens</span>
                </h1>

                <a href="#" class="btn btn-white btn-animated">Discover our tours</a>
            </div>
        </div>
    </body>
<!-- after -->
    <body>
        <div class="header">
            <div class="header__logo-box">
                <img src="img/logo-white.png" alt="Logo" class="header__logo"/>
            </div>

            <div class="header__text-box">
                <h1 class="heading-primary">
                    <span class="heading-primary--main">Outdoors</span>
                    <span class="heading-primary--sub">Is where life happens</span>
                </h1>

                <a href="#" class="btn btn--white btn--animated">Discover our tours</a>
            </div>
        </div>
    </body>
```

CSS セレクタもこの変更の通りのセレクタに変更するだけ

#### SASS `@mixin`

`@mixin`キーワードで CSS ファイル内のオブジェクトを定義できる

DRY 原則を CSS に適用できる

いくつかのアイテムを格納したオブジェクトを定義できるようになれば、
再利用することができる

```CSS
/* 定義 */
@mixin clearfix {
    content: "",
    clear: both;
    display: table;
}

nav {
    margin: 100px;
    /* 利用 */
    @include clearfix;
}
```

関数のように引数をとる mixin も定義できる

```CSS

$text-purple: purple;

/* 引数$colをとる */
@mixin style-link-text($col) {
    text-decoration: none;
    text-transform: translateX(2);
    /* $colはmixinのなかで変数のように使える */
    color: $col
}

a {
    /* ... */
    &:link {
        @include style-link-text($text-purple);
    }
}
```

関数そのものを作ることもできる

```CSS
@function divide($a, $b) {
    @return $a / $b;
}

nav {
    /*
    単位を設けるためのベスト・ぷらくてぃスだそうで
    */
    margin: divide(30, 2) * px;
}
```

`@extends`という拡張機能もある

```CSS
.error {
  border: 1px #f00;
  background-color: #fdd;
}

.error--serious {
  border-width: 3px;
}

/* 解決方法 */
.error {
  border: 1px #f00;
  background-color: #fdd;

  &--serious {
    @extend .error;
    border-width: 3px;
  }
}
```

> ページをデザインするときに、あるクラスに別のクラスのすべてのスタイルと、それ自体の特定のスタイルを含める必要がある場合がよくあります。
> たとえば、BEM 方法論では、ブロックまたは要素クラスと同じ要素に属する修飾子クラスを推奨します。ただし、これにより HTML が乱雑になる可能性があり、両方のクラスを含めるのを忘れるとエラーが発生しやすくなり、マークアップに非セマンティックスタイルの懸念が生じる可能性があります。

> Sass の`@extends <selector>`は呼び出し側に<selector>の内容を継承させる

あんまり@mixin と違いがわからない

あとあまり推奨されていないかも

#### Sass: command line

`node-sass <コンパイル元> <出力先>`

```bash
# -wでnodemonみたいに常に変更をコンパイルし続ける
$ node-sass sass/main.scss css/style.css -w
```

#### Sass: 独自の構文

同じネスト内であれば、
そのネストのセレクタと関連するセレクタを内部に追加できる

`&`でセレクタ名を省略できる

```CSS
/* before */
.header {
  position: relative;
  height: 95vh;
  background-image: linear-gradient(
      to right bottom,
      rgba($color-primary-light, 0.8),
      rgba($color-primary-dark, 0.8)
    ),
    url(../img/hero.jpg);
  background-size: cover;
  /* ブラウザのウィンドウをリサイズしたときにどこを基準に拡縮するのか指定する */
  background-position: top;

  /* shapeを変形させるﾌﾟﾛﾊﾟﾃｨ */
  /* ４頂点：左上、右上、右下、左下 */
  clip-path: polygon(0 0, 100% 0, 100% 75vh, 0 100%);
}

.header__logo-box {
  position: absolute;
  top: 4rem;
  left: 4rem;
}

.header__logo {
  height: 3.5rem;
}

.header__text-box {
  position: absolute;
  top: 40%;
  left: 50%;
  transform: translate(-50%, -50%);

  text-align: center;
}

/* after */

.header {
  position: relative;
  height: 95vh;
  background-image: linear-gradient(
      to right bottom,
      rgba($color-primary-light, 0.8),
      rgba($color-primary-dark, 0.8)
    ),
    url(../img/hero.jpg);
  background-size: cover;
  /* ブラウザのウィンドウをリサイズしたときにどこを基準に拡縮するのか指定する */
  background-position: top;

  /* shapeを変形させるﾌﾟﾛﾊﾟﾃｨ */
  /* ４頂点：左上、右上、右下、左下 */
  clip-path: polygon(0 0, 100% 0, 100% 75vh, 0 100%);

  &__logo-box {
    position: absolute;
    top: 4rem;
    left: 4rem;
  }

  &__logo {
    height: 3.5rem;
  }

  &__text-box {
    position: absolute;
    top: 40%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
  }
}

```

#### 7-1 Pattern

先までの main.scss に記述したすべてのセレクタを
7-1 パターンにあてはめて各ファイルに分割していく

```CSS
/* main.scss */

/* 全称セレクタやhtml, bodyなどの定義 */
@import "base/base";
/* @keyframesなどｱﾆﾒｰｼｮﾝにかかわる定義 */
@import "base/animations";
/* 文字セレクタにかかわる定義 */
/* 今回は.heading-primaryがこれに当たる */
@import "base/typography";
/*  */
@import "base/utilities";
/* @functions定義 */
@import "abstracts/functions";
/* @mixins定義 */
@import "abstracts/mixins";
/* $~など変数の定義 */
@import "abstracts/variables";
/* .headerなど */
@import "pages/home";
/* パーツなど */
@import "components/button";

```

**順番は重要である**

たとえば後のほうで import する scss ファイルで変数を使う場合、

先に変数をまとめた scss ファイルを import しておく必要がある

## BASIC RESPONSIVE DESIGN PRINCIPLES

基本レスポンシブデザイン原則:

- Fluid layouts
- Responsive units
- Flexible images
- Media queries

Summary:

1. Fluid layouts

- 現在の viewport(幅や高さ)に適応することを可能とさせること
  そのために...
- %をつかって px は使わない
- `max-width`を`width`の代わりに使う

2. Responsive units

- `rem`を`px`の代わりに使うこと
- レイアウトを容易にそして自動的に拡縮可能にすること

3. Flexible images

- デフォルトとして、viewport の変化に対して image は自動的に拡縮させない
- image の寸法に対して常に%を使うこと
  `max-width`も使うこと

4. Media queries

特定の viewport における width を変更させること

Details:

Fluid Layouts

float レイアウトはもはや古い技術として淘汰されつつある

代わりに flexbox, grid が現代的

flexbox は 1 次元レイアウトを構築するのに向いている

css grid は 2 次元レイアウトを構築するのに向いている

float を学習することはそれぞれの特徴を理解するのに役立つ

あと、

float レイアウトを理解しないと古い技術を使う現場で困る

## Learn Grid Layout using float

- `max-width`:

max-width は、十分な空きスペースがある場合は指定した幅になりますが、

十分な幅がない場合は、基本的にビューポートがここで指定した幅よりも小さい場合、

つまりこの場合はビューポートが 114rem より小さい場合、ビューポートは、使用可能なスペースの 100％を埋めるだけです。

- 一番最後の要素以外すべてに適用させたいとき

```SCSS
.row {
  max-width: $grid-width;
  background-color: #eee;
  margin: 0 auto;

//   適用される要素のうち一番最後の要素以外
  &:not(:last-child) {
    margin-bottom: $gutter-vartical;
  }

  .col-1-of-2 {
      width: calc((100% - #{$gutter-horizontal}) / 2);
  }
}
```

- SASS のテンプレートリテラル

```SCSS
  .col-1-of-2 {
      width: calc((100% - #{$gutter-horizontal}) / 2);
  }
```

`#{}`で囲えば変数が使える

#### clearfix と float

clearfix ってなに？

`clear`:

> clear は CSS のプロパティで、要素をその前にある浮動要素の下に移動 (clear) する必要があるかどうかを設定します。
> clear プロパティは、浮動要素と非浮動要素のどちらにも適用されます。

```SCSS
@mixin clearfix {
    &::after {
        content: "";
        display: table;
        clear: both;
    }
}
```

疑似要素`::after`を`display: table`で表示する

`clear: both`で、**要素は先行する左右両方の浮動要素と切り離されて下に移動する**

つまり、

float を解除したいところで`clearfix`を付けると
float から切り離すことができる

以下の例では、

すべての`div.row`に`clearfix`が適用されるので

`div.row`には float が解除される

```SCSS
.row {
  max-width: $grid-width;
  background-color: #eee;
  margin: 0 auto;

  //   適用される要素のうち一番最後の要素以外
  &:not(:last-child) {
    margin-bottom: $gutter-vertical;
  }

  @include clearfix;

  .col-1-of-2 {
    width: calc((100% - #{$gutter-horizontal}) / 2);
    background-color: orangered;
    float: left;

    // 最後の要素以外margin-rigthを設ける
    &:not(:last-child) {
      margin-right: $gutter-horizontal;
    }
  }
}

```

#### 共通の class 名のスタイルを一括で定義したいとき

たとえばどの class 名も`col-`で始まる時

そのすべての class 名で共通のスタイルを定義したかったら

こんな感じにする

```SCSS
// before
{
    // ...
  .col-1-of-2 {
    width: calc((100% - #{$gutter-horizontal}) / 2);
      // ~~一括で定義したい~~
    background-color: orangered;
    float: left;
    &:not(:last-child) {
      margin-right: $gutter-horizontal;
    }
    // ~~~~~~~~~~~~~~~~~~~~
  }

  .col-1-of-3 {
      width: calc((100% - #{$gutter-horizontal}) / 3);
  }
  .col-1-of-4 {
      width: calc((100% - #{$gutter-horizontal}) / 4);
  }
}

// after
{
[class^="col-"] {
    // ...共通のスタイルを定義する
    background-color: orangered;
    float: left;
    &:not(:last-child) {
      margin-right: $gutter-horizontal;
    }
}
  .col-1-of-2 {
    width: calc((100% - #{$gutter-horizontal}) / 2);
  }
  .col-1-of-3 {
      width: calc((100% - #{$gutter-horizontal}) / 3);
  }
  .col-1-of-4 {
      width: calc((100% - #{$gutter-horizontal}) / 4);
  }
}
```

#### inline-block 要素を中央に配置する

> この heading-secondary を覚えておいてください。これはインラインブロックとして定義されているため、親を text-align-center に設定すると、その中のインラインブロック要素はテキストとして扱われるため、親の中央に配置されます

```html
<section class="section-about">
  <div class="u-center-text">
    <h2 class="heading-secondary">Exciting tours for adventure</h2>
  </div>
</section>
```

```CSS
.u-center-text {
    text-align: center;
}

.heading-secondary {
    display: inline-block;
}
```

`text-align`はブロック要素または表セルボックスの水平方向の配置を設定します

`inline-block`はブロック要素を生成しているので

これの配置を操作できるようになる

- インライン要素とは

https://developer.mozilla.org/ja/docs/Web/HTML/Inline_elements

たとえば一塊の文章を表示するとして

その文章のうち特定の文章だけ太字に表示したいとする

このとき文章の流れに変更をもたらさずに

その文章を囲得る要素をインライン要素と呼ぶ

```HTML
<div>The following span is an <span class="highlight">inline element</span>;
its background has been colored to display both the beginning and end of
the inline element's influence.</div>
```

上記の例では`span`はテキストの流れを変更しないので

改行などが起こらない

```HTML

```

一方`p`要素で囲うとこれは改行をもたらし

つまりテキストの流れを分断する

これらの結果が異なるのは

`span`がインライン要素で`p`がブロック要素だからである

#### utilities クラスの利用の仕方

`.u-center-text`が定義されている div に対して margin-bottom を追加したい...

そんなとき。

直接`.u-center-text`へ追加するハードコーディングを避けたいなら、

いつでも追加できるようにあらかじめ定義してあるやつを追加すればいい

```html
<!-- u-margin-bottom-8というCSSを追加した -->
<div class="u-center-text u-margin-bottom-8">
  <h2 class="heading-secondary">Exciting tours for adventure</h2>
</div>
```

```CSS
.u-margin-bottom-8 {
    margin-bottom: 8rem;
}
```

こうすればあとから簡単に着脱できるし

`.u-center-text`に`margin-bottom`を追加しなくて済むことで

`.u-center-text`の再利用性を損なわずに済む

再利用性が高く、追加しやすい定義群

本来のスタイルが定義されている要素に utilities のクラス名を追加することで

すぐに欲しいスタイルを追加できる

インスタントな存在である

## CSS Tips: Icon を使う時は SVG を使うこと

ページの拡大縮小に影響を受けるので
PNG だと解像度が変わってしまったりする

SVG なら問題なし

## CSS Tips: skew で背景を斜めにして、セクション間のギャップを埋める方法

(drawio で図形を作って markdown に埋め込んでみた)

![](<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="591px" viewBox="-0.5 -0.5 591 591" content="&lt;mxfile host=&quot;06dfog0h3hjo8pr7rodigffo1hf3l8u6edfg4g5qr45rm1npunhg&quot; modified=&quot;2022-04-05T12:53:31.611Z&quot; agent=&quot;Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Code/1.66.0 Chrome/98.0.4758.109 Electron/17.2.0 Safari/537.36&quot; etag=&quot;r_h_nrrfcLwnYE-yE1TZ&quot; version=&quot;12.2.4&quot; pages=&quot;1&quot;&gt;&lt;diagram id=&quot;m8At_CxRcnWjZ6WXLhQg&quot; name=&quot;Page-1&quot;&gt;3ZVNc4IwEEB/DXchingsau2lJw89R7JCxkCYGAX767uYIJ8dndZeemGStxuSfdkBhyzTcqNonrxLBsLxJqx0yMrxPN9d4LMCFwNmi7kBseLMILcBW/4JFk4sPXEGx06illJonndhJLMMIt1hVClZdNP2UnR3zWkMA7CNqBjSD850YmjgzRv+BjxO6p1d3xac0jrZVnJMKJNFC5G1Q5ZKSm1GabkEUbmrvZh1r99EbwdTkOlHFnhmwZmKk63Nnktf6mKLhGvY5jSq5gXep0PCRKcCZy4O6TE3ive8BHxrODyCPdUZlIayheyRNiBT0OqCKTZa27l0p0XjerawLGl5vkFq7ze+vblRgANrYdwIGRjBOriCui36erAk3TVy1EoeYCmFVEgymWFmuOdC9BAVPM5wGuEGgDysBHHsshcbSDlj1Tbh2BUoecpYJXw1eZL0nnV3RLsbjGgPnmB9er8PuwXf6cq2bscjbAYBmw7uBiOBtyO+/zdtSx7s29t37TcGZwODOxod4qu1f9y0PeXBg8p/0LM4bb7L11jr50bWXw==&lt;/diagram&gt;&lt;/mxfile&gt;" onclick="(function(svg){var src=window.event.target||window.event.srcElement;while (src!=null&amp;&amp;src.nodeName.toLowerCase()!='a'){src=src.parentNode;}if(src==null){if(svg.wnd!=null&amp;&amp;!svg.wnd.closed){svg.wnd.focus();}else{var r=function(evt){if(evt.data=='ready'&amp;&amp;evt.source==svg.wnd){svg.wnd.postMessage(decodeURIComponent(svg.getAttribute('content')),'*');window.removeEventListener('message',r);}};window.addEventListener('message',r);svg.wnd=window.open('https://www.draw.io/?client=1&amp;lightbox=1&amp;edit=_blank');}}})(this);" style="cursor:pointer;max-width:100%;max-height:591px;"><defs/><g><rect x="0" y="0" width="590" height="590" fill="#ffffff" stroke="#000000" pointer-events="all"/><rect x="200" y="100" width="180" height="80" fill="none" stroke="none" pointer-events="all"/><g transform="translate(259.5,133.5)"><foreignObject style="overflow:visible;" pointer-events="all" width="60" height="13"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 61px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;white-space:normal;">entire page</div></div></foreignObject></g><rect x="0" y="210" width="590" height="210" fill="#d5e8d4" stroke="#82b366" pointer-events="all"/><rect x="0" y="260" width="590" height="80" fill="none" stroke="none" pointer-events="all"/><g transform="translate(263.5,293.5)"><foreignObject style="overflow:visible;" pointer-events="all" width="63" height="13"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 64px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;white-space:normal;">background</div></div></foreignObject></g></g></svg>)

![](<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="593px" viewBox="-0.5 -0.5 593 561" content="&lt;mxfile host=&quot;06dfog0h3hjo8pr7rodigffo1hf3l8u6edfg4g5qr45rm1npunhg&quot; modified=&quot;2022-04-05T13:15:27.428Z&quot; agent=&quot;Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Code/1.66.0 Chrome/98.0.4758.109 Electron/17.2.0 Safari/537.36&quot; etag=&quot;q_G-wP6uWH6IGkQ7hhL9&quot; version=&quot;12.2.4&quot; pages=&quot;1&quot;&gt;&lt;diagram id=&quot;m8At_CxRcnWjZ6WXLhQg&quot; name=&quot;Page-1&quot;&gt;3ZfLjpswFIafBqndjDC3wDLJXBeVRkqltksPeMAag5FxJkmfvsfB5mZGE3WoWnVD8G9j+3zn/IY4/rY83glcF194RpjjudnR8a8dz1t5MVyVcNKC67VCLmjWSqgXdvQn0aKr1T3NSDMaKDlnktZjMeVVRVI50rAQ/DAe9szZeNUa58QSdilmtvqNZrJo1dhb9fo9oXlhVkZR0vaU2AzWkTQFzvhhIPk3jr8VnMv2rjxuCVPsDJf2uds3eruNCVLJSx7Q3F8x2+vY9L7kyQR7KKgkuxqnqn2AfDr+ppAlgxaCW3tFvYlXIiQ5DiS9gzvCSyLFCYboXgNDV0Oim4cebWi0YoA18LWIdTrzbuY+YrjRQc8D8C0AEAcVxFTBlAaEJMcAGin4C9lyxgUoFa9g5OaZMjaRMKN5Bc0UFiCgbxQgCkW11h0lzTK1zGaOuOD7KiNq0+5C0CfUkWdjR/EM9ngB6sbIH6i7DDfFmQea4HY8/zq4Qbch6LSEHK6buj0EFDlBGjhOjGtRp3R+7WvaYjlD/NKa7iJ+r6i9cAG80ft04eCp1S1EhBkjjOcClxB6TQSF9VSBjvse+47ZAh2kRnCJJeWqphPXzk4WkjgLLOdAT+w9+VG0TAJQ4l2FoxwEdgq8ILRTECbo4ykIrRQ84fQlP9v4Pz5UJlUfX1j1i5wpyEJugSZVtlbvf0WM4aah6Rg7RC5O3xWPKzeOjPDjLCSr2AjXR42sbZ2GrYFPBlRJZn1RWJgbvhepGaX3LrHIiRnmzrMfwp1hazRBGLjydbyNOeB6hUdOYYNvpbZLo5mh3b1+aPjFMZlnNZknmMzTRmzNc85+F/RlBWF/3Nw+fAfh6/3DDn7u1o9w/fSkej13aE/4SCT48z9q09GLb4GDMpgkJA5sz0YzdRVFC3g2WNSzKAjHnnUT7896dujP6K/aM06u/Bi5QYRCP4brKKmRG/yeW300nshfLWVXaPZ/c9rh/X9F/+YX&lt;/diagram&gt;&lt;/mxfile&gt;" onclick="(function(svg){var src=window.event.target||window.event.srcElement;while (src!=null&amp;&amp;src.nodeName.toLowerCase()!='a'){src=src.parentNode;}if(src==null){if(svg.wnd!=null&amp;&amp;!svg.wnd.closed){svg.wnd.focus();}else{var r=function(evt){if(evt.data=='ready'&amp;&amp;evt.source==svg.wnd){svg.wnd.postMessage(decodeURIComponent(svg.getAttribute('content')),'*');window.removeEventListener('message',r);}};window.addEventListener('message',r);svg.wnd=window.open('https://www.draw.io/?client=1&amp;lightbox=1&amp;edit=_blank');}}})(this);" style="cursor:pointer;max-width:100%;max-height:561px;"><defs/><g><rect x="0" y="0" width="590" height="430" fill="#ffffff" stroke="#000000" pointer-events="all"/><rect x="200" y="30" width="180" height="80" fill="none" stroke="none" pointer-events="all"/><g transform="translate(259.5,62.5)"><foreignObject style="overflow:visible;" pointer-events="all" width="60" height="13"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 61px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;white-space:normal;">entire page</div></div></foreignObject></g><rect x="0" y="120" width="590" height="250" fill="#d4e1f5" stroke="#000000" stroke-dasharray="3 3" pointer-events="all"/><path d="M 172.5 541 L 221.5 -50 L 417.5 -50 L 368.5 541 Z" fill="#d5e8d4" stroke="#82b366" stroke-miterlimit="10" transform="rotate(90,295,245.5)" pointer-events="all"/><rect x="0" y="190" width="590" height="80" fill="none" stroke="none" pointer-events="all"/><g transform="translate(263.5,223.5)"><foreignObject style="overflow:visible;" pointer-events="all" width="63" height="13"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 64px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;white-space:normal;">background</div></div></foreignObject></g><path d="M 249.38 494 L 56.07 367.98" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="stroke"/><path d="M 51.68 365.11 L 59.45 366 L 56.07 367.98 L 55.63 371.87 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="all"/><rect x="20" y="494" width="560" height="66" fill="none" stroke="none" pointer-events="all"/><g transform="translate(197.5,519.5)"><foreignObject style="overflow:visible;" pointer-events="all" width="205" height="13"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 206px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;white-space:normal;">FIX THIS GAP (blue background area)</div></div></foreignObject></g><path d="M 269.38 514 L 532.31 163.62" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="stroke"/><path d="M 535.46 159.42 L 534.06 167.12 L 532.31 163.62 L 528.46 162.92 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="all"/></g></svg>)

(いまいましい SVG め)

1. 緑色の背景のように斜めにする方法
2. 緑色の背景の中のコンテンツは斜めにさせない方法
3. 斜めにしたことで生まれるギャップを埋める方法

異常の 3 つを学習する

```HTML
<!-- 斜めにする背景 -->
<section class="section-features">
    <!-- 背景上のコンテンツ -->
    <div class="feature-box">content 1</div>
    <div class="feature-box">content 2</div>
    <div class="feature-box">content 3</div>
</section>
```

- まず全体を斜めにする

`transform: skew`を使う

```scss
.section-features {
  padding: 2rem 0;
  background-image: linear-gradient(
      to right bottom,
      rgba($color-primary-light, 0.8),
      rgba($color-primary-dark, 0.8)
    ), url(../img/nat-4.jpg);
  background-size: cover;

  // 全体を斜めにする
  transform: skewY(-7deg);
}
```

- `section-feature`の**直接の子要素**はすべて斜めにさせない

`skewY(7deg)`させるだけ

`feature-box`にハードコーディングしてもいいけれど
それだと常に.section-feature の子要素のプロパティに
必ず斜めにさせないスタイルを突っ込まないといけない

なので親要素のプロパティで指定する

直接の子要素だけをしていさせるのは全体をゆがませないため

```scss
.section-features {
  padding: 2rem 0;
  background-image: linear-gradient(
      to right bottom,
      rgba($color-primary-light, 0.8),
      rgba($color-primary-dark, 0.8)
    ), url(../img/nat-4.jpg);
  background-size: cover;

  // 全体を斜めにする
  transform: skewY(-7deg);

  // 全ての「直接の」子要素
  //
  // 反対の角度にskewさせるだけ
  & > * {
    transform: skewY(7deg);
  }
}
```

- ギャップを埋める

**ネガティブマージン**(負の方向への margin)を使う

```SCSS
.section-features {
    padding: 2rem 0;
    background-image: linear-gradient(
            to right bottom,
            rgba($color-primary-light, 0.8),
            rgba($color-primary-dark, 0.8)
        ),
        url(../img/nat-4.jpg);
        background-size: cover;
        //
        // 上方向にmarginを
        margin-top: -10rem;

        transform: skewY(-7deg);
        & > * {
            transform: skewY(7deg)
        }
}

```

https://coliss.com/articles/build-websites/operation/css/css-using-negative-margins.html

参考サイトが言うには

静的な要素（float なし要素）にネガティブマージンを使った場合、その指定した方向へ要素を引っ張るそうです

## CSS Tips: カードをめくる、めくるときの 3 次元的な動き

- `transform: translateY(180deg)`でカードがめくるように動く

```SCSS
.card {
    // ...
    transition: all .5s;
    &:hover {
        transform: translateY(180deg);
    }
}
```

- `perspective`で 3 次元的な遠近感を与える

MDN より

> perspective は CSS のプロパティで、 z=0 の平面とユーザーとの間の距離を定めて三次元に配置された要素に遠近感を与えます。

この`perspetive`を先の`.card`に適用すると、
まるでカードをめくっているかのようにふるまわせることができる

```SCSS
.card {
    // ...
    // ユーザーと z=0 平面間の距離を表す <length> です
    perspective: 150rem;
    transition: all .5s;
    &:hover {
        transform: translateY(180deg);
    }
}
```

## CSS Tips: めくれるカードの裏表をどうやって実装するのか

```html
<div class="card">
  <div class="card__side card__side--front">FRONT</div>
  <div class="card__side card__side--back">BACK</div>
</div>
```

- 第 1 段階：それぞれ常に反対方向をみせるようにする

```SCSS
.card {
    perspective: 150rem;
    -moz-perspective: 150rem;


    &__side {
        background-color: orangered;
        color: #fff;
        font-size: 1.6rem;
        height: 50rem;
        transition: all 0.8s;

        &--front {
            background-color: orangered;
        }

        &--back {
            background-color: green;
            // 平時は裏を向けている
            transform: rotateY(180deg);
        }
    }


    &:hover &__side--front {
        // frontが180度回転するとき
        transform: rotateY(180deg);
    }
    &:hover &__side--back {
        // 元々180度回転していたbackを0度にする
        transform: rotateY(0);
    }
}
```

- 第二段階：front と back の位置を合わせる

今のところ上下に front と back の card が並んでいるので
これを同じ位置に配置して重ね合わせる

`position`を使う

伴って、

幅が値であるテキストに一致するようになってしまっているので

`width: 100%`を与えて card(というか grid の.col-1-of-3)の本来の幅に合わせる

`backface-visibility: hidden`で裏は非表示にさせることができる

```SCSS
.card {
  perspective: 150rem;
  -moz-perspective: 150rem;
  position: relative;

  &__side {
    color: #fff;
    font-size: 1.6rem;

    height: 50rem;
    transition: all 0.8s;

    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    backface-visibility: hidden;

    &--front {
      background-color: orangered;
    }

    &--back {
      background-color: green;
      transform: rotateY(180deg);
    }
  }

  &:hover &__side--front {
    transform: rotateY(180deg);
  }
  &:hover &__side--back {
    transform: rotateY(0);
  }
}
```

- 第三段階：高さの修正

`position: absolute`は通常のフローから外れ要素の為のレイアウトが作成されない

**これを`display: float`のなかでやると次の要素と重なってしまう**

なので.card とその次の要素が重なってしまうのである

これを解決するために.card へ明示的に高さを与える

```SCSS
.card {
  perspective: 150rem;
  -moz-perspective: 150rem;
  position: relative;

    // 明示的な高さを設定する
  height: 50rem;

  &__side {
    color: #fff;
    font-size: 1.6rem;

    height: 50rem;
    transition: all 0.8s;

    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    backface-visibility: hidden;

    &--front {
      background-color: #fff;
    }

    &--back {
      background-color: green;
      transform: rotateY(180deg);
    }
  }

  &:hover &__side--front {
    transform: rotateY(180deg);
  }
  &:hover &__side--back {
    transform: rotateY(0);
  }
}
```

## EMMET-Tips

`.composition>(img.composition_photo.composition_photo-p1)*3`

で次が出力される

```HTML

<div class="composition">
    <img src="" alt="" class="composition_photo composition_photo-p1">
    <img src="" alt="" class="composition_photo composition_photo-p1">
    <img src="" alt="" class="composition_photo composition_photo-p1">
</div>
```

## お役立ち

- ダミーテキストを生成してくれるサービス

[Lorem Ipsum](#https://www.lipsum.com/)

- 文字コード一覧

たとえば矢印とか表示してくれる文字コードを探すのにいい

講師のページ

https://css-tricks.com/snippets/html/glyphs/

- Linea Icon free icon

https://www.linea.is/
