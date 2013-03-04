# Slim

[![Gem Version](https://badge.fury.io/rb/slim.png)](http://rubygems.org/gems/slim) [![Build Status](https://secure.travis-ci.org/slim-template/slim.png?branch=master)](http://travis-ci.org/slim-template/slim) [![Dependency Status](https://gemnasium.com/slim-template/slim.png?travis)](https://gemnasium.com/slim-template/slim) [![Code Climate](https://codeclimate.com/github/slim-template/slim.png)](https://codeclimate.com/github/slim-template/slim)

Slim は 不可解にならないように view の構文を本質的な部品まで減らすことを目指したテンプレート言語です。標準的な HTML テンプレートからどれだけのものが削除できるか確かめるところから始まりました。(<, >, 閉じタグなど) 多くの人が Slim に興味を持ったことで, 機能性は発展し, 柔軟な構文をもたらしました。

機能の短いリスト

* すっきりした構文
    * 閉じタグの無い短い構文 (代わりにインデントを用いる)
    * 閉じタグを用いた HTML 形式の構文
    * 設定可能なショートカットタグ (デフォルトでは `#` は `<div id="...">` に, `.` は `<div class="...">` に)
* 安全性
    * デフォルトで自動 HTML エスケープ
    * Rails の `html_safe?` に対応
* プラグインを用いた柔軟な設定と拡張性
    * Mustache のようなロジックレスモードをプラグインで実現
    * 多言語化/I18n をプラグインで実現
* 高性能
    * ERB/Erubis に匹敵するスピード
    * Rails のストリーミングに対応
* 全てのメジャーフレームワークが対応 (Rails, Sinatra, ...)
* Ruby 1.9 では タグや属性の Unicode に完全対応
* Markdown や Textile のような埋め込みエンジン

## Upgrade to version 2.0

__NOTE:__ Slim 2.0 はまだリリースされていませんが, preview バージョンを試すことができます。

すでに Slim 1.3 や 1.2 を使用していて最新バージョンの 2.0 にアップグレードしたい場合, まず非推奨機能の
エラーを出す Slim 1.3.7 にアップグレードするべきです。これはあなたのアプリケーションが Slim 2.0 に準拠
しているか簡単に確認する方法です。

Slim 2.0 は 1.3 シリーズから非推奨機能を取り除き構文のマイナーな矛盾を
整理しています (属性の囲みや引用符で囲まれた属性のエスケープ)。
もちろん新機能やバグフィックスも含みます。詳細は CHANGES を参照してください。

ほとんどの場合アップグレードを恐れてはいけません。わたしたちは後方互換を保つように努め, [semantic versions](http://semver.org/) 
に可能なかぎり対応しています。

## イントロダクション

### Slim とは?

Slim は Rails3 に対応した高速, 軽量なテンプレートエンジンです。すべての主要な Ruby の実装でしっかりテストされています。
私たちは継続的インテグレーションを採用しています。(travis-ci)

Slim の核となる構文は1つの考えによって導かれます: "この動作を行うために最低限必要なものは何か"。

多くの人々の Slim への貢献によって, 彼らが使う [Haml](https://github.com/haml/haml) や [Jade](https://github.com/visionmedia/jade) の影響を受け構文の追加が行われています。 Slim の開発チームは美は見る人の目の中にあることを分っているのでこういった追加にオープンです。

Slim は 構文解析/コンパイルに [Temple](https://github.com/judofyr/temple) を使い [Tilt](https://github.com/rtomayko/tilt) に組み込まれます。これにより [Sinatra](https://github.com/sinatra/sinatra) やプレーンな [Rack](https://github.com/rack/rack) とも一緒に使えます。

Temple のアーキテクチャはとても柔軟でモンキーパッチなしで構文解析とコンパイルのプロセスの拡張を可能にします。これはロジックレスのプラグインや I18n が提供する翻訳プラグインに
使用されます。

### なぜ Slim を使うのか?

Rails コミュニティの中で, _Erb_ と _Haml_ は間違いなく最も人気がある2つのテンプレートエンジンです。しかしながら, _Erb_ の構文は扱いにくく, _Haml_ の構文は初心者にはとても謎めいている場合があります。

ロジックレスエンジンのいくつかの発展もあります (例: 標準のHTMLを書かなければいけない [Mustache](https://github.com/defunkt/mustache))。あなたの好みで Slim の構文で HTML をビルドすることができます。Slim の構文で HTML をビルドすることが好きだけどテンプレートに Ruby のコードを書きたくない場合でも Slim のロジックレスモードを使うことができます。

Slim は 最小限の構文とスピードをもたらすために生まれました。 もし Slim を選択しない場合, その理由はスピード以外の理由によるものでしょう。

___そう, Slim は速い!___ ベンチマークはコミット毎に <http://travis-ci.org/#!/slim-template/slim> で取られています。
この数字が信じられませんか? それは仕方ないことです。是非 rake タスクを使って自分でベンチマークを取ってみてください!

### どう始めるの?

Slim を gem としてインストール:

    gem install slim

あなたの Gemfile に `gem 'slim'` と書いてインクルードするか, ファイルに `require 'slim'` と書く必要があります。これだけです! 後は拡張子に .slim を使うだけで準備はできています。

### 構文例

Slim テンプレートがどのようなものか簡単な例を示します:

    doctype html
    html
      head
        title Slim のファイル例
        meta name="keywords" content="template language"
        meta name="author" content=author
        link rel="icon" type="image/png" href=file_path("favicon.png")
        javascript:
          alert('Slim は javascript の埋め込みに対応します!')

      body
        h1 マークアップ例

        #content
          p このマークアップ例はあなたに Slim の典型的なファイルがどのようなものか示します。

        = yield

        - if items.any?
          table#items
            - for item in items do
              tr
                td.name = item.name
                td.price = item.price
        - else
          p アイテムが見つかりませんでした。いくつか目録を追加してください。
            ありがとう!

        div id="footer"
          = render 'footer'
          | Copyright &copy; #{@year} #{@author}

インデントについて, インデントの深さはあなたの好みで選択できます。もし最初のインデントをスペース2つ, その次に5スペースを使いたい場合, それはあなたの選択次第です。マークアップを入れ子にするにはスペース1つのインデントが必要なだけです。

## ラインインジケータ

### テキスト `|`

パイプは Slim に行をコピーしろと命じます。基本的にどのような処理でもエスケープします。
パイプよりも深くインデントされた各行がコピーされます。

    body
      p
        |
          これはテキストブロックのテストです。

  構文解析結果は以下:

    <body><p>これはテキストブロックのテストです。</p></body>

  ブロックの左端はパイプ +1 スペースのインデントに設定されています。 
  追加のスペースはコピーされます。

    body
      p
        |  この行は左端になります。
            この行はスペース1つを持つことになります。
              この行はスペース2つを持つことになります。
                以下同様に...

テキスト行に HTML を埋め込むこともできます。

    - articles.each do |a|
      | <tr><td>#{a.name}</td><td>#{a.description}</td></tr>

### テキスト行のスペースをたどる `'`

シングルクォートは Slim に行をコピーしろと命じます (`|` と同様に) が,  単一行の末尾にスペースが追加されているかチェックします。

### インライン html `<` (HTML 形式)

あなたは html タグを直接 Slim の中に書くことができます。Slim は閉じタグを使った html タグ形式や html と Slim を混ぜてテンプレートの中に書くことができます。

    <html>
      head
        title 記述例
      <body>
        - if articles.empty?
        - else
          table
            - articles.each do |a|
              <tr><td>#{a.name}</td><td>#{a.description}</td></tr>
      </body>
    </html>

### 制御コード `-`

ダッシュは制御コードを意味します。制御コードの例としてループと条件文があります。`end` は `-` の後ろに置くことができません。ブロックはインデントによってのみ定義されます。
複数行にわたる Ruby のコードが必要な場合, 行末にバックスラッシュ `\` を追加します。行末がカンマ `,` で終わる場合 (例 関数呼び出し) には行末にバックスラッシュを追加する必要はありません。

    body
      - if articles.empty?
        | 在庫なし

### 出力 `=`

イコールはバッファに追加する出力を生成する Ruby 呼び出しを Slim に命令します。Ruby のコードが複数行にわたる場合, 例のように行末にバックスラッシュを追加します。

    = javascript_include_tag \
       "jquery", \
       "application"

行末がカンマ `,` で終わる場合 (例 関数呼び出し) には行末にバックスラッシュを追加する必要はありません。

### 出力のスペースをたどる `='`

後に続くスペースを追加することを除き, 単一のイコール (`=`) と同じです。

### HTML エスケープを伴わない出力 `==`

単一のイコール (`=`) と同じですが, `escape_html` メソッドを経由しません。

### HTML エスケープを伴わず出力のスペースをたどらない `=='`

後に続くスペースを追加することを除き, 二重のイコールと同じです。

### コードコメント `/`

コードコメントにはスラッシュを使います。スラッシュ以降は最終的なレンダリング結果に表示されません。コードコメントには `/` を, html コメントには `/!` を使います。

    body
      p
        / この行は表示されません。
          この行も表示されません。
        /! html コメントとして表示されます。

  構文解析結果は以下:

    <body><p><!--html コメントとして表示されます。--></p></body>

### HTML コメント `/!`

html コメントにはスラッシュの直後にエクスクラメーションマークを使います (`<!-- ... -->`)。

### IE コンディショナルコメント `/[...]`

    /[if IE]
        p もっと良いブラウザを使ってください。

レンダリング結果

    <!--[if IE]><p>もっと良いブラウザを使ってください。</p><![endif]-->

## HTML タグ

### ドキュメントタイプタグ

ドキュメントタイプタグはとても簡単な方法で複雑なドキュメントタイプを生成するために使われる特別なタグです。

XML 宣言

    doctype xml
      <?xml version="1.0" encoding="utf-8" ?>

    doctype xml ISO-8859-1
      <?xml version="1.0" encoding="iso-8859-1" ?>

XHTML ドキュメントタイプ

    doctype html
      <!DOCTYPE html>

    doctype 5
      <!DOCTYPE html>

    doctype 1.1
      <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN"
        "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

    doctype strict
      <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
        "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

    doctype frameset
      <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN"
        "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">

    doctype mobile
      <!DOCTYPE html PUBLIC "-//WAPFORUM//DTD XHTML Mobile 1.2//EN"
        "http://www.openmobilealliance.org/tech/DTD/xhtml-mobile12.dtd">

    doctype basic
      <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML Basic 1.1//EN"
        "http://www.w3.org/TR/xhtml-basic/xhtml-basic11.dtd">

    doctype transitional
      <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
        "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

HTML 4 ドキュメントタイプ

    doctype strict
      <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN"
        "http://www.w3.org/TR/html4/strict.dtd">

    doctype frameset
      <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN"
        "http://www.w3.org/TR/html4/frameset.dtd">

    doctype transitional
      <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">

### 閉じタグ (末尾 `/`)

末尾に `/` を付けることで明示的にタグを閉じることができます。

    img src="image.png"/

(注) 標準的な html タグ (img, br, ...) は自動的にタグを閉じるので,
通常必要ありません。

### インラインタグ

時々タグをよりコンパクトにインラインにしたくなるかもしれません。

    ul
      li.first: a href="/a" A リンク
      li: a href="/b" B リンク

読みやすくするために, 属性を囲むことができるのを忘れないでください。

    ul
      li.first: a[href="/a"] A リンク
      li: a[href="/b"] B リンク

### テキストコンテンツ

タグと同じ行で開始するか

    body
      h1 id="headline" 私のサイトへようこそ。

入れ子にするのかどちらかです。エスケープ処理を行うためにはパイプかバッククォートを使わなければなりません。

    body
      h1 id="headline"
        | 私のサイトへようこそ。

### 動的コンテンツ (`=` と `==`)

同じ行で呼び出すか

    body
      h1 id="headline" = page_headline

入れ子にすることができます。

    body
      h1 id="headline"
        = page_headline

### 属性

タグの後に直接属性を書きます。属性のテキストにはダブルクォート `"` か シングルクォート `'` を使わなければなりません (引用符で囲まれた属性)。

    a href="http://slim-lang.com" title='Slim のホームページ' Slim のホームページへ

引用符で囲まれたテキストを属性として使えます。

#### 属性の囲み

区切り文字が構文を読みやすくするのであれば,
`{...}`, `(...)`, `[...]` が属性の囲みに使えます。

    body
      h1(id="logo") = page_logo
      h2[id="tagline" class="small tagline"] = page_tagline

属性を囲んだ場合, 属性を複数行にわたって書くことができます:

    h2[id="tagline"
       class="small tagline"] = page_tagline

属性の囲みや変数まわりにスペースを使うことができます:

    h1 id = "logo" = page_logo
    h2 [ id = "tagline" ] = page_tagline

#### 引用符で囲まれた属性

例:

    a href="http://slim-lang.com" title='Slim のホームページ' Slim のホームページへ

引用符で囲まれたテキストを属性として使えます:

    a href="http://#{url}" #{url} へ

属性値はデフォルトでエスケープされます。属性のエスケープを無効にしたい場合 == を使います。

    a href=="&amp;"

引用符で囲まれた属性をバックスラッシュ `\` で改行できます。

    a data-title="help" data-content="極めて長い長い長いヘルプテキストで\
      1つずつ1つずつその後はまたやり直して繰り返し...."

#### Ruby コードを用いた属性

`=` の後に直接 Ruby コードを書きます。コードにスペースが含まれる場合,
`(...)` の括弧でコードを囲まなければなりません。ハッシュを `{...}` に, 配列を `[...]` に書くこともできます。 

    body
      table
        - for user in users do
          td id="user_#{user.id}" class=user.role
            a href=user_action(user, :edit) Edit #{user.name}
            a href=(path_to_user user) = user.name

属性値はデフォルトでエスケープされます。属性のエスケープを無効にしたい場合 == を使います。

    a href==action_path(:start)

Ruby コードの属性は, コントロールセクションにあるようにバックスラッシュ `\` や `,` を用いて改行できます。

#### 真偽値属性

属性値の `true`, `false` や `nil` は真偽値として
評価されます。属性を括弧で囲む場合, 属性値の指定を省略することができます。

    input type="text" disabled="disabled"
    input type="text" disabled=true
    input(type="text" disabled)

    input type="text"
    input type="text" disabled=false
    input type="text" disabled=nil

#### 属性の結合

複数の属性が与えられた場合に属性をまとめるように設定することができます (`:merge_attrs` 参照)。デフォルト設定では
 class 属性は空白区切りで結合されます。

    a.menu class="highlight" href="http://slim-lang.com/" Slim-lang.com

レンダリング結果

    <a class="menu highlight" href="http://slim-lang.com/">Slim-lang.com</a>

また, `Array` や配列要素を属性値として区切り文字で結合し使うこともできます。

    a class=["menu","highlight"]
    a class=:menu,:highlight

#### アスタリスク属性 `*`

アスタリスクによってハッシュを属性/値のペアとして使うことができます。

    .card*{'data-url'=>place_path(place), 'data-id'=>place.id} = place.name

レンダリング結果

    <div class="card" data-id="1234" data-url="/place/1234">Slim の家</div>

次のようにハッシュを返すメソッドやインスタンス変数を使うこともできます"

    .card *method_which_returns_hash = place.name
    .card *@hash_instance_variable = place.name

属性の結合 (Slim オプション `:merge_attrs` 参照) に対応するハッシュ属性には `Array` を与えることもできます。

    .first *{:class => [:second, :third]} テキスト

レンダリング結果

    div class="first second third"

#### 動的タグ `*`

アスタリスク属性を使用することで完全に動的なタグを作ることができます。:tag をキーにもつハッシュを返すメソッドを
作るだけです。

    ruby:
      def a_unless_current
        @page_current ? {:tag => 'span'} : {:tag => 'a', :href => 'http://slim-lang.com/'}
      end
    - @page_current = true
    *a_unless_current リンク
    - @page_current = false
    *a_unless_current リンク

レンダリング結果

    <span>リンク</span><a href="http://slim-lang.com/">リンク</a>

### ショートカット

#### タグショートカット

`:shortcut` オプションを設定することで独自のタグショートカットを定義できます。

    Slim::Engine.set_default_options :shortcut => {'c' => {:tag => 'container'}, '#' => {:attr => 'id'}, '.' => {:attr => 'class'} }

Slim コードの中でこの様に使用できます。

    c.content テキスト

レンダリング結果

    <container class="content">テキスト</container>

#### 属性のショートカット

カスタムショートカットを定義することができます (`#` が id で `.` が class であるように)。

例として `&` で作った type 属性付きの input 要素のショートカットを作成し追加します。

    Slim::Engine.set_default_options :shortcut => {'&' => {:tag => 'input', :attr => 'type'}, '#' => {:attr => 'id'}, '.' => {:attr => 'class'}}

Slim コードの中でこの様に使用できます。

    &text name="user"
    &password name="pw"
    &submit

レンダリング結果

    <input type="text" name="user" />
    <input type="password" name="pw" />
    <input type="submit" />

別の例として `@` で作った role 属性のショートカットを作成し追加します。

    Slim::Engine.set_default_options :shortcut => {'@' => 'role', '#' => 'id', '.' => 'class'}

Slim コードの中でこの様に使用できます。

    .person@admin = person.name

レンダリング結果

    <div class="person" role="admin">Daniel</div>

#### ID ショートカット `#` と class ショートカット `.`

Haml と同じように, `id` と `class` の属性を次のショートカットで指定できます。

    body
      h1#headline
        = page_headline
      h2#tagline.small.tagline
        = page_tagline
      .content
        = show_content

これは次に同じです

    body
      h1 id="headline"
        = page_headline
      h2 id="tagline" class="small tagline"
        = page_tagline
      div class="content"
        = show_content

## テキストの展開

Ruby の標準的な展開方法を使用します。テキストはデフォルトで html エスケープされます。

    body
      h1 ようこそ #{current_user.name} ショーへ。
      | エスケープしない #{{content}} こともできます。

展開したテキストのエスケープ方法 (言い換えればそのままのレンダリング)

    body
      h1 ようこそ \#{current_user.name} ショーへ。

## 埋め込みエンジン (Markdown, ...)

ありがとう [Tilt](https://github.com/rtomayko/tilt), Slim は他のテンプレートエンジンの埋め込みに見事に対応します。

例:

    coffee:
      square = (x) -> x * x

    markdown:
      #Header
        #{"Markdown"} からこんにちわ!
        2行目!

対応エンジン:

<table>
<thead style="font-weight:bold"><tr><td>フィルタ</td><td>必要な gem</td><td>種類</td><td>説明</td></tr></thead>
<tbody>
<tr><td>ruby:</td><td>なし</td><td>ショートカット</td><td>Ruby コードを埋め込むショートカット</td></tr>
<tr><td>javascript:</td><td>なし</td><td>ショートカット</td><td> javascript コードを埋め込むショートカットで script タグで囲む</td></tr>
<tr><td>css:</td><td>なし</td><td>ショートカット</td><td> css コードを埋め込むショートカットで style タグで囲む</td></tr>
<tr><td>sass:</td><td>sass</td><td>コンパイル</td><td> sass コードを埋め込むショートカットで style タグで囲む</td></tr>
<tr><td>scss:</td><td>sass</td><td>コンパイル</td><td> scss コードを埋め込むショートカットで style タグで囲む</td></tr>
<tr><td>less:</td><td>less</td><td>コンパイル</td><td> less コードを埋め込むショートカットで style タグで囲む</td></tr>
<tr><td>styl:</td><td>styl</td><td>コンパイル</td><td> stylus コードを埋め込むショートカットで style タグで囲む</td></tr>
<tr><td>coffee:</td><td>coffee-script</td><td>コンパイル</td><td>コンパイルした CoffeeScript で script タグで囲む</td></tr>
<tr><td>asciidoc:</td><td>asciidoctor</td><td>コンパイル + 展開</td><td> AsciiDoc コードのコンパイルとテキスト中の #\{variables} の展開</td></tr>
<tr><td>markdown:</td><td>redcarpet/rdiscount/kramdown</td><td>コンパイル + 展開</td><td>Markdownのコンパイルとテキスト中の #\{variables} の展開</td></tr>
<tr><td>textile:</td><td>redcloth</td><td>コンパイル + 展開</td><td>textile のコンパイルとテキスト中の #\{variables} の展開</td></tr>
<tr><td>creole:</td><td>creole</td><td>コンパイル + 展開</td><td>cleole のコンパイルとテキスト中の #\{variables} の展開</td></tr>
<tr><td>wiki:, mediawiki:</td><td>wikicloth</td><td>コンパイル + 展開</td><td>wiki のコンパイルとテキスト中の #\{variables} の展開</td></tr>
<tr><td>rdoc:</td><td>rdoc</td><td>コンパイル + 展開</td><td>RDoc のコンパイルとテキスト中の #\{variables} の展開</td></tr>
<tr><td>builder:</td><td>builder</td><td>プレコンパイル</td><td>builder コードの埋め込み</td></tr>
<tr><td>nokogiri:</td><td>nokogiri</td><td>プレコンパイル</td><td>nokogiri コードの埋め込み</td></tr>
<tr><td>erb:</td><td>なし</td><td>プレコンパイル</td><td>erb コードの埋め込み</td></tr>
</tbody>
</table>

埋め込みエンジンは Slim の `Slim::Embedded` フィルタのオプションで直接設定されます。例:

    Slim::Embedded.default_options[:markdown] = {:auto_ids => false}

## Slim の設定

Slim とその基礎となる [Temple](https://github.com/judofyr/temple) は非常に柔軟に設定可能です。
Slim を設定する方法はコンパイル機構に少し依存します。(Rails や [Tilt](https://github.com/rtomayko/tilt))。デフォルトオプションの設定は `Slim::Engine` クラスでいつでも可能です。Rails の 環境設定ファイルで設定可能です。例えば, config/environments/developers.rb で設定したいとします:

### デフォルトオプション

    # デバック用に html をきれいにインデントし属性をソートしない (Ruby 1.8)
    Slim::Engine.set_default_options :pretty => true. :sort_attrs => false

    # デバック用に html をきれいにインデントし属性をソートしない (Ruby 1.9)
    Slim::Engine.set_default_options pretty: true, sort_attrs: false

ハッシュで直接オプションにアクセスすることもできます:

    Slim::Engine.default_options[:pretty] = true

### 実行時のオプション設定

実行時のオプション設定の方法は2つあります。Tilt テンプレート (`Slim::Template`) の場合, テンプレートを
インスタンス化する時にオプションを設定できます。

    Slim::Template.new('template.slim', optional_option_hash).render(scope)

他の方法は Rails に主に関係がありますがスレッド毎にオプション設定を行う方法です:

    Slim::Engine.with_options(option_hash) do
       # ここで作成される Slim エンジンは option_hash を使用します
       # Rails での使用例:
       render :page, :layout => true
    end

Rails ではコンパイルされたテンプレートエンジンのコードとオプションはテンプレート毎にキャッシュされ, 後でオプションを変更できないことに注意する必要があります。

    # 最初のレンダリング呼び出し
    Slim::Engine.with_options(:pretty => true) do
       render :page, :layout => true
    end

    # 2回目のレンダリング呼び出し
    Slim::Engine.with_options(:pretty => false) do
       render :page, :layout => true # :pretty is still true because it is cached
    end

### 可能なオプション

次のオプションが `Slim::Engine` によって用意され `Slim::Engine.set_default_options` で設定することができます。
沢山ありますが良いことに, Slim はもし誤った設定キーを使用しようとした場合キーをチェックしエラーを報告します。

<table>
<thead style="font-weight:bold"><tr><td>種類</td><td>名前</td><td>デフォルト</td><td>用途</td></tr></thead>
<tbody>
<tr><td>文字列</td><td>:file</td><td>nil</td><td>解析対象のファイル名ですが, Slim::Template によって自動的に設定されます</td></tr>
<tr><td>数値</td><td>:tabsize</td><td>4</td><td>1 タブあたりのスペース数 (構文解析で利用されます)</td></tr>
<tr><td>文字列</td><td>:encoding</td><td>"utf-8"</td><td>テンプレートのエンコーディングを設定</td></tr>
<tr><td>文字列</td><td>:default_tag</td><td>"div"</td><td>タグ名が省略されている場合デフォルトのタグとして使用される</td></tr>
<tr><td>ハッシュ</td><td>:shortcut</td><td>\{'.' => {:attr => 'class'}, '#' => {:attr => 'id'}}</td><td>属性のショートカット</td></tr>
<tr><td>配列&lt;シンボル,文字列&gt;</td><td>:enable_engines</td><td>nil <i>(すべて可)</i></td><td>有効な埋め込みエンジンリスト (ホワイトリスト)</td></tr>
<tr><td>配列&lt;シンボル,文字列&gt;</td><td>:disable_engines</td><td>nil <i>(無効なし)</i></td><td>無効な埋め込みエンジンリスト (ブラックリスト)</td></tr>
<tr><td>真偽値</td><td>:disable_capture</td><td>false (Rails では true)</td><td>ブロック内キャプチャ無効 (ブロックはデフォルトのバッファに書き込む)</td></tr>
<tr><td>真偽値</td><td>:disable_escape</td><td>false</td><td>文字列の自動エスケープ無効</td></tr>
<tr><td>真偽値</td><td>:use_html_safe</td><td>false (Rails では true)</td><td>ActiveSupport の String#html_safe? を使う (:disable_escape と一緒に機能する)</td></tr>
<tr><td>シンボル</td><td>:format</td><td>:xhtml</td><td>html の出力フォーマット (対応フォーマット :xhtml, :html4, :html5, :html)</td></tr>
<tr><td>文字列</td><td>:attr_quote</td><td>'"'</td><td>html の属性を囲む文字 (' または " が可能)</td></tr>
<tr><td>ハッシュ</td><td>:merge_attrs</td><td>\{'class' => ' '}</td><td>複数の html 属性が与えられた場合結合に使われる文字列 (例: class="class1 class2")</td></tr>
<tr><td>配列&lt;文字列&gt;</td><td>:hyphen_attrs</td><td>%w(data)</td><td>属性にハッシュが与えられた場合ハイフンつなぎされます。(例: data={a:1,b:2} は data-a="1" data-b="2" のように)</td></tr>
<tr><td>真偽値</td><td>:sort_attrs</td><td>true</td><td>名前によって属性をソート</td></tr>
<tr><td>シンボル</td><td>:js_wrapper</td><td>nil</td><td>:comment, :cdata や :both で JavaScript をラップします。:guess を指定することで :format オプションに基いて設定することもできます。</td></tr>
<tr><td>真偽値</td><td>:pretty</td><td>false</td><td>綺麗な html インデント<b>(遅くなります!)</b></td></tr>
<tr><td>文字列</td><td>:indent</td><td>'  '</td><td>インデントに使用される文字列</td></tr>
<tr><td>真偽値</td><td>:streaming</td><td>false (Rails > 3.1 では true)</td><td>ストリーミング出力の有効化</td></tr>
<tr><td>クラス</td><td>:generator</td><td>Temple::Generators::ArrayBuffer/RailsOutputBuffer</td><td>Temple コードジェネレータ (デフォルトのジェネレータは配列バッファを生成します)</td></tr>
<tr><td>文字列</td><td>:buffer</td><td>'_buf' (Rails では '@output_buffer')</td><td>バッファに使用される変数</td></tr>
</tbody>
</table>

Temple フィルタによってもっと多くのオプションがサポートされていますが一覧には載せず公式にはサポートしません。
Slim と Temple のコードを確認しなければなりません。

### オプションの優先順位と継承

Slim や Temple のアーキテクチャについてよく知っている開発者は異なる場所で設定を
上書きすることができます。 Temple はサブクラスがスーパークラスのオプションを上書きできるように
継承メカニズムを採用しています。オプションの優先順位は次のとおりです:

1. `Slim::Template` オプションはエンジン初期化時に適用されます
2. `Slim::Template.default_options`
3. `Slim::Engine.thread_options`, `Slim::Engine.default_options`
5. パーサ/フィルタ/ジェネレータ `thread_options`, `default_options` (例: `Slim::Parser`, `Slim::Compiler`)

`Temple::Engine` のようにスーパークラスのオプションを設定することも可能です。しかしこれはすべての Temple テンプレートエンジンに影響します。

    Slim::Engine < Temple::Engine
    Slim::Compiler < Temple::Filter

## プラグイン

### ロジックレスモード

<a name="logicless">ロジックレスモード</a>は [Mustache](https://github.com/defunkt/mustache) に影響されています。ロジックレスモードは辞書オブジェクトを使います。
例えば動的コンテンツを含んだハッシュツリーです。

#### 条件

オブジェクトが false または empty? ではない場合, コンテンツは表示されます。

    - article
      h1 = title

#### 反対条件

オブジェクトが false または empty? の場合, コンテンツは表示されます。

    -! article
      p Sorry, article not found

#### 繰り返し

オブジェクトが配列の場合, このセクションは繰り返されます。

    - articles
      tr: td = title

#### ラムダ

mustache のように Slim はラムダに対応します

    = person
      = name

ラムダ式はこのように定義できるでしょう

    def lambda_method
      "<div class='person'>#{yield(:name => 'Andrew')}</div>"
    end

`yield` に1つ以上のハッシュを与えることができます。複数のハッシュを与えた場合, 上記のようにブロックは繰り返されます。

#### 辞書オブジェクトの利用

コード例:

    - article
      h1 = title

辞書オブジェクトは `:dictionary_access` に与えられた順に利用するできます。デフォルト順: 

1. `:symbol` - `article.respond_to?(:has_key?)` かつ `article.has_key?(:title)` の場合, Slim は `article[:title]` を実行
2. `:string` - `article.respond_to?(:has_key?)` かつ `article.has_key?('title')` の場合, Slim は `article['title']` を実行
3. `:method` - `article.respond_to?(:title)` の場合, Slim は `article.send(:title)` を実行
4. `:instance_variable` - `article.instance_variable_defined?(@title)` の場合, Slim は `article.instance_variable_get @title` を実行

上記すべてに失敗した場合, Slim は親オブジェクトに対して同じ順序で title の参照を解決しようとします。例では親オブジェクトはテンプレートに対してレンダリングしている辞書オブジェクトでしょう。

ご想像の通り, article の参照は辞書オブジェクトに対して同じステップを実行します。インスタンス変数はビューのコード内では使用できませんが, Slim はインスタンス変数を探しだして使います。実際の所, テンプレートに @ プレフィックスをつけて使います。引数付きメソッドは使用できません。

#### Rails でロジックレス

インストール:

    $ gem install slim

読み込み:

    gem 'slim', :require => 'slim/logic_less'

いくつかのアクションに対してのみロジックレスモードを有効化したい場合には, 設定で最初にグローバルにロジックレスモードを無効化する必要があります。

    Slim::Engine.set_default_options :logic_less => false

そしてアクションの中でレンダリング毎にロジックレスモードを有効化します。

    class Controller
      def action
        Slim::Engine.with_options(:logic_less => true) do
          render
        end
      end
    end

#### Sinatra でロジックレス

Sinatra は Slim をビルトインサポートしています。ロジックレスの Slim プラグインを読み込むだけです。config.ru で読み込むことができます:

    require 'slim/logic_less'

rock する準備が整いました!

いくつかのアクションに対してのみロジックレスモードを有効化したい場合には, 設定で最初にグローバルにロジックレスモードを無効化する必要があります。

    Slim::Engine.set_default_options :logic_less => false

そしてアプリケーションの中でレンダリング毎にロジックレスモードを有効化します。

    get '/page'
      slim :page, :logic_less => true
    end

#### オプション

<table>
<thead style="font-weight:bold"><tr><td>種類</td><td>名前</td><td>デフォルト</td><td>用途</td></tr></thead>
<tbody>
<tr><td>真偽値</td><td>:logic_less</td><td>true</td><td>ロジックレスモードの有効化 ('slim/logic_less' が読み込まれた場合有効化)</td></tr>
<tr><td>文字列</td><td>:dictionary</td><td>"self"</td><td>変数が検索される辞書</td></tr>
<tr><td>シンボル/配列&lt;シンボル&gt;</td><td>:dictionary_access</td><td>[:symbol, :string, :method, :instance_variable]</td><td>辞書オブジェクトの実行順 (:symbol, :string, :method, :instance_variable)</td></tr>
</tbody>
</table>

### 多言語化/I18n

多言語化プラグインは Gettext, Fast-Gettext または Rails の i18n を使用してテンプレートの自動翻訳を提供します。
静的なテキストが翻訳されたバージョンに置き換えられます。

例:

    h1 Welcom to #{url}!

Gettext 書込みが %1, %2, ... によって書き換えられることで文字列を英語からドイツ語に翻訳します。

    "Welcome to %1!" -> "Willkommen auf %1!"

レンダリング結果

    <h1>Willkommen auf slim-lang.com!</h1>

多言語化プラグインは次のように有効化します

    require 'slim/translator'

#### オプション

<table>
<thead style="font-weight:bold"><tr><td>種類</td><td>名前</td><td>名前</td><td>用途</td></tr></thead>
<tbody>
<tr><td>真偽値</td><td>:tr</td><td>true</td><td>多言語化の有効化 ('slim/translator' が読み込まれると有効化)</td></tr>
<tr><td>シンボル</td><td>:tr_mode</td><td>:dynamic</td><td>多言語化するタイミング: :static = コンパイル時, :dynamic = 実行時</td></tr>
<tr><td>文字列</td><td>:tr_fn</td><td>インストールした多言語化ライブラリに依存</td><td>多言語化関数は gettext の場合 `_`</td></tr>
</tbody>
</table>

## フレームワークサポート

### Tilt

Slim は生成されたコードをコンパイルするために [Tilt](https://github.com/rtomayko/tilt) を使用します。Slim テンプレートを直接使いたい場合, Tilt インターフェイスが使用できます。

    Tilt.new['template.slim'].render(scope)
    Slim::Template.new('template.slim', optional_option_hash).render(scope)
    Slim::Template.new(optional_option_hash) { source }.render(scope)

optional_option_hash は前述のオプションを持つことができます。

### Sinatra

<pre>require 'sinatra'
require 'slim'

get('/') { slim :index }

 __END__
@@ index
doctype html
html
  head
    title Slim で Sinatra
  body
    h1 Slim は楽しい!
</pre>

### Rails

Rails のジェネレータは [slim-rails](https://github.com/slim-template/slim-rails) によって提供されます。
slim-rails は Rails で Slim を使用する場合に必須ではありません。Slim をインストールし Gemfile に `gem 'slim'` を追加するだけです。
後は .slim 拡張子を使えば Rails で使用できます。

#### Streaming

HTTP ストリーミングは Rails がそれをサポートしているバージョンであればデフォルトで有効化されています。

## ツール

### Slim コマンド 'slimrb'

gem の 'slim' にはコマンドラインから Slim をテストするための小さなツール 'slimrb' が付属します。

<pre>
$ slimrb --help
Usage: slimrb [options]
    -s, --stdin                      Read input from standard input instead of an input file
        --trace                      Show a full traceback on error
    -c, --compile                    Compile only but do not run
    -r, --rails                      Generate rails compatible code (Implies --compile)
    -t, --translator                 Enable translator plugin
    -l, --logic-less                 Enable logic less plugin
    -p, --pretty                     Produce pretty html
    -o, --option [NAME=CODE]         Set slim option
    -h, --help                       Show this message
    -v, --version                    Print version
</pre>

'slimrb' で起動し, コードをタイプし Ctrl-d で EOF を送ります。使い方例: 

<pre>
$ slimrb
markdown:
  最初の段落。

  2つ目の段落。

  * 1つ
  * 2つ
  * 3つ

//Enter Ctrl-d
&lt;p&gt;最初の段落。 &lt;/p&gt;

&lt;p&gt;2つめの段落。 &lt;/p&gt;

&lt;ul&gt;
&lt;li&gt;1つ&lt;/li&gt;
&lt;li&gt;2つ&lt;/li&gt;
&lt;li&gt;3つ&lt;/li&gt;
&lt;/ul&gt;
</pre>

### 構文ハイライト

様々なテキストエディタのためのプラグインがあります。(最も重要なものも含めて - Vim, Emacs や Textmate):

* [Vim](https://github.com/slim-template/vim-slim)
* [Emacs](https://github.com/slim-template/emacs-slim)
* [Textmate / Sublime Text](https://github.com/slim-template/ruby-slim.tmbundle)
* [Espresso text editor](https://github.com/slim-template/Slim-Sugar)
* [Coda](https://github.com/slim-template/Coda-2-Slim.mode)

### テンプレート変換 (HAML, ERB, ...)

* [Haml2Slim converter](https://github.com/slim-template/haml2slim)
* [HTML2Slim converter](https://github.com/slim-template/html2slim)

## テスト

### ベンチマーク

  *そうです, Slim は最速の Ruby のテンプレートエンジンです!
   production モードの Slim は Erubis (最速のテンプレートエンジン) と同じくらい高速です。
   ありがたいことに何らかの理由であなたが Slim を選択した場合, 私たちは
   パフォーマンスが障害にならないだろうことを保証します。*

ベンチマークは `rake bench` で実行します。時間が余計にかかりますが遅い解析ベンチマークを
実行したい場合 `slow` オプションを追加できます。

    rake bench slow=1 iterations=1000

私たちはコミット毎に Travis-CI でベンチマークをとっています。最新のベンチマーク結果はリンク先を確認: <http://travis-ci.org/#!/slim-template/slim>

### テストスイートと継続的インテグレーション

Slim は minitest ベースの拡張性のあるテストスイートを提供します。テストは 'rake test' または
rails のインテグレーションテストの場合 'rake test:rails' で実行できます。

私たちは現在 markdown ファイルで書かれた人間が読めるテストを試しています: [TESTS.md](test/literate/TESTS.md)

Travis-CI は継続的インテグレーションテストに利用されています: <http://travis-ci.org/#!/slim-template/slim>

Slim はすべての主要な Ruby 実装で動作します:

* Ruby 1.8.7
* Ruby 1.9.2
* Ruby 1.9.3
* Ruby EE
* JRuby
* Rubinius 2.0

## 貢献

Slim の改良を支援したい場合, Git で管理されているプロジェクトを clone してください。

    $ git clone git://github.com/slim-template/slim

魔法をかけた後 pull request を送ってください。私たちは pull request が大好きです！

Ruby の 1.9.2 と 1.8.7 でテストをすることを覚えておいてください。

もしドキュメントの不足を見つけたら (きっと見つけるでしょう), README.md をアップデートして私たちを助けて下さい。Slim に割く時間がないが, 私たちが知るべきものを何か見つけた場合には issue を送ってください。

## License

Slim は [MIT license](http://www.opensource.org/licenses/MIT) に基づいてリリースされています。

## 作者

* [Daniel Mendler](https://github.com/minad) (Lead developer)
* [Andrew Stone](https://github.com/stonean)
* [Fred Wu](https://github.com/fredwu)

## 議論

* [Google Group](http://groups.google.com/group/slim-template)

## 関連プロジェクト

テンプレートのコンパイルフレームワーク:

* [Temple](https://github.com/judofyr/temple)

フレームワークサポート:

* [Rails 3 generators (slim-rails)](https://github.com/slim-template/slim-rails)

構文ハイライト:

* [Vim](https://github.com/slim-template/vim-slim)
* [Emacs](https://github.com/slim-template/emacs-slim)
* [Textmate / Sublime Text](https://github.com/slim-template/ruby-slim.tmbundle)
* [Espresso text editor](https://github.com/slim-template/Slim-Sugar)
* [Coda](https://github.com/slim-template/Coda-2-Slim.mode)

テンプレート変換 (HAML, ERB, ...):

* [Haml2Slim converter](https://github.com/slim-template/haml2slim)
* [HTML2Slim converter](https://github.com/slim-template/html2slim)

移植言語/同様の言語:

* [Coffee script plugin for Slim](https://github.com/yury/coffee-views)
* [Clojure port of Slim](https://github.com/chaslemley/slim.clj)
* [Hamlet.rb (Similar template language)](https://github.com/gregwebs/hamlet.rb)
* [Plim (Python port of Slim)](https://github.com/2nd/plim)
* [Skim (Slim for Javascript)](https://github.com/jfirebaugh/skim)
* [Haml (Older engine which inspired Slim)](https://github.com/haml/haml)
* [Jade (Similar engine for javascript)](https://github.com/visionmedia/jade)
