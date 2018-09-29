# A Bad Singly-Linked Stack

<!-- This one's gonna be *by far* the longest, as we need to introduce basically -->
<!-- all of Rust, and are gonna build up some things "the hard way" to better -->
<!-- understand the language. -->

この章ではRustのおおむね全ての機能を紹介する必要があるので、他よりも *はるかに*
長くなります。この章であなたは言語をより深く理解するための「難しい方法」でそれを
習得します。

<!-- We'll put our first list in `src/first.rs`. We need to tell Rust that `first.rs` is -->
<!-- something that our lib uses. All that requires is that we put this at the top of -->
<!-- `src/lib.rs` (which Cargo made for us): -->

我々の最初のリストを `src/first.rs` に書きます。我々はRustに `first.rs` がlibで
使われることを教える必要があります。そのためには以下の行を(Cargoが作ってくれた)
`src/lib.rs` の一番上に置きます:

```rust
// in lib.rs
pub mod first;
```

