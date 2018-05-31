# FlutterでQRコードリーダー的なアプリを雑に作ってみた

## スライド

* https://speakerdeck.com/operando/flutterde-qrkodorida-woza-nizuo-tutemita

## Demo


## アプリのコード

* Flutter QRCode Scanner APP
* https://github.com/operando/FlutterQRScanner-App


## 作業時間

* 5時間くらい

## 作る過程をざっくり紹介


## なぜQRコードリーダーアプリを作るのか

* Androidの標準カメラアプリはQRコード読み込みできない
  * iOSはできる
* 世にあるQRコードリーダーアプリ広告が出て嫌ー
* 自分で作ればいいのでは？
* Flutterでやるかー


## QRコードリーダーのLibraryを探す

* QR Code Scanner Appを作ってるYouTubeを見つけた
* Flutter: QR Code Scanner App | Barcode Scan
  * https://www.youtube.com/watch?v=siuJhQ9BqsU
* Flutter QRCode Scanner APP
  * https://github.com/iampawan/FlutterQRScanner-App

## QRコードリーダーのLibraryを探す

* この方のYouTubeチャンネルFlutter動画結構あって良さそう
* MTechViral
  * https://www.youtube.com/channel/UCFTM1FGjZSkoSPDZgtbp7hA

## QR Code Scanner Appを動かしてみる

* git cloneして、IntelliJ IDEAで開く
* Androidアプリでビルドする

## 動いた 🍣


## QR Code Scanner Appのコードを見てみる

* QRコード読み込みにはLibraryを使ってみるみたい
* barcode_scan
* https://github.com/apptreesoftware/flutter_barcode_reader


## QR Code Scanner Appのコードを見てみる

* barcode_scanはQRコード読み込みにzxingを使ってる
* https://github.com/zxing/zxing
* GoogleのMobile Visionに差し替えたい欲が出てきた


## Flutter Mobile Vision Packageを探してみる

* あったー！
* flutter_mobile_vision
* https://github.com/edufolly/flutter_mobile_vision
* ただiOSは未対応っぽい


## flutter_mobile_visionに差し替える

* 簡単に差し替えられた

```dart
Future _scanQR() async {
    List<Barcode> barcodes = [];
    try {
    barcodes = await FlutterMobileVision.scan(
        flash: false,
        autoFocus: true,
        formats: Barcode.ALL_FORMATS,
        multiple: false,
        showText: true,
        camera: FlutterMobileVision.CAMERA_BACK,
        fps: 30.0,
    );
    setState(() {
        result = barcodes[0].displayValue;
    });
    } on Exception {
    barcodes.add(new Barcode('Failed to get barcode.'));
    }
}

@override
Widget build(BuildContext context) {
    ...
    body: Center(
    ...
    floatingActionButton: FloatingActionButton.extended(
        icon: Icon(Icons.camera_alt),
        label: Text("Scan"),
        onPressed: _scanQR,
    ),
    floatingActionButtonLocation: FloatingActionButtonLocation.centerFloat,
    );
}
```

## なんだ、できちまったぜ... 🍣


## 機能追加してみよう

* 読み込んだQRコードの文字列をシェアする
* 読み込み履歴を作る
  * 今回はこっちの話をします


## 読み込み履歴を作る

* 読み込み履歴画面を作る
* 読み込み履歴画面への遷移を作る
* 読み込んだ情報をDBに保存する
* 読み込み履歴一覧を表示する


## 読み込み履歴画面を作る

* HistoryStateの中身は後ほど

```dart
class History extends StatefulWidget {
  @override
  HistoryState createState() {
    return HistoryState();
  }
}
```

## 読み込み履歴画面への遷移を作る

```
void main() => runApp(MaterialApp(
      debugShowCheckedModeBanner: false,
      home: HomePage(),
      routes: <String, WidgetBuilder>{'history': (_) => History()},
    ));
```

## 読み込み履歴画面にListViewを表示する

* ListViewの使い方を公式ドキュメントから学ぶ
* とりあえず固定データを表示してみる

## 読み込み履歴画面にListViewを表示する

```dart
class HistoryState extends State<History> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text("Scanner History"),
        ),
        body: new ListView.builder(
            itemCount: 1,
            itemBuilder: (context, index) {
              return ListTile(title: Text("ニート"));
            }));
  }
}
```

## 読み込んだ情報をDBに保存する

* DB Library探してみる
* sqflite
* https://github.com/tekartik/sqflite


## sqfliteの使い方を学ぶ

* README読んだり、example読んだりする 
* あとは雰囲気でコピペしてきた、書き換える
* exampleがちょっとわかりにくかったので頑張る...


## DBに保存された情報見たい...

* AndroidならStetho使えば！！
* flutter_stetho
  * https://github.com/brianegan/flutter_stetho
* でもDB見れなかった...
  * sqfliteでDBを作ってるディレクトリがちょっとあれかも


## 保存した読み込み履歴をListViewに表示する

* DBから履歴を読み込んで、取得できたらsetStateするだけ


## 保存した読み込み履歴をListViewに表示する

```dart
class HistoryState extends State<History> {
  List<ScanItem> _items = [];

  @override
  void initState() {
    super.initState();
    _initDatabase();
  }

  _initDatabase() async {
    String path = await getDatabaseFilePath("scan_history.db");
    Database db = await openReadOnlyDatabase(path);

    List<Map> data = await db.query("scan_hisoty", columns: ["text"]);

    List<ScanItem> items = [];
    data.forEach((e) => items.add(ScanItem.fromMap(e)));

    setState(() {
      _items = items;
    });

    await db.close();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        ...
        body: new ListView.builder(
            itemCount: _items.length,
            itemBuilder: (context, index) {
              return ListTile(title: Text("${_items[index].text}"));
            }));
  }
}
```

## アプリの最終的なDependencies

```yaml
dependencies:
  flutter:
    sdk: flutter

  cupertino_icons: ^0.1.2
  flutter_mobile_vision: ^0.1.0
  share: "^0.5.0"
  sqflite: "^0.8.9"
  path_provider: "^0.4.0"

dev_dependencies:
  flutter_stetho: 0.1.1
  flutter_test:
    sdk: flutter
```


## 作ってみて感じたこと 🍣

* とりあえずLibrary探しがち
   * 自分が実装しなくてもきっとある感
   * 実際に大体はある
* 片方のプラットフォームだけ実装しても全然良さそう
* 作りたいものをすぐ作れる
   * よく知らないから細かいことにこだわらなくなる

## 作ってみて感じたこと 🍣

* async / await雑に使いがち
   * 理解してこうな！
* Text表示する方法忘れがち
  * コンポーネントに文字列指定するプロパティがあると勘違いする
  * 基本Widgetだよ！

## 作ってみて感じたこと 🍣

* 最悪main.dartに全部書いても良さそう
  * あとで分ければいいし
* コピペしすぎるとハマりがち
  * コピペしたコードが予期せぬ動きをしてるのあるよね
  * プログラミングの懐かしさを感じた

## 作ってみて感じたこと 🍣

* SDKの使い方わからなかったらドキュメント読むか中のコード読む方が早そう
* ORM Libraryがほしいところ...
  * Dartで実装できるかな？

## 作ってみて感じたこと 🍣

* droidkaigi2018-flutter見てみると良さそう
* https://github.com/konifar/droidkaigi2018-flutter

## Thanks！