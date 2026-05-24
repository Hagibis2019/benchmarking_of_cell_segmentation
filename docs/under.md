# 四、欠分割修复（Under-segmentation Split）

欠分割是指多个独立细胞被错误地标记为同一个标签。CellRefine-3D 提供三种修复模式：基于几何平面的 Plane 分割、基于高斯混合模型的 Auto 自动分裂，以及由用户指定初始中心的 Manual 手动分裂。三种模式共用同一组 <kbd>Target Labels</kbd>，并均可配合 <kbd>Connected Components Split</kbd> 进行后处理校正。

在插件面板 <kbd>Cell Boundary Refine</kbd> 区域的 <kbd>Under-segmentation (Split)</kbd> 子面板中，通过 <kbd>Mode</kbd> 下拉框切换模式。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image9.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 9 Under-segmentation fix 面板</div>
</div>

## 4.1 Plane 模式（平面分割）

适用于形态上可被某一平面清晰分离的粘连细胞。

### 4.1.1 设定目标标签

在 <kbd>Target Labels</kbd> 输入框中填写需要分割的标签编号（空格分隔，如 114 514）。

### 4.1.2 初始化平面位置

点击 <kbd>Draw a Plane</kbd>。若当前尚未放置平面基准点，插件会先在 napari 左下角面板中创建 <kbd>Plane Label Splitting-Points</kbd> 图层（蓝色点），并自动激活该图层进入添加模式，同时提示切换至 2D 视图。此时用户在目标细胞所在位置点击鼠标，插件即以此点击位置为平面经过中心，自动计算默认法向量（垂直于当前 2D 视图切片），并在面板中依次新增 <kbd>Plane Label Splitting-Shapes</kbd>（深灰色半透明矩形平面与黄色法向量箭头）和 <kbd>Plane-Spherical-Axes</kbd>（红绿蓝虚线坐标轴）两个辅助图层，同时在 3D 视图中实时渲染出分割平面与方位参考轴。插件面板中的 <kbd>X</kbd> / <kbd>Y</kbd> / <kbd>Z</kbd> 与 <kbd>Phi</kbd> / <kbd>Theta</kbd> 数值框也会自动同步为当前平面参数。上述三个辅助图层用于交互式辅助定位，按快捷键 <kbd>Shift</kbd>+<kbd>C</kbd> 一键清理点、坐标、平面和图层，详见 [CellRefine-3D 的基本操作](loading.md)。

### 4.1.3 微调平面方位

通过 <kbd>X</kbd>(Red) / <kbd>Y</kbd>(Green) / <kbd>Z</kbd>(Blue) 数值框调节平面经过点的空间坐标，通过 <kbd>Phi</kbd> / <kbd>Theta</kbd> 数值框调节平面法向量的球坐标角度。每次参数变化后，3D 视图中的半透明矩形平面与黄色法向量箭头会实时更新，直至平面恰好位于两细胞之间的粘连界面。

### 4.1.4 执行分割

点击 <kbd>Split by Plane</kbd>。插件将位于平面法向量正方向一侧的体素赋予递增的新标签 ID，另一侧保留原标签。分割完成后，可在 2D/3D 视图中验证两细胞是否已被正确分离。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image10.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 10 平面构建与调整</div>
</div>

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image11.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 11 平面分割结果示例图</div>
</div>

## 4.2 Auto 模式（GMM 自动分割）

### 4.2.1 设定算法参数

- <kbd>Sampling Rate</kbd>：体素采样比例（默认 0.3），控制参与 GMM 训练的样本量。比例越低速度越快，比例越高精度越稳。
- <kbd>Split Dist</kbd>：主轴长度分裂阈值（默认 10）。当某标签内点云的主轴长度超过该值时，插件会沿主轴方向将其分裂为两个子簇。
- <kbd>Merge Dist</kbd>：高斯中心合并阈值（默认 3）。当两个 GMM 高斯分布的中心距离小于该值时，会被合并为一个细胞。

### 4.2.2 设定目标标签

在 <kbd>Target Labels</kbd> 输入框中填写需要自动重分割的标签编号（空格分隔）。留空则对 <code>bbox</code> 内所有非零标签执行重分割。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image12.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 12 Auto 模式面板</div>
</div>

### 54.2.3 执行自动分裂

点击 <kbd>Under-segmentation Fix</kbd>。插件依次执行：分层采样 → GMM 聚类 → 中心合并 → 主轴分裂 → KNN 全图预测。新产生的标签 ID 从全局最大未使用 ID 开始递增，确保不与现有标签冲突。分裂完成后，插件自动更新细胞中心索引。

<div style="text-align: center; margin: 2em 0;">
  <div style="display: inline-block; vertical-align: middle; text-align: center; width: 280px; margin: 0 15px;">
    <img src="../images/media/image14.png" style="width: 100%; display: block; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1);">
    <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 13 GMM 自动分割前示例</div>
  </div>
  <div style="display: inline-block; vertical-align: middle; font-size: 2.5em; color: #888; margin: 0 5px;">→</div>
  <div style="display: inline-block; vertical-align: middle; text-align: center; width: 280px; margin: 0 15px;">
    <img src="../images/media/image13.png" style="width: 100%; display: block; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1);">
    <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 14 GMM 自动分割后示例</div>
  </div>
</div>

## 4.3 Manual 模式（手动指定中心）

### 4.3.1 输入或交互式选择中心坐标

在 <kbd>Coords of Centers</kbd> 输入框中手动填写各细胞中心坐标，格式为 Z,Y,X（空格分隔多组，如 10,20,30 15,25,35）；或点击 <kbd>Select Coordinates</kbd> 按钮，插件会创建 <kbd>GMM-Coordinates</kbd> 图层（白色点），并自动激活该图层进入添加模式，提示切换至 2D 视图。在 2D 视图中交互式点击放置中心点，插件会自动将 <kbd>Points</kbd> 图层中的坐标同步到输入框，并自动识别这些点所在的标签值回填至 <kbd>Target Labels</kbd>。该辅助图层不参与最终分割计算，分割后可按快捷键 <kbd>Shift</kbd>+<kbd>C</kbd> 一键清理点和图层，详见 [CellRefine-3D 的基本操作](loading.md)。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image15.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 15 Manual 模式选点示例</div>
</div>

### 4.3.2 执行分裂

点击 <kbd>Under-segmentation Fix</kbd>。插件以手动指定的坐标作为 GMM 初始中心，直接对 <kbd>Target Labels</kbd> 执行聚类与 KNN 预测，跳过自动合并与分裂步骤。

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/media/image16.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">图 16 Manual 选点分割结果示例</div>
</div>

## 4.4 连通域重标号

若分割结果中存在同一标签内部不连通的"孤岛"，均可点击 <kbd>Connected Components Split</kbd> 进行后处理。插件会对 <kbd>Target Labels</kbd> 中的每个标签独立执行三维连通域分析，保留最大连通域为原标签，其余连通域按递增 ID 赋予新标签，从根本上消除同一标签跨多个物理细胞的情况。
