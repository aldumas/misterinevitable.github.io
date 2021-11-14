---
layout: post
title:  "Generating a range of integers"
code_lang: C#
date:   2021-11-04 19:25:00 -0400
tags: array linq C# recipe sequence range
toc: false
---


## Problem

You need the sequence of integers [0, n).


## Solution

```csharp
using System.Linq;

var sequence = (new int[n]).Select((_, i) => i);
```

The basic idea is to create an array with the number of elements you need and map each element to its index (ignoring its value). The result is an [IEnumerable\<int\>][docs-ienumerable-t]. If you need the results in an array or list, you can use [ToArray\<int\>()][docs-toarray] or [ToList\<int\>()][docs-tolist], respectively.

If you need a different range of numbers, you can modify the lambda to transform the index instead of just returning it.

This solution requires allocating an array of size `n`, which takes both memory and processing time (as each element has to be initialized to its default value), so this solution is best used when `n` is small. Keep in mind that as long as the object referenced by `sequence` is alive, the array is not eligible for garbage collection.


<iframe width="100%" height="400" src="https://dotnetfiddle.net/Widget/wC2nD8" frameborder="0"></iframe>

### How it works ###

The expression `new int[n]` allocations on the heap an array of type `int[]` containing `n` instances of `int`. The parentheses that surround this expression are not needed (see [operator precedence and associativity][operator-precedence]), but I think they improve readability a bit.

[Select\<TSource, TResult\>()][docs-select] creates an object that transforms values of type `TSource` into other values of type `TResult`, given a delegate that performs the mapping of a single element. Like Python's `map()`, `Select()` doesn't actually do the iteration and mapping; it just returns an object that will perform the mapping as you request the values. You can also compose it with other operations like filtering, grouping, etc.

We call `Select()` on the `int[]`, giving it a lambda that returns the index of each item. This method normally would not be available except that we added the `using System.Linq` statement. This brings in a number of extension methods which extend [IEnumerable\<T\>][docs-ienumerable-t], and arrays qualify because `T[]` implements `IEnumerable<T>`. When we call `Select()` on an `IEnumerable<T>`, `TSource` in the `Select()` generic method gets set to `T` from the `IEnumerable<T>` which, in our case, is `int`. `TResult` is determined by the type of the return value of the delegate that is passed into `Select()`.

`Select()` has an [overload][docs-select-overload] which takes a delegate that receives both the current value and the index of that value within the `IEnumerable<T>`. The lambda we are passing in, `(_, i) => i`, does nothing with the value and just returns its 2nd argument, the index, which means the return value of the delegate that gets created from this lambda must have the same type as `i`. This means `TResult` in the `Select()` generic method must be `int` since the `Select()` [overload][docs-select-overload] requires the 2nd parameter to be `int`. The compiler converts the lambda `(_, i) => i` into a delegate of type [Func\<int, int, int\>][docs-func] and also instantiates the overload just mentioned, namely `Select<int, int>(this IEnumerable<int> source, Func<int, int, int> selector)`.

The return value of `Select<int, int>()` is an `IEnumerable<int>` which you can then iterate over, convert to an array, etc. As each value is requested, its index is returned instead of the value, thereby returning the sequence `[0, n)`.


[docs-func]: https://docs.microsoft.com/en-us/dotnet/api/system.func-3
[docs-ienumerable-t]: https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1
[docs-select]: https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.select
[docs-select-overload]: https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.select?view=net-6.0#System_Linq_Enumerable_Select__2_System_Collections_Generic_IEnumerable___0__System_Func___0_System_Int32___1__
[docs-toarray]: https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.toarray
[docs-tolist]: https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.tolist
[operator-precedence]: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/#operator-precedence