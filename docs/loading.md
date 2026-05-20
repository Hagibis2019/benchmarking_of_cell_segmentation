# 三、数据加载与图层准备

**3.1加载图像与标签**
启动 napari，通过菜单栏 File → Open Files
加载原始荧光图像（TIFF）与预分割标签图像（TIFF）；或将文件直接拖入
napari
窗口完成加载。加载成功后，左下方面板会显示两个图层：Image（原始图像）与
Labels（预分割标签）。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image1.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 1 数据加载控制面板</div>
</div>

**3.2启动 CellRefine-3D 插件**
点击菜单栏 Plugins → CellRefine3D (CellRefine3D)，插件面板将停靠在
napari 窗口右侧。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image2.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 2 插件启动控制面板</div>
</div>

**3.3确认图层映射**
插件启动后，Cell Bounding Box Extraction 区域的 Label Layer 与 Image
Layer (Reference) 下拉框会自动识别 napari 中的可用图层。请手动核对：
• Label Layer 必须对应预分割标签图层（Labels 类型）；
• Image Layer (Reference) 必须对应原始荧光图像图层（Image 类型）。
若存在多个同类型图层（例如加载了多组标签），请在下拉框中手动切换至当前需要编辑的目标图层。

**3.4配置插件工作环境**
在开始任何编辑操作前，建议先完成以下基础设置：
• 界面布局：点击插件顶部 Settings 区域的 Dock Right
按钮，可将插件面板固定到 napari 主窗口右侧。
• 字体调节：如在高分辨率显示器上操作，可通过 Font Size
数值框实时调节插件界面字体大小，数值即时生效。
• JSON 日志路径：点击 Select JSON Save Path
指定操作日志的保存位置。该文件用于记录所有编辑历史与 Commit
快照，是实现后续回溯与复原的关键；默认文件名与当前 Image Layer
同名，强烈建议在标注开始前完成设置。设置完成后，Current JSON Path
文本框会显示当前生效的绝对路径。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image3.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 3 插件基础设置面板</div>
</div>

**3.5确认数据方位**
利用 napari 左下角的滑块浏览不同切片，或点击 2D / 3D
按钮切换二维/三维视图进行旋转观察，确认细胞分布与预分割质量。napari
基础视图操作详见其官方文档。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image4.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 4 载入数据后软件主界面（三维视图）</div>
</div>

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image5.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 5 载入数据后软件主界面（二维视图）</div>
</div>