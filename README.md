# Domore.Async.TextReading

**Domore.Async.TextReading** decodes text files and streams asynchronously, with pooled buffers and encoding detection. The core library targets .NET 6 and later; its WPF companion displays decoded text as it loads.

Both packages are MIT licensed and ship with SourceLink and symbol packages, so you can step straight into the source while debugging.

## Packages

| Package | What it does |
|---------|--------------|
| [Domore.Async.TextReading](#domoreasynctextreading) | Decode files and streams asynchronously with pooled buffers and encoding detection. |
| [Domore.Async.TextReading.Windows](#domoreasynctextreadingwindows) | Display asynchronously decoded text in a WPF control. |

Install any of them with `dotnet add package <name>`.

---

## Domore.Async.TextReading

Decode text files and streams asynchronously, with pooled buffers and encoding detection. List candidate encodings, and the first one in your list that decodes the content cleanly wins, which is handy for files that might be UTF-8 or might be legacy Latin-1. Byte-order marks are detected automatically.

```csharp
using Domore.IO.Extensions;
using Domore.Text;
using Domore.Text.Builders;

var options = new DecodedTextOptions { Encoding = { "utf-8", "iso-8859-1" } };
using (options.Disposable()) {
    var decoded = await new FileInfo("data.txt").DecodeText(
        new TextLineBuilder(onLine: line => Console.WriteLine(line)),
        options,
        token);

    Console.WriteLine(decoded.EncodingWebName); // e.g. iso-8859-1
}
```

Stream lines as they're decoded with `TextLineBuilder`, consume them as an `IAsyncEnumerable` with `TextStreamBuilder`, or collect the whole text with `TextStringBuilder`. Targets .NET 6 and later.

📖 [Full Domore.Async.TextReading documentation](source/Domore.Async.TextReading/README.md)

## Domore.Async.TextReading.Windows

A WPF companion to Domore.Async.TextReading. Its `TextReader` control displays text as it is decoded and exposes the detected encoding and loading status. It accepts an `IStreamText`, a file path, a `FileInfo`, or a local file `Uri`; relative string paths are resolved from the application directory. Call `Reload()` to read the current source again.

📖 [WPF control documentation](source/Domore.Async.TextReading.Windows/README.md)

---

## License

[MIT](LICENSE) © Ken Yourek
