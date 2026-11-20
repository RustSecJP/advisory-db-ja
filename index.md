# RustSec Advisory Database の文書（advisory-db README・CONTRIBUTING 日本語訳）

RustSec の助言データベース（[rustsec/advisory-db](https://github.com/rustsec/advisory-db)）の README と、助言を出すための手順書の、非公式な日本語訳です。**このサイトは助言データベースそのものではありません。** 助言の本体は [rustsec.org](https://rustsec.org/advisories/) にあります。

> **ご注意**: RustSecJP は RustSec とは提携していません。この翻訳は RustSec の確認や公認を受けたものではありません。実際に助言を出すときは、必ず最新の原文を確かめてください。助言そのものは英語で書きます。

## 文書の一覧

{% for c in site.data.book.chapters %}
{{ forloop.index }}. [{{ c.title }}]({{ '/' | append: c.file | relative_url }})
{% endfor %}

## 出典

- 原文: [rustsec/advisory-db](https://github.com/rustsec/advisory-db) の `README.md`、`CONTRIBUTING.md`、`EXAMPLE_ADVISORY.md`、`HOWTO_UNMAINTAINED.md`
- 原著者: RustSec（Rust Secure Code WG）
- ライセンス: [CC0 1.0](https://github.com/rustsec/advisory-db/blob/main/LICENSE.txt)
- 訳した版: 2026-11-19 時点（コミット `074ea9a`）
