---
title: "CA1061: Do not hide base class methods"
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
  - "CA1061"
  - "DoNotHideBaseClassMethods"
helpviewer_keywords:
  - "DoNotHideBaseClassMethods"
  - "CA1061"
ms.assetid: 0bda9dc8-87b4-4038-ab9d-563298387466
author: gewarren
ms.author: gewarren
manager: jillfra
ms.workload:
  - "multiple"
---
# CA1061: Do not hide base class methods

|||
|-|-|
|TypeName|DoNotHideBaseClassMethods|
|CheckId|CA1061|
|Category|Microsoft.Design|
|Breaking change|Breaking|

## Cause
A derived type declares a method with the same name and with the same number of parameters as one of its base methods; one or more of the parameters is a base type of the corresponding parameter in the base method; and any remaining parameters have types that are identical to the corresponding parameters in the base method.

## Rule description
A method in a base type is hidden by an identically named method in a derived type when the parameter signature of the derived method differs only by types that are more weakly derived than the corresponding types in the parameter signature of the base method.

## How to fix violations
To fix a violation of this rule, remove or rename the method, or change the parameter signature so that the method does not hide the base method.

## When to suppress warnings
Do not suppress a warning from this rule.

## Example
The following example shows a method that violates the rule.

[!code-csharp[FxCop.Design.HideBaseMethod#1](../code-quality/codesnippet/CSharp/ca1061-do-not-hide-base-class-methods_1.cs)]