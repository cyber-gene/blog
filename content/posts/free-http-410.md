---
title: "いつかサーバーを閉じるとき 〜お金をかけずに 410 を返す方法〜"
date: 2020-12-09T01:43:04+09:00
draft: false
---

この記事は Fediverse Advent Calendar 2020 9日目の記事です。

昨日の記事は、Fediverse でおなじみのコーヒー豆屋さんNelson Coffee Roasterさんの仙台でおすすめのお店記事でした。

Nelsonさんとこからは、僕も何度か豆を買わせていただいてます。仙台行ってみたい。

[Fediverseでお世話になっている豆屋です｜まめや｜note](https://note.com/ncr/n/n852581e69a46)

今日の記事は、サーバーを閉じる時のお話です。

ActivityPub サーバーをやってきたけど、いろんな理由で閉じたい… と言うことがあると思います。

連合しているサーバーにできるだけ影響を与えず、かつ可能な限りお金をかけずにサーバーを閉じる方法を書きます。

とりあえず勢いで書きました。あとで追記するかも…

## tl;dr

- Netlify を使って、無料で 410 Gone を返すヤツを作った
- tootctl self-destruct はどう動くか知らん

## 410 を返せとは言われるけど

サーバーを閉じたいと誰かに相談すれば、だいたい「しばらくの間は410 Goneを返した方がいい」と言われるわけですが、410 Goneを返すにもサーバーが必要なわけで、特に経済的な理由で閉じたいとき解決になってない、という話があります。

410 Gone をお金をかけずに返す方法があれば、そういう苦難から解放されるわけです。

それ、Netlify を使えば無料で出来ます。

## 無料で 410 Gone を返すヤツを作る

Netlify という静的なサイトを無料でホスティングするサービスがあります。

こいつを活用すると、閉鎖したサーバーに来たリクエストに対して 410 Goneを返すことができます。しかも**無料**です。

デプロイするだけでそれができるリポジトリを作りました。

[cyber-gene/netlify-410](https://github.com/cyber-gene/netlify-410)

### 使い方

1. リポジトリをフォークしましょう。
Netlify にデプロイするには、自分のリポジトリである必要があります。

2. Netlify でサイトを作る


    [Netlify](https://www.netlify.com/)

    アカウントとかを作って、New site from Git を押す

    ![Netlify overview](https://blog.cyber-gene.com/netlify-overview.png)

    GitHub 等のアカウントと連携させる画面が出てきます。Forkしたリポジトリがあるサービスを選択して連携させます。

    ![Netlify step 1](https://blog.cyber-gene.com/netlify-step1.png)

    連携させると、リポジトリを選択するのが出てくるので、netlify-410 を選択

    ![Netlify step 2](https://blog.cyber-gene.com/netlify-step2.png)

    ビルド設定はそのままで動きます。Deploy siteを押すとサイトができます。

    ![Netlify step 3](https://blog.cyber-gene.com/netlify-step3.png)

    ドメインの設定をします。

    Site setting → Domain management → Custom domains からドメインを追加します。

    追加したら、お使いのDNSサービスでCNAMEレコードを作り、ドメインへのアクセスがNetlifyに向かうように設定します。

    それをNetlify側が確認できたら、SSLの証明書とかを勝手に作ってくれます。らくちん！

    ※ 自分のドメインでアクセスできるようになるまでしばらく時間がかかります。気長に待ちましょう。

    このへんが全部終わったら、Webブラウザからアクセスしてみましょう。

    こんな画面が出ていればOKです！

    ![Netlify step 4](https://blog.cyber-gene.com/netlify-step4.png)

## 本当に410返してんの？

Webブラウザの開発者ツールを使って、本当に410を返してるのか見てみましょう。ステータスが410になっていますね？

![Netlify confirm 1](ttps://blog.cyber-gene.com/netlify-confirm-1.png)

他のサーバーから配信を受ける /inbox も見てみましょう。410なのでオッケー。

![Netlify confirm 2](https://blog.cyber-gene.com/netlify-confirm-2.png)

これで、410を無料で連合しているサーバーに返すことができます。

## で、いつまで410返したらいいの？

ドメインの有効期限があるうちは、410返しとけばいいと思います。

ドメインを更新してまで410を返すべきとは、僕は思いません。無理の無い範囲でやっていけばいいと思います。