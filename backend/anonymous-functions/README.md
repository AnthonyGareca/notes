# Anonymous Function

[back](../README.md)

An Anonymous function is an "inline" statement or expression that can be used wherever a delegate type is expected. You can use it to initialize a named delegate or pass it instead of a named delegate type as a method parameter.

You can use a lambda expression or an anonymous method to create an anonymous function.

## Lambda Expressions

``` cs
  // Expression Lambda
  (input-parameters) => expression

  // Statement Lambda
  (input-parameters) => { <sequence-of-statements> }
```

Use the lambda declaration operator **=>** to separate the lambda parameter list from its body. To create lambda expression, you specify input parameters (if any) on the left side of the lambda operator and an expression on a statement block on the other side.

Any lambda expression can be converted to a delegate type.



## Resources

[Anonymous Functions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/anonymous-functions)

[Lambda expressions](https://bootcamp.jala.services/#/client/MTEAYwBwb3N0Z3Jlc3Fs)