---
title: CA1404：紧跟在 P 调用之后调用 GetLastError |Microsoft Docs
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.technology: vs-ide-code-analysis
ms.topic: reference
f1_keywords:
- CallGetLastErrorImmediatelyAfterPInvoke
- CA1404
helpviewer_keywords:
- CallGetLastErrorImmediatelyAfterPInvoke
- CA1404
ms.assetid: 52ae9eff-50f9-4b2f-8039-ca7e49fba88e
caps.latest.revision: 20
author: jillre
ms.author: jillfra
manager: wpickett
ms.openlocfilehash: 2664837c17894f7ca336d650a7e08e21c45d955f
ms.sourcegitcommit: a8e8f4bd5d508da34bbe9f2d4d9fa94da0539de0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2019
ms.locfileid: "72661323"
---
# <a name="ca1404-call-getlasterror-immediately-after-pinvoke"></a>CA1404：紧接在 P/Invoke 之后调用 GetLastError
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

|||
|-|-|
|TypeName|CallGetLastErrorImmediatelyAfterPInvoke|
|CheckId|CA1404|
|类别|Microsoft. 互操作性|
|是否重大更改|不间断|

## <a name="cause"></a>原因
 调用 <xref:System.Runtime.InteropServices.Marshal.GetLastWin32Error%2A?displayProperty=fullName> 方法或等效的 Win32 `GetLastError` 函数，并且紧靠在之前的调用不是平台调用方法。

## <a name="rule-description"></a>规则说明
 平台调用方法访问非托管代码，并使用 [!INCLUDE[vbprvb](../includes/vbprvb-md.md)] 或 <xref:System.Runtime.InteropServices.DllImportAttribute?displayProperty=fullName> 特性中的 `Declare` 关键字定义。 通常，在失败时，非托管函数会调用 Win32 `SetLastError` 函数来设置与失败关联的错误代码。 Failed 函数的调用方调用 Win32 `GetLastError` 函数来检索错误代码，并确定失败的原因。 错误代码在每个线程的基础上维护，并在下一次调用 `SetLastError` 时被覆盖。 调用失败的平台调用方法后，托管代码可以通过调用 <xref:System.Runtime.InteropServices.Marshal.GetLastWin32Error%2A> 方法来检索错误代码。 由于错误代码可以被其他托管类库方法的内部调用覆盖，因此应在平台调用方法调用之后立即调用 `GetLastError` 或 <xref:System.Runtime.InteropServices.Marshal.GetLastWin32Error%2A> 方法。

 当对平台调用方法的调用与对 <xref:System.Runtime.InteropServices.Marshal.GetLastWin32Error%2A> 的调用之间发生时，规则将忽略对以下托管成员的调用。 这些成员不会更改错误代码，并有助于确定一些平台调用方法调用是否成功。

- <xref:System.IntPtr.Zero?displayProperty=fullName>

- <xref:System.IntPtr.op_Equality%2A?displayProperty=fullName>

- <xref:System.IntPtr.op_Inequality%2A?displayProperty=fullName>

- <xref:System.Runtime.InteropServices.SafeHandle.IsInvalid%2A?displayProperty=fullName>

## <a name="how-to-fix-violations"></a>如何解决冲突
 若要修复与此规则的冲突，请将对的调用移动到 <xref:System.Runtime.InteropServices.Marshal.GetLastWin32Error%2A>，以便它紧跟在对平台调用方法的调用之后。

## <a name="when-to-suppress-warnings"></a>何时禁止显示警告
 如果平台调用方法调用和 <xref:System.Runtime.InteropServices.Marshal.GetLastWin32Error%2A> 方法调用之间的代码不能显式或隐式导致错误代码发生更改，则可以安全地禁止显示此规则发出的警告。

## <a name="example"></a>示例
 下面的示例演示违反规则的方法和满足规则的方法。

 [!code-csharp[FxCop.Interoperability.LastErrorPInvoke#1](../snippets/csharp/VS_Snippets_CodeAnalysis/FxCop.Interoperability.LastErrorPInvoke/cs/FxCop.Interoperability.LastErrorPInvoke.cs#1)]
 [!code-vb[FxCop.Interoperability.LastErrorPInvoke#1](../snippets/visualbasic/VS_Snippets_CodeAnalysis/FxCop.Interoperability.LastErrorPInvoke/vb/FxCop.Interoperability.LastErrorPInvoke.vb#1)]

## <a name="related-rules"></a>相关规则
 [CA1060：将 P/Invoke 移动到](../code-quality/ca1060-move-p-invokes-to-nativemethods-class.md) NativeMethods 类

 [CA1400：P/Invoke 入口点应该存在](../code-quality/ca1400-p-invoke-entry-points-should-exist.md)

 [CA1401：P/Invokes 应为不可见](../code-quality/ca1401-p-invokes-should-not-be-visible.md)

 [CA2101：指定对 P/Invoke 字符串参数进行封送处理](../code-quality/ca2101-specify-marshaling-for-p-invoke-string-arguments.md)

 [CA2205：使用 Win32 API 的托管等效项](../code-quality/ca2205-use-managed-equivalents-of-win32-api.md)
