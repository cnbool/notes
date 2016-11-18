title: C#方法参数关键字
id: 409
categories:
  - 'C#'
date: 2012-11-27 07:12:39
tags:
---

**1.ref关键字**

*   ref 关键字会导致通过引用传递的参数，而不是值。
*   传递给 ref 参数的参数必须初始化
*   “通过引用传递”概念与“引用类型”概念不同
*   方法参数无论是值类型还是引用类型，都可通过 ref 进行修饰<!--more-->
[code lang="csharp"]
class RefExample
    {
        static void Method(ref int i)
        {
            // Rest the mouse pointer over i to verify that it is an int.
            // The following statement would cause a compiler error if i
            // were boxed as an object.
            i = i + 44;
        }

        static void Main()
        {
            int val = 1;
            Method(ref val);
            Console.WriteLine(val);

            // Output: 45
        }
    }
```

**2.out关键字**

*   out 关键字会导致参数通过引用来传递
*   方法定义和调用方法都必须显式使用 out 关键字
*   out 参数传递的变量不必在传递之前进行初始化，但被调用的方法需要在返回之前赋一个值
希望方法返回多个值时，声明 out 方法很有用

[code lang="csharp"]
class OutReturnExample
    {
        static void Method(out int i, out string s1, out string s2)
        {
            i = 44;
            s1 = "I've been returned";
            s2 = null;
        }
        static void Main()
        {
            int value;
            string str1, str2;
            Method(out value, out str1, out str2);
            // value is now 44
            // str1 is now "I've been returned"
            // str2 is (still) null;
        }
    }
```

如果两个方法唯一的区别是：一个接受 ref 参数，另一个接受 out 参数，则无法重载这两个方法

[code lang="csharp"]
class CS0663_Example
{
    // Compiler error CS0663: "Cannot define overloaded
    // methods that differ only on ref and out".
    public void SampleMethod(out int i) { }
    public void SampleMethod(ref int i) { }
}
```

如果一个方法采用 ref 或 out 参数，而另一个方法不采用这两类参数，则可以进行重载

[code lang="csharp"]
class OutOverloadExample
{
    public void SampleMethod(int i) { }
    public void SampleMethod(out int i) { i = 5; }
}
```

**3.params关键字**

*   params 关键字可以指定采用数目可变的参数的方法参数
*   params 关键字之后不允许任何其他参数
*   在方法声明中只允许一个 params 关键字
[code lang="csharp"]
public class MyClass
{
    public static void UseParams(params int[] list)
    {
        for (int i = 0; i < list.Length; i++)
        {
            Console.Write(list[i] + " ");
        }
        Console.WriteLine();
    }

    public static void UseParams2(params object[] list)
    {
        for (int i = 0; i < list.Length; i++)
        {
            Console.Write(list[i] + " ");
        }
        Console.WriteLine();
    }

    static void Main()
    {
        // You can send a comma-separated list of arguments of the specified type.
        UseParams(1, 2, 3, 4);
        UseParams2(1, 'a', "test");

        // A params parameter accepts zero or more arguments.
        // The following calling statement displays only a blank line.
        UseParams2();

        // An array argument can be passed, as long as the array
        // type matches the parameter type of the method being called.
        int[] myIntArray = { 5, 6, 7, 8, 9 };
        UseParams(myIntArray);

        object[] myObjArray = { 2, 'b', "test", "again" };
        UseParams2(myObjArray);

        // The following call causes a compiler error because the object
        // array cannot be converted into an integer array.
        //UseParams(myObjArray);

        // The following call does not cause an error, but the entire
        // integer array becomes the first element of the params array.
        UseParams2(myIntArray);
    }
}
/*
Output:
    1 2 3 4
    1 a test

    5 6 7 8 9
    2 b test again
    System.Int32[]
*/
```

参考源:
[ http://msdn.microsoft.com/zh-cn/library/vstudio/14akc2c7.aspx](http://msdn.microsoft.com/zh-cn/library/vstudio/14akc2c7.aspx)
[ http://msdn.microsoft.com/zh-cn/library/vstudio/dd469487.aspx](http://msdn.microsoft.com/zh-cn/library/vstudio/dd469487.aspx)
[ http://msdn.microsoft.com/zh-cn/library/vstudio/w5zay9db.aspx](http://msdn.microsoft.com/zh-cn/library/vstudio/w5zay9db.aspx)
