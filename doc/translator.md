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
