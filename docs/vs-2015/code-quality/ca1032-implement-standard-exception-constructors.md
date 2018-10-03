---
title: CA1032： 实现标准异常构造函数 |Microsoft Docs
ms.custom: ''
ms.date: 2018-06-30
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.technology:
- vs-devops-test
ms.tgt_pltfrm: ''
ms.topic: article
f1_keywords:
- CA1032
- ImplementStandardExceptionConstructors
helpviewer_keywords:
- CA1032
- ImplementStandardExceptionConstructors
ms.assetid: a8623c56-273a-4c95-8d83-95911a042be7
caps.latest.revision: 18
author: gewarren
ms.author: gewarren
manager: wpickett
ms.openlocfilehash: 4d8c2f4439cf63f0bab7294898c96b2acb742e7d
ms.sourcegitcommit: 99d097d82ee4f9eff6f588e5ebb6b17d8f724b04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "47587644"
---
# <a name="ca1032-implement-standard-exception-constructors"></a>CA1032：实现标准异常构造函数
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

本主题的最新版本，请参阅[CA1032： 实现标准异常构造函数](https://docs.microsoft.com/visualstudio/code-quality/ca1032-implement-standard-exception-constructors)。

|||
|-|-|
|TypeName|ImplementStandardExceptionConstructors|
|CheckId|CA1032|
|类别|Microsoft.Design|
|是否重大更改|非换行|

## <a name="cause"></a>原因
 类型扩展<xref:System.Exception?displayProperty=fullName>，且不声明所需的所有构造函数。

## <a name="rule-description"></a>规则说明
 异常类型必须实现以下构造函数：

-   公共 NewException()

-   公共 NewException(string)

-   公共 NewException (string，异常)

-   受保护或私有 NewException （SerializationInfo，StreamingContext）

 如果不能提供完整的构造函数集，要正确处理异常将变得比较困难。 例如，已签名的构造函数`NewException(string, Exception)`用于创建的其他异常导致的异常。 而无需此构造函数中不能创建和引发自定义异常，其中包含内部 （嵌套） 异常的实例，这是托管的代码应在这种情况中执行操作。 前三种异常构造函数是公共的约定。 第四个构造函数是在未密封类中，受保护和私钥在密封类中。 有关详细信息，请参阅[CA2229： 实现序列化构造函数](../code-quality/ca2229-implement-serialization-constructors.md)

## <a name="how-to-fix-violations"></a>如何解决冲突
 若要修复此规则的冲突，请将缺少的构造函数添加到异常，并确保它们具有正确的可访问性。

## <a name="when-to-suppress-warnings"></a>何时禁止显示警告
 它可以安全地禁止显示此规则的警告时冲突导致的公共构造函数使用不同的访问级别。

## <a name="example"></a>示例
 下面的示例包含与此规则冲突的异常类型和一个正确实现的异常类型。

 [!code-csharp[FxCop.Design.ExceptionMultipleCtors#1](../snippets/csharp/VS_Snippets_CodeAnalysis/FxCop.Design.ExceptionMultipleCtors/cs/FxCop.Design.ExceptionMultipleCtors.cs#1)]


