---
title: "CA2300: Do not use insecure deserializer BinaryFormatter"
ms.date: 04/05/2019
ms.topic: reference
author: dotpaul
ms.author: paulming
manager: jillfra
dev_langs:
 - CSharp
 - VB
ms.workload:
  - "multiple"
f1_keywords:
  - "CA2300"
  - "DoNotUseInsecureDeserializerBinaryFormatter"
---
# CA2300: Do not use insecure deserializer BinaryFormatter

|||
|-|-|
|TypeName|DoNotUseInsecureDeserializerBinaryFormatter|
|CheckId|CA2300|
|Category|Microsoft.Security|
|Breaking change|Non-breaking|

## Cause

A <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter?displayProperty=nameWithType> deserialization method was called or referenced.

## Rule description

[!INCLUDE[insecure-deserializers-description](includes/insecure-deserializers-description-md.md)]

This rule finds <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter?displayProperty=nameWithType> deserialization method calls or references. If you want to deserialize only when the <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Binder> property is set to restrict types, disable this rule and enable rules [CA2301](ca2301-do-not-call-binaryformatter-deserialize-without-first-setting-binaryformatter-binder.md) and [CA2302](ca2302-ensure-binaryformatter-binder-is-set-before-calling-binaryformatter-deserialize.md) instead.

## How to fix violations

- If possible, use a secure serializer instead, and **don't allow an attacker to specify an arbitrary type to deserialize**. Some safer serializers include:
  - <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType>
  - <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=nameWithType>
  - <xref:System.Web.Script.Serialization.JavaScriptSerializer?displayProperty=nameWithType> - Never use <xref:System.Web.Script.Serialization.SimpleTypeResolver?displayProperty=nameWithType>. If you must use a type resolver, restrict deserialized types to an expected list.
  - <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType>
  - Newtonsoft Json.NET - Use TypeNameHandling.None. If you must use another value for TypeNameHandling, restrict deserialized types to an expected list with a custom ISerializationBinder.
  - Protocol Buffers
- Make the serialized data tamper-proof. After serialization, cryptographically sign the serialized data. Before deserialization, validate the cryptographic signature. Protect the cryptographic key from being disclosed and design for key rotations.
- Restrict deserialized types. Implement a custom <xref:System.Runtime.Serialization.SerializationBinder?displayProperty=nameWithType>. Before deserializing with <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>, set the <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Binder> property to an instance of your custom <xref:System.Runtime.Serialization.SerializationBinder>. In the overridden <xref:System.Runtime.Serialization.SerializationBinder.BindToType%2A> method, if the type is unexpected, throw an exception to stop deserialization.
  - If you restrict deserialized types, you may want to disable this rule and enable rules [CA2301](ca2301-do-not-call-binaryformatter-deserialize-without-first-setting-binaryformatter-binder.md) and [CA2302](ca2302-ensure-binaryformatter-binder-is-set-before-calling-binaryformatter-deserialize.md). Rules [CA2301](ca2301-do-not-call-binaryformatter-deserialize-without-first-setting-binaryformatter-binder.md) and [CA2302](ca2302-ensure-binaryformatter-binder-is-set-before-calling-binaryformatter-deserialize.md) help to ensure that the <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Binder> property is always set before deserializing.

## When to suppress warnings

[!INCLUDE[insecure-deserializers-common-safe-to-suppress](includes/insecure-deserializers-common-safe-to-suppress-md.md)]

## Pseudo-code examples

### Violation

```csharp
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;

public class ExampleClass
{
    public object MyDeserialize(byte[] bytes)
    {
        BinaryFormatter formatter = new BinaryFormatter();
        return formatter.Deserialize(new MemoryStream(bytes));
    }
}
```

```vb
Imports System.IO
Imports System.Runtime.Serialization.Formatters.Binary

Public Class ExampleClass
    Public Function MyDeserialize(bytes As Byte()) As Object
        Dim formatter As BinaryFormatter = New BinaryFormatter()
        Return formatter.Deserialize(New MemoryStream(bytes))
    End Function
End Class
```

## Related rules

[CA2301: Do not call BinaryFormatter.Deserialize without first setting BinaryFormatter.Binder](ca2301-do-not-call-binaryformatter-deserialize-without-first-setting-binaryformatter-binder.md)

[CA2302: Ensure BinaryFormatter.Binder is set before calling BinaryFormatter.Deserialize](ca2302-ensure-binaryformatter-binder-is-set-before-calling-binaryformatter-deserialize.md)