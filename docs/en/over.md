# V. Over-segmentation Merge

Over-segmentation refers to a single cell or the same connected region being incorrectly split into multiple independent labels. CellRefine-3D provides a one-click merge function to combine multiple fragmented labels into one, or directly erase isolated labels that were falsely detected.

Operate in the <kbd>Over-segmentation (Merge)</kbd> sub-panel in the <kbd>Cell Boundary Refine</kbd> area of the plugin panel.

<div style="text-align: center; margin: 1.5em 0;">
  <img src="../../images/media/image17.png" style="max-width: 70%; height: auto; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: block; margin: 0 auto;">
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 17 Over-segmentation merge panel</div>
</div>

## 5.1 Multi-label Merge

Enter the label numbers to be merged in the <kbd>Label IDs to merge</kbd> input box (separated by spaces, e.g., 10 12 15), and click <kbd>Label Merge</kbd>. The plugin will rewrite all input labels to the smallest label ID among them, and merge all voxels corresponding to the remaining IDs under this retained label. The original label numbers automatically become invalid.

## 5.2 Single-label Erasure

If only a single non-zero label is entered in the input box (e.g., 10), after clicking <kbd>Label Merge</kbd>, the plugin will set all voxels of this label to the background value 0, effectively deleting this cell region from the segmentation result. This operation is commonly used to clear obviously falsely detected noise fragments or background residual labels.

<div style="text-align: center; margin: 2em 0;">
  <div style="display: inline-block; vertical-align: middle; text-align: center; width: 280px; margin: 0 15px;">
    <img src="../../images/media/image19.png" style="width: 100%; display: block; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1);">
    <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 18 Before merge example</div>
  </div>
  <div style="display: inline-block; vertical-align: middle; font-size: 2.5em; color: #888; margin: 0 5px;">→</div>
  <div style="display: inline-block; vertical-align: middle; text-align: center; width: 280px; margin: 0 15px;">
    <img src="../../images/media/image18.png" style="width: 100%; display: block; border-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,0.1);">
    <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">Figure 19 After merge example</div>
  </div>
</div>
