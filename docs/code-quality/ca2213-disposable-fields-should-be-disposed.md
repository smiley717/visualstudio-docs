---
title: "CA2213: Disposable fields should be disposed"
ms.date: 05/14/2019
ms.topic: reference
f1_keywords:
  - "DisposableFieldsShouldBeDisposed"
  - "CA2213"
helpviewer_keywords:
  - "CA2213"
  - "DisposableFieldsShouldBeDisposed"
ms.assetid: e99442c9-70e2-47f3-b61a-d8ac003bc6e5
author: gewarren
ms.author: gewarren
manager: jillfra
ms.workload:
  - "multiple"
---
# CA2213: Disposable fields should be disposed

|||
|-|-|
|TypeName|DisposableFieldsShouldBeDisposed|
|CheckId|CA2213|
|Category|Microsoft.Usage|
|Breaking change|Non-breaking|

## Cause

A type that implements <xref:System.IDisposable?displayProperty=fullName> declares fields that are of types that also implement <xref:System.IDisposable>. The <xref:System.IDisposable.Dispose%2A> method of the field is not called by the <xref:System.IDisposable.Dispose%2A> method of the declaring type.

## Rule description

A type is responsible for disposing of all its unmanaged resources. Rule CA2213 checks to see whether a disposable type (that is, one that implements <xref:System.IDisposable>) `T` declares a field `F` that is an instance of a disposable type `FT`. For each field `F` that's assigned a locally created object within the methods or initializers of the containing type `T`, the rule attempts to locate a call to `FT.Dispose`. The rule searches the methods called by `T.Dispose` and one level lower (that is, the methods called by the methods called by `T.Dispose`).

> [!NOTE]
> Other than the [special cases](#special-cases), rule CA2213 fires only for fields that are assigned a locally created disposable object within the containing type's methods and initializers. If the object is created or assigned outside of type `T`, the rule does not fire. This reduces noise for cases where the containing type doesn't own the responsibility for disposing of the object.

### Special cases

Rule CA2213 can also fire for fields of the following types even if the object they're assigned isn't created locally:

- <xref:System.IO.Stream?displayProperty=nameWithType>
- <xref:System.IO.TextReader?displayProperty=nameWithType>
- <xref:System.IO.TextWriter?displayProperty=nameWithType>
- <xref:System.Resources.IResourceReader?displayProperty=nameWithType>

Passing an object of one of these types to a constructor and then assigning it to a field indicates a *dispose ownership transfer* to the newly constructed type. That is, the newly constructed type is now responsible for disposing of the object. If the object is not disposed, a violation of CA2213 occurs.

## How to fix violations

To fix a violation of this rule, call <xref:System.IDisposable.Dispose%2A> on fields that are of types that implement <xref:System.IDisposable>.

## When to suppress warnings

It's safe to suppress a warning from this rule if:

- The flagged type is not responsible for releasing the resource held by the field (that is, the type does not have *dispose ownership*)
- The call to <xref:System.IDisposable.Dispose%2A> occurs at a deeper calling level than the rule checks

## Example

The following snippet shows a type `TypeA` that implements <xref:System.IDisposable>.

[!code-csharp[FxCop.Usage.IDisposablePattern#1](../code-quality/codesnippet/CSharp/ca2213-disposable-fields-should-be-disposed_1.cs)]

The following snippet shows a type `TypeB` that violates rule CA2213 by declaring a field `aFieldOfADisposableType` as a disposable type (`TypeA`) and not calling <xref:System.IDisposable.Dispose%2A> on the field.

[!code-csharp[FxCop.Usage.IDisposableFields#1](../code-quality/codesnippet/CSharp/ca2213-disposable-fields-should-be-disposed_2.cs)]

To fix the violation, call `Dispose()` on the disposable field:

```csharp
protected virtual void Dispose(bool disposing)
{
   if (!disposed)
   {
      // Dispose of resources held by this instance.
      aFieldOfADisposableType.Dispose();

      disposed = true;

      // Suppress finalization of this disposed instance.
      if (disposing)
      {
          GC.SuppressFinalize(this);
      }
   }
}
```

## See also

- <xref:System.IDisposable?displayProperty=fullName>
- [Dispose pattern](/dotnet/standard/design-guidelines/dispose-pattern)