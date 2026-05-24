# 七、丢细胞检测（Missing Cell Detection）

丢细胞检测用于识别预分割标签中遗漏的真实细胞——即荧光图像中存在明显信号团，但对应位置未被赋予任何标签的背景区域。插件基于分水岭算法，以用户指定的种子点为前景标记，在限定范围内自动生长出新的细胞标签。

在插件面板 <kbd>Missing Cell Detection</kbd> 区域操作。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image23.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 23 Missing Cell Detection 面板</div>
</div>

## 7.1 放置种子点

点击 <kbd>Select Seed Point</kbd>。若尚未创建种子点图层，插件会自动在 napari 中生成 <kbd>Missing-Cell-Seeds</kbd> 图层（黄色 <kbd>Points</kbd>），并切换至添加模式。此时需在 2D 视图下，在疑似漏标记的荧光亮点中心逐层点击放置种子点。每个种子点对应一个待检测的新细胞候选区域；可在多个 Z 层放置多个种子点，插件会依次处理。完成丢细胞检测后，可按快捷键 <kbd>Shift</kbd>+<kbd>C</kbd> 一键清理点和图层，详见 [CellRefine-3D 的基本操作](loading.md)。

## 7.2 参数说明

- <kbd>Threshold</kbd>：背景/前景亮度阈值（默认 500）。原始图像中亮度低于该值的体素被视为背景，在分水岭标记中被强制置为背景标记（值为 2）。数值越高，对荧光信号的要求越严格，仅高亮区域被纳入候选；数值越低，越多的弱信号区域被纳入候选，但也可能引入噪声。需根据图像信噪比调整，建议初次使用时以能清晰区分细胞与背景为准。

- <kbd>Max Distance</kbd>：分水岭生长最大距离（默认 10，单位：像素）。以种子点为起点，算法仅在欧氏距离小于该值的邻域内执行分水岭分割。数值越大，允许新标签向周围扩展的范围越广，适用于体积较大的细胞；数值过小可能导致检测出的新区域明显小于真实细胞。

## 7.3 执行检测

确认已在 <kbd>Missing-Cell-Seeds</kbd> 图层放置至少一个种子点，且 <kbd>Threshold</kbd> 与 <kbd>Max Distance</kbd> 参数合适后，点击 <kbd>Detect Missing Cell</kbd>。插件依次执行：

- 以各种子点为前景标记（值为 1），以低于阈值的区域为背景标记（值为 2）；
- 计算距离变换场（到最近种子点的欧氏距离）；
- 以 <kbd>Max Distance</kbd> 为半径限制分水岭计算范围，避免无限制生长到远处其他细胞；
- 执行分水岭分割，提取各前景标记对应的独立区域；
- 将分割结果中前景区域赋予递增的新标签 ID，写回 <kbd>LabelFix</kbd> 图层。

每个种子点理论上产生一个新标签，但是产生的标签覆盖区域可能与实际细胞覆盖区域很可能部分不相符，此时需要用 <kbd>Boundary Fix</kbd> 进一步修复边界，详见[边界修复](boundary.md)。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image24.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 24 分水岭选点示例</div>
</div>

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image25.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 25 分水岭结果示例</div>
</div>
