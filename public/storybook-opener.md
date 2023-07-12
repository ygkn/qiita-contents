---
title: Storybook の目的のページを一瞬で開ける VS Code 拡張機能「Storybook Opener」を作った
tags:
  - "VS Code"
  - "Storybook"
private: true
updated_at: ""
id: null
organization_url_name: null
---

<!-- textlint-disable ja-technical-writing/no-exclamation-question-mark -->

<!-- まえがき -->

Storybook を開くときにちょっと手間取った経験はありませんか？　 Storybook は UI コンポーネントテスト機能が強化されつつあり、フロントエンド開発で使用する機会も増えています。Storybook に頻繁にアクセスするようになると、もっと楽に Storybook を開きたいと感じることがあります。

この記事では、そんな問題を解決する「Storybook Opener」（[Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ygkn.storybook-opener), [GitHub](https://github.com/ygkn/storybook-opener)）と、その開発中に得られた知見や裏話も共有します。

## 開発の経緯

ある日 Storybook を使って開発しているとこんなことがありました。

<!-- textlint-disable ja-technical-writing/sentence-length, ja-technical-writing/no-hankaku-kana, ja-technical-writing/ja-no-successive-word -->

「よ〜しこのコンポーネントを実装しちゃうぞ〜」
「コンポーネントと Stories のファイルを作って〜」
「Storybook を開いて〜」
「あのコンポーネントの Story はどこかな〜　ﾎﾟﾁ……ﾎﾟﾁ……　よし、あった！」
（実装中…）
「よし、できた！」

「お、新しいプルリクがあるな、レビューしよう。チェックアウトして〜」
「レビューするコンポーネントの Story はどこかな〜　ﾎﾟﾁ…ﾎﾟﾁ…」
「うん、動いてるしコードも問題なさそう」
「もう何個か実装してくれたコンポーネントがあるのか、 Story を開いて〜　ﾎﾟﾁ…ﾎﾟﾁ…」
「ヨシ！　 LGTM！」

「次はこのコンポーネントを実装するか」
「コンポーネントと Stories のファイルを作って〜」
「また Storybook をﾎﾟﾁﾎﾟﾁして Story を開かなきゃ…」

「あ、さっき Storybook 落としてたんだった！　 VS Code に戻って起動しなきゃ…」
「うわっ Storybook で VS Code の Quick Open のショートカット押してしまって印刷ダイアログが出てきたっ！」

🤯

<!-- textlint-enable ja-technical-writing/sentence-length, ja-technical-writing/no-hankaku-kana, ja-technical-writing/ja-no-successive-word -->

このようなことが起こってしまう原因として、以下のようなものを考えました。

- VS Code と Storybook を交互に操作する必要がある
- Story がどの階層にあるかを確かめ、操作するまで覚える必要がある
- 上記のことをコンポーネントごとに行う必要がある

さらに、Storybook を開くときは目的のコンポーネントなどをすでにエディタで開いていることが多いことに気づきました。

そこで VS Code から直接 Storybook を開くことができれば、Storybook へのアクセスが楽になるのではと考え「Storybook Opener」を開発することにしました。

## 「Storybook Opener」の紹介

「Storybook Opener」は、下の GIF のようにエディタから開いているコンポーネントなどを Storybook で簡単に開ける VS Code 拡張機能です。

![demo.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/104663/d92f6f18-0eb0-7982-69c8-a26dbf175b7f.gif)

### 主な機能

#### Storybook の設定に柔軟に対応

`title` や `titlePrefix`など、`preview.ts` や Stories ファイルには Storybook の URL を変えるような設定があります。これらが設定されていても正しいページが開けます。

#### 様々なフレームワークに対応

React はもちろん、Vue、Svelte、HTML など、Storybook が対応しているフレームワークは（おそらく）使えます。

#### ファイルのコロケーションに対応

`SomeComponent.stories.tsx` の Story を以下のような同じベース名のファイルから開けます。

- `SomeComponent.tsx`
- `SomeComponent.module.scss`
- `SomeComponent.vue`
- `SomeComponent.svelte`
- など

#### Storybook 開発サーバーを自動で起動

Storybook を開く際に Storybook が起動していなかったら起動するか確認後、起動します。

### インストール方法

[Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ygkn.storybook-opener) や、VS Code の「Extensions」からインストールできます。

### 使い方

1. VS Code で Storybook の設定ディレクトリ (`.storybook`など。後述の設定で変更可能) をルートに含むフォルダを開く
2. Stories やコンポーネントなどのファイルをエディタで開く
3. 以下の方法で Storybook を開けます！
   - [editor actions](https://code.visualstudio.com/api/ux-guidelines/editor-actions) ボタン (エディタのツールバーのボタン) をクリックする
   - 右クリックメニューから `Open Storybook` を選択
   - コマンドパレットで `Storybook Opener: Open Storybook` を実行

### 設定

以下の項目が設定できます。

#### Storybook の URL を決定するための設定

- **`"storybook-opener.storybookOption.configDir"`**
  - Storybook の設定ディレクトリ
  - Storybook CLI の `-c` / `--config-dir` オプションと同等
  - デフォルトは `".storybook"`
- **`"storybook-opener.storybookOption.host"`**
  - Storybook を実行するホスト
  - Storybook CLI の `-h` / `--host` オプションと同等
  - デフォルトは `"localhost"`
- **`"storybook-opener.storybookOption.configDir"`**
  - Storybook を HTTPS でサーブするか
  - Storybook CLI の `--https` オプションと同等
  - デフォルトは `false`
- **`"storybook-opener.storybookOption.port"`**
  - Storybook を実行するポート
  - Storybook CLI の `-p` / `--port` オプションと同等
  - デフォルトは `6006`

#### Storybook を自動で起動するための設定

- **`""storybook-opener.storybookOption.startCommand""`**
  - Storybook を起動するコマンド
  - デフォルトは `npx storybook dev --no-open （上記のオプション）`

## 「Storybook Opener」開発の舞台裏

ここでは、Storybook Opener の開発中に得た知見などを紹介します。脚注は、典拠となるドキュメントやソースコードへのリンクです。

### Storybook はどのように URL を決定しているか

Storybook Opener の機能を実現するために、エディタで開いているファイルから Storybook の URL を取得する方法を探らなければいけません。その謎を解くため、Storybook の奥地へと向かいました。

#### ID とその計算ロジック

Storybook で表示できるものには Story と Docs がありますが、まずは Story について考えます。Story の URL を見ると、以下のような形式になっています。

- `http://localhost:6006/?path=/story/example-button--primary`

URL の `path` クエリパラメータの値は、`/story/` の後に Story の ID (`example-button--primary`) が続きます。ID は、マニュアルで設定されなければデフォルトで `meta` のタイトルと Story の名前から得られます [^1]。明示的に指定しなければ、`meta` のタイトルは（CSF 3.0 以降）自動でファイル名から付けられ[^2]、Story の名前は export された名前から付けられます。

例えば、以下のような Story を考えます。

```FooBar.stories.tsx
const meta: Meta<typeof Foo> = {
  component: Foo,
};

export default meta;

type Story = StoryObj<typeof Foo>;

export const Baz: Story = {};
```

この場合、計算される値は以下のようになります。

|      名称       |                        値                         |
| :-------------: | :-----------------------------------------------: |
| meta のタイトル |             `FooBar` (ファイル名より)             |
|  Story の名前   |                       `Baz`                       |
|   Story の ID   |                  `foo-bar--baz`                   |
|       URL       | `http://localhost:6006/?path=/story/foo-bar--baz` |

[^1]: [Storybook の ドキュメント「Sidebar & URLS」ページ中「Permalink to stories」セクション](https://storybook.js.org/docs/react/configure/sidebar-and-urls#permalink-to-stories)
[^2]: [Storybook の ドキュメント「Sidebar & URLS」ページ中「CSF 3.0 auto-titless」セクション](https://storybook.js.org/docs/react/configure/sidebar-and-urls#csf-30-auto-titles)

さて、これらのロジックはどう実装されているのでしょうか。ID を計算するロジックは `@storybook/csf` パッケージに `toId` 関数として定義されています [^3]。また、`@storybook/csf-tools` にパッケージ`loadCsf` 関数という関数が定義されています。この関数は、 CSF (Component Story Format) で記述された Stories ファイルを Babel を用いて静的に解析します。そして先述のような方法で meta のタイトルや Story の名前を計算し、 `toId` 関数を用いて Story の ID を決定しています [^4]。

[^3]: [ソースコード: GitHub リポジトリ ComponentDriven/csf の `src/index.ts`](https://github.com/ComponentDriven/csf/blob/4c735fea4f0c9605b93497238303cb4ab9304727/src/index.ts#L31-L32)
[^4]: [ソースコード: GitHub リポジトリ storybookjs/storybook の `code/lib/csf-tools/src/CsfFile.ts`](https://github.com/storybookjs/storybook/blob/55d32c30a730b39d1b905d7532ef9d88e191ebf1/code/lib/csf-tools/src/CsfFile.ts)

#### Story Indexer

Story Indexer は、Story を読み込む処理をさらに抽象化したものです。Storybook は CSF 以外にも Docs Addon を追加することで MDX で Story を書くことができます。そのような場合にファイルを解析するためには前処理が必要なので、さらに抽象化層が必要となります[^5]。

[^5]: [Storybook の ドキュメント「Sidebar & URLS」ページ中「Story Indexers」セクション](https://storybook.js.org/docs/react/configure/sidebar-and-urls#story-indexers)

例えば、通常の CSF で記述された Stories ファイルを読み込むような Story Indexer は、先述の `loadCsf` 関数を用いて以下のように書けます。

```ts
const indexer = {
  test: /(stories|story)\.[tj]sx?$/,
  indexer: async (fileName, opts) => {
    const code = readFileSync(fileName, { encoding: "utf-8" });
    return loadCsf(code, { ...opts, fileName }).parse();
  },
};
```

一方、MDX で書かれた Story や Docs を読み込む Story Indexer は以下のように書く必要があります。

```ts
const indexer = {
  test: /.mdx?$/,
  indexer: async (fileName, opts) => {
    const rawCode = readFileSync(fileName, { encoding: "utf-8" });

    // MDX を CSF に変換する
    const code = await compile(code);

    return loadCsf(code, { ...opts, fileName }).parse();
  },
};
```

#### Presets

Story Indexer は Stroybook config の `storyIndexers` プロパティで設定できます [^5]。しかしほとんどの場合 `storyIndexers` は`.storybook/main.ts` では直接設定されておらず、デフォルトや Docs Addon などの Addon によって設定されています。このような Storybook のユーザが設定せず、 Addon によって設定される config を preset と言います。[^6]

[^6]: [Storybook のドキュメント「Write a preset addon」ページ](https://storybook.js.org/docs/react/addons/writing-presets)

#### `StoryIndexGenerator`

Storybook のサーバー内で Story Indexer を呼び出しているのが `StoryIndexGenerator` クラスです。`extractStories` メソッドでは Story Indexer を用いて Story の ID などの情報を取得しています。また、`extractDocs` メソッドでは Docs の ID を対象の Story や name から計算してます[^7]。

`StoryIndexGenerator` クラスは、Storybook の開発サーバーの立ち上げ時 [^8]や静的ファイルのビルド時[^9]に呼び出されています。

[^7]: [ソースコード: GitHub リポジトリ storybookjs/storybook の `/code/lib/core-server/src/utils/StoryIndexGenerator.ts`](https://github.com/storybookjs/storybook/blob/55d32c30a730b39d1b905d7532ef9d88e191ebf1/code/lib/core-server/src/utils/StoryIndexGenerator.ts)
[^8]: [ソースコード: GitHub リポジトリ storybookjs/storybook の `/code/lib/core-server/src/dev-server.ts`](https://github.com/storybookjs/storybook/blob/55d32c30a730b39d1b905d7532ef9d88e191ebf1/code/lib/core-server/src/dev-server.ts)
[^9]: [ソースコード: GitHub リポジトリ storybookjs/storybook の `/code/lib/core-server/src/build-static.ts`](https://github.com/storybookjs/storybook/blob/55d32c30a730b39d1b905d7532ef9d88e191ebf1/code/lib/core-server/src/build-static.ts)

#### まとめ

Storybook の奥地を探索した結果、どうやら次のようにファイルのパスから Storybook の URL が取得できそうです。

1. preset を含む Storybook の config を読み込む
2. `StoryIndexGenerator` と同様のアルゴリズムで Story Indexer を用いて Stories ファイルを読み込む
3. ID から URL を得る

### VS Code 拡張機能内で Storybook のモジュールをどう読み込むか

次に、どのようにして先述の処理を VS Code 拡張機能内で実行するかという問題がありました。プロジェクトごとに使用されている Storybook のバージョンや使用されているフレームワーク、Addon などは異なります。そのため、Storybook をそのまま dependencies として入れるのではなく、ユーザがワークスペース内にインストールした Storybook を使用する必要がありました。

#### Tailwind CSS IntelliSense の場合

VS Code 拡張機能の[「Tailwind CSS IntelliSense」](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss)の処理が以下の理由で Storybook Opener の参考になりそうだと考えました。

- 両者とも設定ファイルと、そこで動的に指定されるファイル(`contents`, `stories`)を見る必要がある
- 両者ともプラグインのような機構を持っている
- ソースコードが公開されている

ソースを読んだ結果、Tailwind CSS IntelliSense 内では [enhanced-resolve](https://github.com/webpack/enhanced-resolve) を用いてワークスペース内の Tailwind 本体を読み込んでいることがわかりました[^10]。

[^10]: [ソースコード: GitHub リポジトリ tailwindlabs/tailwindcss-intellisense の `/packages/tailwindcss-language-server/src/util/resolveFrom.ts`](https://github.com/tailwindlabs/tailwindcss-intellisense/blob/a6c19d7cb49fe7de40d86c3157fdeb445ee5c16c/packages/tailwindcss-language-server/src/util/resolveFrom.ts)

#### enhanced-resolve と `require.resolve`

enhanced-resolve の機能は、Node.js の `require.resolve` に似ていますが、非同期かつエイリアスや解決する拡張子などの設定が可能という違いがあります。Storybook Opener では `require.resolve` を試してみた結果十分そうだということが分かったため、`require.resolve` を使用することにしました。

例えば、ユーザがワークスペースにインストールした `@storybook/core-common` パッケージの `loadMainConfig` を読み込むには以下のようにします。

```ts
const { loadMainConfig } = require(require.resolve(
  "@storybook/core-common",
  // ワークスペースのディレクトリ
  workingDir
));
```

Storybook Opener では、利便性のため、`require` と `require.resolve` を `requireFrom` という関数にまとめました。さらに、このままでは読み込んだモジュールに型が付かないのですが、devDependencies に Storybook をインストールし、型のみを別途 import することで解決しました。

```ts
const { loadMainConfig } = requireFrom(
  "@storybook/core-common",
  // ワークスペースのディレクトリ
  workingDir
) as typeof import("@storybook/core-common");
```

`requireFrom` 関数の型定義を工夫することで `as` 以降を書かずに型を付けたかったのですが、TypeScript 力（ぢから）不足で現在出来てません。解決方法お待ちしています。

### Storybook の自動起動

Storybook サーバーが起動していなかったら起動する機能はかなり単純な仕組みになっています。

1. `node-fetch` で Storybook の URL を fetch
2. 失敗したらサーバーが起動していないとみなし、新規作成したターミナルで起動
3. `wait-on` で完全に起動するまで待つ

VS Code の Node.js バージョンは 16 台なので、ビルトインの `fetch` の代わりに `node-fetch` を使用しています。

### 実装

Storybook Opener の `src` ディレクトリは以下のように実装しました。

- `storybook/`
  - Storybook を直接呼び出すモジュール
- `utils/`
  - ユーティリティ関数
- `types/`
  - 型定義
- `extension.ts`
  - 拡張機能のエントリーポイント

`storybook/` ディレクトリでは、[eslint-plugin-import-access](https://github.com/uhyo/eslint-plugin-import-access) を使って同ディレクトリから呼び出せる関数を制限しています。また、基本的に Storybook 内部のコードを参考にしているため、Storybook 本体の変更に追従できるようにコードへのパーマリンクをコメントに記載しています。

### アイコン

アイコンは、[Storybook のロゴ](https://github.com/storybookjs/brand) を変形させて開いた本のようにしました。

![アイコン](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/104663/df014985-322c-8f82-5975-eebf01212784.png)

## おわりに

以上、Storybook Opener と、開発で得られた知見について紹介しました。我ながら Storybook Opener はかなり便利な VS Code 拡張機能だと思っています。コンポーネントの実装時やレビュー時にぜひ役立ててください。

自分の中で Storybook のよくわかないけどなんかいい感じに動いている謎技術だった機能がありましたが、今回の開発や調査の中でそれらの仕組みが理解出来ました。例えば、meta のタイトル文字列リテラルしか指定できない理由が Stories ファイルを静的解析して取得しているからだったなどがそれにあたります。

また、PR や Issue などの Contribution やフィードバックは大歓迎です。先日早速 [Storybook の起動コマンドのバグ修正 PR](https://github.com/ygkn/storybook-opener/pull/6) を早速いただいたりしました。
