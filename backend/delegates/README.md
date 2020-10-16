# Delegates

[back](../README.md)

A delegate is a type that represents references to methods with a particular parameter list and return type.

The declaration of a delegate type is similar to a method signature.

``` cs
  public delegate void MessageDelegate(string message);
  public delegate int AnotherDelegate(MyType m, long num);
```

Delegates are used to pass methods as arguments to other methods.

The method can be either static or an instance method.

Any method from any accessible class or struct that matches the delegate type can be assigned to the delegate.

A delegate object is normally constructed by providing the name of the method the delegate will wrap, or with an anonymous function.

Once a delegate is instantiated, a method call made to the delegate will be passed by the delegate to that method. The parameters passed to de delegate by the caller are passed to the method, and the return value, if any, from the method is returned to the caller by the delegate. This is know as invoking the delegate.

``` cs
  // Declare a delegate
  public delegate void Del(string message);

  // Create a method for a delegate
  public static void DelegateMethod(string message)
  {
    Console.WriteLine(message);
  }

  // Instantiate the delegate
  Del handler = DelegateMethod;

  // Call the delegate.
  handler("Hi!");
```

Delegate types are derived from the Delegate class in .NET. Delegate types are sealed (they cannot be derived from) and it is not possible to derive custom classes from Delegate

Delegates can be used to define callback methods.

``` cs
  public static void MethodWithCallback(int paran1, int param2, Del callback)
  {
    callback($"The number is: {(param1 + param2).ToString()}");
  }

  MethodWithCallback(1, 2, handler); // The number is: 3
```

When a delegate is constructed to wrap an instance method, the delegate references both the instance and the method. When a delegate is constructed to wrap a static method, it only references the method.

A delegate can call more than one method when invoked. This is referred to as multicasting.

To add extra methods to the delegate's list of methods just use the addition or addition assignment operators (=, +=).

To remove a method from the invocation list, use the subtraction or subtraction assignment operator (-, -=).


``` cs
  public class MethodClass
  {
    public void Method1(string message) { }
    public void Method2(string message) { }
  }

  // main method
  var obj = new MethodClass();
  Del d1 = obj.Method1;
  Del d2 = obj.Method2;
  Del d3 = DelegatedMethod; \\ Shown previously

  // Both types of assignment are valid.
  Del allMethodsDelegate = d1 + d2;
  allMethodsDelegate += d3;

  // remove Method1
  allMethodsDelegate -= d1;

  // copy AllMethodsDelegate while removing d2
  Del oneMethodDelegate = allMethodsDelegate - d2;
```

When *allMethodsDelegate* is invoked, all methods are called in order.

When any of the methods throws an exception that passes to the caller of the delegate and no subsequent methods in the invocation list are called.

Comparing delegates of two different types assigned at compile-time will result in a compilation error. If the delegate instances are statically of the type System.Delegate, then the comparison is allowed, but will return false at run time.

``` cs
  delegate void Delegate1();
  delegate void Delegate2();

  static void method(Delegate1 d1, Delegate2 d2, System.Delegate sd)
  {
    // Compile-time error
    // Console.WriteLine(d1 == d2);

    // Ok at compile-time
    Console.WriteLine( d1 == sd); // false in run time.
  }
```

Delegate can be associated with a named method and with an anonymous method.

``` cs
  // Declare a delegate
  delegate void Del(int x);

  // Declare a named method.
  void DoWork(int k) { /*...*/ }

  // Declare a static method.
  static void DoSomething(int a) { /*...*/ }

  // Create an instance of the delegate. C# 1.0
  Del d1 = new Del(DoWork);
  Del d2 = new Del(DoSomething);

  // Instantiate the delegate. C# 2.0
  Del d3 = obj.DoWork;
  Del d4 = Class.DoSomething;

  // Instantiate using an anonymous method
  Del d5 = delegate(int q) { /*...*/ }

  // Instantiate a delegate without any list of parameters.
  Action<int, double> greet = delegate { /*...*/ } // is valid.

  // Instantiate delegate by using a lambda expression. C# 3.0
  Del d7 = (value) => { /*...*/ }

  // Instantiate delegate using discards (discard variables that are intentionally unused). C# 9.0
  Func<int, int, int> constant = delegate(int _, int _) { /*...*/ }

  // Instantiate delegate with static anonymous method (the method can't capture local variables or instances state from enclosing scopes). C# 9.0
  Func<int, int, int> sum = static delegate (int a, int b) { /*...*/ }
```

Delegate object is immutable.

A delegate can be called synchronous or asynchronous by using *BeginInvoke* and *EndInvoke* methods.

in .NET, System.Action and System.Func types provide generic definitions for many common delegates.

A delegate is a reference type that can ba used to encapsulate a named or an anonymous method.

The delegates are the basis for Events.

The delegate must be instantiated with a method or lambda expression that has a compatible return type and input parameters. For use with anonymous methods, the delegate and the code to be associated with it are declared together.

## Resources

[Delegates](https://bootcamp.jala.services/#/client/MTEAYwBwb3N0Z3Jlc3Fs)

[The delegate type](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/reference-types#code-try-0)

[Using Delegates](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/using-delegates)

[Delegates with Named vs. Anonymous Methods](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/delegates-with-named-vs-anonymous-methods)

[How to declare, instantiate, and use a Delegate](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/how-to-declare-instantiate-and-use-a-delegate)

[Delegate Operator](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/delegate-operator)
