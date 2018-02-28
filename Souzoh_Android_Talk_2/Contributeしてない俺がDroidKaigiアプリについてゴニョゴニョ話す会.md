# Contributeしてない俺がDroidKaigiアプリについてゴニョゴニョ話す会

## スライド

* https://speakerdeck.com/operando/contributesitenaian-kadroidkaigiahurinituitekoniyokoniyohua-suhui

## DroidKaigiアプリ

* https://github.com/konifar/droidkaigi2018-flutter
* https://github.com/DroidKaigi/conference-app-2018
* https://github.com/kikuchy/DroidKaigi2018iOS

## DroidKaigiアプリ

* 今日話すのはDroidKaigi/conference-app-2018


## 合わせて読みたい！

* DroidKaigi 2018 App Architecture by takahirom
  * https://speakerdeck.com/takahirom/droidkaigi-2018-app-architecture
* DroidKaigi アプリの内部を見る by tatsuhama50
  * https://www.slideshare.net/kenichitatsuhama/droidkaigi-88921455


## CircleCIのイメージは公式ので良さそう

* きっと公式イメージをFROMで指定して、rubyを入れてるのかな？

```yaml
jobs:
  build:
    docker:
      - image: punchdrunker/android-27-ruby
```

## CircleCIのイメージは公式ので良さそう

* 実は公式のイメージにもRubyは入ってる！
* https://github.com/CircleCI-Public/circleci-dockerfiles/blob/master/android/images/api-27-alpha/Dockerfile#L125-L134


## CircleCIのImage

* CircleCIが提供してるImageのDockerfileはGithubにまとまってる
  * https://github.com/CircleCI-Public/circleci-dockerfiles 
* AndroidのDockerfileももちろんあるよ
  * https://github.com/CircleCI-Public/circleci-dockerfiles/tree/master/android/images
* circleci/android:api-27-alphaのDockerfileなら以下
  * https://github.com/CircleCI-Public/circleci-dockerfiles/blob/master/android/images/api-27-alpha/Dockerfile


## ページを保持してくれ！頼む！

* SearchFragmentはViewPagerとTabLayoutの構成
* Speakerタブに移動して、Sessionタブを開くとPageが再構築になる
* スクロールしてた位置変わっちゃう
* Sessionタブに切り替える時に若干もっさりする


## ページを保持してくれ！頼む！

* そこでViewPagerのoffscreenPageLimitですよね！
* offscreenのlimitを変更する
* デフォルトでは1
* デフォルト 1なので、Speakerタブに移動するとSessionタブが消える


## ページを保持してくれ！頼む！

* Speakerタブに移動してもSessionタブが消えないようにしたい！
* offscreenPageLimitを2にすればよさそう！
* binding.sessionsViewPager.offscreenPageLimit = 2


## ページ保持できた！

* やったね！
* ページ数がどれくらいかわかってるなら最適化するほうがいい
* ページの保持する分、メモリを使うので注意


## ViewPager#offscreenPageLimit

* ViewPager#offscreenPageLimit API Document
* https://developer.android.com/reference/android/support/v4/view/ViewPager.html#setOffscreenPageLimit(int)


## OkHttpのLogはリリース版は出さない方がいい！

* TODO


## つまり


## OkHttpのLogはリリース版は出さない方がいい！

* リリース版のアプリではHttpLoggingInterceptorをInterceptorに設定しない方がいい
* またはNONEに設定する
  * https://square.github.io/okhttp/3.x/logging-interceptor/okhttp3/logging/HttpLoggingInterceptor.Level.html
* OkHttpの限らず、Logcatに流す内容はマジで気をつけてください！


## タブ タップした時にListの先頭に戻るのいいよね！

* またまたSearchFragmentさんです
* TabLayout.OnTabSelectedListenerを使って、タップイベントを取ってる
* タブがタップされると、ViewPagerにアタッチされてるFragmentにタッチイベントが伝わる
* FragmentのRecyclerViewにsmoothScrollToPosition(0)して、先頭までスクロールさせる


## タブ タップした時にListの先頭に戻るのいいよね！

* 実装簡単だけど、めっちゃ便利になるので気に入ったらみんな実装してみてね！
* メルカリ カウルでも似たような実装してるよ！


## @CheckResult Annotation

* APIとかDBとかRepositoryの戻り値とかに ちょいちょいつけられてた
* DroidKaigiアプリで使われてるところ
  * https://github.com/DroidKaigi/conference-app-2018/search?q=CheckResult&type=Code&utf8=%E2%9C%93


## @CheckResult Annotation 公式から説明引用

* “メソッドの結果または戻り値が実際に使用されているかどうかを検証するには @CheckResult アノテーションを使用します。  すべての非 void メソッドに @CheckResult アノテーションを付けるのではなく、複雑なメソッドの結果を明確にするためにアノテーションを追加します。”


## @CheckResult Annotation 公式から説明引用

* “たとえば、経験の浅い Java デベロッパーは、<String>.trim() が元の文字列から空白を削除するものだと勘違いすることがあります。メソッドに @CheckResult アノテーションを付けると、呼び出し元で <String>.trim() の戻り値を一切使用していない場合に警告が出ます。”


## @CheckResult Annotation

* https://developer.android.com/studio/write/annotations.html#check-result
* https://developer.android.com/reference/android/support/annotation/CheckResult.html


## 国際化を意識したNotification Channel

* Notification Channelとは？
* 通知をChannelという単位にカテゴライズしよう！的な
* 詳しくはドキュメント読んで！
* https://developer.android.com/guide/topics/ui/notifiers/notifications.html#ManageChannels


## なぜ国際化を意識しないといけないのか

* NotificationChannelをnewする時のnameはユーザに見えるチャンネルの概要的なやつ
* これ設定アプリとかがresourceから出すわけではない
* Channelを生成した時の言語のリソースに依存するってこと
* 日本語設定でChannelを作成して、英語設定にするとChannelの説明が日本語になるという問題が起きる


## 国際化を意識したNotification Channel

* 言語設定が変わったタイミングでChannelを作り直す
* android.intent.action.LOCALE_CHANGEDをBroadcastReceiverでフックする
* そのタイミングでチャンネルを作り直す
* NotificationManager.createNotificationChannelは、同じChannel IDに対して再度生成を呼び出すとnameやdescriptionを更新できる


## 国際化を意識したNotification Channel

* このことちゃんとドキュメントにも書いてある
* https://developer.android.com/reference/android/app/NotificationChannel.html#NotificationChannel(java.lang.String, java.lang.CharSequence, int)


## 国際化を意識したNotification Channel

* "You can rename this channel when the system locale changes by listening for the ACTION_LOCALE_CHANGED broadcast."


## 国際化を意識したNotification Channel

* Incomplete Implementation of Notification Channel
  * https://github.com/DroidKaigi/conference-app-2018/pull/406
* 【Android O】通知チャンネルを国際化する
  * https://qiita.com/Shiozawa/items/095e77d38fc00681e898


## DrawerLayout

* よくできてる！
* しかもどの画面でもDrawer出せるようになってる！
* Googleが推奨??してる動くなのでよい


## resValue

* GradleからResouceも生成できる！
* 便利！
* `resValue("string", "app_name", "DroidKaigi 2018 Dev")`


## resValue

* app/build/generated/res/resValues/debug/values配下に生成される

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>

    <!-- Automatically generated file. DO NOT MODIFY -->

    <!-- Values from the variant -->
    <string name="versionInfo" translatable="false">1.0.0-debug</string>
    <!-- Values from build type: debug -->
    <string name="app_name" translatable="false">DroidKaigi 2018 Dev</string>

</resources>
```

## BindingAdapter

* TextBinding.ktになるほどーっていうのあった！
* android:textでDate渡せるようにする実装！

```kotlin
@BindingAdapter(value = ["android:text"])
fun TextView.setDateText(date: Date?) {
    date ?: return
    text = date.toReadableDateTimeString()
}
```


## あとなんかアプリクラッシュしたｗ

```java
Shutting down VM
FATAL EXCEPTION: main
Process: io.github.droidkaigi.confsched2018, PID: 11577
b.a
	at io.github.droidkaigi.confsched2018.presentation.feed.a.a$a.onPreDraw(FeedItem.kt:58)
	at android.view.ViewTreeObserver.dispatchOnPreDraw(ViewTreeObserver.java:977)
	at android.view.ViewRootImpl.performTraversals(ViewRootImpl.java:2349)
	at android.view.ViewRootImpl.doTraversal(ViewRootImpl.java:1392)
	at android.view.ViewRootImpl$TraversalRunnable.run(ViewRootImpl.java:6752)
	at android.view.Choreographer$CallbackRecord.run(Choreographer.java:911)
	at android.view.Choreographer.doCallbacks(Choreographer.java:723)
	at android.view.Choreographer.doFrame(Choreographer.java:658)
	at android.view.Choreographer$FrameDisplayEventReceiver.run(Choreographer.java:897)
	at android.os.Handler.handleCallback(Handler.java:790)
	at android.os.Handler.dispatchMessage(Handler.java:99)
	at android.os.Looper.loop(Looper.java:164)
	at android.app.ActivityThread.main(ActivityThread.java:6494)
	at java.lang.reflect.Method.invoke(Native Method)
	at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:438)
	at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:807)
```


## さらに読むと面白そうなところ

* Mapperいいっすね
* Room
  * https://developer.android.com/training/data-storage/room/index.html
* `binding.prevSession = prevSession`と`binding.nextSession = nextSession`
* RTL対応


## さらに読むと面白そうなところ

* Firebase Firestore
  * https://firebase.google.com/docs/firestore/
* Firestore offlineは無効化してる
  * in memory cacheはある？
  * https://firebase.google.com/docs/firestore/manage-data/enable-offline?hl=ja
  * いいね！オフラインの状態で開くとアプリのProcess killするまで取得できない？？
  * オンラインにしてProcessレベルでアプリ立ち上げ直すと取得できる


## 話したかったけど時間の都合上話せなかったこと

* EmojiCompat
  * Support LibraryのDownloadable FontsやEmojiCompatに対応したアプリを作ろう by takahirom
  * https://speakerdeck.com/takahirom/support-libraryfalsedownloadable-fontsyaemojicompatnidui-ying-sitaapuriwozuo-rou
* Resultパターン
* FeedItemなんかbugってる


## 話したかったけど時間の都合上話せなかったこと

* NotificationHelper.kt
  * NotificationChannelTypeよさ
* https://github.com/DroidKaigi/conference-app-2018/blob/master/app/src/main/java/io/github/droidkaigi/confsched2018/presentation/common/notification/NotificationHelper.kt
* Gradle Versions Plugin
  * 入ってるけどCIとかでは使われてないので忘れがちだけどめっちゃ便利
  * https://github.com/ben-manes/gradle-versions-plugin
* AlarmManager.setAndAllowWhileIdle + Doze
  * https://qiita.com/FumihikoSHIROYAMA/items/b1d6dbda120462d0e209


## 話したかったけど時間の都合上話せなかったこと

* Open Source Notices
  * https://developers.google.com/android/guides/opensource
  * https://qiita.com/sho5nn/items/f63ebd7ccc0c86d98e4b
* Kotlin DSL
  * https://github.com/gradle/kotlin-dsl
  * Kotlin + buildSrc for Better Gradle Dependency Management
  * https://handstandsam.com/2018/02/11/kotlin-buildsrc-for-better-gradle-dependency-management/
  * マルチ モジュールだからバージョンの管理はこうした方が楽


## 話したかったけど時間の都合上話せなかったこと

* MessageProcessorを返すのか
  * なるほどー
  * 処理をMessageProcessorごとに書けるので良さそう
  * 似たようなことをメルカリ カウルでもCustom URLでやってる

## さらにこの実装もあれば良かったー

* Runtime Permission
  * いらないなら無理にやる必要ないよね


## DroidKaigi 2018 Flutter App

* https://github.com/konifar/droidkaigi2018-flutter
* git cloneしてからAndroidビルドできねーw


## Thanks
