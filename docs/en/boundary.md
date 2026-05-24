# VI. Boundary Fix

Boundary Fix is used to correct deviations between pre-segmentation labels and real cell fluorescence contours, such as label boundaries over-expanding into the background, or shrinking and missing real cell edges. The plugin re-searches for the optimal boundary within a limited range based on the local image density field, and generates smooth, connected cell masks through morphological post-processing.

Operate in the <kbd>Boundary Fix</kbd> sub-panel in the <kbd>Cell Boundary Refine</kbd> area of the plugin panel.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image20.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 20 Boundary Fix panel</div>
</div>

## 6.1 Parameter Description

- <kbd>Percentile</kbd>: Density threshold percentile (default 35). The plugin uses the image brightness distribution of voxels within the target cell as a basis, takes the brightness value corresponding to this percentile multiplied by 0.9 as the binarization threshold. The higher the value, the stricter the threshold, the fewer high-brightness voxels that meet the condition, the smaller the retained region, and the more the boundary shrinks inward; the lower the value, the looser the threshold, the more voxels that meet the condition, the larger the retained region, and the more the boundary expands outward. If the cell boundary over-expands and includes too much background, this value can be appropriately increased to tighten the boundary; if the cell boundary shrinks excessively and misses real weak-signal edges, or if you wish to capture outward-extending protrusions, this value can be appropriately decreased to expand the boundary.

- <kbd>Scaling</kbd>: Bounding box scaling coefficient (default 1.00). Based on the original <code>bounding box</code> of the target cell, the search range is proportionally expanded or shrunk according to this coefficient. Values greater than 1.0 allow the algorithm to search for the optimal boundary in a larger range, suitable for scenarios where cells have slender protrusions and the original <code>bounding box</code> fails to fully enclose them; values less than 1.0 limit the search range, suitable for preventing the boundary from over-expanding toward neighboring cells. It is recommended to keep the default value for first-time use.

- <kbd>Use Hull</kbd>: Whether to enable 3D convex hull constraint (checked by default). When checked, the repair range is limited within the proportionally expanded/shrunk region of the 3D convex hull of the original label point cloud; when unchecked, it degenerates into a complete rectangular <code>bounding box</code>, suitable for scenarios where cell shapes are extremely irregular and the convex hull is severely distorted.

## 6.2 Executing the Fix

Confirm that the <kbd>Label ID</kbd> input box has been filled with the label numbers requiring boundary repair (supports multiple labels, separated by spaces). After adjusting <kbd>Percentile</kbd>, <kbd>Scaling</kbd>, and <kbd>Use Hull</kbd>, click <kbd>Boundary Fix</kbd>.

The plugin sequentially executes the following workflow:

- Mean filtering and smoothing of the local raw image to suppress noise;
- Calculating the expanded <code>bounding box</code> according to the <kbd>Scaling</kbd> coefficient;
- If <kbd>Use Hull</kbd> is enabled, constructing the 3D convex hull of the target label as the effective <kbd>ROI</kbd>;
- Screening high-density voxels within the <kbd>ROI</kbd> according to the <kbd>Percentile</kbd> threshold, and extracting the largest connected component;
- Performing morphological closing, dilation, erosion, small hole filling, and small object filtering to generate a smooth boundary;
- Writing the new boundary back to the <kbd>LabelFix</kbd> layer, strictly limited within the "original target region + background" range, never covering neighboring other cells.

After repair, it is recommended to rotate and observe the fit between the new boundary and the fluorescence signal in 2D slices or 3D view. If the result is not ideal, parameters can be adjusted and <kbd>Boundary Fix</kbd> executed again. This operation is also often combined with the paintbrush, eraser, etc. See [Napari Basics](napari.md) for details.

<div style="text-align: center; margin: 2em 0;">
  <div style="display: inline-block; vertical-align: middle; text-align: center; width: 280px; margin: 0 15px;">
    <img src="../../images/media/image22.png" style="width: 100%; display: block; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1);">
    <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 21 Boundary Fix before-use schematic</div>
  </div>
  <div style="display: inline-block; vertical-align: middle; font-size: 2.5em; color: #888; margin: 0 5px;">→</div>
  <div style="display: inline-block; vertical-align: middle; text-align: center; width: 280px; margin: 0 15px;">
    <img src="../../images/media/image21.png" style="width: 100%; display: block; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1);">
    <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 22 Boundary Fix after-use schematic (including two operations with Percentile 60, Scaling 1.30, plus paintbrush refinement)</div>
  </div>
</div>
