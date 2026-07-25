# SCommon RESLines usage example

```csharp
using System;
using System.Linq;
using HLTStudio.Commons;

namespace HLTStudio.Tests
{
	public class UsageExample_SCommon_RESLines
	{
		public static void Test01()
		{
			{
				string RES = @"

AAA
BBB
CCC
/
DDD
EEE
/
FFF
/

";

				string[][] blocks = SCommon.RESLinesToBlocks(SCommon.TextToLines(RES.Trim()), line => line == "/");

				foreach (var block in blocks)
				{
					Console.WriteLine(string.Join("\t", block));
				}
			}

			{
				string RES = @"

AAA

BBB

CCC



DDD


EEE



FFF

";

				string[][] blocks = SCommon.RESLinesToBlocks(SCommon.TextToLines(RES.Trim()), 3);

				foreach (var block in blocks)
				{
					Console.WriteLine(string.Join("\t", block.Select(s => $"({s})")));
				}
			}

			{
				string RES = @"

*AAA
BBB
*CCC
DDD
EEE
*FFF
GGG
HHH
III

";

				string[][] blocks = SCommon.RESLinesToBlocks_HDR(SCommon.TextToLines(RES.Trim()), line => line[0] == '*');

				foreach (var block in blocks)
				{
					Console.WriteLine(string.Join("\t", block));
				}
			}

			{
				string RES = @"

AAA
BBB
CCC

DDD

EEE

FFF
GGG

";

				string[][] blocks = SCommon.RESLinesToBlocks_FBS(SCommon.TextToLines(RES.Trim()), 3);

				foreach (var block in blocks)
				{
					Console.WriteLine(string.Join("\t", block.Select(s => $"({s})")));
				}
			}

			{
				string RES = @"

AAA
	BBB
		CCC
		DDD
	EEE
		FFF
		GGG
		HHH
III
	JJJ
		KKK
			LLL

";

				// 空行であっても { インデント=0, Line="" } という有効なノードとして読み込まれることに注意！

				SCommon.RESTree_t root = SCommon.RESLinesToTree(SCommon.TextToLines(RES.Trim()));

				Test01_a(root, "");
			}
		}

		private static void Test01_a(SCommon.RESTree_t node, string indent)
		{
			Console.WriteLine(indent + node.Line);

			foreach (var child in node.Children)
			{
				Test01_a(child, indent + "\t");
			}
		}

#if false // 出力

AAA     BBB     CCC
DDD     EEE
FFF
(AAA)   ()      (BBB)   ()      (CCC)
(DDD)   ()      ()      (EEE)
(FFF)
AAA     BBB
CCC     DDD     EEE
FFF     GGG     HHH     III
(AAA)   (BBB)   (CCC)
()      (DDD)   ()
(EEE)   ()      (FFF)
(GGG)
<ROOT>
        AAA
                BBB
                        CCC
                        DDD
                EEE
                        FFF
                        GGG
                        HHH
        III
                JJJ
                        KKK
                                LLL
Press ENTER key.

#endif
	}
}
```
