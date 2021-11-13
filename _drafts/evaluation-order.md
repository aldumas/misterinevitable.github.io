---
layout: post
title: "Evaluation order"
---

In languages which support assignment as an expression, it's important to understand the evaluation order when the variable being assigned appears in other parts of the statement since this may affect the result of the statement. For example, what do the following statements assign as the value of `z`?

```csharp
int x = 1;
int z = x + (x = 2);
```

In the calculation of `z`, if the `x` on the left hand side of the `+` expression is evaluated first, then `z` will be `3`. On the other hand, if `(x = 2)` is evaluated first, then `z` z will be `4`.

Furthermore, we need to understand what is guaranteed by the language versus what is undefined, because some implementations of the compiler or runtime could differ in areas the language specification leaves undefined.

For C#, Microsoft's reference documentation states this about [operand evaluation][docs-opeval]:

> Unrelated to operator precedence and associativity, operands in an expression are evaluated from left to right.

The official [ISO specification][iso-spec] has this to say in section 12.4.1:

> Operands in an expression are evaluated from left to right. [Example: In F(i) + G(i++) * H(i),
method F is called using the old value of i, then method G is called with the old value of i, and, finally,
method H is called with the new value of i. This is separate from and unrelated to operator precedence.
end example]

Evaluation order of function arguments for functions which multiple arguments is another concern. In the code comments below, the number on the left is the expected result if the first function argument is evaluated first, while the the number on the right is the expected result if the second number is evaluated first.

<iframe width="100%" height="475" src="https://dotnetfiddle.net/Widget/5JhPH0" frameborder="0"></iframe>

As you can see, it appears that the first function argument is evaluated first, but what does the specification say?

TODO left off here

12.6.2.3

> During the run-time processing of a function member invocation (§12.6.6), the expressions or variable
references of an argument list are evaluated in order, from left to right

TODO should I keep the reference to MS documentation since the link could break?

https://www.ecma-international.org/publications-and-standards/standards/ecma-334/

[docs-opeval]: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/#operand-evaluation

[iso-spec]: /assets/pdf/c075178_ISO_IEC_23270_2018.pdf