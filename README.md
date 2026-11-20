# RustSec Advisory Database

[![原文][original-image]][original-link]
![日本語訳: 非公式][unofficial-image]
[![Project Chat][chat-image]][chat-link]

> [!NOTE]
> これは [rustsec/advisory-db](https://github.com/rustsec/advisory-db) の README と報告手順書の**非公式な日本語訳**です。RustSec による確認や公認を受けたものではありません。**このリポジトリは助言データベースではなく、RUSTSEC の助言 ID も発行しません。** 助言の本体は [rustsec.org](https://rustsec.org/advisories/) にあり、脆弱性の報告は本家へ行ってください。
> This is an unofficial Japanese translation of the docs. It is not an advisory database.

RustSec Advisory Database は、<https://crates.io> で公開されている Rust のクレートに対して出された、セキュリティ助言（アドバイザリ）のリポジトリです。人が読みやすい形の助言データベースは、<https://rustsec.org/advisories/> にあります。

データは [OSV](https://github.com/ossf/osv-schema) 形式にも[エクスポート](https://github.com/rustsec/advisory-db/tree/osv)されています。すべてのデータは [osv.dev](https://osv.dev/list?ecosystem=crates.io&q=) と、その [API](https://osv.dev/#use-the-api) から利用できます。

[GitHub Advisory Database](https://github.com/advisories/) は、RustSec の助言を取り込んでいます。

次のツールはこの助言データベースを利用しており、監査と報告に使えます（追加したいツールがあれば、原文のリポジトリに PR を送ってください）。

* [cargo-audit]: `Cargo.lock` ファイルを監査し、セキュリティ上の脆弱性があるクレートを調べます。
* [cargo-deny]: `Cargo.lock` ファイルを監査し、セキュリティ上の脆弱性があるクレートを調べます。特定の依存の利用、そのライセンス、ダウンロード元の制限や、依存ツリー内にある同じパッケージの複数バージョンの検出などもできます。
* [trivy]: コンテナなどの成果物を対象にした、簡単で網羅的な、脆弱性・設定ミス・シークレットのスキャナです。Trivy は、OS のパッケージと言語ごとのパッケージの脆弱性を検出します。[OSV](https://osv.dev) を経由して動作します。
* [dependabot]: Dependabot は、セキュリティ更新を含む Pull Request を出して、脆弱な依存を直してくれます。[GHSA](https://github.com/advisories) を経由して動作します。

## 助言を作る

脆弱性を見つけたら、上流（そのクレートの開発元）のセキュリティポリシーに従って報告してください。その後、下のテンプレートを使って Pull Request を開くことで、助言を作成できます。詳しくは[助言を作る](https://rustsecjp.github.io/advisory-db-ja/how-to-report.html)を見てください。

保守されていないクレートについて助言を出す前に、[保守されていない crate の助言](https://rustsecjp.github.io/advisory-db-ja/unmaintained.html)を見てください。

<a href="https://rustsecjp.github.io/advisory-db-ja/how-to-report.html">
  <img alt="脆弱性を報告する" width="250px" height="60px" src="https://rustsec.org/img/report-vuln-button.svg">
</a>

## 助言の書式

テンプレートは[助言の例](https://rustsecjp.github.io/advisory-db-ja/example-advisory.html)を見てください。

助言は、[TOML] の「フロントマター」が付いた [Markdown] で書きます。

助言の [TOML]「フロントマター」の部分のスキーマは、次のとおりです。コメントは日本語に訳してあります。実際に提出する助言は英語で書きます。

```toml
# このテンプレートを使って PR を提出する前に、各フィールドを説明するコメントと、
# 使っていないフィールドを **削除してください**。

[advisory]
# 助言の識別子（必須）。"RUSTSEC-YYYY-NNNN" という識別子が割り当てられます
# （例: RUSTSEC-2018-0001）。PR では "RUSTSEC-0000-0000" を使ってください。
id = "RUSTSEC-0000-0000"

# 影響を受けるクレートの名前（必須）
package = "mycrate"

# 助言の開示日。RFC 3339 の日付（必須）
date = "2021-01-31"

# 助言が取り下げられたかどうか（任意）
#withdrawn = "YYYY-MM-DD"

# この問題の詳しい説明への URL。たとえば GitHub の issue や PR、変更履歴の項目、
# リリースを告知するブログ記事など（任意。ただし、帰属の表示を求めるライセンスを
# 使う助言では必須）。
url = "https://github.com/mystuff/mycrate/issues/123"

# 助言に関する、役に立つ追加の参考資料への URL（任意）
#references = ["https://github.com/mystuff/mycrate/discussions/1"]

# 任意: 情報提供のセキュリティ助言の種類を示す
#  - "unsound": 健全性（soundness）の問題
#  - "unmaintained": 保守されなくなったクレート
#  - "notice": そのほかの情報提供
#informational = "unmaintained"

# 任意: この助言が当てはまる分類。
#
# <https://docs.rs/rustsec/latest/rustsec/advisory/enum.Category.html> を参照。
categories = ["crypto-failure"]

# 任意: CVSS（共通脆弱性評価システム）のスコア。詳しくは CVSS の
# ウェブサイト https://www.first.org/cvss/ を参照。
#cvss = "CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H"

# この脆弱性を説明する自由形式のキーワード。Cargo のものと同様（任意）
keywords = ["ssl", "mitm"]

# 脆弱性の別名。CVE の ID など（任意だが推奨）
#aliases = ["CVE-2018-XXXX"]

# 関連する脆弱性（任意）
# 例: -sys クレートが包んでいる C ライブラリの CVE
#related = ["CVE-2018-YYYY", "CVE-2018-ZZZZ"]

# 任意: 助言のライセンス。SPDX の識別子で書く。既定は "CC0-1.0"。
# 使える値は "CC0-1.0" と "CC-BY-4.0"。
# "CC-BY-4.0" を使う場合は、`url` フィールドに元の助言へのリンクを
# 入れなければならない。これは、GitHub Advisory Database（"GHSA"）から
# 取り込んだ助言にだけ使うこと。
#license = "CC-BY-4.0"

# 任意: この助言が影響する範囲を絞り込むメタデータ
[affected]
# この脆弱性の影響を受ける CPU アーキテクチャ（任意）。
# 脆弱性が特定の CPU アーキテクチャに固有の場合にだけ使う。
# 例: 脆弱性が x86 のアセンブリにある場合。
# CPU アーキテクチャを表す文字列の一覧は、"platforms" クレートを参照:
# <https://docs.rs/platforms/latest/platforms/target/enum.Arch.html>
#arch = ["x86", "x86_64"]

# この脆弱性の影響を受けるオペレーティングシステム（任意）
# 脆弱性が特定の OS に固有の場合にだけ使う。例: Windows 固有の API への
# バインディングにあった場合。
# OS を表す文字列の一覧は、"platforms" クレートを参照:
# <https://docs.rs/platforms/latest/platforms/target/enum.OS.html>
#os = ["windows"]

# 脆弱な関数の正規のパスの表（任意）。
# この助言の影響を受けるバージョンのうち、どのバージョンがその名前を
# 使っていたかに対応づける（例: バージョンの間で関数の名前が変わった場合）。
# パスの書き方は `cratename::path::to::function`。引数などの追加情報は付けず、
# その後にバージョン要求のリストを続ける。
[affected.functions]
"mycrate::MyType::vulnerable_function" = ["< 1.2.0, >= 1.1.0"]

# この脆弱性の修正を含むバージョン（必須）
# Cargo が対応しているセレクタは、すべてここで使える:
# https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html
# 修正が無い場合（保守されていないクレートなど）は、patched = [] とする
[versions]
patched = [">= 1.2.0"]

# 一度も脆弱でなかったバージョン（任意）
#unaffected = ["< 1.1.0"]
```

上の [TOML]「フロントマター」の後に、[Markdown] 形式の詳しい説明が続きます。

## 訳した版

| | |
|---|---|
| 原文の版 | 2026-11-19 時点（コミット [`074ea9a`](https://github.com/rustsec/advisory-db/commit/074ea9a7ed)） |
| 翻訳日 | 2026-11-20 |

原文がこの版より後に更新されている場合、訳は古い内容のままです。

## ライセンス

原文のリポジトリのすべての内容は、別に定めのある場合を除き、パブリックドメインに置かれています。

[![Public Domain](http://i.creativecommons.org/p/zero/1.0/88x31.png)](https://github.com/RustSec/advisory-db/blob/main/LICENSES/CC0-1.0.txt)

例外は、[GitHub Advisory Database](https://docs.github.com/en/site-policy/github-terms/github-terms-for-additional-products-and-features#advisory-database) から取り込まれた助言で、CC-BY 4.0 ライセンスの下に置かれています。これらの助言には、ライセンスを明示する `license` フィールドと、適切な帰属表示のために元の助言を指す `url` フィールドが含まれます。

[![Creative Commons Attribution](https://licensebuttons.net/l/by/4.0/88x31.png)](https://github.com/RustSec/advisory-db/blob/main/LICENSES/CC-BY-4.0.txt)

[Markdown]: https://www.markdownguide.org/
[TOML]: https://github.com/toml-lang/toml
[cargo-audit]: https://github.com/rustsec/cargo-audit
[cargo-deny]: https://github.com/EmbarkStudios/cargo-deny
[trivy]: https://aquasecurity.github.io/trivy/
[dependabot]: https://docs.github.com/en/code-security/dependabot/dependabot-security-updates/about-dependabot-security-updates

[//]: # (badges)

[original-image]: https://img.shields.io/badge/原文-rustsec%2Fadvisory--db-blue.svg
[original-link]: https://github.com/rustsec/advisory-db
[unofficial-image]: https://img.shields.io/badge/日本語訳-非公式-orange.svg
[chat-image]: https://img.shields.io/badge/zulip-join_chat-blue.svg
[chat-link]: https://rust-lang.zulipchat.com/#narrow/stream/146229-wg-secure-code/
