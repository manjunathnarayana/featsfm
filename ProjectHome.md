# <font color='#0000FF'>Feature</font> <font color='#00CCFF'>S</font>tructure <font color='#00CCFF'>F</font>rom <font color='#00CCFF'>M</font>otion #

Copyright (C) 2012-2013 **Balint Cristian** et. all

## Description ##

<font color='#0000FF'>Feat</font><font color='#00CCFF'>SFM</font>**aim to bring state of
art algorithms for 3D vector model reconstruction from raster image collections. It relay
on extracting feature descriptors across any raster image collections using multi-threading
environment with latest CVision algorithms, match all collections using nearest neighbour lookup techniques and compute bundle adjustment parameters and a final dense point cloud using various state of art methods. It will be available for Linux and MS-Windows platform in booth CLI and GUI mode. It also feature image collection cluster analysis, multi view options booth in 2D and 3D graphical environment and many export features. GIS extensions for operations like geo-reference point cloud, rectify or mash rasters are planned.**

![http://featsfm.googlecode.com/svn/trunk/web/images/featsfm-sample.gif](http://featsfm.googlecode.com/svn/trunk/web/images/featsfm-sample.gif)
<a href='Hidden comment: 
This text will be removed from the rendered page.
'></a>

## Acknowledgement ##

  * Peoples:
    * [Noah Snavely](http://www.cs.cornell.edu/~snavely/) bundle adjustment logic code
    * [Yasutaka Furukawa](http://www.cs.washington.edu/homes/furukawa/) patch based dense reconstruction code
    * [Tai-Pang WU](http://www.cse.ust.hk/~pang/Welcome.html) tensor-markov patch based dense reconstruction code
    * [Marius Muja](http://people.cs.ubc.ca/~mariusm/) FLANN based SSSE3 hamming template optimisation
    * [Nghia Ho](http://nghiaho.com/) Levenberg-Marquard optimisation code
    * [Geo-Spatial.Org](http://earth.unibuc.ro) Folks for support
  * External libraries:
    * math: [OpenCV](http://www.opencv.org), [FLANN](http://people.cs.ubc.ca/~mariusm/index.php/FLANN/FLANN), [lmFIT](http://joachimwuttke.de/lmfit/), [LAPACK](http://www.netlib.org/lapack/), [ATLAS](http://math-atlas.sourceforge.net/), [SBA](http://www.ics.forth.gr/~lourakis/sba/), [PBA](http://grail.cs.washington.edu/projects/mcba/)
    * i/o: [GDAL](http://www.gdal.org/), [HDF5](http://www.hdfgroup.org/HDF5/), [SQLite3](http://www.sqlite.org/)
    * gui: [QT4](http://qt.nokia.com/products/), [OpenSceneGraph](http://www.openscenegraph.org/projects/osg)

## Features ##

  * CLI mode tools, also visual GUI 2D/3D interface will be available.

### Modules ###

- **feat2d**
  * Detect, extract and match feature key points across images. Output HDF5 databases.
    * detectors: ORB, AGAST, FAST, STAR, MSER and (_non-free_) SIFT, SURF
    * descriptors:
      * hamming: BRISK, FREAK, ORB, BRIEF
      * euclid: SIFT, SURF (_non-free_)
    * matchers: muti-thread FLANN in euclid and hamming space (with SSSE3 and SSE4.2 optimisations)
    * cameras: sqlite3 database with CCD parameters is provided when there is no EXIF info
    * output: GDAL/GIS compatible HDF5 raster object with multidimensional features as additional vector data
- **bundler**
  * Compute camera parameters and alignment doing bundle adjustment. Original reused codes: [Bundler](http://phototour.cs.washington.edu/bundler/)
    * modes:
      * [SBA](http://www.ics.forth.gr/~lourakis/sba/) sequential adjustment (external plugin)
      * [PBA](http://grail.cs.washington.edu/projects/mcba/) parallel adjustment (external plugin)
    * output:
      * HDF5 vector data including camera parameters, also RTk parameter sets.
      * [bundle](http://phototour.cs.washington.edu/bundler/bundler-v0.4-manual.html) and [PLY](http://en.wikipedia.org/wiki/PLY_%28file_format%29) sparse point cloud
      * undistorted raster in GeoTIFF format.
- **feat3d**
  * Dense point cloud computation. Original reused code: [PMVS2](http://grail.cs.washington.edu/software/pmvs/) [TMVS](http://www.cse.ust.hk/~pang/Welcome.html) [CMVS](http://grail.cs.washington.edu/software/cmvs/)
    * modes:
      * [PMVS2](http://grail.cs.washington.edu/software/pmvs/) using projected patches
      * [TMVS](http://www.cse.ust.hk/~pang/Welcome.html) using patches in tensor based MRF network.
    * output:
      * [PLY](http://en.wikipedia.org/wiki/PLY_%28file_format%29) dense point cloud
      * [OBJ](http://people.sc.fsu.edu/~jburkardt/data/obj/obj.html) meshed point cloud with raster texture
- **gui**
  * GUI interface for user work flow. It is able to auto detect multiple raster file formats using GDAL, and provides a complete work flow for users with visual outputs.
    * _**view modes**_:
      * 2D mode:
        * view collection of rasters, order by cluster-groups if its the case
        * view 2d graph based relations between raster pairs using eigen relations
        * view composition of proposed ortho-mosaic from raster homography
        * view and manage GCP control points
      * 3D mode:
        * view point cloud, camera positions and raster projections
        * view 3d graph based relations between raster pairs using eigen relations
    * _**operations**_:
      * compute descriptors using:
        * threshold keys by 0-100% percent range
        * limit keys to best amount of 0-1.000.000 points range
      * match keys across all raster pairs
      * bundle adjust scene(s)
      * manage GCP across raster collection(s)
      * export ortho-mosaic from pre-selected rasters homography
      * compute dense pointcloud(s)
      * mesh point cloud networks with raster texture

