---
title: "6章"
free: false
---

## 文字列リテラル
rustにおける文字列は常にUnicode
文字列リテラルの型は`&'static str`
  - `mut`がついていないのでコンパイラが変更することができない
  - `static`なのでプログラムの終了まで解放されない
`str`は常に有効なutf-8であるバイト列
&nbsp;
utf8は可変長バイトなのでいろんな文字により対応できる様になってるし 、メモリも効率的
  - でも文字のバイトの長さがそれぞれ異なるので探索に弱いらしい、(4文字目の文字を探してみたいな)

### エスケープ文字
C言語と共通らしいので省略ということで
https://doc.rust-lang.org/reference/tokens.html

デフォルトに複数行に対応していて便利
改行したくない時は行の最後に`/`

```Rust
fn main() {
    let haiku: &'static str = "
        書いてみたり
        けしたり果ては
        けしの花
        - 立花北枝";
    println!("{}", haiku);
    
    
    println!("こんにちは \
    世界") // 世界の前にある間隔は無視されます
}

```

`r#"`で始まり`"#`で終わる文字列はエスケープしない

### 文字列スライス
文字列`&str`は`str`のスライスとして扱う

**&str の一般的なメソッド**
- `len`は文字列リテラルの長さをバイト単位で取得する。（文字数ではない）
- 基本的なテストのための`starts_with` / `ends_with`
- `is_empty`は長さがゼロの時に`true`を返す。
- `find`はテキストの最初の位置の`Option<usize>`を返す。
&nbsp;
Unicodeだと探索が面倒なのでRustではutf-8バイトのシーケンスを`char`型の文字のベクトルとして取得する方法が提供されているらしい。
&nbsp;
`char`は常に 4 バイトの長さ。(これによって個々の文字を効率的に検索できるようになっているらしい)

```Rust
fn main() {
    // 文字をcharのベクトルとして集める
    let chars = "hi 🦀".chars().collect::<Vec<char>>();
    println!("{}", chars.len()); // should be 4
    // chars は 4 バイトなので、u32 に変換することができる
    println!("{}", chars[3] as u32);
```

## String
**Stringの主な共通メソッド**

- `ush_str`文字列の最後にutf-8バイトを追加します。
- replace utf-8バイト列を他のutf-8バイト列で置換します。
- `to_lowercase` / `to_uppercase`大文字や小文字に変えます。
- `trim` 空白を切り取ります。
&nbsp;
Rustの`+`演算子で文字列を扱うのはイマイチらしい
```Rust
let a = String::from("Hello, ");
let b = "world!";
let c = a + b;

println!("{}", a); // ❌ エラー：a は move されて使えない
```
&nbsp;
`+`演算子で文字列を扱う際には
左辺が`String`で右辺が文字列`&str`じゃないと使えないらしい
```Rust
let a = String::from("Hello, ");
let b = String::from("world!");
let c = a + b; // ❌ エラー：右も String だから！
```
&nbsp;
`format!`マクロの方が便利
```Rust
let a = String::from("Hello, ");
let b = String::from("world!");
let c = format!("{}{}", a, b);  // 所有権も移動しないし、直感的！
```
&nbsp;
文字列リテラルや文字列は文字列スライス`&str`として渡される、不変な借用なので所有権の移動などはあまり考慮しなくていいらしい
&nbsp;

**文字列のその他マクロ例**

- `concat`
  - そのまま足すやつ

- `join`
  - `,`で繋ぐやつ

- `format`は普通にパラメータをプレースホルダに入れるやつ
```Rust
let a = 42;
    let f = format!("secret to life: {}",a);
```

- `.to_string`を用いて型変換

- `let b = a_string.parse::<i32>()?;`
  - `parse`を用いると型付きの値に変換できる、失敗する可能性があるので`Result`を返す