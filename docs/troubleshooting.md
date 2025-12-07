# Troubleshooting / 開発メモ（自動生成）

このファイルは「詰まった→直した」を短く残すためのメモ。面接で話せる“経験”の証拠になります。

---

## 2025-12-07: `gradlew` が見つからない（PowerShell）

* 症状

  * `.\gradlew` が認識されず、`The term '.\gradlew' is not recognized...` が出る。
* 原因

  * 実行場所がプロジェクト直下ではなく、`gradlew(.bat)` が存在しないディレクトリで実行していた。
* 対処

  * プロジェクト直下へ移動してから実行。
  * PowerShellでは確実に `gradlew.bat` を使う。

```powershell
cd C:\Users\choco\tetris-java
.\gradlew.bat :app:run
.\gradlew.bat :app:test
```

* 結果

  * `gradlew.bat` が実行できるようになった。

---

## 2025-12-07: `:app:run` が configuration cache で失敗

* 症状

  * `:app:run` が `Invocation of 'Task.extensions' ... unsupported with the configuration cache` で失敗。
* 原因

  * Configuration Cache（設定キャッシュ）有効時に、`run(JavaExec)` が禁止動作に触れた。
* 対処

  * 一時対応：`--no-configuration-cache` を付けて実行。
  * 恒久対応：`gradle.properties` で configuration cache を無効化（必要なら）。

```powershell
.\gradlew.bat :app:run --no-configuration-cache
```

* 結果

  * `:app:run` は成功（JavaFX起動OK）。

---

## 2025-12-07: `:app:test` が JUnit Platform を読み込めず失敗

* 症状

  * `Failed to load JUnit Platform ... including the JUnit Platform launcher.`
* 原因

  * テスト実行時（runtime）のクラスパスに `junit-platform-launcher` が不足。
* 対処

  * `app/build.gradle` に `testRuntimeOnly 'org.junit.platform:junit-platform-launcher'` を追加。

```groovy
dependencies {
    testImplementation platform('org.junit:junit-bom:5.14.1')
    testImplementation 'org.junit.jupiter:junit-jupiter'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}

test {
    useJUnitPlatform()
}
```

* 確認コマンド

```powershell
.\gradlew.bat :app:clean :app:test --no-configuration-cache
```

* 結果

  * （ここに結果を書く：通った/まだエラー など）

---


