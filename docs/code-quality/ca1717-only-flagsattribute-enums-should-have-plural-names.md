---
title: "CA1717: Only FlagsAttribute enums should have plural names"
ms.date: 03/11/2019
ms.topic: reference
f1_keywords:
  - "CA1717"
  - "OnlyFlagsEnumsShouldHavePluralNames"
helpviewer_keywords:
  - "OnlyFlagsEnumsShouldHavePluralNames"
  - "CA1717"
ms.assetid: a6855d8b-d78a-42c1-834e-61c31f5572ed
author: gewarren
ms.author: gewarren
manager: jillfra
ms.workload:
  - "multiple"
---
# CA1717: Only FlagsAttribute enums should have plural names

|||
|-|-|
|TypeName|OnlyFlagsEnumsShouldHavePluralNames|
|CheckId|CA1717|
|Category|Microsoft.Naming|
|Breaking change|Breaking|

## Cause

The name of an enumeration ends in a plural word and the enumeration is not marked with the <xref:System.FlagsAttribute?displayProperty=fullName> attribute.

By default, this rule only looks at externally visible enumerations, but this is [configurable](#configurability).

## Rule description

Naming conventions dictate that a plural name for an enumeration indicates that more than one value of the enumeration can be specified simultaneously. The <xref:System.FlagsAttribute> tells compilers that the enumeration should be treated as a bit field that enables bitwise operations on the enumeration.

If only one value of an enumeration can be specified at a time, the name of the enumeration should be a singular word. For example, an enumeration that defines the days of the week might be intended for use in an application where you can specify multiple days. This enumeration should have the <xref:System.FlagsAttribute> and could be called 'Days'. A similar enumeration that allows only a single day to be specified would not have the attribute, and could be called 'Day'.

Naming conventions provide a common look for libraries that target the common language runtime. This reduces the time that is required to learn a new software library, and increases customer confidence that the library was developed by someone who has expertise in developing managed code.

## How to fix violations

Make the name of the enumeration a singular word or add the <xref:System.FlagsAttribute>.

## When to suppress warnings

It is safe to suppress a warning from the rule if the name ends in a singular word.

## Configurability

If you're running this rule from [FxCop analyzers](install-fxcop-analyzers.md) (and not with legacy analysis), you can configure which parts of your codebase to run this rule on, based on their accessibility. For example, to specify that the rule should run only against the non-public API surface, add the following key-value pair to an .editorconfig file in your project:

```ini
dotnet_code_quality.ca1717.api_surface = private, internal
```

You can configure this option for just this rule, for all rules, or for all rules in this category (Naming). For more information, see [Configure FxCop analyzers](configure-fxcop-analyzers.md).

## Related rules

- [CA1714: Flags enums should have plural names](../code-quality/ca1714-flags-enums-should-have-plural-names.md)
- [CA1027: Mark enums with FlagsAttribute](../code-quality/ca1027-mark-enums-with-flagsattribute.md)
- [CA2217: Do not mark enums with FlagsAttribute](../code-quality/ca2217-do-not-mark-enums-with-flagsattribute.md)

## See also

- <xref:System.FlagsAttribute?displayProperty=fullName>
- [Enum design](/dotnet/standard/design-guidelines/enum)