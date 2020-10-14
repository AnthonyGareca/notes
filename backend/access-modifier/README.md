# Access Modifier

[back](../README.md)

All type member have an accessibility level. The accessibility level controls whether they can be used from other code in your assembly or other assemblies.

public: The type or member can be accessed by any other code in the same assembly or another assembly.

private: only by code in the same class or struct.

protected: only by code in the same class, or in a class that is derived from that class.

internal: by any code in the same assembly, but not from another assembly.

protected internal: by any code in the assembly in which it's declared, or from within a derived class in another assembly.

private protected: only within its declaring assembly, by code in the same class or in a type that is derived from that class.

## Resources

[docs.microsoft.com](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/access-modifiers)