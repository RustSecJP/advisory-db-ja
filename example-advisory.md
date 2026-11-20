# 助言の例

RustSec の助言は、TOML のデータと、その後に続く Markdown の説明からなる 1 つのファイルです。以下は原文のテンプレートで、説明の部分だけを日本語に訳しています。実際に提出する助言は英語で書きます。

```toml
[advisory]
id = "RUSTSEC-0000-0000"
package = "crate-name"
date = "2020-01-31"
#withdrawn = "YYYY-MM-DD"
url = "https://example.com"
#references = ["https://github.com/mystuff/mycrate/discussions/1"]
#informational = "unmaintained"
# See https://docs.rs/rustsec/latest/rustsec/advisory/enum.Category.html
categories = ["code-execution", "privilege-escalation"]
#cvss = "CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H"
keywords = ["example", "freeform", "keywords"]
#aliases = ["CVE-YYYY-NNNN"]
#related = []
#license = "CC0-1.0"

[affected]
#arch = ["x86"]
#os = ["windows"]

#[affected.functions]
# NB: change hyphens in the crate name to underscores, i.e. "crate-name" -> "crate_name"
#"crate_name::MyStruct::vulnerable_fn" = [">= 1.3.0, < 1.3.4"]

[versions]
patched = [">= 1.2.3, < 1.3.0", ">= 1.3.4"]
unaffected = ["<= 0.1.2"]
```

## RustSec 助言テンプレート - ここに助言の題名を書く

これは RustSec の助言のテンプレートの例です。これを `crates/<crate の名前>` にコピーし、`RUSTSEC-0000-0000.md` という名前に変えてください。

助言のこの部分には、脆弱性の詳しい説明を書けます。説明は HTML に変換され、<https://rustsec.org> に表示されます。

- Markdown の書式
- TOML の「フロントマター」。スキーマは `README.md` を参照
- 詳細は好きなだけ含めてください

よく構成された助言には、次のような情報が含まれます。

この crate の影響を受ける版は、`MyBuffer::with_capacity()` でバッファを確保するときに、整数オーバーフローを正しく検査していませんでした（バグの説明・場所・根本原因）。

その結果、大きな整数が引数に与えられたとき（発生の条件）、メモリ破壊が起こりえます（バグの帰結）。

この欠陥は、コミット abc123 で、バッファのサイズを計算するときに `saturating_mul()` を使うことで修正されました（修正の説明）。
