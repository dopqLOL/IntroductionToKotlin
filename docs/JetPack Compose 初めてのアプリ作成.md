# JetPack Compose 初めてのアプリ作成

**目次**

1.  プロジェクトのセットアップ
2.  初めてのコンポーザブル関数
3.  UI要素: Text コンポーザブル
4.  プレビュー機能の活用 (`@Preview`)
5.  アプリの実行と MainActivity (`onCreate`, `setContent`)
6.  レイアウトの基本: Surface と Modifier
7.  まとめ

---

このドキュメントでは、Android StudioでJetpack Composeを使用して最初の簡単なアプリを作成する基本的な手順を要約します。

## 1. プロジェクトのセットアップ

* Android Studioを開き、「New Project」を選択します。
* テンプレートから「Empty Activity (Compose)」を選びます。
* プロジェクト名（例: `MyFirstComposeApp`）、パッケージ名、保存場所、言語（Kotlin）、最小SDKなどを設定して「Finish」をクリックします。
* Android Studioがプロジェクトの初期設定とビルドを行います。

## 2. 初めてのコンポーザブル関数

Compose UIは、**コンポーザブル関数** (Composable functions) を組み合わせて構築されます。これらは `Unit` を返す通常のKotlin関数ですが、`@Composable` アノテーションが付いています。

* `MainActivity.kt` を開くと、デフォルトでいくつかのコンポーザブル関数が定義されています（例: `Greeting`）。
* コンポーザブル関数はUIの一部を記述します。例えば、テキストを表示するには `Text()` コンポーザブルを使用します。

```kotlin
import androidx.compose.material3.Text // Textコンポーザブルをインポート
import androidx.compose.runtime.Composable
import androidx.compose.ui.tooling.preview.Preview // Previewアノテーションをインポート
// プロジェクト固有のテーマをインポート (名前は異なる場合がある)
import com.example.myfirstcomposeapp.ui.theme.MyFirstComposeAppTheme

// `@Composable` アノテーションを付けた関数でUIを定義
@Composable
fun WelcomeDisplay(userName: String) {
    // Textコンポーザブルでテキストを表示
    Text(text = "こんにちは、$userName さん!")
}

// `@Preview` アノテーションでプレビューを表示
@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    // アプリのテーマ内でプレビューするコンポーザブルを呼び出す
    MyFirstComposeAppTheme {
        WelcomeDisplay("Android")
    }
}
```

## 3. UI要素: Text コンポーザブル

* `Text()` は、画面にテキストを表示するための基本的なコンポーザブル関数です。
* `text` パラメータに表示したい文字列を渡します。
* 文字列テンプレート (`"こんにちは、$userName さん!"`) を使用して、変数や式の結果を文字列に埋め込むことができます。

## 4. プレビュー機能の活用 (`@Preview`)

* `@Preview` アノテーションをコンポーザブル関数に追加すると、Android StudioのデザインペインでそのUIのプレビューを確認できます。
* これにより、アプリを実際にデバイスやエミュレータで実行しなくても、UIの見た目を素早く確認・修正できます。
* `@Preview` には `showBackground = true` のようなパラメータを指定して、プレビューの背景を表示させることも可能です。
* 通常、プレビュー用の関数（例: `DefaultPreview`）を作成し、その中でプレビューしたいコンポーザブル（例: `WelcomeDisplay`）をアプリのテーマで囲んで呼び出します。

## 5. アプリの実行と MainActivity (`onCreate`, `setContent`)

* `MainActivity.kt` 内の `onCreate()` メソッドがアプリ起動時に最初に実行されます。
* `setContent { ... }` ブロック内で、アプリのメインUIとなるコンポーザブル関数を呼び出します。
* デフォルトでは、プロジェクト作成時に生成されたテーマ（例: `MyFirstComposeAppTheme`）と、その中で `Greeting`（または変更後の `WelcomeDisplay` など）が呼び出されるようになっています。

```kotlin
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.fillMaxSize // レイアウト関連のインポート
import androidx.compose.material3.MaterialTheme // テーマ関連のインポート
import androidx.compose.material3.Surface
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier // Modifierのインポート
import androidx.compose.ui.tooling.preview.Preview
// プロジェクト固有のテーマをインポート (名前は異なる場合がある)
import com.example.myfirstcomposeapp.ui.theme.MyFirstComposeAppTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // setContentブロックでCompose UIを設定
        setContent {
            // アプリのテーマを適用
            MyFirstComposeAppTheme {
                // SurfaceはUI要素の背景などを提供するコンテナ
                Surface(
                    modifier = Modifier.fillMaxSize(), // 画面全体に広げるModifier
                    color = MaterialTheme.colorScheme.background // テーマの背景色を使用
                ) {
                    // 表示したいメインのコンポーザブルを呼び出す
                    WelcomeDisplay("開発者")
                }
            }
        }
    }
}

// (WelcomeDisplay と DefaultPreview の定義は上記参照)

```

## 6. レイアウトの基本: Surface と Modifier

* `Surface` は、背景色や形状などを設定できるコンテナのようなコンポーザブルです。UI要素をグループ化し、共通の背景などを適用するのに役立ちます。
* `Modifier` は、コンポーザブルのサイズ、レイアウト、外観、操作などを変更（修飾）するために使います。
    * `Modifier.fillMaxSize()` は、利用可能なスペース全体を埋めるようにコンポーザブルに指示します。
    * `Modifier` は、`.` で繋げて複数の変更を連鎖させることができます (例: `Modifier.padding(16.dp).fillMaxWidth()`)。

## 7. まとめ

* Composeプロジェクトを作成し、基本的な構造を理解しました。
* `@Composable` アノテーションを使ってUI部品（コンポーザブル関数）を定義する方法を学びました。
* `Text()` コンポーザブルでテキストを表示しました。
* `@Preview` アノテーションでUIのプレビューを確認しました。
* `MainActivity` の `setContent` でアプリのメインUIを設定しました。
* `Surface` と `Modifier` を使った基本的なレイアウト調整に触れました。
