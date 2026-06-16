# 二、CellRefine-3D 的安装与设置

## 2.1 系统要求

- **操作系统**：Windows 10，或 macOS 26 (Tahoe) 及以上
- **硬件建议**：Intel 或 AMD 处理器，16 GB 内存

上述配置为经过验证的推荐环境，其他满足要求的硬件平台亦可正常运行。

## 2.2 安装 CellRefine-3D

CellRefine-3D 是一款基于 [napari](https://napari.org/) 三维可视化平台开发的插件。本插件主要适配 napari 0.6 及以下版本，更高版本的部分功能可能存在兼容性问题。安装本插件前，请确保已正确安装并配置 napari 环境，然后从 GitHub 克隆仓库并以 editable 模式本地安装：

```bash
# 1. 克隆仓库
git clone https://github.com/Hagibis2019/CellRefine3D.git
cd CellRefine3D

# 2. 激活 napari 环境（根据你的实际环境名调整）
conda activate napari-env

# 3. 以 editable 模式安装
pip install -e .
```

**说明**：
- `cd CellRefine3D` 表示进入项目**根目录**（即包含 `setup.py` 或 `pyproject.toml` 的目录），不要进入 `src` 子目录。
- `-e`（`--editable`）表示可编辑安装：你对源代码的任何修改会立即生效，无需重复运行 `pip install`。

## 2.3 卸载

如需卸载，在对应环境中运行：

```bash
pip uninstall cellrefine3d
```


## 2.4 启动 CellRefine-3D 插件

点击菜单栏 <kbd>Plugins</kbd> → <kbd>CellRefine-3D(CellRefine-3D)</kbd>，插件面板将默认停靠在 napari 窗口右侧。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image2.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 4 插件启动控制面板</div>
</div>

## 2.5 确认图层映射

插件启动后，<kbd>Cell Bounding Box Extraction</kbd> 区域的 <kbd>Label Layer</kbd> 与 <kbd>Image Layer (Reference)</kbd> 下拉框会自动识别 napari 中的可用图层。请手动核对：

- <kbd>Label Layer</kbd> 必须对应预分割标签图层（<kbd>Labels</kbd> 类型）；
- <kbd>Image Layer (Reference)</kbd> 必须对应原始荧光图像图层（<kbd>Image</kbd> 类型）。

若存在多个同类型图层（例如加载了多组标签），请在下拉框中手动切换至当前需要编辑的目标图层。

**Tips**：如果无法自动检查到 <kbd>Label Layer</kbd>，请切换预分割标签图像。

## 2.6 配置插件工作环境

在开始任何编辑操作前，建议先在 <kbd>CellRefine-3D Settings</kbd> 区域（图 5）完成以下基础设置：

- **界面布局**：如果 CellRefine-3D 的控制面板不在 napari 主窗口右侧，可以点击 <kbd>Dock Right</kbd> 按钮将插件面板固定到 napari 主窗口右侧。
- **字体调节**：如果感觉 CellRefine-3D 控制界面的字体太小，可通过 <kbd>Font Size</kbd> 数值框实时调节插件字体大小。
- **JSON 日志路径**：点击 <kbd>Select JSON Save Path</kbd> 指定操作日志的保存位置。该文件用于记录所有编辑历史与 Commit 快照，是实现后续回溯与复原的关键；默认文件名与当前 <kbd>Image Layer</kbd> 同名，强烈建议在标注开始前完成设置，标注过程中请勿更改。设置完成后，<kbd>Current JSON Path</kbd> 文本框会显示生效的绝对路径。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image3.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 5 插件基础设置面板</div>
</div>
