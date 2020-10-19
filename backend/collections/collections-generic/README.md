# `System.Collections.Generic` classes

[back](../README.md)

The `System.Collections.Generic` namespace contains interfaces and classes that define generic collections, which allow users to create strongly typed collections that provide better type safety and performance than non-generic strongly typed collections.

## `IDictionary<TKey, TValue>` Interface

Represents a generic collection of key/value pairs.

Retrieving a value by using its key is fast because the Dictionary class is implemented as a hash table.

Derived:

* `System.Collections.Generic.Dictionary<TKey,TValue>`
* `System.Collections.Concurrent.ConcurrentDictionary<TKey,TValue>`
* `System.Collections.Generic.SortedDictionary<TKey,TValue>`
* `System.Collections.Generic.SortedList<TKey,TValue>`
* `System.Collections.Immutable.ImmutableDictionary<TKey,TValue>`
* `System.Collections.ObjectModel.ReadOnlyDictionary<TKey,TValue>`
* `System.Json.JsonObject`
* `System.Net.Http.HttpRequestOptions`

Properties:
* `Count` -> Gets the number of elements contained in the `ICollection<T>`.
* `IsReadOnly` -> Gets a value indicating whether the `ICollection<T>` is read-only.
* `Item[TKey]` -> Gets or sets the element with the specific key.
* `Keys` -> Gets an `ICollection<T>` containing the keys.
* Values Get an `ICollection<T>` containing the values in the `IDictionary<TKey, TValue>`.

Methods:

* `Add(T)`
* `Add(TKey, TValue)` -> Add an element with the provided key and value.
* `Clear()`
* `Contains(T)` -> Determines whether the `ICollection<T>` contains a specific value.
* `containsKey(TKey)` -> Determines whether it contains an element with the specified key.
* `CopyTo(T[], Int32)` -> Copies th e elements of the `ICollection<T>` to an Array, stating at particular Array index.
* `GetEnumerator()` -> Return an enumerator that iterates through a collection.
* `Remove(TKey)` -> Removes the element with the specified key from the `IDictionary<TKey, TValue>`.
* `TryGetValue(TKey, TValue)` -> Gets the value associated with the specified key.

### Dictionary<TKey, TValue> Class

Represents a collection of keys and values.

A `Dictionary<TKey,TValue>` can support multiple readers concurrently, as long as the collection is not modified.

### SortedList Class

Represents a collection of key/value pairs that are sorted by the keys and are accessible by key and by index.

## Resources

[`System.Collections.Generic` classes](https://docs.microsoft.com/en-us/dotnet/standard/generics/collections)

[`IDictionary<TKey, TValue>` Interface](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.idictionary-2?view=netcore-3.1)