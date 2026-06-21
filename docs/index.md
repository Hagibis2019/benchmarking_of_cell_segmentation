# NuPatch3D概述
**NuPatch3D** 是一款用于细胞核分割结果后处理的 **[napari](https://napari.org/)** 插件，旨在修复 Cellpose 等细胞核分割算法产生的常见错误，包括欠分割、过分割、细胞核漏检和边界错误等。NuPatch3D 提供局部区域提取、目标细胞突出显示、编辑日志自动保存、撤销与重做等功能，帮助用户高效、准确地修复分割结果，实现全流程可追溯的编辑与质量控制。此外，NuPatch3D 与 napari 原生标注工具无缝兼容，支持画笔、橡皮擦和填充等操作，使用户能够对分割结果进行体素级精修。


<div style="text-align: center; margin: 2em 0;">
  <video width="80%" style="max-width: 800px;" controls poster="images/media/image4.png">
    <source src="https://github.com/Hagibis2019/benchmarking_of_cell_segmentation/releases/download/v1.0/introduction_ch.mp4" type="video/mp4">
    您的浏览器不支持视频播放。
  </video>
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">NuPatch3D 视频教程</div>
</div>