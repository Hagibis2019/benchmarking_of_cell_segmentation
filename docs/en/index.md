# NuPatch3D Overview

**NuPatch3D** is a **[napari](https://napari.org/)** plugin designed for post-processing cell nucleus segmentation results. It targets common errors produced by algorithms such as Cellpose, including under-segmentation, over-segmentation, missing nuclei, and boundary inaccuracies. NuPatch3D provides local region extraction, target cell highlighting, automatic editing log saving, undo/redo functionality, and more—enabling users to efficiently and accurately refine segmentation results while maintaining full traceability throughout the editing and quality-control workflow. Furthermore, NuPatch3D seamlessly integrates with napari's native annotation tools, supporting brush, eraser, and fill operations for voxel-level manual refinement.


<div style="text-align: center; margin: 2em 0;">
  <video width="80%" style="max-width: 800px;" controls poster="../../images/media/image4.png">
    <source src="https://github.com/Hagibis2019/benchmarking_of_cell_segmentation/releases/download/v1.0/introduction_ch.mp4" type="video/mp4">
    Your browser does not support video playback.
  </video>
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">NuPatch3D Video Tutorial</div>
</div>
