# Difference between var and dynamic in C#

[back](../README.md)

## Background

Variables declared with *var* are implicitly but statically typed. Variables declared with *dynamic* are dynamically typed. This means that *dynamic* declarations are resolved at runtime, *var* declarations are resolved at compile time.

## Table of Difference

|var|dynamic|
|-|-|
|Introduced in **C# 3.0**|Introduced in **C# 4.0**|
|**Statically typed** - the type of variable declared is decided by the compiler at compile time.|**Dynamic typed** - the type of variable declared is decided by the compiler at runtime time.|
|**Need** to initialize at the time of declaration.|**No need** to initialize at the time of declaration.|
|Errors are caught at **compile time.**|Error are caught at **runtime.**|
|Visual Studio **shows intellisense.**|Intellisense is **not available.**|
|**Throw a compile error** when the variable is not initialized.|**Compile** although the variable is not initialized.|
|**Do not support multiple type assignation over the same variable.**|**Support multiple type assignation over the same variable.**|

## **var** C# Reference

declared at method scope can have an implicit "type" var.

The compiler determines the type.

var is used with nullable reference types enabled.

It use allows to not repeat a type name in a variable declaration and object instantiation.

``` cs
  var xs = new List<int>();
```

Beginning with C#9.0, It is possible use a **target-typed new expression** as an alternative.

### Example

``` cs
  // Example #1: var is optional when
  // the select clause specifies a string
  string[] words = { "apple", "strawberry",   "grape", "peach", "banana" };
  var wordQuery = from word in words
                  where word[0] == 'g'
                  select word;

  // Because each element in the sequence is a   string,
  // not an anonymous type, var is optional here   also.
  foreach (string s in wordQuery)
  {
      Console.WriteLine(s);
  }

  // Example #2: var is required because
  // the select clause specifies an anonymous type
  var custQuery = from cust in customers
                  where cust.City == "Phoenix"
                  select new { cust.Name, cust.  Phone };

  // var must be used because each item
  // in the sequence is an anonymous type
  foreach (var item in custQuery)
  {
      Console.WriteLine("Name={0}, Phone={1}",   item.Name, item.Phone);
  }
```

## **dynamic**

The type is a static type, but an object of type *dynamic* bypasses static type checking.

At compile time, an element that is typed as *dynamic* is assumed to support any operation.

### Example bypasses static type checking

``` cs
class Program
{
  static void Main(string[] args)
  {
    ExampleClass ec = new ExampleClass();
    // The following call to exampleMethod1 causes a compiler error
    // if exampleMethod1 has only one parameter. Uncomment the line
    // to see the error.
    //ec.exampleMethod1(10, 4);

    dynamic dynamic_ec = new ExampleClass();
    // The following line is not identified as an error by the
    // compiler, but it causes a run-time exception.
    dynamic_ec.exampleMethod1(10, 4);

    // The following calls also do not cause compiler errors, whether
    // appropriate methods exist or not.
    dynamic_ec.someMethod("some argument", 7, null);
    dynamic_ec.nonexistentMethod();
  }
}

class ExampleClass
{
    public ExampleClass() { }
    public ExampleClass(int v) { }

    public void exampleMethod1(int i) { }

    public void exampleMethod2(string str) { }
}
```

At run time, the stored information is examined, and any statement that is not valid causes a run-time exception.

The result of most dynamic operations is itself *dynamic*.

``` cs
  dynamic d = 1;
  var result = d + 3; // result is of type dynamic
```

Operations in which the result is not dynamic include:

* Conversion from *dynamic* to another type.
* Constructor calls that include arguments of type *dynamic*.

Conversely, an implicit conversion can be dynamically applied to any expression of type dynamic.

``` cs
  dynamic d1 = 7;
  dynamic d2 = System.DateTime.Today;

  int i = d1;
  DateTime dt = d2;
```



## Resources

[codeproject.com](https://www.codeproject.com/Tips/460614/Difference-between-var-and-dynamic-in-Csharp)

[var reference](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/var)

[Using type dynamic](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/using-type-dynamic)
