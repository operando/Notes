# Screenshots Test Spoon + Espresso(shibuya.apk #2)

## Spoon

https://github.com/square/spoon

## Espresso

https://code.google.com/p/android-test-kit/wiki/Espresso

## Spoon Gradle Plugin

https://github.com/stanfy/spoon-gradle-plugin

## Demo

https://github.com/operando/EspressoSpoonSample

## 俺が考えた最強のBest Practice

### 画面表示ができればOK

画面表示までに全力を注ぐ

Espressoで細かい操作はしない


### Espressoを最小限に使う

あくまで最小限

最大限使わない

頑張らない


### createIntentパターンを使う

Activity起動に必要な情報がわかりやすい

表示したい画面のIntentが大事


```java
public class LoginSuccessActivity extends AppCompatActivity {

    private static final String EMAIL = "email";

    public static Intent createIntent(Context context, String email) {
        Intent i = new Intent(context, LoginSuccessActivity.class);
        i.putExtra(EMAIL, email);
        return i;
    }
}
```


### 実機とエミュレータを使い分ける

実機は機種依存が分かる

エミュレータは解像度が柔軟に変更できる


### 大雑把に確認する

厳密に結果を確認しない

表示するデータのこがわらない


### 全画面撮ろうとしない

アプリの規模によっては全画面無理

全画面撮れても価値があるのか疑問

撮って価値ある画面に着目する


### 前提条件が厳しい画面は潔く諦める

とにかく頑張らない


### とにかく頑張らない

気軽に楽しくがモットー

ハマるな危険 = 頑張り過ぎちゃう
