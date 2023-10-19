■ **Gitan.SortedDictionaryとは**

Gitan.SortedDictionaryは、keyに基づいて並び替えを行うクラスです。

プロジェクトURL : [https://github.com/gitan-dev/SortedDictionary](https://github.com/gitan-dev/SortedDictionary)

■ **仕様**

・ 


■ **使用方法**

NuGetパッケージ : Gitan.SortedDictionary

NuGetを使用してGitan.SortedDictionaryパッケージをインストールします。

Gitan.SortedDictionaryを使用する方法を以下に記載します。

    using Gitan.SortedDictionary;

    public void AnyFirstTest()
    {
        var dic = new SortedDictionary<double, double>(false);
        KeyValuePair<double, double> item;

        Assert.IsTrue(dic.Any() == false);

        dic.Add(100, 1);
        Assert.IsTrue(dic.Any() == true);
        item = dic.First();
        Assert.IsTrue(item.Key == 100);
        Assert.IsTrue(item.Value == 1);

        dic.Add(200, 2);
        Assert.IsTrue(dic.Any() == true);
        item = dic.First();
        Assert.IsTrue(item.Key == 100);
        Assert.IsTrue(item.Value == 1);

        dic.Add(50, 0.5);
        Assert.IsTrue(dic.Any() == true);
        item = dic.First();
        Assert.IsTrue(item.Key == 50);
        Assert.IsTrue(item.Value == 0.5);

        dic.Remove(50);
        Assert.IsTrue(dic.Any() == true);
        item = dic.First();
        Assert.IsTrue(item.Key == 100);
        Assert.IsTrue(item.Value == 1);


        dic.Remove(100);
        Assert.IsTrue(dic.Any() == true);
        item = dic.First();
        Assert.IsTrue(item.Key == 200);
        Assert.IsTrue(item.Value == 2);

        dic.Remove(200);
        Assert.IsTrue(dic.Any() == false);
    }



■ **パフォーマンス**
   
**SortedDictionary**

public class SortedDictionaryBench
{
    public static KeyValuePair<int, int>[] GetPriceSize()
    {
        var priceSizeList = new List<KeyValuePair<int, int>>();

        for (var i = 0; i <= 2; i++)
        {
            for (var j = 0; j < 10000; j++)
            {
                var keyValuePair = new KeyValuePair<int, int>(j, i);
                priceSizeList.Add(keyValuePair);
            }
        }

        Random r = new();
        var priceSizeArray = priceSizeList.OrderBy(x => r.Next()).ToArray();

        return priceSizeArray;
    }

    [Benchmark]
    public void SortedDictionaryBench()
    {
        var priceSizeArray = GetPriceSize();

        var dic = new System.Collections.Generic.SortedDictionary<int, int>();

        foreach (var priceSize in priceSizeArray)
        {
            var (price, size) = priceSize;

            if (size == 0)
            {
                dic.Remove(price);
            }
            else
            {
                dic[price] = size;
            }
        }
    }

    [Benchmark]
    public void GitanSortedDictionaryBench()
    {
        var priceSizeArray = GetPriceSize();

        var dicGitan = new Gitan.SortedDictionary.SortedDictionary<int, int>(false);

        foreach (var priceSize in priceSizeArray)
        {
            var (price, size) = priceSize;

            if (size == 0)
            {
                dicGitan.Remove(price);
            }
            else
            {
                dicGitan.AddOrChangeValue(price, size);
            }
        }
    }
}

Benchmarkの結果、Gitan.SortedDictionary.SortedDictionaryは
System.Collections.Generic.SortedDictionaryから30％程度速度アップしました。

|                     Method |      Mean |     Error |    StdDev |
|--------------------------- |----------:|----------:|----------:|
|      SortedDictionaryBench | 10.601 ms | 0.1160 ms | 0.1029 ms |
| GitanSortedDictionaryBench |  7.758 ms | 0.0722 ms | 0.0640 ms |


■ **Api定義**

|プロパティ|説明|
| ------- | ---- |
|Count|SortedDictionary<TKey,TValue> に格納されているキー/値ペアの数を返します|
|Compare|指定された値と比較し、小さければ-1,同じなら0,大きければ1を返します|
|TotalCount|SortedDictionary<TKey,TValue> に格納されているキー/値ペアの数を返します|


|メソッド|説明|
| -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
|Add(TKey key, TValue value)|指定したキーおよび値をSortedDictionary<TKey,TValue> に追加します|
|Add(KeyValuePair<TKey, TValue> item)|指定したキーおよび値をSortedDictionary<TKey,TValue> に追加します|
|TryAdd(TKey key, TValue value)|指定したキーおよび値をSortedDictionary<TKey,TValue> に追加します。失敗時はfalseを返します|
|TryAdd(KeyValuePair<TKey, TValue> item)|指定したキーおよび値をSortedDictionary<TKey,TValue> に追加します。失敗時はfalseを返します|
|AddOrChangeValue|指定したキーおよび値をSortedDictionary<TKey,TValue> に追加,変更します|
|AddCore|指定したキーおよび値をSortedDictionary<TKey,TValue> に追加します|
|CopyTo(KeyValuePair<TKey, TValue>[] array)|指定したインデックスを開始位置として、KeyValuePair<TKey, TValue>をコピーします|
|CopyTo(KeyValuePair<TKey, TValue>[] array, int index)|指定したインデックスを開始位置として、KeyValuePair<TKey, TValue>をコピーします|
|CopyTo(KeyValuePair<TKey, TValue>[] array, int index, int count)|指定したインデックスを開始位置として、KeyValuePair<TKey, TValue>をコピーします|
|RemoveOrUnder|指定した値がKeyの最初の値より小さければSortedDictionary<TKey,TValue>から削除します|
|Remove(TKey key)|指定したキーを持つ要素をSortedDictionary<TKey,TValue>から削除します|
|Remove(KeyValuePair<TKey, TValue> item)|指定したキーを持つ要素をSortedDictionary<TKey,TValue>から削除します|
|Clear|すべての要素を削除します|
|Contains(KeyValuePair<TKey,TValue> item)|指定したKeyValuePair<TKey,TValue>があり、同じvalueを持つかどうかを返します|
|ContainsKey(TKey key)|指定したkeyと同じ値があるかどうかを返します|
|TryGetValue(TKey key, [MaybeNullWhen(false)] out TValue value)|指定したキーに関連付けられている値を返します|
|GetEnumerator|foreachの結果を返します|
|FindNode(TKey key)|指定したキーがあるかどうかを返します|
|Any|rootがNullじゃなければTrueを返します|
|First|最初のKeyのKeyValuePair<TKey, TValue>を返します|
|Last|最後のKeyのKeyValuePair<TKey, TValue>を返します|
|GetFirstKey|最初のKeyの値を返します|
|GetLastKey|最後のKeyの値を返します|
|Find(TKey key)|指定したキーがあるかどうかを返します|
|Log2|指定された値の整数の対数を返します|



■ 実装説明

・
