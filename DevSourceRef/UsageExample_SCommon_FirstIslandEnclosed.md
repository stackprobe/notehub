# SCommon Parse/Get First Island/Enclosed usage example

```csharp
using System;
using HLTStudio.Commons;

namespace HLTStudio.Tests
{
	public class UsageExample_SCommon_FirstIslandEnclosed
	{
		public static void Test01()
		{
			{
				string RES = @"

<root>
<AAA/>
<bBb/>
<CcC/>
</root>

";

				string text = RES;

				for (; ; )
				{
					int tagIndex;
					string[] slnd = SCommon.ParseFirstIsland(text, out tagIndex
						, "<AAA/>", false
						, "<bbb/>", true
						, "<ccc/>", true
						);

					if (slnd == null)
						break;

					Console.WriteLine(tagIndex + "\t" + slnd[1]);
					text = slnd[2];
				}
			}

			{
				string RES = @"

<root>
<a>AAA</a>
<b>BBB</b>
<c>CCC</c>
</root>

";

				string text = RES;

				for (; ; )
				{
					int tagIndex;
					string[] encl = SCommon.ParseFirstEnclosed(text, out tagIndex
						, "<a>", "</a>", false
						, "<B>", "</B>", true
						, "<C>", "</c>", true
						);

					if (encl == null)
						break;

					Console.WriteLine(tagIndex + "\t" + encl[2]);
					text = encl[4];
				}
			}

			{
				string RES = @"

<root>
<XXX/>
<YyY/>
<zZz/>
</root>

";

				string text = RES;
				int index = 0;

				for (; ; )
				{
					int tagIndex;
					int[] slnd = SCommon.GetFirstIsland(text, index, out tagIndex
						, "<XXX/>", false
						, "<yyy/>", true
						, "<zzz/>", true
						);

					if (slnd == null)
						break;

					Console.WriteLine(tagIndex + "\t" + text.Substring(slnd[0], slnd[1] - slnd[0]));
					index = slnd[1];
				}
			}

			{
				string RES = @"

<root>
<x>XXX</x>
<y>YYY</y>
<z>ZZZ</z>
</root>

";

				string text = RES;
				int index = 0;

				for (; ; )
				{
					int tagIndex;
					int[] encl = SCommon.GetFirstEnclosed(text, index, out tagIndex
						, "<x>", "</x>", false
						, "<Y>", "</Y>", true
						, "<z>", "</Z>", true
						);

					if (encl == null)
						break;

					Console.WriteLine(tagIndex + "\t" + text.Substring(encl[1], encl[2] - encl[1]));
					index = encl[3];
				}
			}
		}

#if false // 出力

0       <AAA/>
1       <bBb/>
2       <CcC/>
0       AAA
1       BBB
2       CCC
0       <XXX/>
1       <YyY/>
2       <zZz/>
0       XXX
1       YYY
2       ZZZ
Press ENTER key.

#endif
	}
}
```
