# VII. Missing Cell Detection

Missing Cell Detection is used to identify real cells that were missed in the pre-segmentation labels—that is, regions in the fluorescence image with obvious signal clusters but not assigned any label. The plugin is based on the watershed algorithm, using user-specified seed points as foreground markers to automatically grow new cell labels within a limited range.

Operate in the <kbd>Missing Cell Detection</kbd> area of the plugin panel.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image23.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 23 Missing Cell Detection panel</div>
</div>

## 7.1 Placing Seed Points

Click <kbd>Select Seed Point</kbd>. If the seed point layer has not yet been created, the plugin will automatically generate the <kbd>Missing-Cell-Seeds</kbd> layer (yellow <kbd>Points</kbd>) in napari and switch to add mode. At this point, you need to click to place seed points layer by layer at the centers of suspected missed fluorescence bright spots in the 2D view. Each seed point corresponds to a new cell candidate area to be detected; multiple seed points can be placed on multiple Z layers, and the plugin will process them sequentially. After completing Missing Cell Detection, press the shortcut <kbd>Shift</kbd>+<kbd>C</kbd> to clean up points and layers with one click. See [Basic Operations](loading.md) for details.

## 7.2 Parameter Description

- <kbd>Threshold</kbd>: Background/foreground brightness threshold (default 500). Voxels in the raw image with brightness below this value are considered background and forcibly set as background markers (value 2) in the watershed markers. The higher the value, the stricter the requirement for fluorescence signals, and only high-brightness regions are included as candidates; the lower the value, the more weak-signal regions are included as candidates, but noise may also be introduced. Adjust according to the image signal-to-noise ratio. It is recommended to clearly distinguish cells from the background for first-time use.

- <kbd>Max Distance</kbd>: Maximum watershed growth distance (default 10, unit: pixels). Starting from the seed point, the algorithm performs watershed segmentation only within neighborhoods where the Euclidean distance is less than this value. The larger the value, the wider the range allowed for new labels to expand around, suitable for larger cells; too small a value may result in newly detected regions significantly smaller than the real cell.

## 7.3 Executing Detection

Confirm that at least one seed point has been placed on the <kbd>Missing-Cell-Seeds</kbd> layer, and that <kbd>Threshold</kbd> and <kbd>Max Distance</kbd> parameters are appropriate, then click <kbd>Detect Missing Cell</kbd>. The plugin sequentially executes:

- Using all seed points as foreground markers (value 1), and regions below the threshold as background markers (value 2);
- Calculating the distance transform field (Euclidean distance to the nearest seed point);
- Limiting the watershed calculation range with <kbd>Max Distance</kbd> as the radius to avoid unlimited growth to distant other cells;
- Executing watershed segmentation to extract independent regions corresponding to each foreground marker;
- Assigning incrementing new label IDs to foreground regions in the segmentation result, and writing back to the <kbd>LabelFix</kbd> layer.

Each seed point theoretically produces one new label, but the resulting label coverage area may partially not match the actual cell coverage area. At this point, <kbd>Boundary Fix</kbd> needs to be used to further repair the boundary. See [Boundary Fix](boundary.md) for details.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image24.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 24 Watershed seed point selection example</div>
</div>

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image25.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 25 Watershed result example</div>
</div>
