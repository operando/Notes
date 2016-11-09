
## Optionalの注意点??

引数でOptionalを取るようなものはよくない

戻り値としてOptionalを扱う

戻り値がOptionalじゃないけど、nullが返ってくるかもしれないものをOptionalで包む
or
Optionalでラップするメソッドを作る


## よく使用するメソッド

empty
flatMap
map
ifPresent
ofNullable
orElse
orElseGet

filter?(使うかな？)


## of、isPresent、getの使いみちは限られる

```Java
// Optionalオブジェクトを生成するofメソッドは、引数がnullだとヌルポを投げる…
Optional<String> hogeOpt = Optional.of(getHoge());

if (hogeOpt.isPresent()) { // 値が存在しているか? => 従来のnullチェックと同じことをしている…

    // 値を取得するgetメソッドは、値が存在していない場合実行時例外を投げる…(NoSuchElementException)
    String hoge = hogeOpt.get();

    System.out.println(hoge);
}
```


## memo

Optionalを戻り値としたメソッドは決してnullを返すべきでないが、それを防ぐ方法はない
未熟な者や無法者はどこにでもいる

Optionalを使っているが処理構造に変化が無い
Optionalを使わない時よりもコードが増えている

Optional#flatMapでnullを返してはいけない



Java8とのAPIとの比較

Java8にはあるけど、L-S-APIにちょっと無くて残念な機能

Optionalをどう扱うか



Lightweight-Stream-API
https://github.com/aNNiMON/Lightweight-Stream-API

Android Optional
https://developer.android.com/reference/java/util/Optional.html

Java Optional
https://docs.oracle.com/javase/jp/8/docs/api/java/util/Optional.html
https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html

RxJavaのObservable<T>でOptional<T>を代行する
http://sys1yagi.hatenablog.com/entry/2015/01/26/183000

RxJava-Optional
https://github.com/eccyan/RxJava-Optional


Optionalの取り扱いかた
http://irof.hateblo.jp/entry/2015/05/05/071450


Android-Java-8-Stream-Example
https://github.com/aNNiMON/Android-Java-8-Stream-Example
