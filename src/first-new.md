# New

To associate actual code with a type, we use `impl` blocks:

```rust
impl List {
    // TODO, make code happen
}
```

Now we just need to figure out how to actually write code. In Rust we declare
a function like so:

```rust
fn foo(arg1: Type1, arg2: Type2) -> ReturnType {
    // body
}
```

The first thing we want is a way to *construct* a list. Since we hide the
implementation details, we need to provide that as a function. The usual way
to do that in Rust is to provide a static method, which is just a
normal function inside an `impl`:

```rust
impl List {
    pub fn new() -> Self {
        List { head: Link::Empty }
    }
}
```

A few notes on this:

<!-- * Self is an alias for "that type I wrote at top next to `impl`". Great for -->
<!--   not repeating yourself! -->
<!-- * We create an instance of a struct in much the same way we declare it, except -->
<!--   instead of providing the types of its fields, we initialize them with values. -->
<!-- * We refer to variants of an enum using `::`, which is the namespacing operator. -->
<!-- * The last expression of a function is implicitly returned. -->
<!--   This makes simple functions a little neater. You can still use `return` -->
<!--   to return early like other C-like languages. -->
* `Self`は「先頭で`impl`の隣に書いた例の型」を表すための別名です。Great for
  not repeating yourself!
* 構造体のインスタンスを作る方法は宣言する方法とだいたい同じですが、フィールドの型を書くのではなく、
  値を書いてそれで初期化します。
* enumのヴァリアントは名前空間演算子`::`を使って参照します。
* 関数の最後の式は暗黙にreturnされます。これにより単純な関数がちょっときれいに書けるようになります。
  他のCっぽい言語と同様、`return`を使って関数の途中でreturnすることもできます。























