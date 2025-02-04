<!-- # Read and write files -->
# ファイルの読み書き

<!-- ## Concepts -->
## 概念

<!--
- Deno's runtime API provides the
  [Deno.readTextFile](https://doc.deno.land/builtin/stable#Deno.readTextFile)
  and
  [Deno.writeTextFile](https://doc.deno.land/builtin/stable#Deno.writeTextFile)
  asynchronous functions for reading and writing entire text files.
- Like many of Deno's APIs, synchronous alternatives are also available. See
  [Deno.readTextFileSync](https://doc.deno.land/builtin/stable#Deno.readTextFileSync)
  and
  [Deno.writeTextFileSync](https://doc.deno.land/builtin/stable#Deno.writeTextFileSync).
- Use `--allow-read` and `--allow-write` permissions to gain access to the file
  system.
-->
- テキストファイルの読み書きのために、DenoのランタイムAPIは [Deno.readTextFile](https://doc.deno.land/builtin/stable#Deno.readTextFile) と [Deno.writeTextFile](https://doc.deno.land/builtin/stable#Deno.writeTextFile) 非同期関数を提供します。
- 多くのDENOのAPIと同じように、同等の同期関数も利用可能です。[Deno.readTextFileSync](https://doc.deno.land/builtin/stable#Deno.readTextFileSync) と [Deno.writeTextFileSync](https://doc.deno.land/builtin/stable#Deno.writeTextFileSync) を見てください。
- ファイルシステムへのアクセスには `--allow-read` と `--allow-write` パーミッションが必要です。

<!-- ## Overview -->
## 概要

<!--
Interacting with the filesystem to read and write files is a common requirement.
Deno provides a number of ways to do this via the
[standard library](https://deno.land/std) and the
[Deno runtime API](https://doc.deno.land/builtin/stable).
-->
ファイルシステムの読み書きは基本的な要件です。Denoはこれを行うための方法を [標準ライブラリ](https://deno.land/std) と [DenoランタイムAPI](https://doc.deno.land/builtin/stable) を通して提供しています。

<!--
As highlighted in the [Fetch Data example](./fetch_data) Deno restricts access
to Input / Output by default for security reasons. Therefore when interacting
with the filesystem the `--allow-read` and `--allow-write` flags must be used
with the `deno run` command.
-->
[データ取得の例](./fetch_data) で強調されていたようにDenoはセキュリティ上の理由からデフォルトでInput/Outputのアクセスを制限しています。そのためファイルシステムを操作する場合 `deno run` コマンドで `--allow-read` と `--allow-write` フラグを使用しなければいけません。

<!-- ## Reading a text file -->
## テキストファイルの読み込み

<!--
The Deno runtime API makes it possible to read text files via the
`Deno.readTextFile()` method, it just requires a path string or URL object. The
method returns a promise which provides access to the file's text data.
-->
DenoランタイムAPIは `Deno.readTextFile()` メソッドを通してテキストファイルを読み込むことを可能にしています。このメソッドはパスかURLオブジェクトを要求します。
このメソッドはファイルのテキストデータへのアクセスを提供するプロミスを返します。

**Command:** `deno run --allow-read read.ts`

```typescript
/**
 * read.ts
 */
const text = Deno.readTextFile("./people.json");

text.then((response) => console.log(response));

/**
 * Output:
 *
 * [
 *   {"id": 1, "name": "John", "age": 23},
 *   {"id": 2, "name": "Sandra", "age": 51},
 *   {"id": 5, "name": "Devika", "age": 11}
 * ]
 */
```

<!-- ## Writing a text file -->
## テキストファイルの書き込み

<!--
The Deno runtime API allows developers to write text to files via the
`Deno.writeTextFile()` method. It just requires a file path and text string. The
method returns a promise which resolves when the file was successfully written.
-->
DenoランタイムAPIは `Deno.writeTextFile()` メソッドを通してテキストファイルを読み込むことを可能にしています。このメソッドはパスかURLオブジェクトを要求します。このメソッドはファイルが正常に書き込まれたときに解決するプロミスを返します。

<!--
To run the command the `--allow-write` flag must be supplied to the `deno run`
command.
-->
コマンドを実行するためには、`deno run` コマンドに `--allow-write` フラグが必要です。

**Command:** `deno run --allow-write write.ts`

```typescript
/**
 * write.ts
 */
const write = Deno.writeTextFile("./hello.txt", "Hello World!");

write.then(() => console.log("File written to ./hello.txt"));

/**
 * Output: File written to ./hello.txt
 */
```

<!--
By combining `Deno.writeTextFile` and `JSON.stringify` you can easily write
serialized JSON objects to a file. This example uses synchronous
`Deno.writeTextFileSync`, but this can also be done asynchronously using
`await Deno.writeTextFile`.
-->
`Deno.writeTextFile` と `JSON.stringify` を組み合わせることで、シリアライズされたJSONオブジェクトを簡単にファイルに書き込むことができます。この例では、同期的な `Deno.writeTextFileSync` を使用していますが、`await Deno.writeTextFile` を使用して非同期的に行うこともできます。

<!-- To execute the code the `deno run` command needs the write flag. -->
コードを実行するには `deno run` コマンドに write フラグが必要です。

<!-- **Command:** `deno run --allow-write write.ts` -->
**コマンド:** `deno run --allow-write write.ts`

```typescript
/**
 * write.ts
 */
function writeJson(path: string, data: object): string {
  try {
    Deno.writeTextFileSync(path, JSON.stringify(data));

    return "Written to " + path;
  } catch (e) {
    return e.message;
  }
}

console.log(writeJson("./data.json", { hello: "World" }));

/**
 * Output: Written to ./data.json
 */
```