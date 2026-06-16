# 插件概述
CellRefine-3D 是一款基于 napari 三维可视化平台开发的细胞核分割精修插件，面向三维显微镜细胞核图像标注的后处理。研究人员加载原始荧光图像与预分割标签图像（TIFF 格式）后，可在 napari 原生的二维/三维视图环境中，结合维度导航、三维旋转、画笔/橡皮擦/颜料桶等基础标注能力，使用本插件执行局部区域提取、欠分割修复（平面分割 / GMM 分割 / 连通域重标号）、过分割合并、基于图像密度的边界修复、以及基于分水岭的丢细胞检测。CellRefine-3D支持撤销与重做，并可将完整编辑历史保存为 JSON 日志，实现任意时刻的体素级复原与可追溯分析。

<div style="text-align: center; margin: 2em 0;">
  <video width="80%" style="max-width: 800px;" controls poster="images/media/image4.png">
    <source src="https://github.com/Hagibis2019/benchmarking_of_cell_segmentation/releases/download/v1.0/introduction_ch.mp4" type="video/mp4">
    您的浏览器不支持视频播放。
  </video>
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">CellRefine-3D 操作演示视频</div>
</div>