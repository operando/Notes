# Lightweight-Stream-APIのあるAndroidアプリ開発

## SlideShare

http://www.slideshare.net/shinobuokano7/lightweightstreamapiandroid

## Lightweight-Stream-API?

* "Stream API from Java 8 rewritten on iterators for Java 7 and below."

## Lightweight-Stream-API?

* https://github.com/aNNiMON/Lightweight-Stream-API

## Lightweight-Stream-API?

* Androidでも使える！！


## ちなみに...

* Lightweight-Stream-APIって名前が長い...
* LSAって訳すっぽい
* READMEにLSAって記述があった
* 資料ないでLSAと書いてあるのは Lightweight-Stream-APIという意味です


## Lightweight-Stream-API Includes

* Functional interfaces
* Stream/IntStream
* Optional class
* Exceptional class
* Objects from Java 7


## Lightweight-Stream-API Includes

* Streamだけじゃないんやでー！！


## Stream?

* Java 8で導入されたAPI


## Stream?

* "A sequence of elements supporting sequential and parallel aggregate operations."


## Stream(正しくはないけど雑に...)

* 繰り返し処理のforやwhileをStreamにして、パイプライン的に処理する


## Lightweight-Stream-API

* "without parallel processing but with a variety of additional methods and with custom operators"
* parallelは提供されていない


## Lightweight-Stream-API ≠ Java 8 Stream API

* イコールではない
* Java 8 Stream APIのようなインターフェイスもったもの


## Streamの流れ

* Streamの生成
* 中間操作 (map,filter,etc.)
 * あるストリームを別のストリームに変換する操作
* 終端操作 (forEach,count,collect,etc.)
 * 結果または副作用を生成する操作


## Java 8 Stream

* スコアのListから30点以上のものを表示する
* Java 8のCollectionにStreamに変換するメソッドがある

```java
List<Integer> scores = Arrays.asList(100, 30, 35, 20);
scores.stream() // Streamの生成
        .filter(score -> score >= 30) // 中間操作
        .forEach(System.out::println); // 終端操作
```


## for

* スコアのListから30点以上のものを表示する

```java
for (Integer score : scores) {
    if (score >= 30) {
        System.out.println(score);
    }
}
```


## Java 8 Parallel Stream

* parallelStream()で並列処理
* 並列で処理するので結果の順番は変わる可能性がある

```java
List<Integer> scores = Arrays.asList(100, 30, 35, 20);
scores.parallelStream()
        .filter(score -> score >= 30)
        .forEach(System.out::println);
```


## Lightweight-Stream-API

* スコアのListから30点以上のものを表示する
* Stream.ofでStreamを作る

```java
List<Integer> scores = Arrays.asList(100, 30, 35, 20);
Stream.of(scores) // Streamの生成
        .filter(score -> score >= 30) // 中間操作
        .forEach(System.out::println); // 終端操作
```


## 出力じゃなくてListにしたい！(Lightweight-Stream-API)

* 終端操作をcollectにするだけなので簡単！

```java
List<Integer> scores = Arrays.asList(100, 30, 35, 20);
List<Integer> filteredScores = Stream.of(scores)
        .filter(score -> score >= 30)
        .collect(Collectors.toCollection(ArrayList::new));
```


## 中間操作、終端操作のメソッドは色々ある!

* 全部説明できないので、javadocとかみて試してね！
* 大体ほしいと思う機能は揃ってるはず
* Stream - Lightweight-Stream-API javadoc
 * http://static.javadoc.io/com.annimon/stream/1.1.3/com/annimon/stream/Stream.html


## Lightweight-Stream-API

* AndroidでJava 8のStreamっぽいものが使える！
  * 中間操作と終端操作は同じで書ける
  * 流れるように処理を書けるので気持ちいい!
* Java力 UP 💪
* Java 9でStreamに追加されるfunctionも使える!
* good-bye for??


## Optional


## Optional?

* "A container object which may or may not contain a non-null value."


## Optional?(雑)

* 値をラップしてnullかもしれないことを表現するクラス


## good-bye NullPointerException?

* NO 😂


## Optional - Java 8

```java
Optional<String> optional = Optionalを返すメソッド();
optional.ifPresent(System.out::println); // nullでなければ表示される
```

## Optional - Lightweight-Stream-API

```java
Optional<String> optional = Optionalを返すメソッド();
optional.ifPresent(System.out::println); // nullでなければ表示される
```


##  同じや！

* 同じや！


## Optional + Android

* Android SDKのAPIって思った以上にnullを返すものが多い
* ドキュメント読んで「え?null返るの?」と知るものも度々
* FragmentねーgetContextとかgetActivityとか...
 * コードレビューでつっつかれたくない...
* Fragment#getResourcesもmHostがnullだとIllegalStateException


## Fragment#getContext()

* "Return the Context this fragment is currently associated with."
* https://developer.android.com/reference/android/support/v4/app/Fragment.html#getContext()


## Fragment#getContext()

* null返るやん👀

```java
public Context getContext() {
    return mHost == null ? null : mHost.getContext();
}
```


## Fragment + Optional

```java
public static Optional<Context> getOptionalContext(Fragment fragment) {
    if (fragment.isDetached()) {
        return Optional.empty();
    }
    return Optional.ofNullable(fragment.getContext());
}

// 安全!
FragmentUtil.getOptionalContext(this)
        .ifPresent(context -> {
            Toast.makeText(context, "Optional最高！", Toast.LENGTH_SHORT).show();
        });
```


## Fragment + Optional

```java
public static Optional<FragmentActivity> getOptionalFragmentActivity(Fragment fragment) {
    if (fragment.isDetached()) {
        return Optional.empty();
    }
    return Optional.ofNullable(fragment.getActivity());
}

// menuの更新
// 安全!
FragmentUtil.getOptionalFragmentActivity(this)
        .ifPresent(FragmentActivity::supportInvalidateOptionsMenu);
```


## Fragment + Optional

```java
public static Optional<Bundle> getArguments(Fragment fragment) {
    return Optional.ofNullable(fragment.getArguments());
}

// 安全!
FragmentUtil.getArguments(this)
        .ifPresent(bundle -> {
          // bundleから値を取り出す
        });
```


## 複数のOptionalを扱う

* Optional AとOptional Bの両方がnullでなければ...みたいなパターン
* そもそもこれで正しいのかわからない...

```java
Optional<FragmentActivity> optionalFragmentActivity = FragmentUtil.getOptionalFragmentActivity(this);
Optional<String> titleOptional = titleをOptional<String>で返すメソッド();
optionalFragmentActivity.ifPresent(fragmentActivity -> {
    titleOptional.ifPresent(title -> {
        fragmentActivity.setTitle(title);
    });                
});
```

## 複数のOptionalを扱う

* これならどうだ💪
* Tupleで2つのOptionalに値が入っていれば、TupleをOptionalでラップして返す

```java
public static <T, R> Optional<Pair<T, R>> flatMapPair(Optional<T> a, Optional<R> b) {
    if (a.isPresent() && b.isPresent()) {
        // 2つのOptionalに値が入っていればTupleをOptionalでラップして返す
        return Optional.ofNullable(Pair.create(a.get(), b.get()));
    } else {
        // そうでないならempty
        return Optional.empty();
    }
}

OptionalUtil.flatMapPair(optionalFragmentActivity,titleOptional)
    .ifPresent(pair -> {
      pair.getFirst()setTitle(pair.getSecond());
});

```

## 複数のOptionalを扱う💪💪💪

```java
public static <F, R, T> Optional<Triplet<F, R, T>> flatMapTriplet(Optional<F> a, Optional<R> b, Optional<T> c) {
    if (a.isPresent() && b.isPresent() && c.isPresent()) {
        return Optional.ofNullable(Triplet.create(a.get(), b.get(), c.get()));
    } else {
        return Optional.empty();
    }
}
```


## 💪

* 💪


## でも、JavaにTupleないやん...


## Guild

* 💪
* Simple java tuples.
* https://github.com/operando/Guild


## Lightweight-Stream-APIのOptionalにしかない便利なメソッド

* Optional.stream()
 * OptionalからStreamに変換できる
* 他にもいくつかある...

```java
Optional.ofNullable(integers)
        .stream()
        .forEach(System.out::println);
```

* Java8だと

```java
Optional.ofNullable(integers)
        .map(integers1 -> integers1.stream())
        .orElse(Stream.empty())
        .forEach(System.out::println);
```

## Optionalにも中間操作、終端操作的なのがある

* 中間操作?的なの
 * map,flagMap,filter,etc...
* 終端操作?的なの
 * ifPresent,orElse,etc...

 ## Optional

 * Optional javadoc - Lightweight-Stream-API
  * http://static.javadoc.io/com.annimon/stream/1.1.3/com/annimon/stream/Optional.html


## Optional

* AndroidでJava 8のOptionalと同じようなものが使える
* Java力 UP💪
* ifでnullチェックするより自然な感じでプログラムが書ける
* nullがなくなるわけではない
* 扱いが難しいこともある
 * Optionalのラップしてる値がnullだった時の処理を忘れるとか...


## Objectsもいいぞ！

* Java 7から追加されたAPI
* AndroidではminSdkVersion 19以上じゃないとJava 7のAPIは使えないはず

## Objectsもいいぞ！

* ２つの文字列を比較したい

```java
String s1 = String返すメソッド();
String s2 = String返すメソッド();

// s1はnullかもしれない...
s1.equals(s2);

// Lightweight-Stream-API
// どちらかがnullでも安全!
Objects.equals(s1,s2);
```

## Andorid N minSdkVersion 24

* Java 8のAPIがいくつか入ってる
 * StreamとOptionalは入ってる
 * 他にもいくつか...
* Lightweight-Stream-API使わなくてもよい
* minSdkVersion 24...何年先?


## Android-Java-8-Stream-Example

* Lightweight-Stream-APIの作者がAndroidでJava 8っぽく書くサンプルを書いてる
* https://github.com/aNNiMON/Android-Java-8-Stream-Example


## まとめ

* Lightweight-Stream-APIを使えばStreamっぽいものとOptionalが使える
* それ以外にも便利な機能あるよ！
* Java力 UP💪につながる
 * Android開発のJavaだとJava 6,7で技術が止まってしまう可能性も...
 * 僕と一緒に開発する人にはJavaの力もつけてほしい💪
* Java 9も来年出るのでJava自体に関心を！

## Links

* Lightweight-Stream-API version 1.1.3 javadoc
 * http://static.javadoc.io/com.annimon/stream/1.1.3/overview-summary.html
* Stream - Java 8
 *  https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html
* Java 8 Stream Tutorial
 * http://winterbe.com/posts/2014/07/31/java8-stream-tutorial-examples/
* Optional - Java 8
  *  https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html
* Optionalの取り扱いかた
  * http://irof.hateblo.jp/entry/2015/05/05/071450

## Thanks
