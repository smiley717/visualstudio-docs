---
title: "CA1017: Mark assemblies with ComVisibleAttribute"
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
  - "CA1017"
  - "MarkAssembliesWithComVisible"
helpviewer_keywords:
  - "MarkAssembliesWithComVisible"
  - "CA1017"
ms.assetid: 4842cb49-8dd8-4e5d-a2d6-ceeaf6c6cf8e
author: gewarren
ms.author: gewarren
manager: jillfra
dev_langs:
 - CPP
 - CSharp
 - VB
ms.workload:
  - "multiple"
---
# CA1017: Mark assemblies with ComVisibleAttribute

|||
|-|-|
|TypeName|MarkAssembliesWithComVisible|
|CheckId|CA1017|
|Category|Microsoft.Design|
|Breaking change|Non-breaking|

## Cause
An assembly does not have the <xref:System.Runtime.InteropServices.ComVisibleAttribute?displayProperty=fullName> attribute applied to it.

## Rule description
The <xref:System.Runtime.InteropServices.ComVisibleAttribute> attribute determines how COM clients access managed code. Good design dictates that assemblies explicitly indicate COM visibility. COM visibility can be set for a whole assembly and then overridden for individual types and type members. If the attribute is not present, the contents of the assembly are visible to COM clients.

## How to fix violations
To fix a violation of this rule, add the attribute to the assembly. If you do not want the assembly to be visible to COM clients, apply the attribute and set its value to `false`.

## When to suppress warnings
Do not suppress a warning from this rule. If you want the assembly to be visible, apply the attribute and set its value to `true`.

## Example
The following example shows an assembly that has the <xref:System.Runtime.InteropServices.ComVisibleAttribute> attribute applied to prevent it from being visible to COM clients.

[!code-cpp[FxCop.Design.AssembliesCom#1](../code-quality/codesnippet/CPP/ca1017-mark-assemblies-with-comvisibleattribute_1.cpp)]
[!code-vb[FxCop.Design.AssembliesCom#1](../code-quality/codesnippet/VisualBasic/ca1017-mark-assemblies-with-comvisibleattribute_1.vb)]
[!code-csharp[FxCop.Design.AssembliesCom#1](../code-quality/codesnippet/CSharp/ca1017-mark-assemblies-with-comvisibleattribute_1.cs)]

## See also

- [Interoperating with Unmanaged Code](/dotnet/framework/interop/index)
- [Qualifying .NET Types for Interoperation](/dotnet/framework/interop/qualifying-net-types-for-interoperation)