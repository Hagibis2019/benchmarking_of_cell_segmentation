# 五、过分割合并（Over-segmentation Merge）

过分割是指单个细胞或同一连通区域被错误地切分为多个独立标签。CellRefine-3D 提供一键合并功能，将多个细碎标签归并为一个，或将误检出的孤立标签直接抹除。

在插件面板 <kbd>Cell Boundary Refine</kbd> 区域的 <kbd>Over-segmentation (Merge)</kbd> 子面板中操作。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image17.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 17 过分割合并面板</div>
</div>

## 5.1 多标签合并

在 <kbd>Label IDs to merge</kbd> 输入框中填写待合并的标签编号（以空格分隔，如 10 12 15），点击 <kbd>Label Merge</kbd>。插件会将输入的所有标签统一重写为其中数值最小的标签 ID，其余 ID 对应的体素全部归并到该保留标签下，原有标签编号自动失效。

## 5.2 单标签抹除

若输入框中仅填写单个非零标签（如 10），点击 <kbd>Label Merge</kbd> 后，插件会将该标签的所有体素置为背景值 0，相当于从分割结果中删除该细胞区域。此操作常用于清除明显误检的噪声碎片或背景残留标签。

<div style="text-align: center; margin: 2em 0;">
  <div style="display: inline-block; vertical-align: middle; text-align: center; width: 280px; margin: 0 15px;">
    <img src="../images/media/image19.png" style="width: 100%; display: block; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1);">
    <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 18 合并前示例图</div>
  </div>
  <div style="display: inline-block; vertical-align: middle; font-size: 2.5em; color: #888; margin: 0 5px;">→</div>
  <div style="display: inline-block; vertical-align: middle; text-align: center; width: 280px; margin: 0 15px;">
    <img src="../images/media/image18.png" style="width: 100%; display: block; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1);">
    <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 19 合并后示例图</div>
  </div>
</div>
