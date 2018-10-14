# Learning Rust With Entirely Too Many Linked Lists

<!-- Got any issues or want to check out all the final code at once?  -->
<!-- [Everything's on Github!][github] -->

何か問題があったり、全章の最終的なコードを一度にチェックしたいですか？
[Githubへどうぞ!][github]

<!-- I fairly frequently get asked how to implement a linked list in Rust. The -->
<!-- answer honestly depends on what your requirements are, and it's obviously not -->
<!-- super easy to answer the question on the spot. As such I've decided to write -->
<!--this book to comprehensively answer the question once and for all.  -->

私はかなり頻繁にどうやってRustでリンクリスト(linked list)を実装するのか質問を
受けます。その答えは率直に言ってその人が何を欲するかによりますし、当然この質問
に即答するのは簡単ではないのです。そういった事情で、私はこの質問に一度で包括的に
答えるため、このテキストを書くことに決めました。

<!-- In this series I will teach you basic and advanced Rust programming -->
<!-- entirely by having you implement 6 linked lists. In doing so, you should -->
<!-- learn: -->

このシリーズではRustプログラミングの基本と高度な内容を、専ら6つのリンクリストを
実装しながら教えていきます。その中で以下について学ぶでしょう:

<!-- * The following pointer types: `&`, `&mut`, `Box`, `Rc`, `Arc`, `*const`, `*mut` -->
<!-- * Ownership, borrowing, inherited mutability, interior mutability, Copy -->
<!-- * All The Keywords: struct, enum, fn, pub, impl, use, ... -->
<!-- * Pattern matching, generics, destructors -->
<!-- * Testing -->
<!-- * Basic Unsafe Rust -->

* これらのポインタ型: `&`, `&mut`, `Box`, `Rc`, `Arc`, `*const`, `*mut`
* 所有権、借用、可変性の継承、内部可変性、Copyトレイト
* 盛り沢山のRustのキーワード: struct, enum, fn, pub, impl, use, ...
* パターンマッチ、ジェネリクス、デストラクタ
* 自動テスト
* UnsafeなRustの基本

<!-- Yes, linked lists are so truly awful that you deal with all of these concepts in -->
<!-- making them real. -->

そうです、リンクリストは、それを作るのにこれだけの概念を扱わないといけないので
本当に大変なのです。

<!-- Everything's in the sidebar (may be collapsed on mobile), but for quick -->
<!-- reference, here's what we're going to be making: -->

全てのページはサイドバーにあります(モバイルでは隠れているかもしれません)が、
一覧するために、私たちが作るものをリストアップします:

<!-- 1. [A Bad Singly Linked Stack](first.md) -->
<!-- 2. [An Ok Singly Linked Stack](second.md) -->
<!-- 3. [A Persistent Singly Linked Stack](third.md) -->
<!-- 4. [A Bad But Safe Doubly Linked Deque](fourth.md) -->
<!-- 5. [An Unsafe Singly Linked Queue](fifth.md) -->
<!-- 6. [TODO: An Ok Unsafe Doubly Linked Deque](sixth.md) -->
<!-- 7. [Bonus: A Bunch of Silly Lists](infinity.md) -->

1. [A Bad Singly Linked Stack](first.md)
2. [An Ok Singly Linked Stack](second.md)
3. [A Persistent Singly Linked Stack](third.md)
4. [A Bad But Safe Doubly Linked Deque](fourth.md)
5. [An Unsafe Singly Linked Queue](fifth.md)
6. [TODO: An Ok Unsafe Doubly Linked Deque](sixth.md)
7. [Bonus: A Bunch of Silly Lists](infinity.md)

<!-- Just so we're all the same page, I'll be writing out all the commands that I -->
<!-- feed into my terminal. I'll also be using Rust's standard package manager, Cargo, -->
<!-- to develop the project. Cargo isn't necessary to write a Rust program, but it's -->
<!-- *so much* better than using rustc directly. If you just want to futz around you -->
<!-- can also run some simple programs in the browser via https://play.rust-lang.org/. -->

私とあなたの認識が全く同じになるように、私が端末に打ち込む全てのコマンドを
書き出します。また、開発にはRustの標準ライブラリとCargoを使用します。Cargoは
Rustを書くのに必要ではありませんが、rustcを直接使うより *はるかに* 良いのです。
もしあなたがあれこれ試したくなったら、 https://play.rust-lang.org/ を使って
シンプルなプログラムをブラウザ上で実行することができます。

<!-- Let's get started and make our project: -->

それでは私たちのプロジェクトを作るところから始めましょう:

```text
> cargo new --lib lists
> cd lists
```

<!-- We'll put each list in a separate file so that we don't lose any of our work. -->

それぞれのリストは別々のファイルに置くので、作ったものを失うことはありません。

<!-- It should be noted that the *authentic* Rust learning experience involves -->
<!-- writing code, having the compiler scream at you, and trying to figure out -->
<!-- what the heck that means. I will be carefully ensuring that this occurs as -->
<!-- frequently as possible. Learning to read and understand Rust's generally -->
<!-- excellent compiler errors and documentation is *incredibly* important to -->
<!-- being a productive Rust programmer. -->

*本物の* Rust学習にはコードを書き、コンパイラが喚き、そしてそれが一体どういう
意味なのか理解しようと努めるという体験がつきものなのだということを覚悟しなければ
なりません。私はそうした体験を可能な限り逃さないように注意します。Rustの一般的に
優れたコンパイルエラーとドキュメントを読んで理解できるようになることは、生産的な
Rustプログラマになる上で *非常に* 重要なのです。

<!-- Although actually that's a lie. In writing this I encountered *way* more -->
<!-- compiler errors than I show. In particular, in the later chapters I won't be -->
<!-- showing a lot of the random "I typed (read: copy-pasted) bad" errors that you -->
<!-- expect to encounter in every language. This is a *guided tour* of having the -->
<!-- compiler scream at us. -->

実際にはそれは嘘ですが。これを書く中で私はここで見せるより *かなり* 多くの
コンパイルエラーに遭遇しました。特に、後の章でのどんな言語でも遭遇するような
無作為に試行錯誤した時のエラーはお見せしません。これはコンパイラが浴びせる
金切り声を聞く *ガイド付きツアー* です。

<!-- We're going to be going pretty slow, and I'm honestly not going to be very -->
<!-- serious pretty much the entire time. I think programming should be fun, dang it! -->
<!-- If you're the type of person who wants maximally information-dense, serious, and -->
<!-- formal content, this book is not for you. Nothing I will ever make is for you. -->
<!-- You are wrong. -->

我々はだいぶじっくりと進むつもりですし、正直に言って全体の時間がかなり長くなる
ことはあまり気にするつもりはありません。プログラミングは楽しくあるべきなんだよ、
くそったれ！もしあなたが極限まで情報密度を高めた、真面目な、お堅い内容を求める
タイプの人なら、このテキストはあなたに向いていません。私があなたのためにする
ことは何一つありません。あなたは間違っています。



<!-- # An Obligatory Public Service Announcement -->

# 道義的責任を果たすための公共(啓発)広告

<!-- Just so we're totally 100% clear: I hate linked lists. With -->
<!-- a passion. Linked lists are terrible data structures. Now of course there's -->
<!-- several great use cases for a linked list: -->

100%完全にはっきりさせておきましょう: 私はリンクリストが大嫌いです。生理的に。
リンクリストはひどくまずいデータ構造です。いえ、もちろんリンクリストが適した
ケースはいくつかあります:

<!-- * You want to do *a lot* of splitting or merging of big lists. *A lot*. -->
<!-- * You're doing some awesome lock-free concurrent thing. -->
<!-- * You're writing a kernel/embedded thing and want to use an intrusive list. -->
<!-- * You're using a pure functional language and the limited semantics and absence -->
<!-- of mutation makes linked lists easier to work with. -->
<!-- * ... and more! -->

* 大きなリストを *たくさん* 分割やマージしたい場合。 *たくさん* ね。
* 素晴らしいlock-free(スレッドがロックすることのない)並行処理をしている場合
* カーネルや組み込みの処理を書いていて侵入的リスト(intrusive list)が使いたい場合
* 純粋関数型言語を使っていて、意味論的制約や変更不能性の都合上リンクリストの方が
扱いやすい場合
* ... などなど！

<!-- But all of these cases are *super rare* for anyone writing a Rust program. 99% -->
<!-- of the time you should just use a Vec (array stack), and 99% of the other 1% -->
<!-- of the time you should be using a VecDeque (array deque). These are blatantly -->
<!-- superior data structures for most workloads due to less frequent allocation, -->
<!-- lower memory overhead, true random access, and cache locality. -->

でもこれらの場合というのはRustを書いている人にとっては *激レア* なのです。
99%の場合は単に Vec (array stack) を使うべきですし、残り1%の中の99%の場合は
VecDeque (array deque) を使うべきです。これらは「メモリ割り当ての頻度がより低い」
「メモリのオーバーヘッドが低い」「真のランダムアクセスがある」「キャッシュの
局所性がある」といった理由により大抵リンクリストに比べて計算量の面で明らかに
優れたデータ構造です。

<!-- Linked lists are as *niche* and *vague* of a data structure as a trie. Few would -->
<!-- balk at me claiming a trie is a niche structure that your average programmer -->
<!-- could happily never learn in an entire productive career -- and yet linked lists -->
<!-- have some bizarre celebrity status. We teach every undergrad how to write a -->
<!-- linked list. It's the only niche collection -->
<!-- [I couldn't kill from std::collections][rust-std-list]. It's -->
<!-- [*the* list in C++][cpp-std-list]! -->

リンクリストはトライ木(trie)と同じぐらい *ニッチ* で *いつ役に立つのかよく
分からない* データ構造です。トライ木がニッチなデータ構造であって、平均的な
プログラマがたくさんのものを生み出すその生涯で一度も学ぶことにはならないだろうと
いう私の主張に反論する人は少ないでしょう……。なのに、リンクリストは奇妙なことに
名声ある地位に在るのです。我々は学生時代にリンクリストの書き方を教えられますし、
リンクリストは[私がstd::collectionsから葬り去ることのできない](rust-std-list)
唯一需要のないコレクションです。[C++ではなんと *これが* listです](cpp-std-list)。

<!-- We should all as a community say *no* to linked lists as a "standard" data -->
<!-- structure. It's a fine data structure with several great use cases, but those -->
<!-- use cases are *exceptional*, not common. -->

我々はコミュニティとしてリンクリストを *標準的なデータ構造* として扱うことに
*No* と言わなければなりません。リンクリストは幾つかのケースでは良いデータ構造
ですが、それらのケースは *例外的* であり、ありふれたものではありません。

<!-- Several people apparently read the first paragraph of this PSA and then stop -->
<!-- reading. Like, literally they'll try to rebut my argument by listing one of the -->
<!-- things in my list of *great use cases*. The thing right after the first -->
<!-- paragraph! -->

いくらかの人々はこのPSA(公共広告)の最初の段落を一見して読むのを止めてしまう
みたいです。なんていうか、彼らは私がまさに *適した使い方* の一覧に載せたものを
挙げて反論しようとするんです。第一段落の直後に書いたのに！

<!-- Just so I can link directly to a detailed argument, here are several attempts -->
<!-- at counter-arguments I have seen, and my response to them. Feel free to skip -->
<!-- to [the end](#take-a-breath) if you just want to learn some Rust! -->

ここに詳しい議論を直接貼っておきます。これは私が受けたいくつかの反論とその返答
です。もしあなたがただRustを学びたいだけなら、どうぞ
[この章の最後](#take-a-breath) までスキップしてください！




<!-- ## Performance doesn't always matter -->
## パフォーマンスが常に重要なわけではない

<!-- Yes! Maybe your application is I/O-bound or the code in question is in some -->
<!-- cold case that just doesn't matter. But this isn't even an argument for using -->

<!-- このYesは否定文に対するYesなのでYes it mattersの義 -->
いいえ、重要です。
もしかするとあなたのアプリケーションはI/Oバウンドな（訳注: ファイルへの読み書きなどが多く、
I/Oが速度のボトルネックになるものを指す）ものかもしれません。他にも、全くもってどうでもいい、
実行される頻度が低いコードパスに件のコードがあるのかもしれません。しかし、これらはそもそもリンクリストを使用する
ことを支持する論拠ですらありません。これらは *ありとあらゆるもの* を使用することを支持する論拠です。
なぜリンクリストに甘んじる必要があるのでしょうか？linked hash map<!-- 定訳がわかりませんでしたごめんなさい -->
を使えばよいのです！

もしパフォーマンスが重要でないなら、*もちろん*自然な既定値である配列を使って申し分ないでしょう。

## They have O(1) split-append-insert-remove if you have a pointer there

Yep! Although as [Bjarne Stroustrup notes][bjarne] *this doesn't actually
matter* if the time it takes to get that pointer completely dwarfs the
time it would take to just copy over all the elements in an array (which is
really quite fast).

Unless you have a workload that is heavily dominated by splitting and merging
costs, the penalty *every other* operation takes due to caching effects and code
complexity will eliminate any theoretical gains.

*But yes, if you're profiling your application to spend a lot of time in
splitting and merging, you may have gains in a linked list*.





## I can't afford amortization

You've already entered a pretty niche space -- most can afford amortization.
Still, arrays are amortized *in the worst case*. Just because you're using an
array, doesn't mean you have amortized costs. If you can predict how many
elements you're going to store (or even have an upper-bound), you can
pre-reserve all the space you need. In my experience it's *very* common to be
able to predict how many elements you'll need. In Rust in particular, all
iterators provide a `size_hint` for exactly this case.

Then `push` and `pop` will be truly O(1) operations. And they're going to be
*considerably* faster than `push` and `pop` on linked list. You do a pointer
offset, write the bytes, and increment an integer. No need to go to any kind of
allocator.

How's that for low latency?

*But yes, if you can't predict your load, there are worst-case
latency savings to be had!*





## Linked lists waste less space

Well, this is complicated. A "standard" array resizing strategy is to grow
or shrink so that at most half the array is empty. This is indeed a lot of
wasted space. Especially in Rust, we don't automatically shrink collections
(it's a waste if you're just going to fill it back up again), so the wastage
can approach infinity!

But this is a worst-case scenario. In the best-case, an array stack only has
three pointers of overhead for the entire array. Basically no overhead.

Linked lists on the other hand unconditionally waste space per element.
A singly-linked lists wastes one pointer while a doubly-linked list wastes
two. Unlike an array, the relative wasteage is proportional to the size of
the element. If you have *huge* elements this approaches 0 waste. If you have
tiny elements (say, bytes), then this can be as much as 16x memory overhead
(8x on 32-bit)!

Actually, it's more like 23x (11x on 32-bit) because padding will be added
to the byte to align the whole node's size to a pointer.

This is also assuming the best-case for your allocator: that allocating and
deallocating nodes is being done densely and you're not losing memory to
fragmentation.

*But yes, if you have huge elements, can't predict your load, and have a
decent allocator, there are memory savings to be had!*





## I use linked lists all the time in &lt;functional language&gt;

Great! Linked lists are super elegant to use in functional languages
because you can manipulate them without any mutation, can describe them
recursively, and also work with infinite lists due to the magic of laziness.

Specifically, linked lists are nice because they represent an iteration without
the need for any mutable state. The next step is just visiting the next sublist.

However it should be noted that Rust can pattern match on arrays and talk
about sub-arrays [using slices][slices]! It's actually even more expressive
than a functional list in some regards because you can talk about the last
element or even "the array without the first and last two elements" or
whatever other crazy thing you want.

It is true that you can't *build* a list using slices. You can only tear
them apart.

For laziness we instead have [iterators][]. These can be infinite and you
can map, filter, reverse, and concatenate them just like a functional list,
and it will all be done just as lazily. No surprise here: slices can also be
coerced to an iterator.

*But yes, if you're limited to immutable semantics, linked lists can be very
nice*.

Note that I'm not saying that functional programming is necessarily weak or
bad. However it *is* fundamentally semantically limited: you're largely only
allowed to talk about how things *are*, and not how they should be *done*. This
is actually a *feature*, because it enables the compiler to do tons of [exotic
transformations][ghc] and potentially figure out the *best* way to do things
without you having to worry about it. However this comes at the cost of being
*able* to worry about it. There are usually escape hatches, but at some limit
you're just writing procedural code again.

Even in functional languages, you should endeavour to use the appropriate data
structure for the job when you actually need a data structure. Yes, singly
linked lists are your primary tool for control flow, but they're a really poor
way to actually store a bunch of data and query it.


## Linked lists are great for building concurrent data structures!

Yes! Although writing a concurrent data structure is really a whole different
beast, and isn't something that should be taken lightly. Certainly not something
many people will even *consider* doing. Once one's been written, you're also not
really choosing to use a linked list. You're choosing to use an MPSC queue or
whatever. The implementation strategy is pretty far removed in this case!

*But yes, linked lists are the defacto heroes of the dark world of lock-free
concurrency.*




## Mumble mumble kernel embedded something something intrusive.

It's niche. You're talking about a situation where you're not even using
your language's *runtime*. Is that not a red flag that you're doing something
strange?

It's also wildly unsafe.

*But sure. Build your awesome zero-allocation lists on the stack.*





## Iterators don't get invalidated by unrelated insertions/removals

That's a delicate dance you're playing. Especially if you don't have
a garbage collector. I might argue that your control flow and ownership
patterns are probably a bit too tangled, depending on the details.

*But yes, you can do some really cool crazy stuff with cursors.*





## They're simple and great for teaching!

Well, yeah. You're reading a book dedicated to that premise.
Well, singly linked lists are pretty simple. Doubly linked lists
can get kinda gnarly, as we'll see.




<!-- # Take a Breath -->

# 一息つきましょう

<!-- Ok. That's out of the way. Let's write a bajillion linked lists. -->

さて、脱線は終わりです。たっくさんリンクリストを書きますよ。



[rust-std-list]: https://doc.rust-lang.org/std/collections/struct.LinkedList.html
[cpp-std-list]: http://en.cppreference.com/w/cpp/container/list
[github]: https://github.com/Gankro/too-many-lists
[bjarne]: https://www.youtube.com/watch?v=YQs6IC-vgmo
[slices]: https://doc.rust-lang.org/book/slice-patterns.html
[iterators]: https://doc.rust-lang.org/std/iter/trait.Iterator.html
[ghc]: https://wiki.haskell.org/GHC_optimisations#Fusion
