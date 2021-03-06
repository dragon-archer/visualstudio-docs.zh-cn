---
title: 演练：对 C/C++ 代码进行缺陷分析
ms.date: 11/04/2016
ms.topic: conceptual
helpviewer_keywords:
- C/C++, code analysis
- code analysis, walkthroughs
- code, analyzing C/C++
- code analysis tool, walkthroughs
author: mikeblome
ms.author: mblome
manager: markl
ms.workload:
- cplusplus
ms.openlocfilehash: e2154a07d498012c9c45f992ebed51b0218e823a
ms.sourcegitcommit: 8e123bcb21279f2770b28696995450270b4ec0e9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75401023"
---
# <a name="walkthrough-analyzing-cc-code-for-defects"></a>演练：对 C/C++ 代码进行缺陷分析

本演练演示如何使用适用于 CC++ /C++代码的代码分析工具分析 c/code 是否存在可能的代码缺陷。

- 在本机代码上运行代码分析。
- 分析代码缺陷警告。
- 将警告视为错误。
- 批注源代码以改进代码缺陷分析。

## <a name="prerequisites"></a>先决条件

- [演示示例](../code-quality/demo-sample.md)的副本。
- 基本了解 C/C++。

### <a name="to-run-code-defect-analysis-on-native-code"></a>在本机代码上运行代码缺陷分析

1. 在 [!INCLUDE[vsprvs](../code-quality/includes/vsprvs_md.md)]中打开演示解决方案。

     演示解决方案现在将填充**解决方案资源管理器**。

2. 在“生成”菜单上，单击“重新生成解决方案”。

     解决方案生成时不会出现任何错误或警告。

3. 在**解决方案资源管理器**中，选择 CodeDefects 项目。

4. 在“项目”菜单上，单击“属性”。

     随即显示 " **CodeDefects 属性页**" 对话框。

5. 单击“代码分析”。

6. 单击 "对**生成启用代码分析C++**  " 复选框。

7. 重新生成 CodeDefects 项目。

     代码分析警告显示在**错误列表**中。

### <a name="to-analyze-code-defect-warnings"></a>分析代码缺陷警告

1. 在 **“视图”** 菜单上单击 **“错误列表”** 。

     根据在 [!INCLUDE[vsprvs](../code-quality/includes/vsprvs_md.md)]中选择的开发人员配置文件，可能需要指向 "**视图**" 菜单上的 "**其他窗口**"，然后单击 "**错误列表**"。

2. 在**错误列表**中，双击以下警告：

     警告 C6230：语义不同类型之间的隐式强制转换：在 Boolean 上下文中使用 HRESULT。

     代码编辑器显示导致函数 `bool ProcessDomain()`中出现警告的行。 此警告意味着在需要布尔值结果的 "if" 语句中使用 `HRESULT`。  这通常是一个错误，因为从其返回 `S_OK` HRESULT 时，它表示成功，但当转换为布尔值时，它的计算结果为 `false`。

3. 使用 `SUCCEEDED` 宏更正此警告，当 `HRESULT` 返回值指示成功时，此宏会转换为 `true`。 你的代码应与以下代码类似：

   ```cpp
   if (SUCCEEDED (ReadUserAccount()) )
   ```

4. 在**错误列表**中，双击以下警告：

     警告 C6282：运算符不正确：在测试上下文中对常量赋值。 Was = = 预期？

5. 通过测试相等性来更正此警告。 你的代码应类似于以下代码：

   ```cpp
   if ((len == ACCOUNT_DOMAIN_LEN) || (g_userAccount[len] != '\\'))
   ```

### <a name="to-treat-warning-as-an-error"></a>将警告视为错误

1. 在 Bug .cpp 文件中，将以下 `#pragma` 语句添加到文件开头，以将警告 C6001 视为错误：

   ```cpp
   #pragma warning (error: 6001)
   ```

2. 重新生成 CodeDefects 项目。

     在**错误列表**中，C6001 现在显示为 "错误"。

3. 通过初始化 `i` 并将 `j` 为0来更正**错误列表**中的其余两个 C6001 错误。

4. 重新生成 CodeDefects 项目。

     项目生成时不会出现任何警告或错误。

### <a name="to-correct-the-source-code-annotation-warnings-in-annotationc"></a>更正 annotation 中的源代码批注警告

1. 在解决方案资源管理器中，选择 "批注" 项目。

2. 在“项目”菜单上，单击“属性”。

     将显示 "**批注属性页**" 对话框。

3. 单击“代码分析”。

4. 选中 "对**生成启用代码分析C++**  " 复选框。

5. 重新生成批注项目。

6. 在**错误列表**中，双击以下警告：

     警告 C6011：取消对 NULL 指针 "newNode" 的引用。

     此警告指示调用方检查返回值失败。 在这种情况下，对**AllocateNode**的调用可能会返回 NULL 值（请参阅 AllocateNode 的函数声明的 .h 标头文件）。

7. 打开批注 .cpp 文件。

8. 若要更正此警告，请使用 "if" 语句测试返回值。 你的代码应与以下代码类似：

   ```cpp
   if (nullptr != newNode)
   {
       newNode->data = value;
       newNode->next = 0;
       node->next = newNode;
   }
   ```

9. 重新生成批注项目。

     项目生成时不会出现任何警告或错误。

### <a name="to-use-source-code-annotation"></a>使用源代码批注

1. 批注函数 `AddTail` 的形参和返回值，以指示指针值可以为 null：

   ```cpp
   _Ret_maybenull_ LinkedList* AddTail(_Maybenull_ LinkedList* node, int value)
   ```

2. 重新生成注释项目。

3. 在**错误列表**中，双击以下警告：

     警告 C6011：取消引用 NULL 指针 "node"。

     此警告表明传递到函数的节点可能为 null，并指示引发警告的行号。

4. 若要更正此警告，请在函数开头使用 "if" 语句来测试传入的值。 你的代码应与以下代码类似：

   ```cpp
   if (nullptr == node)
   {
        return nullptr;
   }
   ```

5. 重新生成注释项目。

     项目现在生成时没有任何警告或错误。

## <a name="see-also"></a>另请参阅

[演练：对托管代码进行代码缺陷分析](../code-quality/walkthrough-analyzing-managed-code-for-code-defects.md)
[C/C++的代码分析](../code-quality/code-analysis-for-c-cpp-overview.md)
