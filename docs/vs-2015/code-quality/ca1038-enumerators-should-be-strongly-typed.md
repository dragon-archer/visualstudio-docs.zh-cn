---
title: CA1038：枚举数应为强类型 |Microsoft Docs
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.technology: vs-ide-code-analysis
ms.topic: reference
f1_keywords:
- EnumeratorsShouldBeStronglyTyped
- CA1038
helpviewer_keywords:
- EnumeratorsShouldBeStronglyTyped
- CA1038
ms.assetid: 8919f526-d487-42a4-87dc-2b2ee25260c4
caps.latest.revision: 18
author: jillre
ms.author: jillfra
manager: wpickett
ms.openlocfilehash: c3a08f4987fe57a94aaee8f3df6129782fd6448c
ms.sourcegitcommit: a8e8f4bd5d508da34bbe9f2d4d9fa94da0539de0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2019
ms.locfileid: "72661770"
---
# <a name="ca1038-enumerators-should-be-strongly-typed"></a>CA1038：枚举数应强类型化
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

|||
|-|-|
|TypeName|EnumeratorsShouldBeStronglyTyped|
|CheckId|CA1038|
|类别|Microsoft. Design|
|是否重大更改|重大|

## <a name="cause"></a>原因
 公共或受保护类型实现 <xref:System.Collections.IEnumerator?displayProperty=fullName>，但不提供 <xref:System.Collections.IEnumerator.Current%2A?displayProperty=fullName> 属性的强类型版本。 从以下类型派生的类型将从此规则中免除：

- <xref:System.Collections.CollectionBase?displayProperty=fullName>

- <xref:System.Collections.DictionaryBase?displayProperty=fullName>

- <xref:System.Collections.ReadOnlyCollectionBase?displayProperty=fullName>

## <a name="rule-description"></a>规则说明
 此规则要求 <xref:System.Collections.IEnumerator> 实现还提供 <xref:System.Collections.IEnumerator.Current%2A> 属性的强类型版本，以便用户在使用该接口提供的功能时无需将返回值强制转换为强类型。 此规则假定实现 <xref:System.Collections.IEnumerator> 的类型包含强于 <xref:System.Object> 类型的实例的集合。

## <a name="how-to-fix-violations"></a>如何解决冲突
 若要修复与此规则的冲突，请显式实现接口属性（将其声明为 `IEnumerator.Current`）。 添加属性的公共强类型版本，声明为 `Current`，并使其返回强类型化对象。

## <a name="when-to-suppress-warnings"></a>何时禁止显示警告
 当您实现基于对象的枚举器以便与基于对象的集合（如二元树）一起使用时，禁止显示此规则发出的警告。 扩展新集合的类型将定义强类型枚举器并公开强类型的属性。

## <a name="example"></a>示例
 下面的示例演示实现强类型 <xref:System.Collections.IEnumerator> 类型的正确方法。

 [!code-csharp[FxCop.Design.IEnumeratorStrongTypes#1](../snippets/csharp/VS_Snippets_CodeAnalysis/FxCop.Design.IEnumeratorStrongTypes/cs/FxCop.Design.IEnumeratorStrongTypes.cs#1)]

## <a name="related-rules"></a>相关规则
 [CA1035：ICollection 实现含有强类型成员](../code-quality/ca1035-icollection-implementations-have-strongly-typed-members.md)

 [CA1039：列表已强类型化](../code-quality/ca1039-lists-are-strongly-typed.md)

## <a name="see-also"></a>请参阅
 <xref:System.Collections.IEnumerator?displayProperty=fullName> <xref:System.Collections.CollectionBase?displayProperty=fullName>
 <xref:System.Collections.DictionaryBase?displayProperty=fullName>
 <xref:System.Collections.ReadOnlyCollectionBase?displayProperty=fullName>
