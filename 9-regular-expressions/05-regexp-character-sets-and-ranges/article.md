# セットと範囲 [...]

角括弧 `[…]` 内の複数の文字または文字クラスは "指定された中の任意の文字を探す" ことを意味します。

<<<<<<< HEAD:5-regular-expressions/05-regexp-character-sets-and-ranges/article.md
[cut]

## セット
=======
## Sets
>>>>>>> 08734734021aa128c13da2382fe8fa062677bb9f:9-regular-expressions/05-regexp-character-sets-and-ranges/article.md

例えば、`pattern:[eao]` は3文字 `'a'`, `'e'`, または `'o'` のいずれかを意味します。

それは *セット* と呼ばれます。セットは通常の文字と併せて正規表現の中で使うことができます。:

```js run
// [t or m], 次に "op" となる文字列を見つける
alert( "Mop top".match(/[tm]op/gi) ); // "Mop", "top"
```

セットには複数の文字がありますが、マッチした中での1文字に相当することに注意してください。

<<<<<<< HEAD:5-regular-expressions/05-regexp-character-sets-and-ranges/article.md
従って、下の例ではマッチするものはありません:
=======
So the example below gives no matches:
>>>>>>> 08734734021aa128c13da2382fe8fa062677bb9f:9-regular-expressions/05-regexp-character-sets-and-ranges/article.md

```js run
// "V" に続き [o or i], その後 "la" となる文字列を見つける
alert( "Voila".match(/V[oi]la/) ); // null, マッチしない
```

パターンは次のように想定します:

- `pattern:V`,
- 次に文字 `pattern:[oi]` の *1つ*,
- 次に `pattern:la`.

なので、`match:Vola` もしくは `match:Vila` がマッチします。

## 範囲

角括弧は *文字の範囲* を含むこともあります。

例えば、`pattern:[a-z]` は `a` から `z` までの範囲の文字で、 `pattern:[0-5]` は `0` から `5` までの数字です。

下の例では、`x` に続いて2桁の数字または `A` から `F` までの文字を探しています:

```js run
alert( "Exception 0xAF".match(/x[0-9A-F][0-9A-F]/g) ); // xAF
```

単語 `subject:Exception` で、部分文字列 `subject:xce` があることに注目していください。それはパターンにマッチしませんでした。理由はパターン `pattern:[0-9A-F]` が大文字であるのに対し、小文字だからです。

それも見つけたい場合は、`a-f` の範囲も追加します: `pattern:[0-9A-Fa-f]`。 `i` フラグも小文字を許容するでしょう。

**文字クラスは、特定の文字セットの短縮形です**

例:

- **\d** -- `pattern:[0-9]` と同じです,
- **\w** -- `pattern:[a-zA-Z0-9_]` と同じです,
- **\s** -- `pattern:[\t\n\v\f\r ]` に他のユニコードの空白文字を加えたものと同じです。

`[…]` の内側でも同様に文字クラスを使うことができます。

例えば、"twenty-third" のような言葉に対し、すべての単語文字またはダッシュにマッチさせたいとします。`pattern:\w` はダッシュを含まないため、`pattern:\w+` ではできません。しかし `pattern:[\w-]` とすることができます。

<<<<<<< HEAD:5-regular-expressions/05-regexp-character-sets-and-ranges/article.md
`pattern：[\s\S]` のように、可能性のあるすべての文字をカバーするためにクラスの組み合わせを使うこともできます。これは空白及び非空白 -- なので任意の文字に一致します。 ドット `"."` は改行以外の任意の文字にマッチするので、これはドットよりも広いです。
=======
We also can use several classes, for example `pattern:[\s\S]` matches spaces or non-spaces -- any character. That's wider than a dot `"."`, because the dot matches any character except a newline (unless `s` flag is set).
>>>>>>> 08734734021aa128c13da2382fe8fa062677bb9f:9-regular-expressions/05-regexp-character-sets-and-ranges/article.md

## 範囲を除外する

通常の範囲に加えて、 `pattern:[^…]` のような、範囲を "除外" するものもあります。

それらは開始時にキャレット文字 `^` で指定され、*指定されたもの以外の文字* と一致します。

例:

- `pattern:[^aeyo]` -- `'a'`, `'e'`, `'y'` または `'o'` を除く任意の文字.
- `pattern:[^0-9]` -- 数字以外の任意の文字, `\D` と同じです.
- `pattern:[^\s]` -- 任意の非空白文字, `\S` と同じです.

下の例では、文字、数字または空白以外の文字を探します:

```js run
alert( "alice15@gmail.com".match(/[^\d\sA-Z]/gi) ); // @ and .
```

## […] でエスケープしない

通常、正確にドット文字を見つけたい場合、`pattern:\.` のようにエスケープが必要です。そしてバックスラッシュが必要な場合には、`pattern:\\` を使います。

角括弧で囲まれた大多数の特殊文字は、エスケープなしで使うことができます:

- ドット `pattern:'.'`.
- プラス `pattern:'+'`.
- 丸括弧 `pattern:'( )'`.
- ダッシュ `pattern:'-'`, 先頭または末尾に(範囲を定義していない場合).
- キャレット `pattern:'^'`, 先頭でない場合(それは除外を意味する).
- そして、角括弧の開始 `pattern:'['`.

つまり、角括弧を意味するものを除いて、すべての特殊文字が許可されます。

角括弧内のドット `"."` は単なるドットを意味します。パターン `pattern:[.,]` は1つの文字を探します: ドットまたはカンマです。

下の例では、正規表現 `pattern:[-().^+]` は `-().^+` の文字の1つを探します:

```js run
// エスケープは必要ありません
let reg = /[-().^+]/g;

alert( "1 + 2 - 3".match(reg) ); // +, - にマッチ
```

...しかし "念の為" エスケープすることを決めた場合にも特に害はありません:

```js run
// すべてエスケープ
let reg = /[\-\(\)\.\^\+]/g;

alert( "1 + 2 - 3".match(reg) ); // 同じく動作します: +, -
```