
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

 byte[]をReadOnlySpan<byte>で分けた時と比較したベンチマーク

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

 longをSpan<byte>とbyte[]に変換した場合の比較ベンチマーク
 
例  public static byte[] GetBytesFromInt64_SpanToArray(long value)
    {
        Span<byte> buffer = stackalloc byte[21];

        int offset = 0;
        uint num1, num2, num3, num4, num5, div;
        ulong valueA, valueB;

        if (value < 0)
        {
            if (value == long.MinValue)
            {
                ReadOnlySpan<byte> minValue = "-9223372036854775808"u8;
                return minValue.ToArray();
            }

            buffer[offset++] = (byte)'-';
            valueA = (ulong)(unchecked(-value));
        }
        else
        {
            valueA = (ulong)value;
        }

        if (valueA < 10000)
        {
            num1 = (uint)valueA;
            if (num1 < 10) { goto L1; }
            if (num1 < 100) { goto L2; }
            if (num1 < 1000) { goto L3; }
            goto L4;
        }
        else
        {
            valueB = valueA / 10000;
            num1 = (uint)(valueA - valueB * 10000);
            if (valueB < 10000)
            {
                num2 = (uint)valueB;
                if (num2 < 100)
                {
                    if (num2 < 10) { goto L5; }
                    goto L6;
                }
                if (num2 < 1000) { goto L7; }
                goto L8;
            }
            else
            {
                valueA = valueB / 10000;
                num2 = (uint)(valueB - valueA * 10000);
                if (valueA < 10000)
                {
                    num3 = (uint)valueA;
                    if (num3 < 100)
                    {
                        if (num3 < 10) { goto L9; }
                        goto L10;
                    }
                    if (num3 < 1000) { goto L11; }
                    goto L12;
                }
                else
                {
                    valueB = valueA / 10000;
                    num3 = (uint)(valueA - valueB * 10000);
                    if (valueB < 10000)
                    {
                        num4 = (uint)valueB;
                        if (num4 < 100)
                        {
                            if (num4 < 10) { goto L13; }
                            goto L14;
                        }
                        if (num4 < 1000) { goto L15; }
                        goto L16;
                    }
                    else
                    {
                        valueA = valueB / 10000;
                        num4 = (uint)(valueB - valueA * 10000);
                        //if (value2 < 10000)
                        {
                            num5 = (uint)valueA;
                            if (num5 < 100)
                            {
                                if (num5 < 10) { goto L17; }
                                goto L18;
                            }
                            if (num5 < 1000) { goto L19; }
                            goto L20;
                        }
                    L20:
                        buffer[offset++] = (byte)('0' + (div = (num5 * 8389) >> 23));
                        num5 -= div * 1000;
                    L19:
                        buffer[offset++] = (byte)('0' + (div = (num5 * 5243) >> 19));
                        num5 -= div * 100;
                    L18:
                        buffer[offset++] = (byte)('0' + (div = (num5 * 6554) >> 16));
                        num5 -= div * 10;
                    L17:
                        buffer[offset++] = (byte)('0' + (num5));
                    }
                L16:
                    buffer[offset++] = (byte)('0' + (div = (num4 * 8389) >> 23));
                    num4 -= div * 1000;
                L15:
                    buffer[offset++] = (byte)('0' + (div = (num4 * 5243) >> 19));
                    num4 -= div * 100;
                L14:
                    buffer[offset++] = (byte)('0' + (div = (num4 * 6554) >> 16));
                    num4 -= div * 10;
                L13:
                    buffer[offset++] = (byte)('0' + (num4));
                }
            L12:
                buffer[offset++] = (byte)('0' + (div = (num3 * 8389) >> 23));
                num3 -= div * 1000;
            L11:
                buffer[offset++] = (byte)('0' + (div = (num3 * 5243) >> 19));
                num3 -= div * 100;
            L10:
                buffer[offset++] = (byte)('0' + (div = (num3 * 6554) >> 16));
                num3 -= div * 10;
            L9:
                buffer[offset++] = (byte)('0' + (num3));
            }
        L8:
            buffer[offset++] = (byte)('0' + (div = (num2 * 8389) >> 23));
            num2 -= div * 1000;
        L7:
            buffer[offset++] = (byte)('0' + (div = (num2 * 5243) >> 19));
            num2 -= div * 100;
        L6:
            buffer[offset++] = (byte)('0' + (div = (num2 * 6554) >> 16));
            num2 -= div * 10;
        L5:
            buffer[offset++] = (byte)('0' + (num2));
        }
    L4:
        buffer[offset++] = (byte)('0' + (div = (num1 * 8389) >> 23));
        num1 -= div * 1000;
    L3:
        buffer[offset++] = (byte)('0' + (div = (num1 * 5243) >> 19));
        num1 -= div * 100;
    L2:
        buffer[offset++] = (byte)('0' + (div = (num1 * 6554) >> 16));
        num1 -= div * 10;
    L1:
        buffer[offset++] = (byte)('0' + (num1));

        return buffer[..offset].ToArray();
    }

    public static byte[] GetBytesFromInt64_DirectArray(long value)
    {
        int minusAdd = 0;
        byte[] buffer;

        int offset = 0;
        uint num1, num2, num3, num4, num5, div;
        ulong valueA, valueB;

        if (value < 0)
        {
            if (value == long.MinValue)
            {
                ReadOnlySpan<byte> minValue = "-9223372036854775808"u8;
                return minValue.ToArray();
            }

            minusAdd = 1;
            offset = 1;
            valueA = (ulong)(unchecked(-value));
        }
        else
        {
            valueA = (ulong)value;
        }

        if (valueA < 10000)
        {
            num1 = (uint)valueA;
            if (num1 < 10) { buffer = new byte[minusAdd + 1]; goto L1; }
            if (num1 < 100) { buffer = new byte[minusAdd + 2]; goto L2; }
            if (num1 < 1000) { buffer = new byte[minusAdd + 3]; goto L3; }
            buffer = new byte[minusAdd + 4]; goto L4;
        }
        else
        {
            valueB = valueA / 10000;
            num1 = (uint)(valueA - valueB * 10000);
            if (valueB < 10000)
            {
                num2 = (uint)valueB;
                if (num2 < 100)
                {
                    if (num2 < 10) { buffer = new byte[minusAdd + 5]; goto L5; }
                    buffer = new byte[minusAdd + 6]; goto L6;
                }
                if (num2 < 1000) { buffer = new byte[minusAdd + 7]; goto L7; }
                buffer = new byte[minusAdd + 8]; goto L8;
            }
            else
            {
                valueA = valueB / 10000;
                num2 = (uint)(valueB - valueA * 10000);
                if (valueA < 10000)
                {
                    num3 = (uint)valueA;
                    if (num3 < 100)
                    {
                        if (num3 < 10) { buffer = new byte[minusAdd + 9]; goto L9; }
                        buffer = new byte[minusAdd + 10]; goto L10;
                    }
                    if (num3 < 1000) { buffer = new byte[minusAdd + 11]; goto L11; }
                    buffer = new byte[minusAdd + 12]; goto L12;
                }
                else
                {
                    valueB = valueA / 10000;
                    num3 = (uint)(valueA - valueB * 10000);
                    if (valueB < 10000)
                    {
                        num4 = (uint)valueB;
                        if (num4 < 100)
                        {
                            if (num4 < 10) { buffer = new byte[minusAdd + 13]; goto L13; }
                            buffer = new byte[minusAdd + 14]; goto L14;
                        }
                        if (num4 < 1000) { buffer = new byte[minusAdd + 15]; goto L15; }
                        buffer = new byte[minusAdd + 16]; goto L16;
                    }
                    else
                    {
                        valueA = valueB / 10000;
                        num4 = (uint)(valueB - valueA * 10000);
                        //if (value2 < 10000)
                        {
                            num5 = (uint)valueA;
                            if (num5 < 100)
                            {
                                if (num5 < 10) { buffer = new byte[minusAdd + 17]; goto L17; }
                                buffer = new byte[minusAdd + 18]; goto L18;
                            }
                            if (num5 < 1000) { buffer = new byte[minusAdd + 19]; goto L19; }
                            buffer = new byte[minusAdd + 20]; goto L20;
                        }
                    L20:
                        buffer[offset++] = (byte)('0' + (div = (num5 * 8389) >> 23));
                        num5 -= div * 1000;
                    L19:
                        buffer[offset++] = (byte)('0' + (div = (num5 * 5243) >> 19));
                        num5 -= div * 100;
                    L18:
                        buffer[offset++] = (byte)('0' + (div = (num5 * 6554) >> 16));
                        num5 -= div * 10;
                    L17:
                        buffer[offset++] = (byte)('0' + (num5));
                    }
                L16:
                    buffer[offset++] = (byte)('0' + (div = (num4 * 8389) >> 23));
                    num4 -= div * 1000;
                L15:
                    buffer[offset++] = (byte)('0' + (div = (num4 * 5243) >> 19));
                    num4 -= div * 100;
                L14:
                    buffer[offset++] = (byte)('0' + (div = (num4 * 6554) >> 16));
                    num4 -= div * 10;
                L13:
                    buffer[offset++] = (byte)('0' + (num4));
                }
            L12:
                buffer[offset++] = (byte)('0' + (div = (num3 * 8389) >> 23));
                num3 -= div * 1000;
            L11:
                buffer[offset++] = (byte)('0' + (div = (num3 * 5243) >> 19));
                num3 -= div * 100;
            L10:
                buffer[offset++] = (byte)('0' + (div = (num3 * 6554) >> 16));
                num3 -= div * 10;
            L9:
                buffer[offset++] = (byte)('0' + (num3));
            }
        L8:
            buffer[offset++] = (byte)('0' + (div = (num2 * 8389) >> 23));
            num2 -= div * 1000;
        L7:
            buffer[offset++] = (byte)('0' + (div = (num2 * 5243) >> 19));
            num2 -= div * 100;
        L6:
            buffer[offset++] = (byte)('0' + (div = (num2 * 6554) >> 16));
            num2 -= div * 10;
        L5:
            buffer[offset++] = (byte)('0' + (num2));
        }
    L4:
        buffer[offset++] = (byte)('0' + (div = (num1 * 8389) >> 23));
        num1 -= div * 1000;
    L3:
        buffer[offset++] = (byte)('0' + (div = (num1 * 5243) >> 19));
        num1 -= div * 100;
    L2:
        buffer[offset++] = (byte)('0' + (div = (num1 * 6554) >> 16));
        num1 -= div * 10;
    L1:
        buffer[offset++] = (byte)('0' + (num1));

        if (minusAdd != 0) { buffer[0] = (byte)'-'; }
        return buffer;
    }


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