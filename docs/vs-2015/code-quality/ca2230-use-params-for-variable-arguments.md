---
title: CA2230：对变量参数使用参数 |Microsoft Docs
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.technology: vs-ide-code-analysis
ms.topic: reference
f1_keywords:
- UseParamsForVariableArguments
- CA2230
helpviewer_keywords:
- CA2230
- UseParamsForVariableArguments
ms.assetid: bf98b733-4855-4110-9f16-eba5a9e79421
caps.latest.revision: 17
author: jillre
ms.author: jillfra
manager: wpickett
ms.openlocfilehash: a690544a7ed03094587a2aaf1c44b7ed68e8f2a9
ms.sourcegitcommit: a8e8f4bd5d508da34bbe9f2d4d9fa94da0539de0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2019
ms.locfileid: "72662834"
---
# <a name="ca2230-use-params-for-variable-arguments"></a>CA2230：对可变数量的参数使用 params
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

|||
|-|-|
|TypeName|UseParamsForVariableArguments|
|CheckId|CA2230|
|类别|Microsoft. 使用情况|
|是否重大更改|重大|

## <a name="cause"></a>原因
 公共或受保护类型包含使用 `VarArgs` 调用约定的公共或受保护方法。

## <a name="rule-description"></a>规则说明
 @No__t_0 调用约定用于采用可变数量的参数的某些方法定义。 使用 `VarArgs` 调用约定的方法不符合公共语言规范（CLS），并且可能无法跨编程语言访问。

 在C#中，当方法的参数列表以 `__arglist` 关键字结尾时，将使用 `VarArgs` 调用约定。 Visual Basic 不支持 `VarArgs` 调用约定，并且视觉对象C++只允许在使用椭圆 `...` 表示法的非托管代码中使用。

## <a name="how-to-fix-violations"></a>如何解决冲突
 若要修复中C#此规则的冲突，请使用[params](https://msdn.microsoft.com/library/1690815e-b52b-4967-8380-5780aff08012)关键字而不是 `__arglist`。

## <a name="when-to-suppress-warnings"></a>何时禁止显示警告
 不禁止显示此规则发出的警告。

## <a name="example"></a>示例
 下面的示例演示了两种方法，一个是违反规则的方法，另一个是满足规则的方法。

 [!code-csharp[FxCop.Usage.UseParams#1](../snippets/csharp/VS_Snippets_CodeAnalysis/FxCop.Usage.UseParams/cs/FxCop.Usage.UseParams.cs#1)]

## <a name="see-also"></a>请参阅
 <xref:System.Reflection.CallingConventions?displayProperty=fullName>[语言独立性和与语言无关的组件](https://msdn.microsoft.com/library/4f0b77d0-4844-464f-af73-6e06bedeafc6)
