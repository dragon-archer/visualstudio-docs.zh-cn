---
title: CA1035： ICollection 实现具有强类型成员 |Microsoft Docs
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.technology: vs-ide-code-analysis
ms.topic: reference
f1_keywords:
- ICollectionImplementationsHaveStronglyTypedMembers
- CA1035
helpviewer_keywords:
- CA1035
- ICollectionImplementationsHaveStronglyTypedMembers
ms.assetid: ad404eb5-cf6a-44b7-b78a-8ebfb654bc7f
caps.latest.revision: 18
author: jillre
ms.author: jillfra
manager: wpickett
ms.openlocfilehash: d112e2dfb704a785db6bbc5becd2b369d90605b2
ms.sourcegitcommit: a8e8f4bd5d508da34bbe9f2d4d9fa94da0539de0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2019
ms.locfileid: "72661842"
---
# <a name="ca1035-icollection-implementations-have-strongly-typed-members"></a>CA1035：ICollection 实现含有强类型成员
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

|||
|-|-|
|TypeName|ICollectionImplementationsHaveStronglyTypedMembers|
|CheckId|CA1035|
|类别|Microsoft. Design|
|是否重大更改|重大|

## <a name="cause"></a>原因
 公共或受保护类型实现 <xref:System.Collections.ICollection?displayProperty=fullName> 但不为 <xref:System.Collections.ICollection.CopyTo%2A?displayProperty=fullName> 提供强类型化方法。 @No__t_0 的强类型版本必须接受两个参数，并且不能将 <xref:System.Array?displayProperty=fullName> 或 <xref:System.Object?displayProperty=fullName> 数组作为其第一个参数。

## <a name="rule-description"></a>规则说明
 此规则需要 <xref:System.Collections.ICollection> 实现来提供强类型成员，以使用户在使用该接口提供的功能时无需将参数转换为 <xref:System.Object> 类型。 此规则假定实现 <xref:System.Collections.ICollection> 的类型执行此操作是为了管理比 <xref:System.Object> 强的类型实例的集合。

 <xref:System.Collections.ICollection> 实现 <xref:System.Collections.IEnumerable?displayProperty=fullName> 接口。 如果集合中的对象 <xref:System.ValueType?displayProperty=fullName> 扩展，则必须为 <xref:System.Collections.IEnumerable.GetEnumerator%2A> 提供强类型成员，以避免由装箱导致的性能降低。 当集合的对象为引用类型时，这不是必需的。

 若要实现接口成员的强类型版本，请使用 `InterfaceName.InterfaceMemberName` 格式的名称（如 <xref:System.Collections.ICollection.CopyTo%2A>）显式实现接口成员。 显式接口成员使用接口声明的数据类型。 使用接口成员名称（如 <xref:System.Collections.ICollection.CopyTo%2A>）实现强类型成员。 将强类型成员声明为公共成员，并将参数和返回值声明为集合管理的强类型。 强类型替换由接口声明的更弱的类型，例如 <xref:System.Object> 和 <xref:System.Array>。

## <a name="how-to-fix-violations"></a>如何解决冲突
 若要修复与此规则的冲突，请显式实现接口成员（将其声明为 <xref:System.Collections.ICollection.CopyTo%2A>）。 添加已声明为 `CopyTo` 的 public 强类型成员，并使其采用强类型化数组作为其第一个参数。

## <a name="when-to-suppress-warnings"></a>何时禁止显示警告
 如果实现新的基于对象的集合（如二进制树，其中扩展新集合的类型确定强类型），则禁止显示此规则发出的警告。 这些类型应符合此规则并公开强类型成员。

## <a name="example"></a>示例
 下面的示例演示实现 <xref:System.Collections.ICollection> 的正确方法。

 [!code-csharp[FxCop.Design.ICollectionStrongTypes#1](../snippets/csharp/VS_Snippets_CodeAnalysis/FxCop.Design.ICollectionStrongTypes/cs/FxCop.Design.ICollectionStrongTypes.cs#1)]

## <a name="related-rules"></a>相关规则
 [CA1038：枚举数应强类型化](../code-quality/ca1038-enumerators-should-be-strongly-typed.md)

 [CA1039：列表已强类型化](../code-quality/ca1039-lists-are-strongly-typed.md)

## <a name="see-also"></a>请参阅
 <xref:System.Array?displayProperty=fullName> <xref:System.Collections.IEnumerable?displayProperty=fullName>
 <xref:System.Collections.ICollection?displayProperty=fullName>
 <xref:System.Object?displayProperty=fullName>
