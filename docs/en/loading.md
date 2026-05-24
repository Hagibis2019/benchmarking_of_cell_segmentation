# III. Basic Operations

All refinement operations of the plugin are centered around local neighborhoods (Bounding Boxes). The user specifies a target cell from the global labels, crops out a local cube containing that cell, and all subsequent splitting, merging, boundary fixing, and manual corrections are performed within this local region to avoid accidentally modifying other cells in the global view.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image6.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 6 Cell Bounding Box Extraction panel</div>
</div>

## 3.1 Calculating Cell Centers

In the plugin panel <kbd>Cell Bounding Box Extraction</kbd> control module, click <kbd>Calculate Cell Centers</kbd>. The plugin will compute 3D spatial center coordinates for all non-zero labels in the global label map and build a label index.

After calculation, the plugin will automatically initialize the <kbd>Box Size</kbd> for each cell to twice its actual diameter based on the current <kbd>Label ID</kbd>, ensuring that the cropped region fully encloses the target cell without excessively including irrelevant neighborhoods.

## 3.2 Setting Target Label and Crop Size

Enter the target label number to be edited in the <kbd>Label ID</kbd> input box, or use the <kbd>Previous</kbd> / <kbd>Next</kbd> buttons to navigate forward and backward through the label center list. The plugin supports keyboard shortcuts for quick navigation: press <kbd>A</kbd> to switch to the previous label, and <kbd>D</kbd> to switch to the next label. You can manually adjust the side length of the cropping cube (in pixels) in the <kbd>Box Size</kbd> value box.

## 3.3 Extracting the Local Editing Region

Click <kbd>Extract Bounding Box</kbd>. The plugin will synchronously crop a local region centered on the target cell from the global labels and raw image, and generate two new layers in napari:

- <code>OriginalName-Crop</code>: local raw image (<kbd>Image</kbd> type), serving as the main reference image for subsequent modifications;
- <code>OriginalName-LabelFix</code>: local editable labels (<kbd>Labels</kbd> type), where all subsequent refinement operations are performed.

After extraction, the plugin will automatically uncheck <kbd>Global View</kbd>, remove the original global layers from the view, and retain only the local <kbd>LabelFix</kbd> and <kbd>Crop</kbd> layers to prevent interference from other global cells during local editing. At this point, you can use napari's basic view functions to rotate, zoom, or browse the local region layer by layer to confirm segmentation quality.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image7.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 7 Software interface after extracting cell neighborhood</div>
</div>

## 3.4 View Label Filtering

Enter the label numbers to be retained and displayed in the <kbd>View Labels</kbd> input box (separated by spaces), and click <kbd>Apply</kbd> to take effect. After <kbd>Apply</kbd> is executed, <kbd>Focus</kbd> mode is automatically activated: only the labels specified in <kbd>View Labels</kbd> remain visible, while the rest are hidden; if <kbd>View Labels</kbd> is left empty, only the label specified by the current <kbd>Label ID</kbd> is visible by default.

The <kbd>Focus</kbd> checkbox is an independent display switch—unchecking it restores the display of all labels, and checking it again returns to the focused state. Press the shortcut key <kbd>F</kbd> to quickly toggle between focused and full display, facilitating flexible switching between local analysis and global observation. In addition, press <kbd>Shift</kbd>+<kbd>R</kbd> to toggle between "display all labels" and "only labels specified by <kbd>View Labels</kbd>", which is equivalent to the <kbd>Apply</kbd> button effect.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image8.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 8 Software interface after view label filtering</div>
</div>

## 3.5 Global View Switching

If you need to refer to global raw data at any time during editing, you can check the <kbd>Global View</kbd> checkbox in the <kbd>Settings</kbd> area at the top of the plugin. The original global <kbd>Image</kbd> and <kbd>Labels</kbd> layers will reappear in the napari view. Unchecking it again restores the display of only the local <kbd>LabelFix</kbd> and <kbd>Crop</kbd> layers.

## 3.6 Cleaning Auxiliary Layers

After using Plane splitting, Manual point selection, Missing Cell Detection, or 3D view navigation, auxiliary layers such as <kbd>Plane Label Splitting-Points</kbd>, <kbd>Plane Label Splitting-Shapes</kbd>, <kbd>Plane-Spherical-Axes</kbd>, <kbd>GMM-Coordinates</kbd>, and <kbd>Missing-Cell-Seeds</kbd> may remain in the napari <kbd>Layers</kbd> panel. See [Under-segmentation](under.md) for details. Click the <kbd>Remove Aux Layer</kbd> button in the <kbd>Settings</kbd> area at the top of the plugin, or press the shortcut <kbd>Shift</kbd>+<kbd>C</kbd>, to delete all auxiliary layers created by the plugin with one click, and automatically reset the active focus to the current editing layer to avoid affecting subsequent operations.
