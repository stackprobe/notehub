# SCommon KVArray usage example

```csharp
using System;
using System.Linq;
using HLTStudio.Commons;

namespace HLTStudio.Tests
{
	public class UsageExample_SCommon_KVArray
	{
		public static void Test01()
		{
			string RES = @"

東京都.新宿区.西新宿
東京都.新宿区.歌舞伎町
東京都.新宿区.大久保
東京都.渋谷区.神南
東京都.渋谷区.恵比寿
東京都.渋谷区.代々木
東京都.港区.六本木
東京都.港区.芝公園
東京都.港区.青山
大阪府.大阪市北区.梅田
大阪府.大阪市北区.中之島
大阪府.大阪市北区.天満
大阪府.大阪市中央区.心斎橋
大阪府.大阪市中央区.本町
大阪府.大阪市中央区.難波
大阪府.堺市堺区.熊野町
大阪府.堺市堺区.戎島町
大阪府.堺市堺区.大仙町
福岡県.福岡市博多区.博多駅前
福岡県.福岡市博多区.祇園町
福岡県.福岡市博多区.中洲
福岡県.福岡市中央区.天神
福岡県.福岡市中央区.大名
福岡県.福岡市中央区.赤坂
福岡県.北九州市小倉北区.魚町
福岡県.北九州市小倉北区.室町
福岡県.北九州市小倉北区.馬借

";

			string[] res = SCommon.TextToLines(RES.Trim());

			SCommon.KVArray<string, string> kva = new SCommon.KVArray<string, string>(
				res,
				res.Select((r, i) => $"{r} ({i})").ToArray(),
				(a, b) =>
				{
					string[] ts1 = a.Split('.');
					string[] ts2 = b.Split('.');

					int depth = Math.Min(ts1.Length, ts2.Length);

					for (int i = 0; i < depth; i++)
					{
						int ret = SCommon.Comp(ts1[i], ts2[i]);

						if (ret != 0)
							return ret;
					}
					return 0;
				});

			Test01_a2(kva, "大阪府");
			Test01_a2(kva, "福岡県.福岡市博多区");
			Test01_a2(kva, "東京都.新宿区.歌舞伎町");

			Test01_a3(kva, "福岡県");
			Test01_a3(kva, "東京都.渋谷区");
			Test01_a3(kva, "大阪府.大阪市中央区.本町");
		}

		private static void Test01_a2(SCommon.KVArray<string, string> kva, string key)
		{
			int[] range = kva.GetRange(key);

			for (int i = range[0] + 1; i < range[1]; i++)
			{
				Console.WriteLine(kva.ElementAt(i).Value);
			}
			Console.WriteLine();
		}

		private static void Test01_a3(SCommon.KVArray<string, string> kva, string key)
		{
			int depth = key.Split('.').Length;

			int[] range = kva.UnsafeGetRange(key, (a, b) =>
			{
				string[] ts1 = a.Split('.');
				string[] ts2 = b.Split('.');

				for (int i = 0; i < depth; i++)
				{
					int ret = SCommon.Comp(ts1[i], ts2[i]);

					if (ret != 0)
						return ret;
				}
				return 0;
			});

			for (int i = range[0] + 1; i < range[1]; i++)
			{
				Console.WriteLine(kva.ElementAt(i).Value);
			}
			Console.WriteLine();
		}

#if false // 出力

大阪府.堺市堺区.大仙町 (17)
大阪府.堺市堺区.戎島町 (16)
大阪府.堺市堺区.熊野町 (15)
大阪府.大阪市中央区.心斎橋 (12)
大阪府.大阪市中央区.本町 (13)
大阪府.大阪市中央区.難波 (14)
大阪府.大阪市北区.中之島 (10)
大阪府.大阪市北区.天満 (11)
大阪府.大阪市北区.梅田 (9)

福岡県.福岡市博多区.中洲 (20)
福岡県.福岡市博多区.博多駅前 (18)
福岡県.福岡市博多区.祇園町 (19)

東京都.新宿区.歌舞伎町 (1)

福岡県.北九州市小倉北区.室町 (25)
福岡県.北九州市小倉北区.馬借 (26)
福岡県.北九州市小倉北区.魚町 (24)
福岡県.福岡市中央区.大名 (22)
福岡県.福岡市中央区.天神 (21)
福岡県.福岡市中央区.赤坂 (23)
福岡県.福岡市博多区.中洲 (20)
福岡県.福岡市博多区.博多駅前 (18)
福岡県.福岡市博多区.祇園町 (19)

東京都.渋谷区.代々木 (5)
東京都.渋谷区.恵比寿 (4)
東京都.渋谷区.神南 (3)

大阪府.大阪市中央区.本町 (13)

Press ENTER key.

#endif
	}
}
```
