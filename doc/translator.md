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

| Type | Name | Default | Purpose |
| ---- | ---- | ------- | ------- |
| 真偽値 | :tr | true | 多言語化の有効化 ('slim/translator' が読み込まれると有効化) |
| シンボル | :tr_mode | :dynamic | 多言語化するタイミング: :static = コンパイル時, :dynamic = 実行時 |
| 文字列 | :tr_fn | インストールした多言語化ライブラリに依存 | 多言語化関数は gettext の場合 `_` |
