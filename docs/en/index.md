# Overview

CellRefine-3D is a cell nucleus segmentation refinement plugin developed on the napari 3D visualization platform, targeting the post-processing of 3D microscope cell nucleus image annotations. After loading raw fluorescence images and pre-segmentation label images (TIFF format), researchers can use this plugin in napari's native 2D/3D view environment—combined with dimension navigation, 3D rotation, and basic annotation capabilities such as paintbrush/eraser/fill bucket—to perform local region extraction, under-segmentation repair (plane splitting / GMM splitting / connected components relabeling), over-segmentation merge, image density-based boundary fix, and watershed-based missing cell detection. CellRefine-3D supports undo and redo, and can save the complete editing history as JSON logs, enabling voxel-level recovery and traceable analysis at any time.

<div style="text-align: center; margin: 2em 0;">
  <video width="80%" style="max-width: 800px;" controls poster="../images/media/image4.png">
    <source src="https://github.com/Hagibis2019/benchmarking_of_cell_segmentation/releases/download/v1.0/introduction_ch.mp4" type="video/mp4">
    Your browser does not support video playback.
  </video>
  <div style="color: #666; font-size: 0.9em; margin-top: 0.5em;">CellRefine-3D Operation Demo Video</div>
</div>