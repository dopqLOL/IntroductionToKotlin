# Java と Kotlin の主な違い (Android Codelab より)

このドキュメントは、指定されたAndroid Codelabの序盤（#0〜#7）で触れられているJavaとKotlinの主な違いをまとめたものです。

## 1. 宣言方法

### 変数 (Variables)

* **Kotlin:**
    * `val`: 読み取り専用（再代入不可）。Javaの`final`に相当。
    * `var`: 再代入可能。
    * **型推論:** 多くの場合、初期値から型が推論されるため、型名を明示的に記述する必要がない。
        ```kotlin
        val greetingMessage = "こんにちは" // String型と推論される
        var itemCounter = 10          // Int型と推論される
        ```
* **Java:**
    * 変数を宣言する際には、必ず型名を明示的に記述する必要がある。
    * `final`: 再代入不可。
        ```java
        final String greetingMessage = "こんにちは";
        int itemCounter = 10;
        ```

### 関数 (Functions)

* **Kotlin:**
    * `fun` キーワードを使用して関数を宣言する。
    * パラメータ名は型の前に記述する (`paramName: Type`)。
    * 戻り値の型は、パラメータリストの後ろにコロン (`:`) を付けて記述する (`: ReturnType`)。
    * 単一式で表現できる関数は、より簡潔に記述できる。
        ```kotlin
        // 2つの整数を加算する関数
        fun sumNumbers(a: Int, b: Int): Int {
            return a + b
        }
        // 単一式関数: 2つの整数を乗算する
        fun multiplyNumbers(a: Int, b: Int) = a * b
        ```
* **Java:**
    * 戻り値の型を関数名の前に記述する (`ReturnType functionName(Type paramName)`)。
        ```java
        // 2つの整数を加算するメソッド
        public int sumNumbers(int a, int b) {
            return a + b;
        }
        ```

### 文字列テンプレート (String Templates)

* **Kotlin:**
    * 文字列リテラル内に変数や式を埋め込むことができる。
    * ドル記号 (`$`) の後に変数名を記述するか、ドル記号と波括弧 (`${}`) で式を囲む。
    * Javaでの文字列連結 (`+`) よりも簡潔で読みやすい。
        ```kotlin
        // メイン関数での使用例
        fun main() {
          val newMessages = 5       // 未読メッセージ数
          val archivedMessages = 100 // 既読メッセージ数
          // 文字列テンプレートを使用して変数と式を埋め込む
          println("受信箱の合計メッセージ数: ${newMessages + archivedMessages}")
          // 出力: 受信箱の合計メッセージ数: 105
        }
        ```
* **Java:**
    * 文字列連結には `+` 演算子を使用するか、`String.format()` や `StringBuilder` を使う必要がある。
        ```java
        int newMessages = 5;
        int archivedMessages = 100;
        System.out.println("受信箱の合計メッセージ数: " + (newMessages + archivedMessages));
        ```

## 2. Null 安全性 (Null Safety)

* **Kotlin:**
    * **Null非許容型 (Non-Nullable Types):** デフォルトでは、変数は `null` を保持できない。これにより、`NullPointerException` をコンパイル時に防ぐことができる。
    * **Null許容型 (Nullable Types):** `null` を許容したい場合は、型名の後ろに `?` を付ける (`String?`, `Int?` など)。
    * **安全呼び出し演算子 (`?.`):** オブジェクトが `null` でない場合にのみメソッドやプロパティにアクセスする。`null` の場合は `null` を返す。
    * **エルビス演算子 (`?:`):** 左辺が `null` の場合に、右辺のデフォルト値を返す。
        ```kotlin
        var userName: String? = "田中" // Null許容のユーザー名
        // 安全呼び出し: nullでなければ長さを取得、nullならnull
        val nameLength = userName?.length
        // エルビス演算子: nullなら"ゲストユーザー"を使用
        val userDisplayName = userName ?: "ゲストユーザー"
        println("ユーザー名: $userDisplayName, 文字数: $nameLength")
        ```
* **Java:**
    * オブジェクト型の変数はデフォルトで `null` を保持できる。
    * `null` の可能性がある変数にアクセスする前に、開発者が明示的に `null` チェック (`if (variable != null)`) を行う必要がある。怠ると実行時に `NullPointerException` が発生する可能性がある。

## 3. 簡潔さ (Conciseness)

* **Kotlin:**
    * Javaと比較して、より少ないコード量で同じ機能を実現できることが多い（ボイラープレートコードの削減）。
    * 型推論、データクラス、拡張関数、文字列テンプレートなどの機能がコードの簡潔化に寄与する。
* **Java:**
    * 比較的記述量が多くなる傾向がある（例: getter/setter、`equals()`, `hashCode()` など）。

## まとめ

Codelabの序盤で紹介されているように、KotlinはJavaと比較して、より**簡潔**で**安全**（特にNull安全）なコードを書けるように設計されています。変数や関数の宣言方法、Nullの扱い方、そして文字列テンプレートのような便利な機能が主な違いとして挙げられます。Kotlinは現代的なプログラミング言語の機能を多く取り入れています。
