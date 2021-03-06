---
title: 图形诊断 |Microsoft Docs
ms.custom: seodec18
ms.date: 11/04/2016
ms.topic: conceptual
f1_keywords:
- vs.graphics
ms.assetid: fa69c550-62a7-41b5-bb1f-7eb04af1a6e8
author: mikejo5000
ms.author: mikejo
manager: jillfra
ms.workload:
- multiple
ms.openlocfilehash: e69d88bb5764836d82232cec26606009eaf694d7
ms.sourcegitcommit: 40bd5b27f247a07c2e2514acb293b23d6ce03c29
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2019
ms.locfileid: "73187744"
---
# <a name="visual-studio-graphics-diagnostics"></a>Visual Studio 图形诊断
Visual Studio*图形诊断*是一组用于记录和分析 Direct3D 应用中的呈现和性能问题的工具。 可对在 Windows PC 上、在 Windows 设备模拟器中或在远程 PC 或设备上本地运行的应用使用图形诊断。

 图形诊断工作流一开始会捕获你的应用如何使用 Direct3D 的记录（在其运行时实时捕获），因此，可以立即对其行为进行分析、共享或保存以供稍候使用。 可以通过 Visual Studio 或命令行捕获工具**dxcap.exe**手动启动和控制捕获会话。 还可以使用图形诊断捕获 Api 以编程方式启动和控制捕获会话。

 录制捕获会话后，随时都可通过 Visual Studio“图形分析器”播放其内容，还可通过使用与所用应用完全相同的资源和呈现命令来重新创建捕获的帧。 然后，使用图形分析器窗口中提供的工具，可以详细分析任何捕获的帧。 这些工具可用于检查任意 Direct3D API 调用、资源、管道状态对象，甚至捕获帧中任何像素的完整历史记录。 通过协同使用这些工具，可以直观地解决呈现问题，从它如何出现在捕获帧中以及如何向下钻取，到应用的资源代码、着色器或图形资产中的根本原因。

 若要诊断性能问题，可以通过使用“帧分析”工具来分析捕获的帧。 此工具通过自动改变应用使用 Direct3D 的方式并对你的所有变量进行基准检查，探索潜在的性能优化。 过去，你可能为了要找出哪些造成了此差异而进行这样的手动更改或对其进行基准检查。 有了帧分析，你只需对你已知的进行更改即可。

 图形诊断可帮助图形丰富的 Direct3D 应用在外观和运行上达到最佳效果。

 若要详细了解 Visual Studio 图形诊断提供的内容，请继续阅读[概述](overview-of-visual-studio-graphics-diagnostics.md)。

## <a name="in-this-section"></a>本节内容
 [概述](overview-of-visual-studio-graphics-diagnostics.md)介绍图形诊断工作流和工具。

 [入门](getting-started-with-visual-studio-graphics-diagnostics.md)在本部分中，你将了解如何安装 Visual Studio 图形诊断以及如何开始在 Direct3D 应用中使用图形诊断。

 [捕获图形信息](capturing-graphics-information.md)若要使用图形诊断来检查应用中的呈现问题，请先记录有关应用如何使用 DirectX 的信息。 在录制会话期间，由于应用正常运行，可以“捕获”（即选择）感兴趣的帧。 包含有关如何呈现帧的详细信息的捕获。 你可以将捕获的信息另存为图形日志文档，以在稍后进行检查或与团队中的其他成员进行共享。

 [GPU 使用情况](../../profiling/gpu-usage.md)若要使用图形诊断来分析你的应用，请使用 GPU 使用情况工具。 GPU 使用情况可配合其他分析工具使用，例如 CPU 使用情况，以关联可能会在你的应用中造成问题的 CPU 和 GPU 活动。

 [图形日志文档](graphics-log-document.md)若要开始检查记录的图形日志，您可以使用 "图形日志文档" 窗口来选择捕获的帧（甚至是特定像素），以便您可以详细检查影响它的*事件*（即 DirectX API 调用）。

 [帧分析](graphics-frame-analysis.md)选择帧后，可以使用图形帧分析来检查和调整呈现性能。

 [事件列表](graphics-event-list.md)选择帧后，可以使用 "**图形事件列表**" 来检查其事件，以确定它们是否与呈现问题相关。

 [状态](graphics-state.md)"状态" 窗口可帮助你了解当前事件发生时处于活动状态的图形状态。

 [管道阶段](graphics-pipeline-stages.md)在 "**图形管道阶段**" 窗口中，您将调查每个图形管道阶段如何处理当前选定的事件，以便您可以识别呈现问题第一次出现的位置。 当由于不正确的转换导致对象无法出现时，或者当某个阶段产生与下一阶段期望的输出不匹配的输出时，检查管道阶段尤其有用。

 [事件调用堆栈](graphics-event-call-stack.md)您可以使用 "**图形事件调用堆栈**" 来检查当前选定事件的调用堆栈，以便可以导航到与呈现问题相关的应用程序代码。

 [像素历史记录](graphics-pixel-history.md)通过使用 "**图形像素历史记录**" 窗口来分析当前所选像素受影响它的事件的影响，您可以确定导致某些种类的呈现问题的事件或事件的组合。 当由于像素着色器输出不正确或与帧缓冲区错误合并而导致不正确地呈现对象时，或者当由于在对象的像素到达帧缓冲区之前已丢弃它们而导致对象甚至并未出现时，像素历史记录尤其有用。

 [对象表](graphics-object-table.md)您可以使用 "**图形对象表**" 来检查对当前选定事件有效的特定 Direct3D 对象和资源的属性和内容。 对象表可以帮助你确定在事件期间处于活动状态的图形设备上下文，并检查图形资源的内容（例如，常量缓冲区、顶点缓冲区和纹理）。

 [HLSL 调试器](hlsl-shader-debugger.md)若要检查当前选定事件和图形管道阶段的着色器代码的行为方式，请使用**HLSL 调试器**逐句通过代码、检查变量的内容，并执行其他典型的调试任务。 你还可以使用 HLSL 调试器检查计算着色器代码，而不管是由图形管道进一步处理结果，还是只由应用读回结果。

 [命令行捕获工具](command-line-capture-tool.md)使用命令行捕获工具可快速捕获和播放图形信息，而无需使用 Visual Studio 或编程捕获。 尤其是，命令行捕获工具可以用来实现自动化，也可在测试环境中使用。

 [示例](graphics-diagnostics-examples.md)几个示例演示如何结合使用图形诊断工具来诊断不同种类的呈现问题。

## <a name="related-sections"></a>相关章节

| Title | 描述 |
| - | - |
| [调试器功能简介](../debugger-feature-tour.md) | 介绍 [!INCLUDE[vsprvs](../../code-quality/includes/vsprvs_md.md)] 中的调试功能。 |
| [DirectX 图形和游戏](/windows/win32/directx) | 提供讨论 DirectX 图形技术的文章。 |
