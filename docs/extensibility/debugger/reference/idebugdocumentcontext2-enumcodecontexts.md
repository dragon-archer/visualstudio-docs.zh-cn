---
title: IDebugDocumentContext2::EnumCodeContexts |Microsoft Docs
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- IDebugDocumentContext2::EnumCodeContexts
helpviewer_keywords:
- IDebugDocumentContext2::EnumCodeContexts
ms.assetid: 627af69c-5cce-4e1d-8233-5f4d8dbc62e5
author: madskristensen
ms.author: madsk
manager: jillfra
ms.workload:
- vssdk
dev_langs:
- CPP
- CSharp
ms.openlocfilehash: caa6be915fad9e64fb065babbf218f33f2848200
ms.sourcegitcommit: 40d612240dc5bea418cd27fdacdf85ea177e2df3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66349973"
---
# <a name="idebugdocumentcontext2enumcodecontexts"></a>IDebugDocumentContext2::EnumCodeContexts
检索与此文档上下文关联的所有代码上下文的列表。

## <a name="syntax"></a>语法

```cpp
HRESULT EnumCodeContexts(
    IEnumDebugCodeContexts2** ppEnumCodeCxts
);
```

```csharp
int EnumCodeContexts(
    out IEnumDebugCodeContexts2 ppEnumCodeCxts
);
```

## <a name="parameters"></a>参数
`ppEnumCodeCxts`\

## <a name="return-value"></a>返回值
如果成功，则返回`S_OK`; 否则为返回错误代码。

## <a name="remarks"></a>备注
单个文档上下文可以生成多个代码上下文，该文档使用模板时，或包含文件。

## <a name="example"></a>示例
下面的示例演示如何实现此方法对于简单`CDebugContext`公开的对象[IDebugDocumentContext2](../../../extensibility/debugger/reference/idebugdocumentcontext2.md)接口。

```cpp
HRESULT CDebugContext::EnumCodeContexts(IEnumDebugCodeContexts2 **ppEnumCodeCxts)
{
    HRESULT hr;

    // Check for a valid IEnumDebugCodeContexts2 interface pointer.
    if (ppEnumCodeCxts)
    {
        *ppEnumCodeCxts = NULL;

        // Create a CEnumDebugCodeContexts object.
        CComObject<CEnumDebugCodeContexts>* pEnum;
        hr = CComObject<CEnumDebugCodeContexts>::CreateInstance(&pEnum);
        assert(hr == S_OK);
        if (hr == S_OK)
        {
            // Get an IID_IDebugCodeContext2 interface.
            CComPtr<IDebugCodeContext2> spCodeCxt;
            hr = QueryInterface(IID_IDebugCodeContext2,
                                (void**)&spCodeCxt);
            assert(hr == S_OK);
            if (hr == S_OK)
            {
                // Initialize the code context enumerator with the
                // IDebugCodeContext2 information.
                IDebugCodeContext2* rgpCodeContext[] = { spCodeCxt.p };
                hr = pEnum->Init(rgpCodeContext,
                                 &(rgpCodeContext[1]),
                                 NULL,
                                 AtlFlagCopy);
                assert(hr == S_OK);
                if (hr == S_OK)
                {
                // Set the passed IEnumDebugCodeContexts2 pointer equal to the pointer
                // value of the created CEnumDebugCodeContexts object.
                hr = pEnum->QueryInterface(ppEnumCodeCxts);
                assert(hr == S_OK);
                }
            }

            // Otherwise, delete the CEnumDebugCodeContexts object.
            if (FAILED(hr))
            {
                delete pEnum;
            }
        }
    }
    else
    {
        hr = E_INVALIDARG;
    }

    return hr;
}
```

## <a name="see-also"></a>请参阅
- [IDebugDocumentContext2](../../../extensibility/debugger/reference/idebugdocumentcontext2.md)
- [IEnumDebugCodeContexts2](../../../extensibility/debugger/reference/ienumdebugcodecontexts2.md)
