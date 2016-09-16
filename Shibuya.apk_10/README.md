# Android + JSON-RPC 2.0

## JSON-RPC??


## JSON-RPC

* JSON-RPC is a stateless, light-weight remote procedure call (RPC) protocol.

## JSON-RPC spec

* http://www.jsonrpc.org/specification

## JSON-RPC 2.0

## RPC

* gRPC
 * http://www.grpc.io/
* Go Conference 2016 SpringでgRPCの現状について発表してきました
 * http://tech.mercari.com/entry/2016/04/27/145227


## JSON-RPC

* HTTP bodyのJSONでリクエストを記述する


## JSON-RPC Request

* Request

```json
{
  "jsonrpc": "2.0",
  "method": "subtract",
  "params": [
    42,
    23
  ],
  "id": 1
}
```

## JSON-RPC Response

* Response

```json
{
  "jsonrpc": "2.0",
  "result": 19,
  "id": 1
}
```

## JSON-RPC Batch

* 1回の通信で複数のリクエストを送れる


## JSON-RPC Request

* Request

```json
[
  {
    "id": 1,
    "jsonrpc": "2.0",
    "method": "shibuya.apk",
    "params": { ... }
  },
  {
    "id": 2,
    "jsonrpc": "2.0",
    "method": "shinobu.apk",
    "params": { ... }
  }
]
```


## JSON-RPC Response

* Response

```json
[
  {
    "id": 1,
    "jsonrpc": "2.0",
    "result": { ... }
  },
  {
    "id": 2,
    "jsonrpc": "2.0",
    "result": { ... }
  }
]
```

## 何に使うの??

* スクショ

## 何に使うの??

* 以下のリクエストが1回のリクエストでできる！！
* タイムラインの投稿を取得
* 現在地の座標を住所に変換する
* バナーを取得する

## 何に使うの??

* タイムラインの投稿を取得
* 現在地の座標を住所に変換する
* バナーを取得する

## GET /home??

## 複数の処理をまとめたAPIは仕様が壊れやすい

## JSON-RPC Batch

* APIは個々の処理をメソッドとして定義
* クライアント側で個々の処理を1つのリクエストにまとめて呼び出す

## JSON-RPC Request

* Request

```json
[
  {
    "id": 1,
    "jsonrpc": "2.0",
    "method": "GetTimeline",
    "params": { ... }
  },
  {
    "id": 2,
    "jsonrpc": "2.0",
    "method": "GetBanner",
    "params": { ... }
  }
]
```

## JSON-RPC Response

* Response

```json
[
  {
    "id": 1,
    "jsonrpc": "2.0",
    "result": { ... }
  },
  {
    "id": 2,
    "jsonrpc": "2.0",
    "result": { ... }
  }
]
```

## よさそう!!

## じゃAndroidで実装するか!!

## OkHttp使って試しに書いてみる Request

* Request

```java
OkHttpClient client;

JSONObject json = new JSONObject();
JsonArray params = new JsonArray();

json.put("method","subtract");
params.put(42);
params.put(23);
json.put("params",params);

RequestBody requestBody = RequestBody.create(MEDIA_TYPE, json.toString().getBytes());

Request request = new Request.Builder()
        .url("https://....")
        .post(requestBody)
        .build();
Response response = client.newCall(request).execute();
```

## OkHttp使って試しに書いてみる Response

```java
Response response = client.newCall(request).execute();
// ... isSuccessfulのチェックとか...
ResponseBody value = response.body();

JSONObject j = new JSONObject(value.string());
int result = j.getInt("result");// return 19
```

## これイケてますかね？？

## イケてないですよね...

## いい感じのLibraryあるんじゃね...検索検索


## あまりない??

* jsonrpc4j
 * https://github.com/briandilley/jsonrpc4j
 * Batchに対応してない??
* retrofit-jsonrpc
 * https://github.com/segmentio/retrofit-jsonrpc
 * Batchに対応してない??

## 結果

## 途方に暮れる日々...

## あーなんでこんなAPIになってるんだ...

## うぅ...僕はわるくない...(。-ω-)


## そうも言ってられない...


## そうだ！自分で作ればいいんだ！


## 雑に作りました！

## 作る前に考えたこと

* 毎回同じ処理は書きたくない
* リクエストの型が決まればレスポンスの型が決まる
 * リクエストとレスポンスを型で定義
* Batch リクエストをいい感じに受け取りたい
* Rxとか使っちゃう？？
* 特定の通信ライブラリに依存しない

## JSON-RPCのクライアント実装で大変だったこと

* Batch リクエストの受け取り方
* エラーハンドリング
* Retrofitの恩恵が受けられない…


## レスポンスのパターン

* OK 1つのレスポンス
* OK 複数のレスポンス(2,3つ)
* NG 全レスポンスエラー
* NG 一部のレスポンスエラー


## Batch リクエストの受け取り方

* 複数のリクエストを作る
* 作ったリクエストをまとめてAPIに投げる
* 複数のレスポンスが返ってくる
* ハンドリングしてレスポンスを通知する

## 複数のレスポンス...??

## 複数のレスポンス...??

* List<Object> ??
* List<?> ??
* List<JSONObject> ??

## どれもイケてない


## そうだ...Tupleだ！！


## 筋肉 2つ

```java
@Getter
public class Pair<F, S> {
    private final F first;
    private final S second;

    private Pair(F first, S second) {
        this.first = first;
        this.second = second;
    }

    public static <F, S> Pair<F, S> create(F first, S second) {
        return new Pair<>(first, second);
    }
}
```

## 筋肉 3つ

```java
@Getter
public class Triplet<F, S, T> {
    private final F first;
    private final S second;
    private final T thread;

    private Triplet(F first, S second, T thread) {
        this.first = first;
        this.second = second;
        this.thread = thread;
    }

    public static <F, S, T> Triplet<F, S, T> create(F first, S second, T thread) {
        return new Triplet<>(first, second, thread);
    }
}
```

## 複数のリクエストを作る

```java
// リクエストとレスポンスをクラスとして定義
// リクエスト : GetBanner レスポンス : GetBanner
class GetBannerResponse {
    String url;
}

// リクエストの型が決まればレスポンスの型が決まる = RequestType<レスポンスの型>
class GetBanner extends RequestType<GetBannerResponse> {
    @Override
    public String getMethod() {
        return "GetBanner";
    }

    @Override
    protected Class<GetBannerResponse> getResponseType() {
        return GetBannerResponse.class;
    }
}
```

## 複数のリクエストを作る

```java
// 同じように別APIのリクエストとレスポンスをクラスとして定義
// リクエスト : GetTimeline レスポンス : GetTimelineResponse
class GetTimelineResponse {
    List<Item> items;
}


class GetTimeline extends RequestType<GetTimelineResponse> {
    private final long timelineId;

    GetTimeline(long timelineId) {
        this.timelineId = timelineId;
    }

    @Override
    public String getMethod() {
        return "GetTimeline";
    }

    @Override
    protected Class<GetTimelineResponse> getResponseType() {
        return GetTimelineResponse.class;
    }
}
```

## 作ったリクエストをまとめてAPIに投げる

```java
GetTimeline getTimeline = new GetTimeline(1);
GetBanner getBanner = new GetBanner();

RxApiClient rxApiClient = new RxApiClient();

 // Observable<Pair<GetBannerResponse, GetTimelineResponse>>が返ってくうる
rxApiClient.responseFrom(getTimeline,getBanner)
....
```

## リクエストのクラスはJSONになってHTTP bodyに設定される

```json
[
  {
    "params": {
      "timelineId": 1
    },
    "method": "GetTimeline",
    "jsonrpc": "2.0",
    "id": "1"
  },
  {
    "params": {},
    "method": "GetBanner",
    "jsonrpc": "2.0",
    "id": "2"
  }
]
```


## 複数のレスポンスが返ってくる + ハンドリングしてレスポンスを通知する


```java
// 複数のレスポンスTupleに包んでRxに流す
// responseFrom内でTupleに包む処理をしてる
rxApiClient.responseFrom(getTimeline, getBanner)
  .subscribeOn(Schedulers.io())
  .observeOn(AndroidSchedulers.mainThread())
  .subscribe(new Action1<Pair<GetTimelineResponse, GetBannerResponse>>() {
    @Override
    public void call(Pair<GetTimelineResponse, GetBannerResponse> pair) {
      // Tupleからレスポンスを取り出す
      pair.getFirst();  // return GetTimelineResponse
      pair.getSecond(); // retrun GetBannerResponse
    }
  });
```
