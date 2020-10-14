# Access Modifier

[back](../README.md)

All type member have an accessibility level. The accessibility level controls whether they can be used from other code in your assembly or other assemblies.

## public

Access is not restricted.

type and type members

The type or member can be accessed by any other code in the same assembly or another assembly.

Public access is the most permissive access level.

## private

Access is limited to the containing type.

type member

only by code in the same class or struct.

Nested types in the same body can also access those private members.

It is a compile-time error to reference a private member outside the class or the struct in which it is declared.

## protected

Access is limited to the containing class or types derived from the containing class.

type members

only by code in the same class, or in a class that is derived from that class.

A protected member is accessible within its class and by derived class instances.


## internal

Access is limited to the current assembly.

type and type members

by any code in the same assembly, but not from another assembly.

Internal types or members are accessible only within files in the same assembly.

## protected internal

Access is limited to the current assembly or types derived from the containing class.

type members

by any code in the assembly in which it's declared, or from within a derived class in another assembly.

A protected internal member is accessible from the current assembly or from types that are derived from the containing class.

## private protected

Access is limited to the containing class or types derived from the containing class within the current assembly. Available since C# 7.2.

type member

only within its declaring assembly, by code in the same class or in a type that is derived from that class.

A private protected member is accessible by types derived from the containing class, but only within its containing assembly.

## Resources

[docs.microsoft.com](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/access-modifiers)


Por quienes es accesible un miembro del tipo private protected?

solo por tipos deribados de la clase que lo declara y si tambien estan en el mismo assembly


Por quienes es accesible un miembro del tipo protected internal?

solo por tipos deribados de la clase que lo declara y  los derivados pueden estar en diferentes assembly


Por quienes es accesible un miembro del tipo private protected?

solo por tipos deribados de la clase que lo declara y si tambien estan en el mismo assembly


Cuales son solo member access modifier?

