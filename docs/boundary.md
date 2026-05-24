# 六、边界修复（Boundary Fix）

边界修复用于修正预分割标签与真实细胞荧光轮廓之间的偏差，例如标签边界过度外扩侵占背景，或收缩遗漏了真实的细胞边缘。插件基于局部图像密度场，在限定范围内重新搜索最优边界，并通过形态学后处理生成平滑、连通的细胞掩膜。

在插件面板 <kbd>Cell Boundary Refine</kbd> 区域的 <kbd>Boundary Fix</kbd> 子面板中操作。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image20.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 20 Boundary Fix 面板</div>
</div>

## 6.1 参数说明

- <kbd>Percentile</kbd>：密度阈值百分位（默认 35）。插件以目标细胞内体素的图像亮度分布为基础，取该百分位对应的亮度值乘以 0.9 作为二值化阈值。数值越高，阈值越严格，满足条件的高亮度体素越少，保留区域越小，边界向内收缩；数值越低，阈值越宽松，满足条件的体素越多，保留区域越大，边界向外扩张。若细胞边界过度外扩、包含了过多背景，可适当提高该值以收紧边界；若细胞边界收缩过度、遗漏了真实的弱信号边缘，或希望捕捉向外延伸的突起，可适当降低该值以扩展边界。

- <kbd>Scaling</kbd>：边界框缩放系数（默认 1.00）。以目标细胞的原始 <code>bounding box</code> 为基准，按此系数等比例扩大或缩小搜索范围。大于 1.0 时允许算法在更大范围内寻找最优边界，适用于细胞具有细长突起、原始 <code>bounding box</code> 未能完全包裹的场景；小于 1.0 时限制搜索范围，适用于防止边界向邻近细胞过度扩张。建议初次使用时保持默认值。

- <kbd>Use Hull</kbd>：是否启用三维凸包约束（默认勾选）。勾选后，修复范围被限制在原始标签点云的 3D 凸包的等比例膨胀/收缩区域内；取消勾选则退化为完整的矩形 <code>bounding box</code>，适用于细胞形状极度不规则、凸包严重失真的场景。

## 6.2 执行修复

确认当前 <kbd>Label ID</kbd> 输入框中已填入需要修复边界的标签编号（支持多个标签，以空格分隔）。调整 <kbd>Percentile</kbd>、<kbd>Scaling</kbd> 与 <kbd>Use Hull</kbd> 后，点击 <kbd>Boundary Fix</kbd>。

插件依次执行以下流程：

- 对局部原始图像进行均值滤波平滑，抑制噪声；
- 按 <kbd>Scaling</kbd> 系数计算扩展后的 <code>bounding box</code>；
- 若启用 <kbd>Use Hull</kbd>，构建目标标签的 3D 凸包作为有效 <kbd>ROI</kbd>；
- 在 <kbd>ROI</kbd> 内按 <kbd>Percentile</kbd> 阈值筛选高密度体素，提取最大连通域；
- 执行形态学闭运算、膨胀、腐蚀、小洞填充与小对象过滤，生成平滑边界；
- 将新边界写回 <kbd>LabelFix</kbd> 图层，且严格限制在"原目标区域 + 背景"范围内，绝不覆盖邻近其他细胞。

修复完成后，建议在 2D 切片或 3D 视图中旋转观察新边界与荧光信号的贴合度。若结果不理想，可调整参数后再次执行 <kbd>Boundary Fix</kbd>。此操作也常与画笔、橡皮擦等结合使用，详见[napari基本操作](napari.md)。

<div style="text-align: center; margin: 2em 0;">
  <div style="display: inline-block; vertical-align: middle; text-align: center; width: 280px; margin: 0 15px;">
    <img src="../images/media/image22.png" style="width: 100%; display: block; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1);">
    <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 21 Boundary Fix 使用前示意图</div>
  </div>
  <div style="display: inline-block; vertical-align: middle; font-size: 2.5em; color: #888; margin: 0 5px;">→</div>
  <div style="display: inline-block; vertical-align: middle; text-align: center; width: 280px; margin: 0 15px;">
    <img src="../images/media/image21.png" style="width: 100%; display: block; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1);">
    <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 22 Boundary Fix 使用后示意图（包含两次 Percentile 60、Scaling 1.30 的操作，外加画笔精修）</div>
  </div>
</div>
