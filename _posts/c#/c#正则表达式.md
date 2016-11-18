title: c#正则表达式
id: 1219
categories:
  - 'C#'
date: 2013-11-08 01:17:00
tags:
---

Match.IsMatch 正则表达式在字符串中是否找到匹配项
[code lang="csharp"]
string content = "小明今年10岁了";
Regex regex = new Regex(@"d{2}");
bool result = regex.IsMatch(content);  // result is true

content = "小王今年9岁了";
result = regex.IsMatch(content);  // result is false
```

Match.Value 获得匹配内容
[code lang="csharp"]
string content = "小明今年10岁了";
Regex regex = new Regex(@"d{2}");
Match match = regex.Match(content);
if (match.Success)
{
    string age = match.Value;  // 10
}
```
<!--more-->
Match.Groups 正则表达式匹配的组的集合
[code lang="csharp"]
string content = "小明今年10岁了";
Regex regex = new Regex(@"(w{1,})今年(d{1,})岁");
Match match = regex.Match(content);
if (match.Success)
{
    string matchStr = match.Groups[0].Value;  // 小明今年10岁
    string name = match.Groups[1].Value;  // 小明
    string age = match.Groups[2].Value;  // 10
}
```

Regex.Matches 搜索正则表达式的所有匹配项
[code lang="csharp"]
string content = "小明今年10岁了,小王今年8岁了,小白今年12岁了.";
Regex regex = new Regex(@"(w{1,})今年(d{1,})岁");
MatchCollection matchColl = regex.Matches(content);
int matchCount = matchColl.Count;  // 捕获数3

IEnumerator i = matchColl.GetEnumerator();
while (i.MoveNext())
{
    Match match = (Match) i.Current;
    string name = match.Groups[1].Value;
    string age = match.Groups[2].Value;
    //output name age
}

//output:
//小明 10
//小王 8
//小白 12
```
