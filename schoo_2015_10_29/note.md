# [はじめる前に知っておきたいAndroidアプリ開発のポイント](https://schoo.jp/class/2898)

https://schoo.jp/class/2898

* Androidアプリ開発を始めるための最初の一歩
* Androidアプリ開発の種類・特徴と大まかな開発の流れ
* 時代に合ったAndroidアプリを開発する方法

この授業では未経験でAndroidアプリを作ってみたい・仕事にしたい、開発に興味がある方々を対象に、Androidアプリ開発はどんなことをするのか、始めに何から取りかかればよいか、
といったさまざまな疑問点、不安点をクリアにすることで、受講生のモチベーションを高めることを目的とした講義形式の授業です。

様々なAndroidアプリ開発から得られた経験を元に、はじめる前に知っておきたい大事なポイントを一つ一つ学んでいきましょう。

---

# 自己紹介

* 岡野 忍 (@operandoOS)
* 株式会社メルカリ
* Androidのコードを読む勉強会を運営
 * まったりAndroid Framework Code Reading

---

# この授業について - 対象

* これからAndroidアプリを作ってみたい、興味がある人
* Androidアプリ開発を仕事にしたい人

---

# この授業について - 目的

* Androidアプリ開発はどんなことをするのか、始めに何から取りかかればよいか、といったさまざまな疑問点、不安点をクリアにする

---

# Androidアプリ開発を始めるための最初の一歩

## Androidについて

* Googleが提供しているモバイル向けOS
* 最新バージョンは6.0
* アプリ開発するために使用する言語は基本的にはJava
* オープンソースなOSで、誰でも開発できる
* 様々なデバイスで動く TV,Auto,Watch,Mobile...

---

## Androidのバージョンの特徴

* 各バージョンごとにコードネームが付いてる
* コードネームは、菓子の名前が付いてる
* コードネームやバージョンで呼ぶかは人それぞれ
* 同じコードネームにMR(Maintenance release)が付くこともある
* 最新バージョン6.0はAndroid M(Marshmallow)と呼ばれる

![](images/android_version_table.png)

---

## Androidのバージョン別シェア率

* 表を見るかぎりでは、主流なバージョンは4.4(KitKat)
* 近頃シェア率が増えてるのは5.0や5.1(Lollipop)
* 今後増えるのは最近リリースされた6.0(Marshmallow)
* しかし、より広いバージョンに対応したアプリ開発が求められる

![](images/android_version_table.png)

![](images/android_version_graf.png)

[Dashboards](https://developer.android.com/about/dashboards/index.html)

---

## Android Studio

* Androidアプリ開発をする上での統合開発環境
* 最新の安定バージョンは1.4
* EclipseよりAndroid Studioを使うべき
* Eclipse向けのサポートは今年中で終了予定

* Android Studio最速入門～効率的にコーディングするための使い方
 * http://gihyo.jp/dev/serial/01/android_studio

![](images/AndroidStudio.png)

---

## Androidアプリ開発の初歩を学ぶ方法

* 入門書で学ぶ
 * [Android学ぶ上での書籍について](http://hack-it-iron.hatenablog.com/entry/2015/03/22/195939)
* サンプルコード等で学ぶ
* とにかく何かアプリを作ってみる

サンプルコード等で学ぶ方法についてスポットを当てる

---

## Androidアプリ開発の初歩を学ぶ方法 -  サンプルコード等で学ぶ

* 公式に用意されているサンプルから学ぶ
 * 網羅的に学べる + 細かい仕様まで説明が書かれている部分があるのでオススメ
 * [Android Training](http://developer.android.com/training/index.html)
 * [API Guides](http://developer.android.com/guide/index.html)
 * [Google Samples](https://github.com/googlesamples?utf8=%E2%9C%93&query=android)

* mixi-inc/AndroidTraining
 * Javaの基礎からアプリ開発を行う場合オススメ
 * https://github.com/mixi-inc/AndroidTraining

---

## 開発を始めるにあたりアプリをどこで動かすか

* エミュレータ
 * Google APIs Emulator - [Google Play Services](https://developers.google.com/android/)の機能も試せるエミュレータ
 * [Genymotion](https://www.genymotion.com) - 標準ではGoogle Play Servicesの機能が試せない。操作速度が早い
 * まず、開発を始めたいならエミュレータがオススメ
* 実機(Android 端末)
 * 実際にアプリを使う人と同じ感覚で開発したアプリを試せる
 * エミュレータではわかりづらい機種依存や操作感の違いがわかる
 * 色んな人に使って欲しいアプリを開発したい場合は実機がオススメ

---

## 日々どこからAndroidの情報を仕入れているか

* [Qiita tags Android](http://qiita.com/tags/android)
 * Androidに関するテックネタが日々投稿されている
* [Android Developers Blog](http://android-developers.blogspot.jp/)
 * Androidの最新情報など、開発に重要な記事が投稿されている
* [Android Weekly](http://androidweekly.net/#latest-issue)
 * Androidの情報を週一でまとめてくれているサイト
* [Android Developers Backstage](http://androidbackstage.blogspot.jp/) , [Fragmented](http://fragmentedpodcast.com/)
 * 海外のAndroid開発が行っているPodcast

---

## その他アプリ開発に役立つもの

* OS標準のデバッグオプション
* Github上で公開されているOSSのアプリ
* Android セキュアコーディングガイド

---

# Androidアプリ開発の種類・特徴と大まかな開発の流れ

---

## Androidアプリ開発の種類

* アプリケーション開発
 * 自社アプリ、受託アプリ、個人アプリ
* アプリケーション開発以外もある
 * Android OSそのものの開発
 * 新しいスマートフォン開発

---

## Androidアプリ開発の特徴

* 少人数での開発が多い(経験則)
* 幅広い端末に対応したアプリ開発が求められる
 * OSバージョン
 * 解像度
 * ハードウェア(RAM,GPU...)
* リリースサイクルが早い
* 大規模な開発がある(OSそのものの開発等)

---

## Androidアプリ開発の大まかな開発の流れ

例) メルカリの開発フロー

![](images/flow.png)

このフローを継続的に行う

---

## 企画・デザイン

* 新機能や機能改善を企画する
* デザインも並行して進める
* 開発によってこの部分の作業はどこが受け持つか変わる
 * 企画・デザインと開発は別々の会社で行う等...

---

## 開発

* アプリの機能を実装する
 * 複数解像度の端末で確認する(スマホ、タブレット)
 * デバッグメニューを使って開発を効率化
 * テストコードを書く(JUnit,Spoon)
 * Github上でCode Reviewする
* 開発したものを社内に配信する
 * [DeployGate](https://deploygate.com)

---

## QA

* 開発したアプリのテストを行う
* 基本的にはテスト専門のエンジニアがテストを行う
 * 開発規模によっては、実装した人がそのままテストも行う
* テスト専門の会社にテスト作業をお願いするケースもある
 * Androidは端末が多く、自社にテストしたい端末がない場合等...

---

## リリース

* 開発したアプリをGoogle Playにリリースする
* 段階的公開を利用するケースもある
 * 大きな変更を行った場合に致命的な問題が全ユーザに影響しないため
 * 段階的に新しい機能の利用率を見る
 * https://support.google.com/googleplay/android-developer/answer/3131213#staged
* Google Playのアプリがアップデートされたか監視する
 * [tonkotsu](https://github.com/operando/tonkotsu)

---

## 分析

* リリース後に致命的な問題が発生していないか
 * [Crashlytics](https://fabric.io/kits/android/crashlytics)
 * Google Playのレビューをチェック
* 次の企画に活かすため様々なログを分析する
* アクセス解析のサービス
 * Google Analytics
 * Mixpanel

---

# 時代に合ったAndroidアプリを開発する方法

---

## Material Design

* Googleが発表したデザインガイドライン
* あらゆるスクリーンサイズ・プラットフォームで統一したもの
 * ビジュアル、モーション、インタラクション
* Material Designを採用するアプリが増えている
* AndroidのデザインそのものがMaterial Designに最適化されている
* Material Designに適したiconが提供されている

---

## Android開発者のカンファレンスに参加する

* Android開発の様々な事例・ナレッジを知ることができる
* 日々開発で疑問に思っていることなど、色んなエンジニアと情報交換ができる
* 定期的に開催している勉強会にも参加してみる

* 日本のカンファレンス
 * [ABC](http://abc.android-group.jp/2015s/)
 * [DroidKaigi 2015](https://droidkaigi.github.io/2015/),[DroidKaigi 2016](https://droidkaigi.github.io/2016/)

* Globalカンファレンス
 * [Droidcon](http://droidcon.com/)
 * [Google IO](https://events.google.com/io2015/)

---

## Android 6.0 / M(Marshmallow)

* 大きな変更が加わっているAndroidの最新バージョン
 * Runtime Permissions,Doze,App Standby,Auto Backup,Fingerprint Authentication
* よりユーザ目線でアプリを作る必要がある
 *  セキュリティ、省電力、Backup & Sync
* 特に新しいRuntime Permissionsの仕組みは開発者にとっても大きな変更
 * 今まではインストール時にPermission(権限)の許可を得ていた
 * MからはユーザがPermissionを必要とする操作をした際に許可を求める

---

# まとめ

---

## まとめ

* Androidの基礎を学ぶ方法・情報収集の方法を知り、継続的に知識を身に付ける
* Android開発の種類・特徴を理解し、自分にあった開発をする
* 時代に合ったAndroidアプリを開発ために新しい情報を積極的学ぶ

---


## アプリに求められる要件

* PCサイトの方が便利
→そう要らせないようなアプリにする

* ブラウザとアプリの体験が全く同じ
→そうしない。mobileに最適化したものを

「アプリらしい」体験の例
* オフラインで使える
* Webサイトより早く目的の情報にたどりつける
* 動作や表示が軽快で操作感が気持ち良い

とにかく色んなアプリを見てみる
* google playの最近のアプリやTopアプリ
* JPだけでなく、他の国のStoreも見る

アプリのライセンスを見る
* どんなライブラリが使われているか

---

## Google Play Services

https://developers.google.com/android/

---

## Material Design

Top
https://design.google.com/

Introduction
https://www.google.com/design/spec/material-design/introduction.html

Material icons
https://www.google.com/design/icons/

---

# TODO

* 時間内で全て書くことは難しいので、よくある質問等に対する答えをどこかにまとめる
 * 話す内容は重視したい部分だけに絞る

* 入門書で学ぶことの難しさをブログに追記 or 別記事で書く。Mが出てるとかASの更新が早いとか・・・

* Developer Option

# 初めてやる時に知りたいこと

* その分野でどんなものが流行っているのか
* どこから情報を得ているのか
* 開発環境について
* 基礎の勉強法・出だし。どこにいい教材があるかどうか

# おまけ

---

## OSSの力を借りる

[iosched](https://github.com/google/iosched)

[santa-tracker-android](https://github.com/google/santa-tracker-android)

[Rebuild](https://github.com/rejasupotaro/Rebuild)

[WordPress-Android](https://github.com/wordpress-mobile/WordPress-Android)

[wordpress-mobile](https://github.com/wordpress-mobile)

---

## Gradle

http://gradle.org/

http://google.github.io/android-gradle-dsl/

http://google.github.io/android-gradle-dsl/current/

http://mixi-inc.github.io/AndroidTraining/introductions/1.05.how-to-build-for-gradle.html

* https://gradle.org/getting-started-android/
* http://wasabeef.jp/android-gradle/
* http://tools.android.com/tech-docs/new-build-system/user-guide

---

## 余談

スマホの普及率 総務省のデータを参考に

過去1カ月のアクティブなAndroid端末は14億台
アプリストア「Google Play」のアクティブユーザーは10億人以上

http://www.itmedia.co.jp/news/articles/1509/30/news074.html

Multi-device Applications sample
https://github.com/googlesamples/android-UniversalMusicPlayer

---

## 複数端末でのテストの話

最近機種や最近のOSには、出来る限り新しいのが出たらすぐに対応する
ダウンロードしてから、「あなた端末は非対応です」はとてもよくないの
対策として、ダウンロードできないようにする

---

## Other

評価大事


デザイナーが決めたUIをどんなViewを使用して実現するのか
* いっぱいViewある
* バージョンごとに見た目が違う
* 解像度がばらばら

技術的判断

---

## 余談

Google Play アプリ ポリシー センター
https://support.google.com/googleplay/android-developer/answer/4430948?hl=ja

Core App Quality
http://developer.android.com/distribute/essentials/quality/core.html

Essentials for a Successful App
http://developer.android.com/distribute/essentials/index.html

---
