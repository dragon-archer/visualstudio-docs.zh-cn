---
title: 使用 Visual Studio 互操作程序集 |Microsoft Docs
ms.date: 11/04/2016
ms.topic: conceptual
helpviewer_keywords:
- Visual Studio, interop assemblies
- interop assemblies, Visual Studio
- managed VSPackages, interop assemblies
ms.assetid: 1043eb95-4f0d-4861-be21-2a25395b3b3c
author: madskristensen
ms.author: madsk
manager: jillfra
ms.workload:
- vssdk
ms.openlocfilehash: d0db6e0e0d5014f09a84316143af40f410bc1b10
ms.sourcegitcommit: 5f6ad1cefbcd3d531ce587ad30e684684f4c4d44
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2019
ms.locfileid: "72722106"
---
# <a name="using-visual-studio-interop-assemblies"></a>使用 Visual Studio 互操作程序集
Visual Studio 互操作程序集允许托管应用程序访问提供 Visual Studio 扩展性的 COM 接口。 直接 COM 接口和其互操作版本之间存在一些差异。 例如，Hresult 通常表示为 int 值，需要以与异常相同的方式进行处理，并且参数（特别是 out parameters）的处理方式不同。

## <a name="handling-hresults-returned-to-managed-code-from-com"></a>处理从 COM 返回到托管代码的 HRESULT
 当从托管代码调用 COM 接口时，请检查 HRESULT 值并根据需要引发异常。 <xref:Microsoft.VisualStudio.ErrorHandler> 类包含 <xref:Microsoft.VisualStudio.ErrorHandler.ThrowOnFailure%2A> 方法，该方法根据传递给它的 HRESULT 值引发 COM 异常。

 默认情况下，每次向 <xref:Microsoft.VisualStudio.ErrorHandler.ThrowOnFailure%2A> 传递值小于零的 HRESULT 时，它都会引发异常。 如果此类 HRESULT 是可接受的值，不应引发异常，那么，应在测试完其他 HRESULT 的值后，将这些值传递给 <xref:Microsoft.VisualStudio.ErrorHandler.ThrowOnFailure%2A>。 如果所测试的 HRESULT 与显式传递到 <xref:Microsoft.VisualStudio.ErrorHandler.ThrowOnFailure%2A> 的任何 HRESULT 值匹配，则不会引发异常。

> [!NOTE]
> @No__t_0 类包含常见 HRESULT 的常量，例如 <xref:Microsoft.VisualStudio.VSConstants.S_OK> 和 <xref:Microsoft.VisualStudio.VSConstants.E_NOTIMPL>，以及 [!INCLUDE[vsprvs](../../code-quality/includes/vsprvs_md.md)] HRESULT，例如 <xref:Microsoft.VisualStudio.VSConstants.VS_E_INCOMPATIBLEDOCDATA> 和 <xref:Microsoft.VisualStudio.VSConstants.VS_E_UNSUPPORTEDFORMAT>。 <xref:Microsoft.VisualStudio.VSConstants> 还提供 <xref:Microsoft.VisualStudio.ErrorHandler.Succeeded%2A> 和 <xref:Microsoft.VisualStudio.ErrorHandler.Failed%2A> 方法，分别对应于 COM 中的 SUCCEEDED 宏和 FAILED 宏。

 例如，在以下函数调用中，<xref:Microsoft.VisualStudio.VSConstants.E_NOTIMPL> 是可接受的返回值，而其他所有小于零的 HRESULT 均表示错误。

 [!code-vb[VSSDKHRESULTInformation#1](../../extensibility/internals/codesnippet/VisualBasic/using-visual-studio-interop-assemblies_1.vb)]
 [!code-csharp[VSSDKHRESULTInformation#1](../../extensibility/internals/codesnippet/CSharp/using-visual-studio-interop-assemblies_1.cs)]

 如果有多个可接受的返回值，只需在调用 <xref:Microsoft.VisualStudio.ErrorHandler.ThrowOnFailure%2A> 时将其他 HRESULT 值追加到列表中。

 [!code-vb[VSSDKHRESULTInformation#2](../../extensibility/internals/codesnippet/VisualBasic/using-visual-studio-interop-assemblies_2.vb)]
 [!code-csharp[VSSDKHRESULTInformation#2](../../extensibility/internals/codesnippet/CSharp/using-visual-studio-interop-assemblies_2.cs)]

## <a name="returning-hresults-to-com-from-managed-code"></a>从托管代码将 HRESULT 返回到 COM
 如果没有发生异常，托管代码会向调用它的 COM 函数返回 <xref:Microsoft.VisualStudio.VSConstants.S_OK>。 COM 互操作支持托管代码中强类型化的常见异常。 例如，收到不可接受的 `null` 参数的方法会引发 <xref:System.ArgumentNullException>。

 如果不确定要引发哪个异常，但知道要返回到 COM 的 HRESULT，则可以使用 <xref:System.Runtime.InteropServices.Marshal.ThrowExceptionForHR%2A> 方法引发相应的异常。 即使是非标准错误（例如 <xref:Microsoft.VisualStudio.VSConstants.VS_E_INCOMPATIBLEDOCDATA>），也同样可行。 <xref:System.Runtime.InteropServices.Marshal.ThrowExceptionForHR%2A> 会尝试将传递给它的 HRESULT 映射到强类型化的异常。 如果无法映射，它会改为引发一般的 COM 异常。 最终结果是，从托管代码传递到 <xref:System.Runtime.InteropServices.Marshal.ThrowExceptionForHR%2A> 的 HRESULT 返回到调用它的 COM 函数中。

> [!NOTE]
> 异常会降低性能，它用于指示程序的异常状况。 对于所发生的状况，通常应以内联方式处理，而不是引发异常。

## <a name="iunknown-parameters-passed-as-type-void"></a>作为 void * * 传递的 IUnknown 参数
 查找在 COM 接口中定义为类型 `void **` 但在 [!INCLUDE[vsprvs](../../code-quality/includes/vsprvs_md.md)] 互操作程序集方法原型中定义为 `[``iid_is``]` 的 [out] 参数。

 有时，COM 接口会生成一个 `IUnknown` 对象，然后 COM 接口会将其作为 `void **` 类型进行传递。 这些接口尤其重要，因为如果在 IDL 中将变量定义为 [out]，则 `IUnknown` 对象是用 `AddRef` 方法进行引用计数的。 如果未正确处理对象，则会发生内存泄漏。

> [!NOTE]
> COM 接口创建的 `IUnknown` 对象在 [out] 变量中返回，如果未显式释放，则会导致内存泄漏。

 处理此类对象的托管方法应将 <xref:System.IntPtr> 视为指向 `IUnknown` 对象的指针，并调用 <xref:System.Runtime.InteropServices.Marshal.GetObjectForIUnknown%2A> 方法以获取对象。 然后，调用方应将返回值强制转换为任何适当的类型。 如果不再需要该对象，请调用 <xref:System.Runtime.InteropServices.Marshal.Release%2A> 将其释放。

 下面是调用 <xref:Microsoft.VisualStudio.Shell.Interop.IVsWindowFrame.QueryViewInterface%2A> 方法并正确处理 `IUnknown` 对象的一个示例：

```
MyClass myclass;
Object object;
IntPtr pObj;
Guid iid = Typeof(MyClass).Guid;
int hr = windowFrame.QueryViewInterface(ref iid, out pObj);
if (NativeMethods.Succeeded(hr))
{
    try
    {
        object = Marshal.GetObjectForIUnknown(pObj);
        myclass = object;
    }
    finally
    {
        Marshal.Release(pObj);
    }
}
else
{
    // error calling QueryViewInterface
}
```

> [!NOTE]
> 已知以下方法可以将 `IUnknown` 对象指针作为 <xref:System.IntPtr> 类型进行传递。 按照此部分中所述处理它们。

- <xref:Microsoft.VisualStudio.Shell.Interop.IVsProjectFactory.CreateProject%2A>

- <xref:Microsoft.VisualStudio.Shell.Interop.IVsOwnedProjectFactory.InitializeForOwner%2A>

- <xref:Microsoft.VisualStudio.Shell.Interop.IVsHierarchy.GetNestedHierarchy%2A>

- <xref:Microsoft.VisualStudio.Shell.Interop.IVsSolution.CreateProject%2A>

- <xref:Microsoft.VisualStudio.Shell.Interop.IVsWindowFrame.QueryViewInterface%2A>

- <xref:Microsoft.VisualStudio.Shell.Interop.IVsProjectCfg2.get_CfgType%2A>

## <a name="optional-out-parameters"></a>可选的 [out] 参数
 在 COM 接口中查找定义为 [out] 数据类型（`int`、`object` 等）的参数，但在 [!INCLUDE[vsprvs](../../code-quality/includes/vsprvs_md.md)] 互操作程序集方法原型中定义为相同数据类型的数组。

 某些 COM 接口（如 <xref:Microsoft.VisualStudio.Shell.Interop.IVsCfgProvider2.GetCfgs%2A>）将 [out] 参数视为可选。 如果对象不是必需的，则这些 COM 接口返回 `null` 指针作为该参数的值，而不是创建 [out] 对象。 这是设计使然。 对于这些接口，将假定 `null` 指针作为 VSPackage 的正确行为的一部分，并且不返回错误。

 由于 CLR 不允许 `null` [out] 参数的值，因此在托管代码中不能直接使用这些接口的设计行为的一部分。 受影响接口的 [!INCLUDE[vsprvs](../../code-quality/includes/vsprvs_md.md)] 互操作程序集方法通过将相关参数定义为数组来解决该问题，因为 CLR 允许传递 `null` 数组。

 如果没有要返回的内容，则这些方法的托管实现应将 `null` 数组放入参数中。 否则，创建一个正确类型的单元素数组，并将返回值放入数组中。

 从具有可选的 [out] 参数的接口接收信息的托管方法接收参数作为数组。 只需检查数组中第一个元素的值。 如果未 `null`，则将第一个元素视为原始参数。

## <a name="passing-constants-in-pointer-parameters"></a>在指针参数中传递常量
 查找在 COM 接口中定义为 [in] 指针但在 [!INCLUDE[vsprvs](../../code-quality/includes/vsprvs_md.md)] 互操作程序集方法原型中定义为 <xref:System.IntPtr> 类型的参数。

 当 COM 接口传递特定值（如0、-1 或-2 而不是对象指针）时，会出现类似问题。 与 [!INCLUDE[vcprvc](../../code-quality/includes/vcprvc_md.md)] 不同，CLR 不允许将常量强制转换为对象。 相反，[!INCLUDE[vsprvs](../../code-quality/includes/vsprvs_md.md)] 互操作程序集将参数定义为 <xref:System.IntPtr> 类型。

 这些方法的托管实现应利用这一事实，即 <xref:System.IntPtr> 类具有 `int` 和 `void *` 构造函数，可根据需要从对象或整数常量创建 <xref:System.IntPtr>。

 接收此类型 <xref:System.IntPtr> 参数的托管方法应使用 <xref:System.IntPtr> 类型转换运算符来处理结果。 首先将 <xref:System.IntPtr> 转换为 `int` 并针对相关的整数常量对其进行测试。 如果没有匹配的值，则将其转换为所需类型的对象，然后继续。

 有关此情况的示例，请参阅 <xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShellOpenDocument.OpenStandardEditor%2A> 和 <xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShellOpenDocument.OpenSpecificEditor%2A>。

## <a name="ole-return-values-passed-as-out-parameters"></a>作为 [out] 参数传递的 OLE 返回值
 查找在 COM 接口中具有 `retval` 返回值的方法，但在 [!INCLUDE[vsprvs](../../code-quality/includes/vsprvs_md.md)] 互操作程序集方法原型中具有 `int` 返回值和附加的 [out] 数组参数。 请注意，这些方法需要特殊处理，因为 [!INCLUDE[vsprvs](../../code-quality/includes/vsprvs_md.md)] 互操作程序集方法原型比 COM 接口方法具有一个以上的参数。

 许多处理 OLE 活动的 COM 接口将有关 OLE 状态的信息发送回调用程序，该信息存储在该接口的 `retval` 返回值中。 对应的 [!INCLUDE[vsprvs](../../code-quality/includes/vsprvs_md.md)] 互操作程序集方法不使用返回值，而是将信息发回存储在 [out] 数组参数中的调用程序。

 这些方法的托管实现应创建与 [out] 参数类型相同的单元素数组，并将其放入参数中。 数组元素的值应该与相应的 COM `retval` 相同。

 调用此类型的接口的托管方法应从 [out] 数组中提取第一个元素。 可以将此元素视为来自相应 COM 接口的 `retval` 返回值。

## <a name="see-also"></a>请参阅
- [与非托管代码交互操作](/dotnet/framework/interop/index)