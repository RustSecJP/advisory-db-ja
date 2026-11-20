# 助言（アドバイザリ）を作る

RustSec のデータベースに助言を追加するには、[このリポジトリ](https://github.com/RustSec/advisory-db)に対して、新しい助言を含む [Pull Request] を開きます。

### 前提となる手順

1. 脆弱性を、上流（その crate の開発元）のセキュリティポリシーに従って報告します。RustSec のメンテナは、すべての crate の内部に詳しいわけではありません。crate のメンテナのほうが、ずっと効率よく対応できます。悪意のある crate の疑いがある場合は、代わりに [crates.io へ報告](https://crates.io/policies/security)してください。
2. メンテナが、その脆弱性が実在すると確認するのを待ちます。
3. （該当する場合は）協調的な開示を行い、対応する側の準備が整う前に脆弱性が公になることのないようにします。

crate の作者自身が助言を出す場合や、上流のリポジトリがすでに存在せず作者にも連絡が取れない場合は、上の手順を省略できます。

RustSec はこれまで、リポジトリの issue トラッカーで公に開示してから 2 週間たっても返答が無い場合にも、助言を受け付けてきました。ただし、最初の一歩としては、協調的な（非公開の）開示がつねに望まれます。

### 必須の手順

4. リポジトリの `crates/<crate の名前>` サブディレクトリに、`RUSTSEC-0000-0000.md` という名前のファイルを作ります（ディレクトリが無ければ作ります）。
5. このリポジトリの README.md にある [TOML の助言テンプレート][TOML advisory template]をコピーして貼り付けます。コメントと余分な空白を消し、助言の詳細を埋めます。TOML のデータは <code>\```toml</code> と <code>\```</code> の印で囲みます。
6. 同じファイルの中で、<code>\```</code> の印と改行の後に、人が読むための説明を Markdown で書きます。[この助言の例][example]を参考にしてください。
7. [Pull Request] を開きます。レビューの後、助言に `RUSTSEC-*` の識別子が割り当てられ、データベースに公開されます。

### 任意の手順

次のどちらか、または両方を、必要に応じて行ってください（両方を勧めます）。

8. 影響を受ける版を [yank][Yank]（取り下げ）します。
9. 脆弱性に CVE を申請します。詳しくは <https://cve.mitre.org/cve/request_id.html> と <https://cveform.mitre.org> を見てください。代わりに、GitHub Security Advisory（GHSA）を作成して、GitHub に CVE を申請してもらうこともできます。その場合は、RustSec の助言の `aliases` フィールドに GHSA の ID を追加できます。

## 基準

RustSec は、セキュリティ上の脆弱性のデータベースです。対象となる脆弱性の例は次のとおりです。

* コード実行（RCE など）
* メモリ破壊
* 権限昇格（OS のレベル、またはアプリやライブラリの内部）
* ファイルの漏えい、ディレクトリトラバーサル
* Web のセキュリティ（XSS、CSRF など）
* 書式のインジェクション。シェルのエスケープ、SQL インジェクションなど（XSS も含む）
* 暗号の失敗（機密性の破れ、完全性の破れ、鍵の漏えいなど）
* 隠れチャネル（Spectre、Meltdown など）
* 「パニックしない」とうたうコードでのパニック（とくに、ネットワーク越しの DoS 攻撃に使える場合）

加えて RustSec は、[健全性][soundness]（soundness）の問題も、脆弱性であるかどうかにかかわらず、情報提供の助言として追跡しています。健全性の問題とは、安全なコードから crate を使ったときに[未定義動作][Undefined Behavior]が起こりうる状態のことです。

[soundness]: https://rust-lang.github.io/unsafe-code-guidelines/glossary.html#soundness-of-code--of-a-library
[Undefined Behavior]: https://doc.rust-lang.org/reference/behavior-considered-undefined.html

迷ったときは、PR を開いてください。

## よくある質問

**Q: 助言を出すには、その crate の所有者である必要がありますか。**

A: いいえ。誰でも、どの crate に対しても助言を出せます。ただし、メンテナがその脆弱性の正しさを確認した後に限るべきです。後になって脆弱性が偽りだと分かった場合は、データベースから削除できます。

**Q: Pull Request を作らずに助言を出せますか。**

A: はい。助言の全体を自分で作る代わりに、[advisory-db リポジトリで issue を開く](https://github.com/RustSec/advisory-db/issues)こともできます。

**Q: このプロジェクトには、公開が差し止められている（エンバーゴ中の）脆弱性を扱うための GPG 鍵などの手段がありますか。**

A: エンバーゴ中の脆弱性は扱いません。RustSec に出す前に、エンバーゴが解除され、詳細が公に開示されていることを確かめてください。

**Q: `rustc` の脆弱性は、ここに報告するのですか。**

A: いいえ。Rust の公式プロジェクトについては、[Rust のセキュリティポリシー](https://www.rust-lang.org/policies/security)を見て、その指針に従ってください。

**Q: crates.io にある、意図的に悪意のあるコードやマルウェアは、ここに報告するのですか。**

A: いいえ。crates.io のポリシーに違反する内容を取り下げてもらうには、[crates.io のセキュリティポリシー](https://crates.io/policies/security)を見てください。

**Q: 私は crate の作者で、自分の crate の脆弱性の報告を受けました。助けてもらえますか。**

A: Rust Foundation には、Rust のエコシステムのセキュリティ問題への対応を助ける資源があります。crates.io のセキュリティポリシーの [crate の作者向けのエコシステムセキュリティ支援](https://crates.io/policies/security#ecosystem-security-help)の節を見てください。

[Pull Request]: https://github.com/RustSec/advisory-db/pulls
[TOML advisory template]: https://github.com/RustSec/advisory-db#advisory-format
[Yank]: https://doc.rust-lang.org/cargo/commands/cargo-yank.html
[example]: example-advisory.md
