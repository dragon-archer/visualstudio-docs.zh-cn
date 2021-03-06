---
title: IDebugReference2::GetReferenceInfo |Microsoft Docs
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.technology: vs-ide-sdk
ms.topic: reference
f1_keywords:
- IDebugReference2::GetReferenceInfo
helpviewer_keywords:
- IDebugReference2::GetReferenceInfo
ms.assetid: ae611714-f114-4cf2-b5bb-37461e6ff289
caps.latest.revision: 11
ms.author: gregvanl
manager: jillfra
ms.openlocfilehash: 77cccb562db79d9f6a53113bda0c4434e19c6813
ms.sourcegitcommit: 94b3a052fb1229c7e7f8804b09c1d403385c7630
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "68155829"
---
# <a name="idebugreference2getreferenceinfo"></a>IDebugReference2::GetReferenceInfo
[!INCLUDE[vs2017banner](../../../includes/vs2017banner.md)]

获取[DEBUG_REFERENCE_INFO](../../../extensibility/debugger/reference/debug-reference-info.md)结构，它描述的引用。 留待将来使用。  
  
## <a name="syntax"></a>语法  
  
```cpp#  
HRESULT GetReferenceInfo (   
   DEBUGREF_INFO_FLAGS   dwFields,  
   DWORD                 nRadix,  
   DWORD                 dwTimeout,  
   IDebugReference2**    rgpArgs,  
   DWORD                 dwArgCount,  
   DEBUG_REFERENCE_INFO* pReferenceInfo  
);  
```  
  
```csharp  
int GetReferenceInfo (   
   enum_DEBUGREF_INFO_FLAGS  dwFields,  
   uint                      nRadix,  
   uint                      dwTimeout,  
   IDebugReference2[]        rgpArgs,  
   uint                      dwArgCount,  
   DEBUG_REFERENCE_INFO[]    pReferenceInfo  
);  
```  
  
#### <a name="parameters"></a>Parameters  
 `dwFields`  
 [in]中的标志的组合[DEBUGREF_INFO_FLAGS](../../../extensibility/debugger/reference/debugref-info-flags.md)枚举，用于确定要在中填写的字段[DEBUG_REFERENCE_INFO](../../../extensibility/debugger/reference/debug-reference-info.md)结构。  
  
 `nRadix`  
 [in]用于格式化数值的任何信息的基数。  
  
 `dwTimeout`  
 [in]最大时间 （毫秒），此方法返回前等待。 使用`INFINITE`无限期等待。  
  
 `rgpArgs`  
 [in]一个数组[IDebugReference2](../../../extensibility/debugger/reference/idebugreference2.md)对象。 保留供将来使用;设置为 null 值。  
  
 `dwArgCount`  
 [in]中的引用参数数目`rgpArgs`数组。 保留供将来使用;设置为 0。  
  
 `pReferenceInfo`  
 [out]一个[DEBUG_REFERENCE_INFO](../../../extensibility/debugger/reference/debug-reference-info.md)填充属性的说明的结构。  
  
## <a name="return-value"></a>返回值  
 始终返回 `E_NOTIMPL`。  
  
## <a name="see-also"></a>请参阅  
 [IDebugReference2](../../../extensibility/debugger/reference/idebugreference2.md)   
 [DEBUGREF_INFO_FLAGS](../../../extensibility/debugger/reference/debugref-info-flags.md)   
 [DEBUG_REFERENCE_INFO](../../../extensibility/debugger/reference/debug-reference-info.md)
