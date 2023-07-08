---
title: ググり方の作法
tags:
  - LifeisTech!
private: false
updated_at: '2016-12-06T00:20:03+09:00'
id: 5febd0dfedcd989dc49a
organization_url_name: null
---
「検索ワード」の横の [:mag:](https://www.google.co.jp/search?q=%E6%A4%9C%E7%B4%A2%E3%83%AF%E3%83%BC%E3%83%89)をクリックするとGoogleで検索します。

どうも。ヤギちゃんです。

'15の春キャンのことでした。僕はBirthDateBaseというWebサービスを開発していました。（今は休止中。「休止」だよ！「終了」じゃないよ！）

アニメキャラや芸能人、さらには歴史上の人物までもの誕生日を投稿し、共有したり保存した人やキャラの誕生日には通知をだすという（予定の）SNSだったのですが、その人やキャラにタグ付けする機能（例えば高槻やよいちゃんだったら「THE IDOLM@STER」、「765プロダクション」など）をつけようと思い、闇に~~呑まれよ！~~ハマりました。

Webサービス開発コース（当時はWebプログラミングコース）では`Sinatra`というフレームワークを使っていたので、

「Sinatra activerecord タグ機能」 [:mag:](https://www.google.co.jp/search?q=Sinatra+activerecord+%E3%82%BF%E3%82%B0%E6%A9%9F%E8%83%BD)

などとググっていました。

しかし、僕が求めているような情報は全くありませんでした。

そして、キャンプが終わり、落ち着いてからあることに気づきました。

タグってことは他対他のリレーショナルを実装せればいいわけだ、と。

そこで、

「Sinatra activerecord 他対他」 [:mag:](https://www.google.co.jp/search?q=Sinatra+activerecord+%E4%BB%96%E5%AF%BE%E4%BB%96)

とググると…

[Rails4で多対多のリレーションをモデルに実装する - Rails Webook](http://ruby-rails.hatenadiary.com/entry/20141204/1417688260)

一発で出てきました。

`has_and_belongs_to_many` を使うのがキーだったようです。

## 何が言いたいか
プログラミングの初心者がつまづくのはググり方もあると思います（初心者でもないのにハマることもありますが…）。

そこで、ググる時のコツは

- 根本的に何をしたいかを考える（タグ機能 -> 他対他、 HTMLエスケープ -> 置換　など）
- 何のことを調べたいかを考える（言語なのか？フレームワークなのか？IDEなのか？フロントエンドなのか？）

なのじゃないかなぁと思います。

こういうのも気をつけよう！というのがあったら教えてください。

さて明日、６日は[@KawakawaRitsuki](http://qiita.com/KawakawaRitsuki)（ごっちゃん！！）の[Crashlyticsを活用しよう](http://qiita.com/KawakawaRitsuki/items/d93e3c5add172b2dd4ae)です！
Android...ですかね？僕はWebにしか明るくないのですが楽しみです！

お楽しみに！
