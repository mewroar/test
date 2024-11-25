
■Benchmarkとは
Benchmarkは、いろんな方法のパフォーマンスを比較するためのベンチマークです。
ベンチマーク用のライブラリ「BenchmarkDotNet」を使用して実装されています。

プロジェクトURL : [https://github.com/gitan-dev/Benchmark](https://github.com/gitan-dev/Benchmark)

■前提条件
・計測したいバージョンに応じた.NET SDKをインストール
・BenchmarkDotNetライブラリ




全てのベンチマークを実行するコマンド

dotnet run -c Release -f net9.0 --filter "*" --runtimes net8.0 net9.0

特定のベンチマークを実行する

BenchmarkDotNet.Running.BenchmarkRunner.Run<（実行したいベンチマーク）>();



■[ByteArrayBench](https://gitan.dev/?p=213)

　Utf8のbyte[]のコマンドを、どのコマンドか調べるために比較コード
　switch/if、SequenceEquaを使って
　書きやすさ、読みやすさ、パフォーマンスを見たベンチマーク

■ByteArrayROSSplitBench


■[DivBench](https://gitan.dev/?p=275)

　Int、UInt、Long、ULongの整数の割り算を比較したベンチマーク

■[ForForeachBench](https://gitan.dev/?p=180)

　配列かListをfor、foreachで要素を足していくベンチマーク

■[HighPerformanceStringBench](https://gitan.dev/?p=336)

　stringの作成を比較したベンチマーク

■IntLongWriteUtf8

■[ListSortBench](https://gitan.dev/?p=124)

　Listの並び替えの速度を比較したベンチマーク

■[ReferenceUpdateBench](https://gitan.dev/?p=171)

　
■[SpanBench](https://gitan.dev/?p=55)

 byte[]のCopyでSpanを使った速度比較

■SpanToArrayDirectArrayBench

■[StreamCopyBench](https://gitan.dev/?p=180)

　Streamのデータを読み込む方法を比較したベンチマーク

■[StringDollerBench](https://gitan.dev/?p=148)

　文字列結合のパフォーマンス

■[TenToTheNConversionBench](https://gitan.dev/?p=230)　

　longで10のn乗するベンチマーク

■ToStringToArrayBench

■[UnixTimeBench](https://gitan.dev/?p=358)

　UnixTimeを作る方法のベンチマーク

■[Utf8Bench](https://gitan.dev/?p=320)

　Utf8文字列の作り方とパフォーマンス

■[VariousBench](https://gitan.dev/?p=109)

　C#のいろいろな、遅くなる要素のベンチマーク