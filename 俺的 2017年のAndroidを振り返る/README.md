# 俺的 2017年のAndroidを振り返る

## ハッシュタグ

* [#opedroid](https://twitter.com/search?f=tweets&q=%23opedroid)

## 自己紹介

* operandoOS
* Androidのアプリを作ってます


## なぜ発表を配信するのか？

* より多くの人に発表を 見て・聞いてもらうため


## なぜ発表を配信するのか？

* IT業界では各地で日々勉強会が開かれており 色んな人が自身の知見を発表しています
* 私も発表してる一人です


## なぜ発表を配信するのか？

* 発表するはいいけど、資料作りが大変だったり、資料に 書かれなかったことを発表の場で話したりします
* なので、資料に書かず話してしまうと後々公開される資料を見ても、どんなことを話したのかわかりづらい場合があります
* もちろん資料をちゃんと作っている方のは、資料だけでも理解できます
* しかし、私は資料作るのが下手くそです


## なぜ発表を配信するのか？

* でも、知見は誰かのためになるかもしれないので共有したいのです！
* なので、資料はざっくり作って、後は話してしまって、それを録画なりして公開すればええんでないかと思いました
* 大きなカンファレンスとかでは、実際に発表の録画が公開されることもあります


## 詳しく

* より多くの人に発表をみて・きいてもらうために
  * https://note.mu/operando_os/n/n0ff7064fbc03


## 意見、要望、質問など

* Twitterのハッシュタグに書いていただければ 嬉しいです
* [#opedroid](https://twitter.com/search?f=tweets&q=%23opedroid)


## 2017年 ざっくりAndroid全体で起きたこと振り返り

* ※書いてあること、話したことがすべてではないです
*  ※個人的に印象に残ってることだけ書いてます


### Android Announces Support for Kotlin

* https://android-developers.googleblog.com/2017/05/android-announces-support-for-kotlin.html
* Kotlin
  * https://kotlinlang.org/
* KotlinConf 2017
  * https://www.kotlinconf.com/


### Android O

* https://developer.android.com/about/versions/oreo/index.html
* 来年はO対応必須っぽい流れになってるので頑張ろうな！
* Push通知めっちゃ運用してるアプリはNotification Channel大変だからな！
  * それ以外はアプリによって対応難しいのはまちまち


### Improving app security and performance on Google Play for years to come

* https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html
* アプリのセキュリティとパフォーマンス向上を目指したい
* target API level上げて対応しないアプリはいくつか問題あるって話
  * パーミッション取り放題、バックグラウンドで通信したい放題 etc.
  * ユーザの体験も悪い
* だからtarget API level最新にしてね！とか64bit対応してね！って感じ


### Target API level requirement from late 2018

* 以下のスケジュールで要求するtarget API level以上にしないとダメだからって話
  * 2018年8月 新規でリリースするアプリはtarget API leve 26(Android 8.0)以上をターゲットにする
  * 2018年11月 既存のアプリのアップデートにはtarget API leve26以上をターゲットにする
  * 2019年以降 毎年targetSdkVersion上げていこうな！

### Target API level requirement from late 2018

* 既存のアプリでもうリリースする予定がないアプリは影響ないよ
* minSdkVersionじゃないから誤解しないでね！
  * targetSdkVersionだからね！


### 64-bit support requirement in 2019

* 詳しくないので割愛
* Native Libraryを含まないアプリは影響ない
* とはいえ、いつNative Library使うかわからんので頭の片隅に入れておく方がいい


### Android Studio 3.0

* 色々便利になったね！
* https://android-developers.googleblog.com/2017/10/android-studio-30.html
* まだちゃんと全部使いこなせてない...


### Support Library

* 26から minimum SDK versionが14になったね
* Recent Support Library Revisions
  * https://developer.android.com/topic/libraries/support-library/revisions.html


### Google Play services

* 10.2から minimum SDK versionが14になったね
* Release Notes
  * https://developers.google.com/android/guides/releases
* 2系対応お疲れ様でしたの年っぽい


### Android Architecture Components

* 来年も盛り上がりそう
* 日々改善されてもっといいLibraryになりそう
* https://developer.android.com/topic/libraries/architecture/index.html


### Android Architecture Components

* Architecture Components 勉強会 第1回目 Lifecycles
  * https://speakerdeck.com/yanzm/architecture-components-mian-qiang-hui-di-1hui-mu-lifecycles
* Architecture Components Playground
  * https://github.com/operando/Architecture-Components-Playground


### ARCore

* https://developers.google.com/ar/
* https://developers-jp.googleblog.com/2017/08/arcore-android.html
* ARCore 取扱説明書 (ARCore Developer Preview2に対応)
  * https://qiita.com/taptappun/items/a5337d29a43d5d673c7f


### Good Bye Jack、Welcome desugar

* Java 8サポートにdesugarを使うようにしたぞい！
  * 昔はそこら辺Jackというものを使う話が進んでたけど移行コスト高いから？やめたみたいは感じ
* https://developers-jp.googleblog.com/2017/03/future-of-java-8-language-feature.html
* https://android-developers.googleblog.com/2017/03/future-of-java-8-language-feature.html
* https://developer.android.com/studio/write/java8-support.html
* https://android.googlesource.com/platform/external/desugar/


### Next-generation Dex Compiler Now in Preview

* D8ですね
* Dexコンパイルが早くなる + Dexのサイズが小さくなる
* Android Studio 3.1からデフォルトでD8が使われるようになる予定
* R8は来年やっていき！っぽい
  * https://r8.googlesource.com/r8


### Next-generation Dex Compiler Now in Preview

* https://android-developers.googleblog.com/2017/08/next-generation-dex-compiler-now-in.html
* https://developers-jp.googleblog.com/2017/08/next-generation-dex-compiler-now-in.html


## ここから 私のやったこと振り返り

### DroidKaigi 2017で発表したねー

* 2セッション話しました
* Androidアプリ開発の体力づくり💪
* コマンドなしでぼくはAndroid開発できない話
* https://droidkaigi.github.io/2017/


### Androidアプリ開発の体力づくり💪

* 資料
  * https://speakerdeck.com/operando/androidapurikai-fa-falseti-li-dukuri
* 資料の書き起こし
  * https://github.com/operando/DroidKaigi/tree/master/2017/muscle_android
* これ動画残ってないの残念...
* 話し直してYouTubeで配信しようかな


### コマンドなしでぼくはAndroid開発できない話

* 資料
  * https://speakerdeck.com/operando/komantonasitehokuhaandroidkai-fa-tekinaihua-1
* 資料の書き起こし
  * https://github.com/operando/DroidKaigi/tree/master/2017/no_command_no_life
* 録画
  * https://academy.realm.io/jp/posts/droidkaigi17-android-development-with-commandline/


### とあるアプリを作りました

* 資料
  * https://speakerdeck.com/operando/merukari-kaurufalsekai-fa-qi-ninaru-narahua-sou
* 今年も一からアプリ作ったので色々学べた
* shibuya.apk #17
  * https://shibuya-apk.connpass.com/event/62499/


### OnActivityResult熱高まる！！

* OnActivityResult - おまえら！もうonActivityResultでswitchとif書く時代は終わりだぞ！
  * https://speakerdeck.com/operando/onactivityresult-omaera-mouonactivityresultdeswitchtoifshu-kushi-dai-hazhong-waridazo
* コントリビュートもしたな
* kyobashi.dex #4
  * https://rmp-quipper.connpass.com/event/47555/


### ConstraintLayout はじめました

* 頑張ってます
* ConstraintLayout がもたらすパフォーマンスのメリットを理解する
  * https://developers-jp.googleblog.com/2017/09/understanding-performance-benefits-of.html
* TechBooster's Playgroundの第1章 はじめてのConstraintLayout読みつつ触り始めてる
  * https://techbooster.booth.pm/items/715946


### アプリ間連携の仕組み について話しました

* 資料
  * https://speakerdeck.com/operando/merukaritosouzouapurifalse-apurijian-lian-xi-falseshi-zu-mi-in-2017-summer
* とりあえず知見の共有活動です
* アプリいっぱいだして連携していこうな！


### VectorDrawable めっちゃ使った

* 新しいアプリの画像はほとんどVectorDrawableにした
* https://developer.android.com/guide/topics/graphics/vector-drawable-resources.html


### Retrofit ちゃんと使った

* Making Retrofit Work For You (Ohio DevFest 2016)
  * https://speakerdeck.com/jakewharton/making-retrofit-work-for-you-ohio-devfest-2016
* Converterを作るのに参考になった


### RxJava 2.0に移行した

* What's different in 2.0
  * https://github.com/ReactiveX/RxJava/wiki/What%27s-different-in-2.0
* RxJava 1.x → 2.x 移行ガイド
  * http://in.fablic.co.jp/entry/2017/04/27/110000
* そこまで大変じゃなかったかなーWikiとか読めば


### Danger 導入した

* http://danger.systems/ruby/
* 雑にいうと、Pull Requestのチェックを自動化するもの
* Lintとかも含め
* Android Danger Sample
  * https://github.com/operando/AndroidDangerSample
  * ここで試しに色々やってます


### Mobile Vision 使った

* 主にScan Barcodes使った
  * https://developers.google.com/vision/
* めっちゃ便利
* Camera APIを内部でラップしてて、カメラのハンドリングが難しい


### AsyncLayoutInflater の話したなー

* 資料なしで話したので資料は残ってない
* 非同期でレイアウトのinflateができるやつ
  * 短時間で膨大なViewを作る時には便利そう
* AsyncLayoutInflater Sample
  * https://github.com/operando/AsyncLayoutInflater-Sample


### React Nativeちょっと触ってみた

* Androiderから見るReact Native
  * https://speakerdeck.com/operando/androiderkarajian-rureact-native
* もう一度 触ってみようかな
* React.js meetup × React Native meetup
  * https://react-native-meetup.connpass.com/event/49024/


### 技術書典 3で本書きました。ちょっとね。

* Debug Menuはじめました。
  * https://techbookfest.org/event/tbf03
* (続) Debug Menuはじめました。
  * https://speakerdeck.com/operando/sok-debug-menuhasimemasita
  * 書いた内容 + おまけの話した
* shibuya.apk #19
  * https://shibuya-apk.connpass.com/event/68094/


### Androidの本で読んだもの

* Androidを支える技術〈Ⅰ〉──60fpsを達成するモダンなGUIシステム
  * http://gihyo.jp/book/2017/978-4-7741-8759-4
* Androidを支える技術〈Ⅱ〉──真のマルチタスクに挑んだモバイルOSの心臓部
  * http://gihyo.jp/book/2017/978-4-7741-8861-4
* 内容は簡単ではないけど、面白いし、Androidやっていく上で知ってるとよい


### あんどろいど ランチ はじめました

* Androidの話しながらランチしようの会
* https://android-lunch.connpass.com/
* 9月からはじめて今年5回もできた
* 来年もやっていくぞ！


### Androidもくもくやろうぜ会 はじめました

* 色んなエンジニアさんと交流したいぞ！って感じではじめた
* https://mercari-android-mokumoku.connpass.com/
* 来年もぼちぼちやっていきます


### gummi 作った

* JSON-RPC 2.0 Client Library
* https://github.com/operando/gummi
* 自作して社内アプリに組み込んでたやつをOSS化した
* めっちゃ使っる


### OpenDroid 作った

* 今開いてるAndroidのOpenGrokの別バージョンをさくっと開くChrome拡張作った
  * http://hack-it-iron.hatenablog.com/entry/2017/09/19/111520
  * https://github.com/operando/OpenDroid
* わりと自分でそこそこ使ってる
* 便利だよ！


## さいごまで ありがとうございました！
