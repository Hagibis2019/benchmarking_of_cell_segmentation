# napari 基本操作

CellRefine-3D 基于 [napari](https://napari.org/) 三维可视化平台构建。熟悉 napari 的基础操作有助于更高效地使用本插件。以下内容仅涵盖与后续章节密切相关的核心功能，完整操作指南请参考 [napari 官方文档](https://napari.org/stable/)。

## 1.1 安装与启动

如尚未安装 napari，可在目标 Python 环境中运行（注意：本插件针对 napari 0.6 及以下版本优化，更高版本可能存在部分兼容性问题）：

```bash
pip install "napari[all]"
```

安装完成后，在终端输入以下命令启动：

```bash
napari
```

启动后将弹出 napari 主窗口。

## 1.2 数据加载

启动 napari，通过菜单栏 <kbd>File</kbd> → <kbd>Open Files</kbd> 加载 TIFF 格式的原始荧光图像与预分割标签图像（图 1）；或将两个配对文件直接拖入 napari 窗口完成加载。加载成功后，左下方面板会显示两个图层：<kbd>Image</kbd>（原始图像）与 <kbd>Labels</kbd>（预分割标签）。

CellRefine-3D 要求同时加载原始荧光图像（TIFF）与预分割标签图像（TIFF）。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image1.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 1 数据加载控制面板</div>
</div>

## 1.3 视图导航

### 二维/三维切换

点击画布左下角的 <kbd>2D / 3D</kbd> 按钮，可在二维切片视图与三维体渲染视图之间切换。

### 切片浏览

在 2D 视图下，拖动底部或左侧的 <kbd>Z / Y / X</kbd> 滑块，可逐层浏览不同切片。

### 三维旋转

切换至 3D 视图后，按住鼠标左键拖动即可旋转视角；滚动鼠标滚轮缩放。

CellRefine-3D 提取细胞邻域后，建议先在 3D 视图下旋转观察，确认分割质量，再进入局部编辑。详见 [CellRefine-3D 的基本操作](loading.md)。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image4.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 2 载入数据后软件主界面（三维视图）</div>
</div>

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image5.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 3 载入数据后软件主界面（二维视图）</div>
</div>

## 1.4 标注工具

napari 提供多种原生标注工具，位于界面左上角工具栏或按快捷键切换：

| 工具 | 快捷键 | 功能 |
|------|--------|------|
| 画笔（Paint） | <kbd>P</kbd> | 在 Label 图层上绘制前景标签 |
| 橡皮擦（Erase） | <kbd>E</kbd> | 擦除已标注的体素 |
| 颜料桶（Fill） | <kbd>F</kbd> | 填充连通区域 |

上述操作均支持 <kbd>Ctrl</kbd>+<kbd>Z</kbd> 撤销。使用前需确保当前选中的是 <kbd>Labels</kbd> 类型图层，而非 <kbd>Image</kbd> 图层。

CellRefine-3D 的局部编辑功能与上述原生工具完全兼容。例如，可先用橡皮擦清除错误桥连，再执行插件的连通域重标号，详见 [欠分割](under.md)；或在边界遗漏处用画笔补全后，再调用插件的密度约束边界修复，详见 [边界修复](boundary.md)。此外，所有原生标注操作均被纳入插件统一的 Undo / Redo 栈与 Commit 提交流，并完整记录于 JSON 操作日志中，可实现体素级回溯与复原，同时保留原生自带的 <kbd>Ctrl</kbd>+<kbd>Z</kbd> 撤销功能，详见 [保存与恢复](interaction.md)。

## 1.5 图层管理

- **显示/隐藏**：点击 <kbd>Layer List</kbd> 中图层名左侧的眼睛图标
- **调整透明度**：拖动图层对应的 <kbd>opacity</kbd> 滑块
- **删除图层**：点击图层名，并点击垃圾桶图标

CellRefine-3D 提取局部编辑区域后，会自动生成 <code>原图名-Crop</code> 和 <code>原图名-LabelFix</code> 两个新图层，并通过 <kbd>Global View</kbd> 开关控制全局/局部图层的显示，避免误改其他细胞，详见 [CellRefine-3D 的基本操作](loading.md)。

---

**提示**：以上仅为 napari 核心功能的简要概述。如需深入了解多通道图像、时间序列、插件系统等高级特性，请参阅 [napari 官方文档](https://napari.org/stable/)。
