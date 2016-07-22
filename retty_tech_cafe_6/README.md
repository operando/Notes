## Inside Android N

* Retty Tech Cafe#6


## Android N Developer Preview 5

* Developer Preview 5 includes near-final system images


## API Diff

## API Diff

* API Differences between 23 and 24
* https://developer.android.com/sdk/api_diff/24/changes.html
* https://developer.android.com/sdk/api_diff/24/changes/jdiff_statistics.html

## Good-bye android.test package

## Good-bye android.test package

* Deprecated in API level 24

## Good-bye android.test package

* 😡

## Good-bye android.test package

* https://developer.android.com/sdk/api_diff/24/changes/pkg_android.test.html
* https://developer.android.com/sdk/api_diff/24/changes/pkg_android.test.mock.html
* https://developer.android.com/sdk/api_diff/24/changes/pkg_android.test.suitebuilder.annotation.html


## Good-bye android.test package🔥🔥🔥

* 生き残りは少しだけいる😊
* Android Testing Support Libraryにしろって話
 * https://developer.android.com/topic/libraries/testing-support-library/index.html
* ちなみ僕は最近MockCursor使ってテストコード書きました😢
 * MockCursorはDeprecatedでーす🔥🔥🔥🔥🔥🔥🔥🔥🔥

## android.provider

## android.provider.Settings.Global.BOOT_COUNT

* Settings.Global.BOOT_COUNT
 * 起動した回数を取得できる
 * うん、で？？ なAPI
 * https://developer.android.com/reference/android/provider/Settings.Global.html#BOOT_COUNT


## android.provider.Settings.ACTION_WEBVIEW_SETTINGS

* Settings.ACTION_WEBVIEW_SETTINGS
 * Allows user to select current webview implementation.
 * 例えばChrome Stable,Beta,Devが入ってた場合、実装を選べるってことみたい
 * https://developer.android.com/reference/android/provider/Settings.html#ACTION_WEBVIEW_SETTINGS


## android.text.Html

* Html.toHtml method deprecated
* Html.fromHtml method deprecated
* https://developer.android.com/sdk/api_diff/24/changes/android.text.Html.html
* optionを引数で追加したメソッド使えってことらしい


## Key Developer Features

## Data Saver

* システムがバックグラウンドで行われる通信をブロックする
* フォアグラウンドでのデー使用をなるべく抑える
* 特定のアプリだけホワイトリストに入れて通信のブロックをさせないこともできる
* https://developer.android.com/preview/features/data-saver.html

## Data Saver

* Data Saverの設定取得・変更監視できる
* ホワイトリストへの追加を要求できる
* 実装簡単だった
* フォアグラウンドでのデータ使用量はなんかドキュメント読んだ感じ開発者に任せます的な勢いだった
 * 要はData SaverがONかどうか確認して、ONならデータ使用量を抑える 努力 をしてね💪
 * おい、開発者の善意に委ねられるってことか...


## Data Saver Sample

* https://github.com/operando/Data-Saver-Sample


## さて、つついてみるか

* Data Saver設定画面にIntentで飛べるか??
 * 今のところの情報じゃ無理っぽい
 * SettingsのActionはもちろん提供されてない
 * 画面がFragmentだし無理そう
 * com.android.settings.datausage.DataSaverSummary
* Data Saverのホワイトリストがどこで管理されているのか
  * netpolicy.xmlかなー多分
  * エミュレーターでもshellで入っても全然Fileが見れないので厳しい...


## SQLite


## SQLite

* Android Nに入ってるSQLiteのversionを調べてみた
* 実は部分的にAOSPのコミットが公開されている


## AOSP changelog

* http://www.androidpolice.com/android_aosp_changelogs/android-m-preview-2-to-android-n-preview-1-AOSP-changelog.html
* http://www.androidpolice.com/android_aosp_changelogs/android-n-preview-1-to-android-n-preview-2-AOSP-changelog.html
* http://www.androidpolice.com/android_aosp_changelogs/android-n-preview-2-to-android-n-preview-3-AOSP-changelog.html
* http://www.androidpolice.com/android_aosp_changelogs/android-n-preview-3-to-android-n-preview-4-AOSP-changelog.html


## SQLite

* 調べてみた感じ SQLite 3.9.3 みたい
* コミット
 * https://android.googlesource.com/platform/external/sqlite/+/253ed64ded244ef3d8a7226efb812e7989bc8026


## SQLite 3.9.x

* json1 extension
* Full Text Search version 5 (FTS5)
* etc.

## SQLite 3.9.x

* 残念です...

## SQLite 3.9.x

* 今紹介した機能はAndroidのSQLiteでは使えません

## SQLite 3.9.x

* ＼(^o^)／


## Why?

* それぞれの機能を使うためにflagをenableにしないといけない
* なるほど...されてない...
* https://android.googlesource.com/platform/external/sqlite/+/master/dist/Android.mk#9


## ワンチャン俺の見間違えだろ...

* Android NのSQLiteでJSON1 Extension試してみた
* https://github.com/operando/Try-Android-N-The-JSON1-Extension

## 結果

* ＼(^o^)／

## JSON1 Extensionを有効にしたSQLiteをbuildする

* JSON1 Extensionを有効にしたSQLiteをbuildする
* Not Android!!
* http://qiita.com/operandoOS/items/4dc7754a23ad5ab58615

## EasterEgg

* ねこあつめ homage EasterEgg🐱

## 🐱

* 🐱

## Android N EasterEgg Neko Atsume Launcher

* Setting Tile長押しだるい🐱
* はやくねこ見たい🐱🐱

## Android N EasterEgg Neko Atsume Launcher

* https://github.com/operando/Neko-Atsume-Launcher


## Reddit ANA(Ask us Anything)

* We’re on the Android engineering team and built Android Nougat. Ask us Anything!
* https://www.reddit.com/r/androiddev/comments/4tm8i6/were_on_the_android_engineering_team_and_built/
* 終わってるけど...

## 気になったPost

* API 24 brought partial introduction of Java 8 types and methods. There are highly noticeable omissions from this addition such as java.time.* (JSR 310), java.lang.invoke.*, and convenience methods on String (like join). There also continues to be an absence of types and methods from Java 7 like java.nio.path.*.
As Android's standard library deviates further from properly mirroring the JDK, what are the views of the platform team with regard to these omissions? Are you concerned about the growing difficulty of the use and development of non-Android-specific libraries moving forward (whether they're pure-Java or want to target both the JVM and Android)?
* https://www.reddit.com/r/androiddev/comments/4tm8i6/were_on_the_android_engineering_team_and_built/d5ieg6r

## 回答

* Anwar: We plan to support more of the Java 8 programming language spec in future releases. We have to prioritize what we spend our time on as we often have to optimize these implementations quite a bit for mobile, so these sometime roll out over multiple releases. In general, we’re shortening the lag between platform support for new language spec.
