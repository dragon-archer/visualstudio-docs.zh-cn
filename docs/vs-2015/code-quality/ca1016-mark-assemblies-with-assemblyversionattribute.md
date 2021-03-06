---
title: CA1016：用 AssemblyVersionAttribute 标记程序集 |Microsoft Docs
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.technology: vs-ide-code-analysis
ms.topic: reference
f1_keywords:
- MarkAssembliesWithAssemblyVersion
- CA1016
helpviewer_keywords:
- CA1016
- MarkAssembliesWithAssemblyVersion
ms.assetid: 4340aed8-d92b-4cde-a398-cb6963c6da5a
caps.latest.revision: 21
author: jillre
ms.author: jillfra
manager: wpickett
ms.openlocfilehash: f1498624d4f79a60854a624ee5c4053a3343f515
ms.sourcegitcommit: a8e8f4bd5d508da34bbe9f2d4d9fa94da0539de0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2019
ms.locfileid: "72663170"
---
# <a name="ca1016-mark-assemblies-with-assemblyversionattribute"></a>CA1016：用 AssemblyVersionAttribute 标记程序集
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

|||
|-|-|
|TypeName|MarkAssembliesWithAssemblyVersion|
|CheckId|CA1016|
|类别|Microsoft. Design|
|是否重大更改|不间断|

## <a name="cause"></a>原因
 程序集没有版本号。

## <a name="rule-description"></a>规则说明
 程序集的标识由以下信息组成：

- 程序集名称

- 版本号

- culture

- 公钥（对于强名称程序集）。

  [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] 使用版本号唯一地标识程序集，并绑定到具有强名称的程序集中的类型。 版本号与版本和发行者策略一起使用。 默认情况下，仅使用用于生成应用程序的程序集版本运行应用程序。

## <a name="how-to-fix-violations"></a>如何解决冲突
 若要修复与此规则的冲突，请使用 <xref:System.Reflection.AssemblyVersionAttribute?displayProperty=fullName> 特性将版本号添加到程序集。 请参见以下示例。

## <a name="when-to-suppress-warnings"></a>何时禁止显示警告
 对于第三方使用的程序集，或在生产环境中，请勿禁止显示此规则发出的警告。

## <a name="example"></a>示例
 下面的示例演示应用了 <xref:System.Reflection.AssemblyVersionAttribute> 特性的程序集。

 [!code-cpp[FxCop.Design.AssembliesVersion#1](../snippets/cpp/VS_Snippets_CodeAnalysis/FxCop.Design.AssembliesVersion/cpp/FxCop.Design.AssembliesVersion.cpp#1)]
 [!code-csharp[FxCop.Design.AssembliesVersion#1](../snippets/csharp/VS_Snippets_CodeAnalysis/FxCop.Design.AssembliesVersion/cs/FxCop.Design.AssembliesVersion.cs#1)]
 [!code-vb[FxCop.Design.AssembliesVersion#1](../snippets/visualbasic/VS_Snippets_CodeAnalysis/FxCop.Design.AssembliesVersion/vb/FxCop.Design.AssembliesVersion.vb#1)]

## <a name="see-also"></a>请参阅
 [程序集版本控制](https://msdn.microsoft.com/library/775ad4fb-914f-453c-98ef-ce1089b6f903)[如何：创建发行者策略](https://msdn.microsoft.com/library/8046bc5d-2fa9-4277-8a5e-6dcc96c281d9)
