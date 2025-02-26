---
title: "CA2004: Remove calls to GC.KeepAlive"
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
  - "RemoveCallsToGCKeepAlive"
  - "CA2004"
helpviewer_keywords:
  - "RemoveCallsToGCKeepAlive"
  - "CA2004"
ms.assetid: bc543b5b-23eb-4b45-abc2-9325cd254ac2
author: gewarren
ms.author: gewarren
manager: jillfra
ms.workload:
  - "multiple"
---
# CA2004: Remove calls to GC.KeepAlive

|||
|-|-|
|TypeName|RemoveCallsToGCKeepAlive|
|CheckId|CA2004|
|Category|Microsoft.Reliability|
|Breaking change|Non-breaking|

## Cause
Classes use `SafeHandle` but still contain calls to `GC.KeepAlive`.

## Rule description
If you are converting to `SafeHandle` usage, remove all calls to `GC.KeepAlive` (object). In this case, classes should not have to call `GC.KeepAlive`,assuming they do not have a finalizer but rely on `SafeHandle` to complete the OS handle for them.  Although the cost of leaving in a call to `GC.KeepAlive` might be negligible as measured by performance, the perception that a call to `GC.KeepAlive` is either necessary or sufficient to solve a lifetime issue that might no longer exist makes the code harder to maintain.

## How to fix violations
Remove calls to `GC.KeepAlive`.

## When to suppress warnings
You can suppress this warning only if it is not technically correct to convert to `SafeHandle` usage in your class.