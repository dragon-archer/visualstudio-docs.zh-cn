---
title: 查询。Pdb 文件 |Microsoft Docs
ms.custom: ''
ms.date: 2018-06-30
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.technology:
- vs-ide-debug
ms.tgt_pltfrm: ''
ms.topic: article
dev_langs:
- C++
helpviewer_keywords:
- PDB files
- .pdb files, querying
ms.assetid: 8da07d1c-2712-45f9-8fbf-f34040408a8a
caps.latest.revision: 13
author: mikejo5000
ms.author: mikejo
manager: ghogen
ms.openlocfilehash: fae3034cf9f02d6d37c55bafe392610b1f1544fb
ms.sourcegitcommit: 55f7ce2d5d2e458e35c45787f1935b237ee5c9f8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2018
ms.locfileid: "47479086"
---
# <a name="querying-the-pdb-file"></a>查询 .Pdb 文件
[!INCLUDE[vs2017banner](../../includes/vs2017banner.md)]

本主题的最新版本，请参阅[进行查询。Pdb 文件](https://docs.microsoft.com/visualstudio/debugger/debug-interface-access/querying-the-dot-pdb-file)。  
  
程序数据库文件 (扩展名为.pdb) 是包含类型和符号化调试信息编译和链接项目的过程中收集的二进制文件。 编译的 C/c + + 程序时创建 PDB 文件 **/ZI**或 **/Zi**或[!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]， [!INCLUDE[csprcs](../../includes/csprcs-md.md)]，或者[!INCLUDE[jsprjscript](../../includes/jsprjscript-md.md)]带有程序 **/debug**选项。 对象文件包含到调试信息的.pdb 文件的引用。 Pdb 文件的详细信息，请参阅[PDB 文件](http://msdn.microsoft.com/en-us/1761c84e-8c2c-4632-9649-b5f99964ed3f)。 DIA 应用程序可以使用以下常规步骤以获取有关各种符号、 对象和对可执行映像中的数据元素的详细信息。  
  
### <a name="to-query-the-pdb-file"></a>查询.pdb 文件  
  
1.  通过创建获得数据源[IDiaDataSource](../../debugger/debug-interface-access/idiadatasource.md)接口。  
  
    ```cpp#  
    CComPtr<IDiaDataSource> pSource;  
    hr = CoCreateInstance( CLSID_DiaSource,  
                           NULL,  
                           CLSCTX_INPROC_SERVER,  
                           __uuidof( IDiaDataSource ),  
                          (void **) &pSource);  
  
    if (FAILED(hr))  
    {  
        Fatal("Could not CoCreate CLSID_DiaSource. Register msdia80.dll." );  
    }  
    ```  
  
2.  调用[idiadatasource:: Loaddatafrompdb](../../debugger/debug-interface-access/idiadatasource-loaddatafrompdb.md)或[idiadatasource:: Loaddataforexe](../../debugger/debug-interface-access/idiadatasource-loaddataforexe.md)加载调试信息。  
  
    ```cpp#  
    wchar_t wszFilename[ _MAX_PATH ];  
    mbstowcs( wszFilename, szFilename, sizeof( wszFilename )/sizeof( wszFilename[0] ) );  
    if ( FAILED( pSource->loadDataFromPdb( wszFilename ) ) )  
    {  
        if ( FAILED( pSource->loadDataForExe( wszFilename, NULL, NULL ) ) )  
        {  
            Fatal( "loadDataFromPdb/Exe" );  
        }  
    }  
    ```  
  
3.  调用[idiadatasource:: Opensession](../../debugger/debug-interface-access/idiadatasource-opensession.md)以打开[IDiaSession](../../debugger/debug-interface-access/idiasession.md)来获取调试信息的访问权限。  
  
    ```cpp#  
    CComPtr<IDiaSession> psession;  
    if ( FAILED( pSource->openSession( &psession ) ) )   
    {  
        Fatal( "openSession" );  
    }  
    ```  
  
4.  使用中的方法`IDiaSession`查询数据源中的符号。  
  
    ```cpp#  
    CComPtr<IDiaSymbol> pglobal;  
    if ( FAILED( psession->get_globalScope( &pglobal) ) )  
    {  
        Fatal( "get_globalScope" );  
    }  
    ```  
  
5.  使用`IDiaEnum*`调试信息的接口枚举和浏览符号或其他元素。  
  
    ```cpp#  
    CComPtr<IDiaEnumTables> pTables;  
    if ( FAILED( psession->getEnumTables( &pTables ) ) )  
    {  
        Fatal( "getEnumTables" );  
    }  
    CComPtr< IDiaTable > pTable;  
    while ( SUCCEEDED( hr = pTables->Next( 1, &pTable, &celt ) ) && celt == 1 )  
    {  
         // Do something with each IDiaTable.  
    }  
    ```  
  
## <a name="see-also"></a>请参阅  
 [IDiaDataSource](../../debugger/debug-interface-access/idiadatasource.md)


