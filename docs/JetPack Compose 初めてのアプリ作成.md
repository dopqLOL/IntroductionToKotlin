# 初めての Android Compose アプリ学習記録 (ステップ0-7)

Android アプリ開発のモダンな UI ツールキットである Jetpack Compose を使って、シンプルな「Hello World」スタイルのアプリを作成するチュートリアルを進めた際の学習記録。

## 目次

* [学習内容](#学習内容)
    * [ステップ 0: はじめに](#ステップ-0-はじめに)
    * [ステップ 1: 新しいプロジェクトを作成する](#ステップ-1-新しいプロジェクトを作成する)
    * [ステップ 2: Composable 関数を更新する](#ステップ-2-composable-関数を更新する)
    * [ステップ 3: UI に別の要素を追加する (Text の変更)](#ステップ-3-ui-に別の要素を追加する-text-の変更)
    * [ステップ 4: 背景色を変更する](#ステップ-4-背景色を変更する)
    * [ステップ 5: パディングを追加する](#ステップ-5-パディングを追加する)
    * [ステップ 6: コードを確認する](#ステップ-6-コードを確認する)
    * [ステップ 7: アプリを実行する](#ステップ-7-アプリを実行する)
* [まとめ](#まとめ)

## 学習内容

### ステップ 0: はじめに

* Jetpack Compose の基本的な概念と、このチュートリアルで作成するシンプルなグリーティング カード アプリの概要を把握した。

### ステップ 1: 新しいプロジェクトを作成する

* Android Studio で「Empty Activity」テンプレート（Compose ベース）を選択し、新規プロジェクトを作成。
* プロジェクト名、パッケージ名、言語（Kotlin）などを設定。
* Android Studio が初期設定とビルドを実行するのを確認した。

### ステップ 2: Composable 関数を更新する

* `MainActivity.kt` を開き、`@Composable` アノテーションが付いた `Greeting` 関数を確認。これが UI の一部を定義することを学んだ。
* `Greeting` 関数内の `Text` の `text` パラメータを変更し、表示メッセージをカスタマイズ。
    * 例: `text = "Hi, my name is <自分の名前>!"`
* `GreetingPreview` 関数も同様に更新し、プレビュー機能で UI を確認できることを理解した。

### ステップ 3: UI に別の要素を追加する (Text の変更)

* このチュートリアルでは、既存の `Greeting` 関数を修正することに焦点を当てた。
* `Greeting` 関数に `name` パラメータを追加し、`Text` 内でその名前を表示するように変更。文字列テンプレート (`$name`) の使い方を学んだ。
    ```kotlin
    @Composable
    fun Greeting(name: String, modifier: Modifier = Modifier) {
        Text(
            text = "こんにちは！ $name!", // 文字列テンプレートを使用
            modifier = modifier
        )
    }
    ```
* `MainActivity` の `setContent` と `GreetingPreview` で `Greeting` 関数を呼び出す際に、引数として名前を渡すように修正した。

### ステップ 4: 背景色を変更する

* UI 要素にスタイルを適用する方法として `Surface` Composable を学んだ。
* `Greeting` 関数内で `Text` を `Surface` で囲み、`color` パラメータで背景色 (`Color.Yellow`) を設定した。
    ```kotlin
    @Composable
    fun Greeting(name: String, modifier: Modifier = Modifier) {
        Surface(color = Color.Yellow) { // Surfaceで囲み、色を指定
            Text(
                text = "こんにちは！ $name!",
                modifier = modifier
            )
        }
    }
    ```

### ステップ 5: パディングを追加する

* UI 要素の周囲に余白（パディング）を追加するために `Modifier.padding()` を使用することを学んだ。
* `Text` Composable の `modifier` パラメータに `.padding(24.dp)` を追加。`dp` が密度非依存ピクセルであることを理解した。
    ```kotlin
    @Composable
    fun Greeting(name: String, modifier: Modifier = Modifier) {
        Surface(color = Color.Yellow) {
            Text(
                text = "こんにちは！ $name!",
                modifier = modifier.padding(24.dp) // Textにパディングを追加
            )
        }
    }
    ```
* `MainActivity` の `setContent` 内で `Scaffold` から提供される `innerPadding` を `Greeting` に渡す `Modifier.padding(innerPadding)` の意味（システム UI との重なり防止）を理解した。

### ステップ 6: コードを確認する

* ここまでの変更を反映した `MainActivity.kt` の全体構造を確認。
* `MainActivity`, `setContent`, `GreetingCardTheme`, `Scaffold`, `Greeting`, `GreetingPreview` の各要素の役割を再確認した。

### ステップ 7: アプリを実行する

* エミュレータまたは実機を選択し、Android Studio からアプリを実行。
* ビルド、インストール、起動を経て、変更が反映された UI（背景色、パディング、テキスト）が表示されることを確認した。

## まとめ

このチュートリアルを通して、Jetpack Compose を使った Android アプリ開発の基本的な流れを体験できた。

* **Composable 関数:** `@Composable` アノテーションを付けた関数で UI を部品化して定義する。
* **基本的な Composable:** `Text` で文字を表示し、`Surface` で背景や形状を設定する。
* **Modifier:** `Modifier` を使って、パディング (`padding()`) などの装飾やレイアウト調整を行う。`dp` 単位の重要性も理解した。
* **パラメータ:** Composable 関数にパラメータを渡すことで、表示内容を動的に変更できる。
* **プレビュー:** `@Preview` アノテーションで、アプリ全体を実行せずに UI を確認できる。
* **基本的なプロジェクト構造:** `MainActivity`, `setContent`, テーマ (`GreetingCardTheme`), `Scaffold` の役割を学んだ。

シンプルなアプリながら、Compose の宣言的な UI 構築の考え方に触れることができた。
