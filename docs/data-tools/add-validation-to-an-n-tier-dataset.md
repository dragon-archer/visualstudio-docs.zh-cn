---
title: 向 n 层数据集添加验证
ms.date: 11/04/2016
ms.topic: conceptual
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- n-tier applications, validating
- validation [Visual Basic], n-tier data applications
- validating n-tier data applications
ms.assetid: 34ce4db6-09bb-4b46-b435-b2514aac52d3
author: ghogen
ms.author: ghogen
manager: jillfra
ms.workload:
- data-storage
ms.openlocfilehash: 426399022c2484dca28bb4f4e1f26c14783a3d19
ms.sourcegitcommit: f3f668ecaf11b4c2738ebc91923c6b5e38e74670
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76113309"
---
# <a name="add-validation-to-an-n-tier-dataset"></a>向 n 层数据集添加验证
向分隔到 n 层解决方案中的数据集添加验证与向单文件数据集（单个项目中的数据集）添加验证基本相同。 对数据执行验证的建议位置是在 <xref:System.Data.DataTable.ColumnChanging> 和/或数据表 <xref:System.Data.DataTable.RowChanging> 事件的过程中。

数据集提供了创建分部类的功能，你可以将用户代码添加到数据集中的数据表的列和行变化事件。 有关将代码添加到 n 层解决方案中的数据集的详细信息，请参阅在[n 层应用程序中将代码添加到数据集](../data-tools/add-code-to-datasets-in-n-tier-applications.md)，并[将代码添加到 n 层应用程序中的 tableadapter](../data-tools/add-code-to-tableadapters-in-n-tier-applications.md)。 有关分部类的详细信息，请参阅[如何：将类拆分为分部类（类设计器）](../ide/class-designer/how-to-split-a-class-into-partial-classes.md)或[分部类和方法](/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)。

> [!NOTE]
> 将数据集与 Tableadapter 分离时（通过设置 "**数据集项目**" 属性），将不会自动移动项目中的现有部分数据集类。 必须将现有部分数据集类手动移动到数据集项目。

> [!NOTE]
> 数据集设计器不会自动在中C#为 <xref:System.Data.DataTable.ColumnChanging> 和 <xref:System.Data.DataTable.RowChanging> 事件创建事件处理程序。 必须手动创建一个事件处理程序，并将事件处理程序挂钩到基础事件。 下面的过程描述如何在 Visual Basic 和C#中创建所需的事件处理程序。

## <a name="validate-changes-to-individual-columns"></a>验证对单个列所做的更改
通过处理 <xref:System.Data.DataTable.ColumnChanging> 事件来验证各个列中的值。 当修改列中的值时，将引发 <xref:System.Data.DataTable.ColumnChanging> 事件。 双击**数据集设计器**上所需的列，为 <xref:System.Data.DataTable.ColumnChanging> 事件创建事件处理程序。

第一次双击列时，设计器将为 <xref:System.Data.DataTable.ColumnChanging> 事件生成一个事件处理程序。 还会创建一个 `If...Then` 语句来测试特定列。 例如，当您双击 Northwind Orders 表中的 "**要求**的" 列时，将生成以下代码：

```vb
Private Sub OrdersDataTable_ColumnChanging(ByVal sender As System.Object, ByVal e As System.Data.DataColumnChangeEventArgs) Handles Me.ColumnChanging
    If (e.Column.ColumnName = Me.RequiredDateColumn.ColumnName) Then
        ' Add validation code here.
    End If
End Sub
```

> [!NOTE]
> 在C#项目中，数据集设计器仅为数据集和数据集中的单个表创建分部类。 数据集设计器不会自动为 <xref:System.Data.DataTable.ColumnChanging> 创建事件处理程序，也不会在C# Visual Basic 中 <xref:System.Data.DataTable.RowChanging> 事件。 在C#项目中，必须手动构造一个方法来处理事件，并将方法挂钩到基础事件。 以下过程提供在 Visual Basic 和C#中创建所需事件处理程序的步骤。

[!INCLUDE[note_settings_general](../data-tools/includes/note_settings_general_md.md)]

#### <a name="to-add-validation-during-changes-to-individual-column-values"></a>在对单个列值的更改过程中添加验证

1. 在**解决方案资源管理器**中双击 *.xsd*文件，打开数据集。 有关详细信息，请参阅[演练：在数据集设计器中创建数据集](walkthrough-creating-a-dataset-with-the-dataset-designer.md)。

2. 双击要验证的列。 此操作创建 <xref:System.Data.DataTable.ColumnChanging> 事件处理程序。

    > [!NOTE]
    > 数据集设计器不会自动为C#事件创建事件处理程序。 下一部分包含处理中C#的事件所需的代码。 `SampleColumnChangingEvent` 在 <xref:System.Data.DataTable.EndInit%2A> 方法中创建并挂钩到 <xref:System.Data.DataTable.ColumnChanging> 事件。

3. 添加代码以验证 `e.ProposedValue` 包含满足你的应用程序要求的数据。 如果建议的值是不可接受的，则设置列以指示它包含错误。

     下面的代码示例验证 "**数量**" 列是否包含大于0的值。 如果**数量**小于或等于0，则将列设置为错误。 如果**数量**大于0，`Else` 子句将清除错误。 列更改事件处理程序中的代码应类似于以下内容：

    ```vb
    If (e.Column.ColumnName = Me.QuantityColumn.ColumnName) Then
        If CType(e.ProposedValue, Short) <= 0 Then
            e.Row.SetColumnError(e.Column, "Quantity must be greater than 0")
        Else
            e.Row.SetColumnError(e.Column, "")
        End If
    End If
    ```

    ```csharp
    // Add this code to the DataTable partial class.

    public override void EndInit()
    {
        base.EndInit();
        // Hook up the ColumnChanging event
        // to call the SampleColumnChangingEvent method.
        ColumnChanging += SampleColumnChangingEvent;
    }

    public void SampleColumnChangingEvent(object sender, System.Data.DataColumnChangeEventArgs e)
    {
        if (e.Column.ColumnName == QuantityColumn.ColumnName)
        {
            if ((short)e.ProposedValue <= 0)
            {
                e.Row.SetColumnError("Quantity", "Quantity must be greater than 0");
            }
            else
            {
                e.Row.SetColumnError("Quantity", "");
            }
        }
    }
    ```

## <a name="validate-changes-to-whole-rows"></a>验证对整行的更改
通过处理 <xref:System.Data.DataTable.RowChanging> 事件来验证整行中的值。 当提交所有列中的值时，将引发 <xref:System.Data.DataTable.RowChanging> 事件。 当一列中的值依赖于另一列中的值时，必须在 <xref:System.Data.DataTable.RowChanging> 事件中进行验证。 例如，请考虑 Northwind 中 Orders 表的订购日期和要求。

输入订单时，验证会确保未在订货日期或之前的到货日期输入订单。 在此示例中，需要比较 "到货日期" 和 "订货日期" 列的值，因此验证单独的列更改没有意义。

双击**数据集设计器**上表的标题栏中的表名称，为 <xref:System.Data.DataTable.RowChanging> 事件创建事件处理程序。

#### <a name="to-add-validation-during-changes-to-whole-rows"></a>在对整行的更改过程中添加验证

1. 在**解决方案资源管理器**中双击 *.xsd*文件，打开数据集。 有关详细信息，请参阅[演练：在数据集设计器中创建数据集](walkthrough-creating-a-dataset-with-the-dataset-designer.md)。

2. 在设计器上双击数据表的标题栏。

     使用 `RowChanging` 事件处理程序创建一个分部类，并在代码编辑器中打开它。

    > [!NOTE]
    > 数据集设计器不会自动为项目中C#的 <xref:System.Data.DataTable.RowChanging> 事件创建事件处理程序。 您必须创建一个方法来处理 <xref:System.Data.DataTable.RowChanging> 事件，并运行代码，然后在表的初始化方法中挂接该事件。

3. 将用户代码添加到分部类声明中。

4. 下面的代码演示在 <xref:System.Data.DataTable.RowChanging> 事件期间添加要验证的用户代码的位置。 该C#示例还包括代码，以将事件处理程序方法挂钩到 `OrdersRowChanging` 事件。

    ```vb
    Partial Class OrdersDataTable
        Private Sub OrdersDataTable_OrdersRowChanging(ByVal sender As System.Object, ByVal e As OrdersRowChangeEvent) Handles Me.OrdersRowChanging
            ' Add logic to validate columns here.
            If e.Row.RequiredDate <= e.Row.OrderDate Then
                ' Set the RowError if validation fails.
                e.Row.RowError = "Required Date cannot be on or before the OrderDate"
            Else
                ' Clear the RowError when validation passes.
                e.Row.RowError = ""
            End If
        End Sub
    End Class
    ```

    ```csharp
    partial class OrdersDataTable
    {
        public override void EndInit()
        {
            base.EndInit();
            // Hook up the event to the
            // RowChangingEvent method.
            OrdersRowChanging += RowChangingEvent;
        }

        public void RowChangingEvent(object sender, OrdersRowChangeEvent e)
        {
            // Perform the validation logic.
            if (e.Row.RequiredDate <= e.Row.OrderDate)
            {
                // Set the row to an error when validation fails.
                e.Row.RowError = "Required Date cannot be on or before the OrderDate";
            }
            else
            {
                // Clear the RowError if validation passes.
                e.Row.RowError = "";
            }
        }
    }
    ```

## <a name="see-also"></a>另请参阅

- [N 层数据应用程序概述](../data-tools/n-tier-data-applications-overview.md)
- [演练：创建 N 层数据应用程序](../data-tools/walkthrough-creating-an-n-tier-data-application.md)
- [验证数据集中的数据](../data-tools/validate-data-in-datasets.md)
