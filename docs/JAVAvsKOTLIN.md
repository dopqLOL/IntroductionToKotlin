### Java と Kotlin の主な違い: 変数、関数、クラス

## 1. 変数宣言

### Kotlin
* **キーワード:** `val`（読み取り専用、再代入不可）または `var`（再代入可能）を使用。
* **構文:** `val/var 変数名: 型 = 初期値`
* **型推論:** 初期値から型が明らかな場合は、型の指定 (`: 型`) を省略可能。
* **Null安全性:** 型名の末尾に `?` を付けることで、nullを許容する型 (`String?`) と許容しない型 (`String`) を明確に区別。コンパイル時にチェックされる。

```kotlin
val messageText: String = "こんにちは" // 型を明示 (読み取り専用)
var userCount = 10                 // 型推論 (再代入可能)
val nullableDescription: String? = null // Null許容型
// nullableDescription = "説明あり" // 再代入は可能 (varの場合)
// messageText = "さようなら" // エラー: valは再代入不可
```

### Java
* **キーワード:** なし。型名を先に記述。`final` を付けると再代入不可。
* **構文:** `型 変数名 = 初期値;` または `final 型 変数名 = 初期値;`
* **型推論:** Java 10以降で `var` を使用したローカル変数の型推論が可能になったが、Kotlinほど広範ではない。
* **Null安全性:** デフォルトでは全ての参照型がnullを許容。`@NonNull` などのアノテーションや規約で対応することが多いが、コンパイル時の強制力はKotlinより弱い。

```java
final String messageText = "こんにちは"; // 再代入不可
int userCount = 10;                 // 再代入可能
String nullableDescription = null;    // Null許容 (デフォルト)
// messageText = "さようなら"; // エラー: finalは再代入不可
userCount = 15;                 // OK
```

## 2. 関数 (メソッド) 定義

### Kotlin
* **キーワード:** `fun` を使用。
* **構文:** `fun 関数名(パラメータ名: 型, ...): 戻り値の型 { ... }`
* **戻り値なし:** 戻り値がない場合は `: Unit` を指定。`Unit` は省略可能。
* **単一式関数:** 関数本体が単一の式の場合、`= 式` の形で簡潔に記述可能 (戻り値の型推論も可)。
* **デフォルト引数:** パラメータにデフォルト値を設定可能 (`param: Type = defaultValue`)。
* **名前付き引数:** 関数呼び出し時に `関数名(パラメータ名 = 値)` の形式で引数を指定でき、順序を問わない。

```kotlin
// 基本的な関数
fun showGreeting(target: String): Unit { // Unitは省略可
    println("Hello, $target!")
}

// 戻り値のある関数
fun calculateSum(a: Int, b: Int): Int {
    return a + b
}

// 単一式関数
fun multiplyNumbers(x: Int, y: Int) = x * y

// デフォルト引数と名前付き引数
fun setupConfiguration(port: Int = 8080, enableLogging: Boolean = false) {
    println("Port: $port, Logging: $enableLogging")
}

// main関数はトップレベルに記述可能
fun main() {
    showGreeting("World")
    val total = calculateSum(5, 3)
    println("合計: $total") // 文字列テンプレート
    setupConfiguration() // デフォルト値を使用
    setupConfiguration(enableLogging = true, port = 9000) // 名前付き引数 (順序不問)
}
```

### Java
* **キーワード:** なし。戻り値の型を先に記述。
* **構文:** `戻り値の型 メソッド名(型 パラメータ名, ...) { ... }`
* **戻り値なし:** 戻り値がない場合は `void` を指定。
* **単一式関数:** Kotlinのような簡略記法はない。
* **デフォルト引数:** サポートされていない。オーバーロード（同名で引数が異なるメソッドを複数定義）で代替することが多い。
* **名前付き引数:** サポートされていない。

```java
public class MainApp { // Javaではクラスが必要
    // 基本的なメソッド
    public static void showGreeting(String target) {
        System.out.println("Hello, " + target + "!"); // 文字列連結
    }

    // 戻り値のあるメソッド
    public static int calculateSum(int a, int b) {
        return a + b;
    }

    // デフォルト引数の代替 (オーバーロード)
    public static void setupConfiguration(int port, boolean enableLogging) {
        System.out.println("Port: " + port + ", Logging: " + enableLogging);
    }
    public static void setupConfiguration(int port) {
        setupConfiguration(port, false); // デフォルト値を内部で設定
    }
    public static void setupConfiguration() {
        setupConfiguration(8080, false); // デフォルト値を内部で設定
    }

    // mainメソッド
    public static void main(String[] args) {
        showGreeting("World");
        int total = calculateSum(5, 3);
        System.out.println("合計: " + total);
        setupConfiguration();
        setupConfiguration(9000, true);
    }
}
```

## 3. クラス定義

### Kotlin
* **簡潔さ:** ボイラープレートコード（決まりきった冗長なコード）が少ない。
* **プライマリコンストラクタ:** クラスヘッダ内で簡潔に定義可能。プロパティ宣言も同時に行える。
* **プロパティ:** `val`/`var` で宣言。ゲッター/セッターは通常自動生成され、明示的な記述は不要（カスタムする場合を除く）。
* **データクラス:** `data class` キーワードで、`equals()`, `hashCode()`, `toString()`, `copy()` などのメソッドが自動生成されるクラスを簡単に作成可能。

```kotlin
// 基本的なクラスとプライマリコンストラクタ
class UserProfile(val userId: String, var displayName: String) {
    fun printInfo() {
        println("ID: $userId, Name: $displayName") // プロパティに直接アクセス
    }
}

// データクラス
data class ServerInfo(val ipAddress: String, val port: Int)

fun main() { // main関数はクラス外でもOK
    val user = UserProfile("user001", "Taro")
    user.displayName = "Jiro" // var なので変更可能 (セッターが呼ばれる)
    user.printInfo()
    println("ユーザーID: ${user.userId}") // ゲッターが呼ばれる

    val server1 = ServerInfo("192.168.1.1", 80)
    val server2 = server1.copy(port = 443) // copy() で一部変更した新しいインスタンスを作成
    println(server1) // toString() が自動生成されている
    println(server2)
    println("サーバーは同じか: ${server1 == server2}") // equals() が自動生成されている (false)
}
```

### Java
* **冗長さ:** フィールド、コンストラクタ、ゲッター/セッターなどを明示的に記述する必要があることが多い。
* **コンストラクタ:** クラス名と同じ名前のメソッドとして定義。
* **フィールドとメソッド:** フィールド（変数）とメソッド（関数）をクラス内に記述。ゲッター/セッターは手動またはIDEの機能で生成。
* **データクラス:** 標準的な機能はない。Lombokなどのライブラリでボイラープレート削減が可能。

```java
public class UserProfile {
    private final String userId; // final で読み取り専用フィールド
    private String displayName;  // フィールド

    // コンストラクタ
    public UserProfile(String userId, String displayName) {
        this.userId = userId;
        this.displayName = displayName;
    }

    // ゲッター
    public String getUserId() {
        return userId;
    }

    public String getDisplayName() {
        return displayName;
    }

    // セッター (displayName用)
    public void setDisplayName(String displayName) {
        this.displayName = displayName;
    }

    public void printInfo() {
        System.out.println("ID: " + userId + ", Name: " + displayName);
    }

    // equals(), hashCode(), toString() などは手動で実装する必要がある
}

// main メソッドを持つ別のクラス (例)
class App {
    public static void main(String[] args) {
        UserProfile user = new UserProfile("user001", "Taro");
        user.setDisplayName("Jiro"); // セッターを呼び出す
        user.printInfo();
        System.out.println("ユーザーID: " + user.getUserId()); // ゲッターを呼び出す
    }
}
```

## 4. その他の主な違い (コード例付き)

* **セミコロン:** Kotlinでは行末のセミコロンは通常不要。Javaでは必須。
    ```kotlin
    println("Kotlinはセミコロン不要") // OK
    ```java
    System.out.println("Javaはセミコロンが必要"); // 必須
    ```

* **文字列テンプレート:** Kotlinは `"$variable"` や `"${expression}"` で簡単に文字列内に値を埋め込める。Javaは `+` での連結が基本。
    ```kotlin
    val language = "Kotlin"
    val year = 2024
    println("$language は ${year}年も便利！") // 出力: Kotlin は 2024年も便利！
    ```java
    String language = "Java";
    int year = 2024;
    System.out.println(language + " は " + year + "年も使われる"); // 出力: Java は 2024年も使われる
    ```

* **拡張関数:** Kotlinでは既存のクラスを継承せずに新しい関数を追加できる。
    ```kotlin
    // IntクラスにisEven (偶数か？) という関数を追加
    fun Int.isEven(): Boolean {
        return this % 2 == 0
    }

    val number = 10
    println("$number は偶数か？ ${number.isEven()}") // 出力: 10 は偶数か？ true
    ```
  *(Javaには直接の代替機能なし。通常はstaticメソッドを持つユーティリティクラスを作成)*

* **スマートキャスト:** Kotlinでは `is` で型チェックした後、そのスコープ内では自動的にキャストされた型として扱える。Javaでは明示的なキャストが必要。
    ```kotlin
    fun processInput(input: Any) { // Anyは何でも受け取れる型
        if (input is String) {
            // このブロック内では input は String として扱える
            println("文字列の長さ: ${input.length}") // 明示的なキャスト不要
        } else if (input is Int) {
            println("数値の2倍: ${input * 2}") // Int として扱える
        }
    }
    processInput("テスト")
    processInput(100)
    ```java
    public static void processInput(Object input) { // Objectは何でも受け取れる型
        if (input instanceof String) {
            String strInput = (String) input; // 明示的なキャストが必要
            System.out.println("文字列の長さ: " + strInput.length());
        } else if (input instanceof Integer) {
            Integer intInput = (Integer) input; // 明示的なキャストが必要
            System.out.println("数値の2倍: " + (intInput * 2));
        }
    }
    ```

* **when式:** Kotlinの `when` はJavaの `switch` より強力で、式としても利用でき、多様な条件分岐に使える。
    ```kotlin
    fun getResponseType(code: Int): String {
        return when (code) { // 式として結果を返す
            200 -> "OK"
            404 -> "Not Found"
            in 500..599 -> "Server Error" // 範囲指定も可能
            else -> "Unknown Code"
        }
    }
    println("コード404の意味: ${getResponseType(404)}") // 出力: コード404の意味: Not Found
    ```
  *(Javaのswitch文は、最近のバージョンで機能強化されたが、Kotlinのwhenほど柔軟ではない)*

* **main関数:** Kotlinの `main` 関数はトップレベル（クラス外）に定義可能で、引数なしでもよい。Javaはクラス内の `public static void main(String[] args)` が必須。
    ```kotlin
    // ファイルのトップレベルに記述可能
    fun main() { // 引数 (args: Array<String>) は省略可能
        println("Kotlinのシンプルなmain関数")
    }
    ```java
    // 必ずクラス内に記述
    public class Application {
        public static void main(String[] args) { // この形式が必須
            System.out.println("Javaのmainメソッド");
        }
    }
    ```

