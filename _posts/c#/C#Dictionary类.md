title: C# Dictionary类
id: 197
categories:
  - 'C#'
date: 2012-10-24 23:20:22
tags:
---

**命名空间:**System.Collections.Generic
**程序集:**mscorlib（在 mscorlib.dll 中）
线程安全
只要不修改该集合，Dictionary 就可以同时支持多个阅读器。即便如此，从头到尾对一个集合进行枚举本质上并不是一个线程安全的过程。当出现枚举与写访问互相争用这种极少发生的情况时，必须在整个枚举过程中锁定集合。若要允许多个线程访问集合以进行读写操作，则必须实现自己的同步。<!--more-->

[code lang="csharp"]
public static void Main()
    {
        Dictionary<string, string> openWith = new Dictionary<string, string>();
        // The Add method throws an exception if the new key is
        // already in the dictionary.
        //如果新添加的key在dictionary已存在时Add方法抛出ArgumentException
        try
        {
            openWith.Add("txt", "winword.exe");
        }
        catch (ArgumentException)
        {
            Console.WriteLine("An element with Key = "txt" already exists.");
        }

        // The indexer throws an exception if the requested key is
        // not in the dictionary.
        //如果查找的key不存在于dictionary中,此索引抛出KeyNotFoundException
        try
        {
            Console.WriteLine("For key = "tif", value = {0}.", openWith["tif"]);
        }
        catch (KeyNotFoundException)
        {
            Console.WriteLine("Key = "tif" is not found.");
        }

        // When a program often has to try keys that turn out not to
        // be in the dictionary, TryGetValue can be a more efficient
        // way to retrieve values.
        //当程序查找key但key常不存在于dictionary中时,TryGetValue可以更有效的得到值
        string value = "";
        if (openWith.TryGetValue("tif", out value))
        {
            Console.WriteLine("For key = "tif", value = {0}.", value);
        }
        else
        {
            Console.WriteLine("Key = "tif" is not found.");
        }

        // The indexer can be used to change the value associated
        // with a key.
        // If a key does not exist, setting the indexer for that key
        // adds a new key/value pair.
        //key的索引可以用来改变key对应的value
        //如果一个key不存在,为此key的索引赋值从而增加一个键值对
        openWith["doc"] = "winword.exe";

        // ContainsKey can be used to test keys before inserting them.
        //ContainsKey方法用来检测dictionary是否存在某key
        if (!openWith.ContainsKey("ht"))
        {
            openWith.Add("ht", "hypertrm.exe");
            Console.WriteLine("Value added for key = "ht": {0}",
                openWith["ht"]);
        }

        // When you use foreach to enumerate dictionary elements,
        // the elements are retrieved as KeyValuePair objects.
        //当用foreach语句遍历dictionary中的元素,遍历时得到的是KeyValuePair对象
        foreach( KeyValuePair<string, string> kvp in openWith )
        {
            Console.WriteLine("Key = {0}, Value = {1}",
                kvp.Key, kvp.Value);
        }

        // To get the values alone, use the Values property.
        //单独得到dictionary中的values,使用Values属性
        Dictionary<string, string>.ValueCollection valueColl = openWith.Values;

        // The elements of the ValueCollection are strongly typed
        // with the type that was specified for dictionary values.
        foreach( string s in valueColl )
        {
            Console.WriteLine("Value = {0}", s);
        }

        // To get the keys alone, use the Keys property.
        //单独得到dictionary中的keys,使用Keys属性
        Dictionary<string, string>.KeyCollection keyColl = openWith.Keys;

        // The elements of the KeyCollection are strongly typed
        // with the type that was specified for dictionary keys.
        foreach( string s in keyColl )
        {
            Console.WriteLine("Key = {0}", s);
        }

        // Use the Remove method to remove a key/value pair.
        //使用Remove方法移除一个键值对
        openWith.Remove("doc");
    }
```
