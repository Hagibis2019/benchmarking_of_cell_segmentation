# I. Napari Basics

CellRefine-3D is built on the [napari](https://napari.org/) multi-dimensional image viewer. Familiarity with napari's basic operations will help you use this plugin more efficiently. The following content covers only the core features closely related to subsequent chapters. For a complete guide, please refer to the [napari official documentation](https://napari.org/stable/).

## 1.1 Installation and Launch

If napari is not yet installed, run the following in your target Python environment:

```bash
pip install "napari[all]"
```

After installation, launch napari from the terminal:

```bash
napari
```

The napari main window will pop up upon launch.

## 1.2 Data Loading

Launch napari and load TIFF-format raw fluorescence images and pre-segmentation label images via the menu bar <kbd>File</kbd> → <kbd>Open Files</kbd> (Figure 1); or drag the paired files directly into the napari window. After successful loading, two layers will appear in the bottom-left panel: <kbd>Image</kbd> (raw image) and <kbd>Labels</kbd> (pre-segmentation labels).

CellRefine-3D requires both raw fluorescence images (TIFF) and pre-segmentation label images (TIFF) to be loaded simultaneously.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image1.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 1 Data loading control panel</div>
</div>

## 1.3 View Navigation

### 2D / 3D Switching

Click the <kbd>2D / 3D</kbd> button at the bottom-left of the canvas to switch between 2D slice view and 3D volume rendering view.

### Slice Browsing

In 2D view, drag the <kbd>Z / Y / X</kbd> sliders at the bottom or left side to browse different slices layer by layer.

### 3D Rotation

After switching to 3D view, hold the left mouse button and drag to rotate the perspective; scroll the mouse wheel to zoom.

After extracting a cell neighborhood with CellRefine-3D, it is recommended to first rotate and inspect in 3D view to confirm segmentation quality before entering local editing. See [Basic Operations](loading.md) for details.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image4.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 2 Main interface after data loading (3D view)</div>
</div>

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image5.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 3 Main interface after data loading (2D view)</div>
</div>

## 1.4 Annotation Tools

napari provides multiple native annotation tools, accessible from the top-left toolbar or via keyboard shortcuts:

| Tool | Shortcut | Function |
|------|----------|----------|
| Paint | <kbd>P</kbd> | Draw foreground labels on the Label layer |
| Erase | <kbd>E</kbd> | Erase labeled voxels |
| Fill | <kbd>F</kbd> | Fill connected regions |

All above operations support undo via <kbd>Ctrl</kbd>+<kbd>Z</kbd>. Ensure that a <kbd>Labels</kbd> type layer is currently selected, not an <kbd>Image</kbd> layer.

CellRefine-3D's local editing functions are fully compatible with these native tools. For example, you may first use the eraser to remove erroneous bridges, then execute the plugin's connected-components relabeling (see [Under-segmentation](under.md)); or use the paintbrush to complete missing boundaries before invoking the plugin's density-constrained boundary fix (see [Boundary Fix](boundary.md)). In addition, all native annotation operations are integrated into the plugin's unified Undo / Redo stack and Commit stream, and are fully recorded in the JSON operation log, enabling voxel-level traceability and recovery, while retaining the native <kbd>Ctrl</kbd>+<kbd>Z</kbd> undo functionality. See [Save and Recovery](interaction.md) for details.

## 1.5 Layer Management

- **Show / Hide**: Click the eye icon to the left of the layer name in the <kbd>Layer List</kbd>
- **Adjust Opacity**: Drag the <kbd>opacity</kbd> slider for the corresponding layer
- **Delete Layer**: Click the layer name, then click the trash-can icon

After extracting a local editing region, CellRefine-3D will automatically generate two new layers named <code>OriginalName-Crop</code> and <code>OriginalName-LabelFix</code>, and uses the <kbd>Global View</kbd> toggle to control the display of global versus local layers, preventing accidental modification of other cells. See [Basic Operations](loading.md) for details.

---

**Note**: The above is only a brief overview of napari's core features. For advanced topics such as multi-channel images, time series, and plugin systems, please refer to the [napari official documentation](https://napari.org/stable/).
