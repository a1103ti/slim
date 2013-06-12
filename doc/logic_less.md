### ロジックレスモード

ロジックレスモードは [Mustache](https://github.com/defunkt/mustache) に影響されています。ロジックレスモードは辞書オブジェクトを使います。
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


#### 文字列

`self` キーワードは展開中の要素の `.to_s` の値を返します。

次を与え, 

    {
      :article => [
        'Article 1',
        'Article 2'
      ]
    }

さらに次のように書くと, 

    - article
      tr: td = self

このように展開されます。

    <tr>
      <td>Article 1</td>
    <tr/>
    <tr>
      <td>Article 2</td>
    </tr>


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

| 種類 | 名前 | デフォルト | 用途 |
| ---- | ---- | ------- | ------- |
| 真偽値 | :logic_less | true | ロジックレスモードの有効化 ('slim/logic_less' が読み込まれた場合有効化) |
| 文字列 | :dictionary | "self" | 変数が検索される辞書 |
| シンボル/配列&lt;シンボル&gt; | :dictionary_access | [:symbol, :string, :method, :instance_variable] | 辞書オブジェクトの実行順 (:symbol, :string, :method, :instance_variable) |
