---
title: "CA2225: Operator overloads have named alternates"
ms.date: 03/11/2019
ms.topic: reference
f1_keywords:
  - "OperatorOverloadsHaveNamedAlternates"
  - "CA2225"
helpviewer_keywords:
  - "OperatorOverloadsHaveNamedAlternates"
  - "CA2225"
ms.assetid: af8f7ab1-63ad-4861-afb9-b7a7a2be15e1
author: gewarren
ms.author: gewarren
manager: jillfra
dev_langs:
  - CSharp
---
# CA2225: Operator overloads have named alternates

|||
|-|-|
|TypeName|OperatorOverloadsHaveNamedAlternates|
|CheckId|CA2225|
|Category|Microsoft.Usage|
|Breaking change|Non-breaking|

## Cause

An operator overload was detected and the expected named alternative method was not found.

By default, this rule only looks at externally visible types, but this is [configurable](#configurability).

## Rule description

Operator overloading allows the use of symbols to represent computations for a type. For example, a type that overloads the plus symbol `+` for addition would typically have an alternative member named `Add`. The named alternative member provides access to the same functionality as the operator. It's provided for developers who program in languages that do not support overloaded operators.

This rule examines:

- Implicit and explicit cast operators in a type by checking for methods named `To<typename>` and `From<typename>`.

- The operators listed in the following table:

|C#|Visual Basic|C++|Alternate method name|
|-|-|-|-|
|+ (binary)|+|+ (binary)|Add|
|+=|+=|+=|Add|
|&|And|&|BitwiseAnd|
|&=|And=|&=|BitwiseAnd|
|&#124;|Or|&#124;|BitwiseOr|
|&#124;=|Or=|&#124;=|BitwiseOr|
|--|N/A|--|Decrement|
|/|/|/|Divide|
|/=|/=|/=|Divide|
|==|=|==|Equals|
|^|Xor|^|Xor|
|^=|Xor=|^=|Xor|
|>|>|>|CompareTo or Compare|
|>=|>=|>=|CompareTo or Compare|
|++|N/A|++|Increment|
|!=|<>|!=|Equals|
|<<|<<|<<|LeftShift|
|<<=|<<=|<<=|LeftShift|
|<|<|<|CompareTo or Compare|
|<=|<=|\<=|CompareTo or Compare|
|&&|N/A|&&|LogicalAnd|
|&#124;&#124;|N/A|&#124;&#124;|LogicalOr|
|!|N/A|!|LogicalNot|
|%|Mod|%|Mod or Remainder|
|%=|N/A|%=|Mod|
|* (binary)|*|*|Multiply|
|*=|N/A|*=|Multiply|
|~|Not|~|OnesComplement|
|>>|>>|>>|RightShift|
=|N/A|>>=|RightShift|
|- (binary)|- (binary)|- (binary)|Subtract|
|-=|N/A|-=|Subtract|
|true|IsTrue|N/A|IsTrue (Property)|
|- (unary)|N/A|-|Negate|
|+ (unary)|N/A|+|Plus|
|false|IsFalse|False|IsTrue (Property)|

*N/A means the operator cannot be overloaded in the selected language.

> [!NOTE]
> In C#, when a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.

## How to fix violations

To fix a violation of this rule, implement the alternative method for the operator. Name it using the recommended alternative name.

## When to suppress warnings

Do not suppress a warning from this rule if you're implementing a shared library. Applications can ignore a warning from this rule.

## Configurability

If you're running this rule from [FxCop analyzers](install-fxcop-analyzers.md) (and not with legacy analysis), you can configure which parts of your codebase to run this rule on, based on their accessibility. For example, to specify that the rule should run only against the non-public API surface, add the following key-value pair to an .editorconfig file in your project:

```ini
dotnet_code_quality.ca2225.api_surface = private, internal
```

You can configure this option for just this rule, for all rules, or for all rules in this category (Usage). For more information, see [Configure FxCop analyzers](configure-fxcop-analyzers.md).

## Example

The following example defines a structure that violates this rule. To correct the example, add a public `Add(int x, int y)` method to the structure.

[!code-csharp[FxCop.Usage.OperatorOverloadsHaveNamedAlternates#1](../code-quality/codesnippet/CSharp/ca2225-operator-overloads-have-named-alternates_1.cs)]

## Related rules

- [CA1046: Do not overload operator equals on reference types](../code-quality/ca1046-do-not-overload-operator-equals-on-reference-types.md)
- [CA2226: Operators should have symmetrical overloads](../code-quality/ca2226-operators-should-have-symmetrical-overloads.md)
- [CA2224: Override equals on overloading operator equals](../code-quality/ca2224-override-equals-on-overloading-operator-equals.md)
- [CA2218: Override GetHashCode on overriding Equals](../code-quality/ca2218-override-gethashcode-on-overriding-equals.md)
- [CA2231: Overload operator equals on overriding ValueType.Equals](../code-quality/ca2231-overload-operator-equals-on-overriding-valuetype-equals.md)