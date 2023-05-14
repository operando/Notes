# App hibernation

## App hibernationとは？

- Android 12から入った仕組み
- 数ヶ月利用されていないアプリをシステムがhibernation stateにしますよーってやつ
- Android 11で導入されたAuto-reset permissionsを進化させたやつ

## App hibernationとは？

https://developer.android.com/topic/performance/app-hibernation


## Android11から導入されたAuto-reset permissionsを調べてみてわかったこと

https://hack-it-iron.hatenablog.com/entry/2020/12/14/081217


## hibernationがアプリに与える影響

図はる


## Making permissions auto-reset available to billions more devices

- https://android-developers.googleblog.com/2021/09/making-permissions-auto-reset-available.html


## システムがアプリを使用しているとみなすアクションはなにか？

図はる


## システムがアプリを使用しているとみなすアクションはなにか？

- https://developer.android.com/topic/performance/app-hibernation#app-usage


## ドキュメントを読んで疑問に思ったこと

- 数ヶ月利用されていないって具体的な数字は？
- どのくらいの頻度でhibernationのチェックが行われるのか？
- App usageってどうやってカウントしてるのか？
- hibernation statusなアプリの実行制御をどうやってやってるのか？


## 数ヶ月利用されていないって具体的な数字は？

- デフォルトでは90日間 = 約三ヶ月
- 結構長いね！

```java
private val DEFAULT_UNUSED_THRESHOLD_MS = TimeUnit.DAYS.toMillis(90)
```


## 数ヶ月利用されていないって具体的な数字は？

- 期間のしきい値を変更することはできる

```java
// 未使用期間のしきい値を1分に変更
adb shell device_config put permissions auto_revoke_unused_threshold_millis2 60000

// 設定できているかを確認
adb shell device_config get permissions auto_revoke_unused_threshold_millis2
```


## どのくらいの頻度でhibernationのチェックが行われるのか？

- デフォルトでは15日間隔
- （あれ...？そんな高頻度じゃないのね...😂）

```java
private val DEFAULT_CHECK_FREQUENCY_MS = TimeUnit.DAYS.toMillis(15)
```

## どのくらいの頻度でhibernationのチェックが行われるのか？

- こちらも頻度を変更することができる

```java
// 実行周期を15分に変更
adb shell device_config put permissions auto_revoke_check_frequency_millis 900000

// 設定できているかを確認
adb shell device_config get permissions auto_revoke_check_frequency_millis
```


## App usageってどうやってカウントしてるのか？

- UsageStatsManagerからのリスナーを受け取れる仕組みがあり、それでカウントしている
- ちなみにアプリが利用されているイベントが飛んできたら、すぐにhibernationからの復帰が実行される

```java
private final UsageEventListener mUsageEventListener = (userId, event) -> {
    if (!isAppHibernationEnabled()) {
        return;
    }
    final int eventType = event.mEventType;
    if (eventType == UsageEvents.Event.USER_INTERACTION
            || eventType == UsageEvents.Event.ACTIVITY_RESUMED
            || eventType == UsageEvents.Event.APP_COMPONENT_USED) {
        final String pkgName = event.mPackage;
        setHibernatingForUser(pkgName, userId, false);
        setHibernatingGlobally(pkgName, false);
    }
};
```


## hibernation statusなアプリの実行制御をどうやってやってるのか？

- わからなかった...🙏🏻


## その他 内部構造を調べてわかったこと

- Android 12ではAncroid 11でのAuto-reset permissionsの実装がHibernationPolicyとかに変わってそう
- AppHibernationServiceは管理する役割をやっている。HibernationControllerはPermissionなどの無効化などの処理をしている。管理と実行で役割が分かれているっぽい
- adb shell dumpsys app_hibernationすればAppHibernationServiceをdumpできる
- hibernationから復帰すると、ACTION_BOOT_COMPLETED とACTION_LOCKED_BOOT_COMPLETEDがbroadcastで発火する
- hibernationの情報は以下みたいな場所に保存している
  - /data/system/hibernation/states
  - /data/system_ce/0/hibernation/states


## 参考コード

- https://cs.android.com/android/platform/superproject/+/master:packages/modules/Permission/PermissionController/src/com/android/permissioncontroller/hibernation/
- https://cs.android.com/android/platform/superproject/+/master:frameworks/base/core/java/android/apphibernation/AppHibernationManager.java
- https://cs.android.com/android/platform/superproject/+/master:packages/modules/Permission/PermissionController/src/com/android/permissioncontroller/hibernation/HibernationController.kt
- https://cs.android.com/android/platform/superproject/+/master:frameworks/base/services/core/java/com/android/server/apphibernation/
- https://cs.android.com/android/platform/superproject/+/master:frameworks/base/services/core/java/com/android/server/apphibernation/AppHibernationService.java
- https://cs.android.com/android/platform/superproject/+/master:frameworks/base/services/core/java/com/android/server/apphibernation/AppHibernationShellCommand.java
- https://cs.android.com/android/platform/superproject/+/master:frameworks/base/services/core/java/com/android/server/apphibernation/GlobalLevelState.java
- https://cs.android.com/android/platform/superproject/+/master:frameworks/base/services/core/java/com/android/server/apphibernation/UserLevelState.java
- https://cs.android.com/android/platform/superproject/+/master:frameworks/base/services/core/java/com/android/server/apphibernation/HibernationStateDiskStore.java
- https://cs.android.com/android/platform/superproject/+/master:packages/modules/Permission/PermissionController/src/com/android/permissioncontroller/hibernation/HibernationPolicy.kt
- https://cs.android.com/android/platform/superproject/+/master:packages/modules/Permission/PermissionController/src/com/android/permissioncontroller/permission/service/AutoRevokePermissions.kt
- https://cs.android.com/android/platform/superproject/+/master:packages/apps/Settings/src/com/android/settings/applications/HibernatedAppsPreferenceController.java
- https://cs.android.com/android/platform/superproject/+/master:frameworks/base/core/java/android/apphibernation/AppHibernationManager.java


## App hibernationの内部構造どう調べたか

- DroidKaigi 2021 「できる！Android Framework Code Reading」
- https://speakerdeck.com/operando/dekiru-android-framework-code-reading
- https://www.youtube.com/watch?v=44ChiZXry1E&list=PLaOdaBFokChxuuWf0eXWaf5H35R1Aroaz
- こちらをぜひ参考に🙏🏻