
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
　
■DivBench
　Int、UInt、Long、ULongの整数の割り算を比較したベンチマーク


■ForForeachBench
■HighPerformanceStringBench
　stringの作成を比較したベンチマーク


■IntLongWriteUtf8
■ListSortBench
　Listの並び替えの速度を比較したベンチマーク

■ReferenceUpdateBench
■SpanBench
■SpanToArrayDirectArrayBench

■StreamCopyBench
　Streamのデータを読み込む方法を比較したベンチマーク

■StringDollerBench
　文字列結合のパフォーマンス

■TenToTheNConversionBench　
　longで10のn乗するベンチマーク
■ToStringToArrayBench
■UnixTimeBench
■Utf8Bench
　Utf8文字列の作り方とパフォーマンス

■VariousBench
　C#のいろいろな、遅くなる要素のベンチマーク