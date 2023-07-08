---
title: キーボードショートカットの正しいマークアップ
tags:
  - Qiita
  - HTML
  - GitHub
private: false
updated_at: '2016-12-24T18:39:59+09:00'
id: 0b485f6aedd15eadec33
organization_url_name: null
---
Qiita、ブログなどの技術系記事やGitHubなどでドキュメントを書くとき、しばしばショートカットキーを書く必要に迫られます。

そんなとき、皆さんはどのようにマークアップしていますか？

直で書いたり、`<pre>`で囲ったり、`<em>`を使う人もいるかもしれません。

僕は最近まで`<pre>`でしたが、ある日[MDN](https://developer.mozilla.org)で[`<kbd>`要素](http://webmem.hatenablog.com/entry/kbd-element)（以下`<kbd>`）というものを見つけました。

同記事には

>キーボードインプット要素（`<kbd>`）はコンピューターへのユーザーの入力コードを表します。要素名はキーボードの略字ですが、音声入力や手書き入力など他の入力方法も含みます。多くのデフォルトスタイルは、等幅フォントのインラインで表示するものとなっています。

とあります。

Qiitaでは（というより多くのMarkdown記法では）`<kbd>`はサポートされていませんのでHTMLで書く必要があります。

例：

```html
<kbd><kbd>Ctrl</kbd>+<kbd>C</kbd></kbd>を押下してください。
```

<kbd><kbd>Ctrl</kbd>+<kbd>C</kbd></kbd>を押下してください。

これを見てあれ？と思った方もいるでしょう。`<kbd>`が無駄にネストされているように見えるのです。

一見これは奇妙ですが、

- 外側の`<kbd>`でキーボード入力であることを示す
- 内側の`<kbd>`で押されたキー自体を示す

ということなのです。

QiitaのCSSもそれに合わせていい感じになっています。

また、GitHubでは![2016-11-17_20:02:57_範囲を選択.png](https://qiita-image-store.s3.amazonaws.com/0/104663/ab676682-7e7f-be16-b763-9ffe69a02cea.png)のように見えます。




ちなみにこの「押下」ですが、[こちらの記事](http://qiita.com/yaju/items/0ceb6a0343561b4d208e)に面白いことが書かれているので、見てみてはいかがでしょうか？

また、`<kbd>`は

```html
<kbd>cmd</kbd>を入力し、OKボタンをクリックします。
```

<kbd>cmd</kbd>を入力し、OKボタンをクリックします。

のように、単なるユーザーが入力した文字にも使えます。

メニュー項目やシステムによってエコーされる場合などにも使える`<kbd>`ですが、ここはショートカットキーの説明の記事なので割愛します。参考の記事にいろいろ書いてあります。

## まとめ
- キーボードショートカットには`<kbd>`を使おう！
- キーを表す場合は`<kbd>`をネストしよう！

## 参考
[kbd 要素 - HTML | MDN](https://developer.mozilla.org/ja/docs/Web/HTML/Element/kbd)
[kbd要素を正しくマークアップできていますか？](http://webmem.hatenablog.com/entry/kbd-element)
