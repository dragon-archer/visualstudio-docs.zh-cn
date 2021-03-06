---
title: 如何：使用表达式编辑器 |Microsoft Docs
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.technology: vs-workflow-designer
ms.topic: reference
f1_keywords:
- System.Activities.Presentation.View.ExpressionTextBox.UI
ms.assetid: b5f961dd-6dda-41a9-9cae-0383d479ef3d
caps.latest.revision: 13
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 54099bc5c0f249cdb3697715d153a94a596ac344
ms.sourcegitcommit: c150d0be93b6f7ccbe9625b41a437541502560f5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2020
ms.locfileid: "75849234"
---
# <a name="how-to-use-the-expression-editor"></a>如何：使用表达式编辑器
表达式编辑器是许多工作流活动中使用的 [!INCLUDE[wfd1](../includes/wfd1-md.md)] 控件，用于输入和计算这些表达式。 表达式编辑器提供全面的 IDE 编辑体验，包括 IntelliSense、着色、ParamInfo、错误波形曲线和其他功能。 编译器在表达式输入后对它进行验证。 如果表达式无效，则显示一个错误图标。 编辑器还可以作为 "**表达式编辑器**" 对话框打开。

 表达式是绑定到自变量或属性的文本值或 [!INCLUDE[vbprvb](../includes/vbprvb-md.md)] 代码。 表达式包含与操作进行组合以生成新值的值元素（如变量、常量、文本、属性）。 表达式使用 VB.NET 语法编写，即使应用程序使用 C# 编程也如此。 这意味着，大小写并不重要，而是使用单个等号（"="）而不是（"= ="）执行比较，而布尔运算符是单词 "and" 和 "or"，而不是使用符号 "&&#124;&#124;&" 和 ""，而是不使用**任何**值而不是**null**。 有关 [!INCLUDE[vbprvb](../includes/vbprvb-md.md)] 和中的表达式和运算符的详细信息，请参阅[Visual Basic 中的运算符和表达式](https://msdn.microsoft.com/library/a1w3te48(VS.100).aspx)。

 **表达式编辑器**的行为如下所示：

- 当焦点不在表达式编辑器上时，该编辑器看上去像一个常规 TextBlock 控件。

- 当焦点在表达式编辑器上时，该编辑器的外观和行为都类似于表达式编辑器控件。 当它失去焦点后，看上去又像一个常规 TextBlock 了。

- 如果在重新承载的工作流设计器中将焦点置于表达式编辑器，则该编辑器的行为类似于 TextBox。 当在重新承载的工作流设计器中失去焦点后，表达式编辑器看上去又像一个常规 TextBlock 了。

> [!NOTE]
> 表达式编辑器的 IntelliSense 仅在 [!INCLUDE[vs2010](../includes/vs2010-md.md)] 内可用。 在 [!INCLUDE[vs2010](../includes/vs2010-md.md)] 和重新承载的方案中，编译器在表达式输入后对它进行验证，如果该表达式无效，则表达式编辑器将显示一个错误图标。

### <a name="using-the-expression-editor"></a>使用表达式编辑器

1. 在 [!INCLUDE[vs2010](../includes/vs2010-md.md)] 中，打开一个新的或现有的工作流项目。

2. 向工作流添加诸如 <xref:System.Activities.Statements.Assign> 之类的活动。

    > [!NOTE]
    > 多种工作流活动具有表达式编辑器。 表达式 TextBlock 还显示在变量设计器、自变量设计器和动态自变量设计器中。 <xref:System.Activities.Statements.Assign> 活动用作一个示例。

3. 在 <xref:System.Activities.Statements.Assign> 活动的活动设计器中单击左侧表达式编辑器。

     灰色水印字符串 **\<到>** 并 **\<输入 VB 表达式>** 是默认文本字符串中的表达式编辑器<xref:System.Activities.Statements.Assign>活动。

4. 输入表达式。 如果您输入一个字符串，请确保在该字符串两端加上引号。 如果你选择将表达式自变量绑定到一个变量，请不要使用引号。

     完成后，请选中表达式编辑器外的一个区域以便将焦点移到设计器的其他部分。 这将使编译器如前所述验证该表达式。

     输入/编辑表达式的另一个方法是在属性网格中单击属性名旁的省略号。 这将打开 "**表达式编辑器**" 对话框。

## <a name="see-also"></a>请参阅
 <xref:System.Activities.Presentation.View.ExpressionTextBox>
