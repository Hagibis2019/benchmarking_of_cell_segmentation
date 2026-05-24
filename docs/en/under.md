# IV. Under-segmentation Fix

Under-segmentation refers to multiple independent cells being incorrectly labeled with the same tag. CellRefine-3D provides three repair modes: Plane splitting based on geometric planes, Auto automatic splitting based on Gaussian Mixture Models (GMM), and Manual splitting where the user specifies initial centers. All three modes share the same set of <kbd>Target Labels</kbd> and can be combined with <kbd>Connected Components Split</kbd> for post-processing correction.

In the <kbd>Cell Boundary Refine</kbd> area of the plugin panel, switch modes via the <kbd>Mode</kbd> dropdown in the <kbd>Under-segmentation (Split)</kbd> sub-panel.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image9.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 9 Under-segmentation fix panel</div>
</div>

## 4.1 Plane Mode (Plane Splitting)

Applicable to adhered cells that can be clearly separated by a plane in morphology.

### 4.1.1 Setting Target Labels

Enter the label numbers to be split in the <kbd>Target Labels</kbd> input box (separated by spaces, e.g., 114 514).

### 4.1.2 Initializing Plane Position

Click <kbd>Draw a Plane</kbd>. If no plane reference point has been placed yet, the plugin will first create the <kbd>Plane Label Splitting-Points</kbd> layer (blue points) in the bottom-left panel of napari, automatically activate this layer into add mode, and prompt you to switch to the 2D view. At this point, the user clicks the mouse at the target cell location, and the plugin uses this click position as the plane center, automatically calculating the default normal vector (perpendicular to the current 2D view slice). It then sequentially adds two auxiliary layers in the panel: <kbd>Plane Label Splitting-Shapes</kbd> (dark gray semi-transparent rectangular plane and yellow normal vector arrow) and <kbd>Plane-Spherical-Axes</kbd> (red, green, and blue dashed coordinate axes). Meanwhile, the split plane and orientation reference axes are rendered in real time in the 3D view. The <kbd>X</kbd> / <kbd>Y</kbd> / <kbd>Z</kbd> and <kbd>Phi</kbd> / <kbd>Theta</kbd> value boxes in the plugin panel will also automatically synchronize to the current plane parameters. The above three auxiliary layers are used for interactive auxiliary positioning. Press the shortcut <kbd>Shift</kbd>+<kbd>C</kbd> to clean up points, coordinates, planes, and layers with one click. See [Basic Operations](loading.md) for details.

### 4.1.3 Fine-tuning Plane Orientation

Adjust the spatial coordinates of the plane passing point via the <kbd>X</kbd>(Red) / <kbd>Y</kbd>(Green) / <kbd>Z</kbd>(Blue) value boxes, and adjust the spherical coordinate angles of the plane normal vector via the <kbd>Phi</kbd> / <kbd>Theta</kbd> value boxes. After each parameter change, the semi-transparent rectangular plane and yellow normal vector arrow in the 3D view will update in real time until the plane is exactly at the adhesion interface between the two cells.

### 4.1.4 Executing the Split

Click <kbd>Split by Plane</kbd>. The plugin assigns voxels on the positive side of the plane normal vector to incrementing new label IDs, while the other side retains the original label. After splitting, verify in the 2D/3D view whether the two cells have been correctly separated.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image10.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 10 Plane construction and adjustment</div>
</div>

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image11.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 11 Plane splitting result example</div>
</div>

## 4.2 Auto Mode (GMM Automatic Splitting)

### 4.2.1 Setting Algorithm Parameters

- <kbd>Sampling Rate</kbd>: Voxel sampling ratio (default 0.3), controlling the sample size participating in GMM training. Lower ratios mean faster speed; higher ratios mean more stable accuracy.
- <kbd>Split Dist</kbd>: Principal axis length splitting threshold (default 10). When the principal axis length of the point cloud within a label exceeds this value, the plugin will split it into two sub-clusters along the principal axis direction.
- <kbd>Merge Dist</kbd>: Gaussian center merging threshold (default 3). When the center distance between two GMM Gaussian distributions is less than this value, they will be merged into one cell.

### 4.2.2 Setting Target Labels

Enter the label numbers requiring automatic re-segmentation in the <kbd>Target Labels</kbd> input box (separated by spaces). Leaving it empty will perform re-segmentation on all non-zero labels within the <code>bbox</code>.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image12.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 12 Auto mode panel</div>
</div>

### 4.2.3 Executing Automatic Splitting

Click <kbd>Under-segmentation Fix</kbd>. The plugin sequentially executes: hierarchical sampling → GMM clustering → center merging → principal axis splitting → KNN full-image prediction. Newly generated label IDs start incrementing from the global maximum unused ID, ensuring no conflict with existing labels. After splitting, the plugin automatically updates the cell center index.

<div style="text-align: center; margin: 2em 0;">
  <div style="display: inline-block; vertical-align: middle; text-align: center; width: 280px; margin: 0 15px;">
    <img src="../../images/media/image14.png" style="width: 100%; display: block; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1);">
    <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 13 GMM automatic splitting before example</div>
  </div>
  <div style="display: inline-block; vertical-align: middle; font-size: 2.5em; color: #888; margin: 0 5px;">→</div>
  <div style="display: inline-block; vertical-align: middle; text-align: center; width: 280px; margin: 0 15px;">
    <img src="../../images/media/image13.png" style="width: 100%; display: block; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1);">
    <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 14 GMM automatic splitting after example</div>
  </div>
</div>

## 4.3 Manual Mode (Manually Specifying Centers)

### 4.3.1 Inputting or Interactively Selecting Center Coordinates

Manually fill in the center coordinates of each cell in the <kbd>Coords of Centers</kbd> input box, in the format Z,Y,X (multiple groups separated by spaces, e.g., 10,20,30 15,25,35); or click the <kbd>Select Coordinates</kbd> button. The plugin will create the <kbd>GMM-Coordinates</kbd> layer (white points), automatically activate this layer into add mode, and prompt you to switch to the 2D view. In the 2D view, interactively click to place center points. The plugin will automatically synchronize the coordinates from the <kbd>Points</kbd> layer to the input box, and automatically identify the label values where these points are located and backfill them to <kbd>Target Labels</kbd>. This auxiliary layer does not participate in the final segmentation calculation. After splitting, press the shortcut <kbd>Shift</kbd>+<kbd>C</kbd> to clean up points and layers with one click. See [Basic Operations](loading.md) for details.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image15.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 15 Manual mode point selection example</div>
</div>

### 4.3.2 Executing the Split

Click <kbd>Under-segmentation Fix</kbd>. The plugin uses the manually specified coordinates as GMM initial centers, directly performs clustering and KNN prediction on <kbd>Target Labels</kbd>, skipping the automatic merging and splitting steps.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image16.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 16 Manual point selection splitting result example</div>
</div>

## 4.4 Connected Components Split

If "islands" that are not connected within the same label exist in the segmentation result, click <kbd>Connected Components Split</kbd> for post-processing. The plugin independently performs 3D connected component analysis on each label in <kbd>Target Labels</kbd>, retains the largest connected component as the original label, and assigns new labels with incrementing IDs to the remaining connected components, fundamentally eliminating the situation where the same label spans multiple physical cells.
