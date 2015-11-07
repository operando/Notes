# [shibuya.apk #4](http://shibuya-apk.connpass.com/event/21474/)

# Slide

* **[JobScheduler Code Reading](http://www.slideshare.net/shinobuokano7/jobscheduler-code-reading)**

# 資料内のリンクとかJobSchedulerの電波メモ

* [operando/JobScheduler-Code-Reading](https://github.com/operando/JobScheduler-Code-Reading)
* [Shibuya.apk #4 資料リンク](https://github.com/operando/Notes/tree/master/Shibuya.apk_4)


# JobScheduler Code Reading

# About Me

* Shinobu Okano([@operandoOS](https://twitter.com/operandoOS))
* Mercari, Inc.
* 今週で23歳になりました！ありがとうございます！
* 最近iPhone6s Plus買いました（笑）

#  [まったりAndroid Framework Code Reading #2](https://mandroidfcr.doorkeeper.jp/events/33925)

* まったりAndroid Framework Code Reading #2やります！
* メルカリでやります！きてね！
* https://mandroidfcr.doorkeeper.jp/events/33925

# Project Volta

* 簡単に言ってバッテリー消費を削減するプロジェクト
* Android Lで行われたもの
* [Battery Historian](https://github.com/google/battery-historian)
* [JobScheduler](http://developer.android.com/about/versions/android-5.0.html#Power)
* [AlarmManagerが省電力化(4.4 KitKat)](http://developer.android.com/about/versions/android-4.4.html#BehaviorAlarms)


# Google I/O 2014 - Introduction to Project Volta

* [Google I/O 2014 - Introduction to Project Volta](https://www.youtube.com/watch?v=KzSKIpJepUw)


# About JobScheduler

* Android Lから導入されたAPI
* 様々な条件のJobをスケジュールしてくれるAPI
* 使うことで消費電力を意識した実装ができる
* 開発者が頑張らなくていいAPI
* JobSchedulerはAndroid Framework Services(System Service系)
* マルチユーザ用にも設計されている(当たり前か)
* Schedulerであって、Alarmではない
* 特定の時間に実行！みたいな感じではない

# JobSchedulerの使い方

* [Android API21から追加されたJobSchedulerに 慣れていこう](http://blog.techfirm.co.jp/2015/10/19/android-api21%E3%81%8B%E3%82%89%E8%BF%BD%E5%8A%A0%E3%81%95%E3%82%8C%E3%81%9Fjobscheduler%E3%81%AB%E6%85%A3%E3%82%8C%E3%81%A6%E3%81%84%E3%81%93%E3%81%86/)
* [Using the JobScheduler API on Android Lollipop](http://code.tutsplus.com/tutorials/using-the-jobscheduler-api-on-android-lollipop--cms-23562)

# JobScheduler Sample

* [googlesamples/android-JobScheduler](https://github.com/googlesamples/android-JobScheduler)
* [operando/JobScheduler-Sample](https://github.com/operando/JobScheduler-Sample)
 * 実験用

# Auto Backup - 条件

*　バックアップは24時間ごとに行われる
* バックアップは充電中、WiFi接続、アイドル状態の3つの条件が満たされた時に行われる
* この条件(24時間,充電中,WiFi接続,アイドル状態)を制御してるのがJobScheduler
* http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/backup/java/com/android/server/backup/FullBackupJob.java

##### http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/backup/java/com/android/server/backup/FullBackupJob.java

```java
public static void schedule(Context ctx, long minDelay) {
    JobScheduler js = (JobScheduler) ctx.getSystemService(Context.JOB_SCHEDULER_SERVICE);
    JobInfo.Builder builder = new JobInfo.Builder(JOB_ID, sIdleService)
             .setRequiresDeviceIdle(true)
             .setRequiredNetworkType(JobInfo.NETWORK_TYPE_UNMETERED)
             .setRequiresCharging(true);
    if (minDelay > 0) {
        builder.setMinimumLatency(minDelay);
    }
    js.schedule(builder.build());
}
```


# scheduleの流れ

# JobScheduler.schedule(JobInfo)

* JobScheduler.schedule(JobInfo)
* プロセス間通信でJobSchedulerStub#scheduleを呼び出す
* Jobの登録ができればJobScheduler.RESULT_SUCCESSが返ってくる。登録に失敗した場合JobScheduler.RESULT_FAILUREが返ってくる。
* なので、Jobの登録ができたかどうかしっかり見てあげよう
* http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/app/JobSchedulerImpl.java#schedule

# JobSchedulerStub#schedule

* JobSchedulerStub#schedule
* enforceValidJobRequestやcanPersistJobsでJob登録ができるかどうかとチェック
* JobSchedulerService#scheduleを呼び出す
* http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobSchedulerService.java#813

# JobSchedulerService#schedule

* JobSchedulerService#schedule
* JobInfoとアプリのUIDでJobStatusを生成。Framework側ではJobInfoはJobStatusとして管理される
* cancelJob + cancelJobImplで同じJobが登録されていたらcancelする(Job ID + UIDが同じJobStatus)
* pendingとしてqueueに溜まってるJobもcancel。ActiveServices(実際に動いてるJob)を全部チェックして、同じJobがあればcancel処理を行う。

# JobSchedulerService#stopTrackingJobs

* JobStoreにある同じJobをremove
* 各StateControllerから同じJobをremove(removeするにもJobStatusが各StateControllerの条件にあっているかどうかをチェックしてる)
* 全JobStatusを管理するクラスとしてJobStoreが使われている
* http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobSchedulerService.java#190

# JobSchedulerService#startTrackingJob

* 基本的にJobSchedulerService#stopTrackingJobと逆のことする(JobStatusのadd)
* 各StateControllerにJobStatusをaddする(addするにもJobStatusが各StateControllerの条件にあっているかどうかをチェックしてる)
* mReadyToRock(JobSchedulerの準備??)がtrueになっていないと、基本的にはaddもremoveもできない
* http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobSchedulerService.java#373

# JobSchedulerService#maybeQueueReadyJobsForExecutionLockedH

* The state of at least one job has changed.(少なくとも一つのジョブの状態が変更された)
* ready(実行可能)になっているjobがいくつ存在するかで決めているっぽい
* Right now the policy is such:
* If >1 of the ready jobs is idle mode we send all of them off
* if more than 2 network connectivity jobs are ready we send them all off.
* If more than 4 jobs total are ready we send them all off.
* http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobSchedulerService.java#622

# Demo

* JobInfo.Builder#setRequiredNetworkType(JobInfo.NETWORK_TYPE_ANY)のjobを１つずつ登録
* 1個目のjobを登録したら、jobはREADYのままで止まっている(Pendingにも入ってない)
* 2個目のjobを登録したら、jobがActiveになった
* 上の条件に合致したので、jobをpending queueに移動させて、実行したのだと推測

# デバイス再起動後も動くJobが作れる

* JobInfo.Builder#setPersisted(true)でJobが再起動後も実行される
* 開発者が再起動後に自分でまたJobを登録する必要がない
* JobInfo.Builder#setExtras(PersistableBundle)で再起動後も値を引き継げる
* 素晴らしい！

#### サンプル(こんな感じ)

```java
PersistableBundle persistableBundle = new PersistableBundle();
persistableBundle.putInt("id", i);
JobInfo jobInfo = new JobInfo.Builder(i, serviceName)
        .setRequiredNetworkType(JobInfo.NETWORK_TYPE_ANY)
        .setPersisted(true)
        .setExtras(persistableBundle)
        .build();
scheduler.schedule(jobInfo);
```

# あれ？再起動後も引き継げるってことは？

* Jobの情報を再起動時のためにどこかに保持する必要がある
* ということは、ファイルとかに書き出さないとだよね？
* おぉ！これは面白そう！

# ということで、こいつ...どこかに保存してるぞ...

* /data/system/job/jobs.xml
* [JobStore](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobStore.java)というクラスがそこら辺管理してる
 * http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobStore.java
* JobSchedulerに新しいJob(Persisted = true)が追加された / Job(Persisted = true)が削除された際に、内容がSyncされる。
* 書き込み処理は主にこれ
 * http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobStore.java#WriteJobsMapToDiskRunnable

# /data/system/job/jobs.xml

```xml
<?xml version='1.0' encoding='utf-8' standalone='yes' ?>
<job-info version="0">
    <job jobid="808" package="android" class="com.android.server.MountServiceIdler" uid="1000">
        <constraints idle="true" charging="true" />
        <one-off delay="1446660001930" />
        <extras />
    </job>
    <job jobid="0" package="com.os.operando.jobschedulersample" class="com.os.operando.jobschedulersample.services.MyJobService" uid="10054">
        <constraints connectivity="true" />
        <periodic period="1000" deadline="1446590637790" delay="1446590636790" />
        <extras />
    </job>
    <job jobid="20536" package="android" class="com.android.server.backup.FullBackupJob" uid="1000">
        <constraints unmetered="true" idle="true" charging="true" />
        <one-off />
        <extras />
    </job>
    <job jobid="20537" package="android" class="com.android.server.backup.KeyValueBackupJob" uid="1000">
        <constraints connectivity="true" charging="true" />
        <one-off deadline="1446675931860" delay="1446604253797" />
        <extras />
    </job>
    <job jobid="1" package="com.android.providers.downloads" class="com.android.providers.downloads.DownloadIdleService" uid="10005">
        <constraints idle="true" charging="true" />
        <periodic period="86400000" deadline="1446675936870" delay="1446589536870" />
        <extras />
    </job>
    <job jobid="800" package="android" class="com.android.server.pm.BackgroundDexOptService" uid="1000">
        <constraints idle="true" charging="true" />
        <one-off delay="1446589526460" />
        <extras />
    </job>
</job-info>
```


# PersistableBundleの中身

* JobにセットしたPersistableBundleの中身も書き込まれます
* xmlへの書き込み処理はここでやってる
 * http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/os/PersistableBundle.java#restoreFromXml
* 書き込み処理はJobStore内から呼び出される
 * http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobStore.java#608

```xml
<job jobid="0" package="com.os.operando.jobschedulersample" class="com.os.operando.jobschedulersample.services.MyJobService" uid="10059">
    <constraints connectivity="true" idle="true" charging="true" />
    <periodic period="1000" deadline="1446648619790" delay="1446648618790" />
    <extras>
        <int name="id" value="0" />
    </extras>
</job>
```


# PersistableBundleに書き込み処理

* PersistableBundleの情報を保存する処理は、保存するXMLだけ用意してあげれば、どんなものでも使える汎用的なものっぽい。
* PersistableBundle#restoreFromXmlがhideなので、サードパーティからは使えないけど・・・。
 * http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/os/PersistableBundle.java#restoreFromXml


# RAMサイズによって同時実行できるJobの数が変わる

* System propertyのro.config.low_ram=trueの場合、**同時実行できるJobの数は1つ**
* System propertyのro.config.low_ram=faseの場合、**同時実行できるJobの数は3つ**
* ココらへん見るとJobの数についてわかる
 * http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobSchedulerService.java#77
 * http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobSchedulerService.java#352
* ro.config.low_ramはAndroid4.4で導入された、メモリ搭載量が少ないターゲット向けの設定
  * https://source.android.com/devices/tech/config/low-ram.html
* Android Wearとかはro.config.low_ram=trueかな？

###### http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobSchedulerService.java#77

```java
/** The number of concurrent jobs we run at one time. */
private static final int MAX_JOB_CONTEXTS_COUNT
        = ActivityManager.isLowRamDeviceStatic() ? 1 : 3;
```

##### http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/app/ActivityManager.java#isLowRamDeviceStatic

```java
// ActivityManager.java
public static boolean isLowRamDeviceStatic() {
    return "true".equals(SystemProperties.get("ro.config.low_ram", "false"));
}
```

# StateControllerの種類

* JobManager
* 各条件の監視をして、Jobの状態をコントロールする
* [StateController](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/StateController.java) (抽象クラス)
 * [AppIdleController](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/AppIdleController.java)
 * [BatteryController](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/BatteryController.java)
 * [ConnectivityController](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/ConnectivityController.java)
 * [IdleController](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/IdleController.java)
 * [JobStatus](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/JobStatus.java)
 * [TimeController](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/TimeController.java)
* [Controllerの生成箇所](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobSchedulerService.java#313)
 * http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobSchedulerService.java#313


# Debugging JobScheduler

* adb shell dumpsys jobscheduler
 * JobSchedulerに登録されているJobをDumpする
 * めっちゃ使う
* adb logcat -s JobSchedulerService
 * JobSchedulerServiceのログ。あんまり出ないけど...
* idle状態にするコマンド
 * adb shell dumpsys battery unplug
 * adb shell dumpsys deviceidle enable
 * adb shell dumpsys deviceidle step
 * adb shell dumpsys deviceidle force-idle


# 面白コメント

# // Let's go!

* http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobSchedulerService.java#348

# // GO GO GO!

* http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobSchedulerService.java#367

# 関連クラス

* [JobSchedulerImpl](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/app/JobSchedulerImpl.java)

* [/frameworks/base/core/java/android/app/job/](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/app/job/)
 * [IJobCallback.aidl](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/app/job/IJobCallback.aidl)
 * [IJobScheduler.aidl](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/app/job/IJobScheduler.aidl)
 * [IJobService.aidl](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/app/job/IJobService.aidl)
 * [JobInfo.aidl](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/app/job/JobInfo.aidl)
 * [JobInfo](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/app/job/JobInfo.java)
 * [JobParameters.aidl](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/app/job/JobParameters.aidl)
 * [JobParameters](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/app/job/JobParameters.java)
 * [JobScheduler](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/app/job/JobScheduler.java)
 * [JobService](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/core/java/android/app/job/JobService.java)

* [/frameworks/base/services/core/java/com/android/server/job/](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/)
 * [JobCompletedListener](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobCompletedListener.java)
 * [JobSchedulerService](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobSchedulerService.java)
 * [JobServiceContext](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobServiceContext.java)
 * [JobStore](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/JobStore.java)
 * [StateChangedListener](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/StateChangedListener.java)

* [/frameworks/base/services/core/java/com/android/server/job/controllers](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers)
 * [StateController](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/StateController.java)
 * [AppIdleController](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/AppIdleController.java)
 * [BatteryController](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/BatteryController.java)
 * [ConnectivityController](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/ConnectivityController.java)
 * [IdleController](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/IdleController.java)
 * [JobStatus](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/JobStatus.java)
 * [TimeController](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/core/java/com/android/server/job/controllers/TimeController.java)

* Auto BackのJobSchedular
 * [FullBackupJob.java](http://tools.oesf.biz/android-6.0.0_r1.0/xref/frameworks/base/services/backup/java/com/android/server/backup/FullBackupJob.java)
