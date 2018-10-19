# 所有権入門

<!-- Now that we can construct a list, it'd be nice to be able to *do* something -->
<!-- with it. We do that with "normal" (non-static) methods. Methods are a special -->
<!-- case of function in Rust because of  the `self` argument, which doesn't have -->
<!-- a declared type: -->
リストが構築できるようになったからには、それを使ってなんか*できる*ようになれば嬉しいです。
そのためには「普通の」（非staticな）メソッドを使います。メソッドというのはRustにおいては特殊な関数で、
宣言された型を持たないselfという特殊な引数を取ります。

```rust,ignore
fn foo(self, arg2: Type2) -> ReturnType {
    // body
}
```

<!-- There are 3 primary forms that self can take: `self`, `&mut self`, and `&self`. -->
<!-- These 3 forms represent the three primary forms of ownership in Rust: -->
selfは主に`self`・`&mut self`・`&self`という3種類を取ります。この3つの形は
Rustにおける主だった3種の所有権を表現します。

<!-- * `self` - Value -->
<!-- * `&mut self` - mutable reference -->
<!-- * `&self` - shared reference -->
* `self` - 値
* `&mut self` - ミュータブル(可変)な参照
* `&self` - 共有参照
<!-- 普通「イミュータブルな参照」と呼ばれているっぽくて困っている -->
<!-- 「共有参照」と訳しているteratailを見つけたのでとりあえず採用 -->

<!-- A value represents *true* ownership. You can do whatever you want with a value: -->
<!-- move it, destroy it, mutate it, or loan it out via a reference. When you pass -->
<!-- something by value, it's *moved* to the new location. The new location now -->
<!-- owns the value, and the old location can no longer access it. For this reason -->
<!-- most methods don't want `self` -- it would be pretty lame if trying to work with -->
<!-- a list made it go away! -->
値というのは、*真に*所有権を持っているということを表現します。値に対しては好き勝手なことをして構いません：
move<!-- technical termな気がしてる -->するもよし、破壊するもよし、変更するもよし、
参照を使って貸し出すもよし。何かを値で渡したとき、その値は新たな場所に*move*されます。
その新たな場所が値の所有者になり、元の場所からはもはやアクセスできなくなります。そのため、
ほとんどのメソッドでは`self`を使いたくありません――リストを扱おうとすることによってリストがどこかに
行ってしまったらめっちゃおもんない<!--かなり困ってググったらこれが出てきた。これよりうまい訳語が思いつかない。-->
ですからね。

<!-- A mutable reference represents temporary *exclusive access* to a value that you -->
<!-- don't own. You're allowed to do absolutely anything you want to a value you -->
<!-- have a mutable reference to as long as when your loan expires, wherever you -->
<!-- loaned it from still sees a valid value. This means you can actually completely -->
<!-- overwrite the value. A really useful special case of this is *swapping* a value -->
<!-- out for another, which we'll be using a lot. The only thing you can't do with an -->
<!-- `&mut` is move the value out with no replacement. `&mut self` is great for -->
<!-- methods that want to mutate `self`. -->
ミュータブルな参照は所有していない値への一時的な*排他的アクセス*を表します。
借用の期限が切れるタイミングで借用元に見えている値が有効なものであるという条件さえ守れば、
ミュータブルな参照を持っている値に対しては好き勝手なことを無条件になんでもすることが許されています。
これはつまりなんと値を完全に上書きしてもよいということです。非常に有用な特殊例として、値を別の値と*交換する*
というのがあり、<!-- swap out の outを上手く訳出したい -->これは今後たくさん使っていきます。
`&mut`に対してできない唯一のことは、代替の値を与えずに値をmove out<!-- これどうしよう -->することです。
`self`を変更したいメソッドには`&mut self`が適しています。

<!-- A shared reference represents temporary *shared access* to a value that you -->
<!-- don't own. Because you have shared access, you're generally not allowed to -->
<!-- mutate anything. Think of `&` as putting the value out on display in a museum. -->
<!-- `&` is great for methods that only want to observe `self`. -->
共有参照は所有していない値への一時的な*共用のアクセス*を表します。アクセス権が共有されているので、
一般的に値への如何なる変更も許されていません。`&`というのを、美術館のディスプレーに値を置くみたいなものだと
思うとよいでしょう。`self`を見るだけのメソッドには`&`が適しています。

<!-- Later we'll see that the rule about mutation can be bypassed in certain cases. -->
<!-- This is why shared references aren't called *immutable* references. Really, -->
<!-- mutable references could be called *unique* references, but we've found that -->
<!-- relating ownership to mutability gives the right intuition 99% of the time. -->
後に、特定の状況下ではこの値の変更に関するルールを無視する方法があるということを学んでいきます。
共有参照が*イミュータブル(不変)*な参照と呼ばれない（訳注：まあ普通に呼ばれている気がしますが）
のはそのためです。本当は、ミュータブルな参照は*一意的*参照と呼ぶこともできるでしょうが、
所有権を値の変更可能性と関連付けることで99%の場合において正しい直感を得られるということが知られています。
