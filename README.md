
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

    [Benchmark]
    public int StringSwitchBench()
    {
        int sum = 0;
        sum += AnalyzeStringSwitch(_start.stringValue);
        sum += AnalyzeStringSwitch(_show.stringValue);
        sum += AnalyzeStringSwitch(_stop.stringValue);
        sum += AnalyzeStringSwitch(_quit.stringValue);

        return sum;
    }

| Method                        | Job        | Runtime  | Mean       | Error     | StdDev    | Median     | Ratio | RatioSD |
|------------------------------ |----------- |--------- |-----------:|----------:|----------:|-----------:|------:|--------:|
| StringSwitchBench             | Job-DDXAAB | .NET 8.0 |   7.044 ns | 0.1661 ns | 0.3575 ns |   7.007 ns |  1.00 |    0.07 |
| StringSwitchBench             | Job-WLOQWX | .NET 9.0 |   7.962 ns | 0.1835 ns | 0.4502 ns |   7.854 ns |  1.13 |    0.08 |
| StringSwitchBench             | InProcess  | .NET 8.0 |   6.754 ns | 0.1600 ns | 0.2490 ns |   6.796 ns |  0.96 |    0.06 |
|                               |            |          |            |           |           |            |       |         |
| StringIfBench                 | Job-DDXAAB | .NET 8.0 |   6.836 ns | 0.1606 ns | 0.4029 ns |   6.765 ns |  1.00 |    0.08 |
| StringIfBench                 | Job-WLOQWX | .NET 9.0 |   7.461 ns | 0.1703 ns | 0.2273 ns |   7.403 ns |  1.10 |    0.07 |
| StringIfBench                 | InProcess  | .NET 8.0 |   7.370 ns | 0.1709 ns | 0.2451 ns |   7.324 ns |  1.08 |    0.07 |
|                               |            |          |            |           |           |            |       |         |
| StaticBytesSequenceEqualBench | Job-DDXAAB | .NET 8.0 | 167.429 ns | 3.3318 ns | 6.4192 ns | 165.706 ns |  1.00 |    0.05 |
| StaticBytesSequenceEqualBench | Job-WLOQWX | .NET 9.0 |  62.324 ns | 0.3659 ns | 0.2857 ns |  62.367 ns |  0.37 |    0.01 |
| StaticBytesSequenceEqualBench | InProcess  | .NET 8.0 | 164.451 ns | 3.3063 ns | 5.3391 ns | 162.255 ns |  0.98 |    0.05 |
|                               |            |          |            |           |           |            |       |         |
| StaticRosSequenceEqualBench   | Job-DDXAAB | .NET 8.0 |  20.165 ns | 0.4247 ns | 0.9673 ns |  19.744 ns |  1.00 |    0.07 |
| StaticRosSequenceEqualBench   | Job-WLOQWX | .NET 9.0 |  18.088 ns | 0.1299 ns | 0.1085 ns |  18.090 ns |  0.90 |    0.04 |
| StaticRosSequenceEqualBench   | InProcess  | .NET 8.0 |  20.198 ns | 0.3852 ns | 0.5272 ns |  20.103 ns |  1.00 |    0.05 |
|                               |            |          |            |           |           |            |       |         |
| StaticBytesMyEqualsBench      | Job-DDXAAB | .NET 8.0 |  18.799 ns | 0.3960 ns | 0.7724 ns |  18.741 ns |  1.00 |    0.06 |
| StaticBytesMyEqualsBench      | Job-WLOQWX | .NET 9.0 |  16.243 ns | 0.3189 ns | 0.3412 ns |  16.091 ns |  0.87 |    0.04 |
| StaticBytesMyEqualsBench      | InProcess  | .NET 8.0 |  17.075 ns | 0.3563 ns | 0.3659 ns |  17.208 ns |  0.91 |    0.04 |
|                               |            |          |            |           |           |            |       |         |
| U8BytesBench                  | Job-DDXAAB | .NET 8.0 |   9.424 ns | 0.1999 ns | 0.5300 ns |   9.326 ns |  1.00 |    0.08 |
| U8BytesBench                  | Job-WLOQWX | .NET 9.0 |   7.160 ns | 0.1011 ns | 0.1038 ns |   7.118 ns |  0.76 |    0.04 |
| U8BytesBench                  | InProcess  | .NET 8.0 |   8.293 ns | 0.0854 ns | 0.0713 ns |   8.311 ns |  0.88 |    0.05 |


■ByteArrayROSSplitBench

 byte[]をReadOnlySpan<byte>で分けた時と比較したベンチマーク

    [Benchmark]
    public int ReadOnlySpanSplitBench()
    {
        int sum = 0;

        for (int i = 0; i < 100; i++)
        {
            ReadOnlySpanSplit(SampleBytes, out var left, out var right);
            sum += left.Length + right.Length;
        }

        return sum;
    }


■[DivBench](https://gitan.dev/?p=275)

　Int、UInt、Long、ULongの整数の割り算を比較したベンチマーク

    [Benchmark]
    public int Div10IntBench()
    {
        int sum = 0;
        for (int i = 0; i < 10000; i++)
        {
            sum += i / 10;
        }
        return sum;
    }


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