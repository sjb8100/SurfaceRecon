# SurfaceRecon
Research project in Computer Graphics, for the University of Technology of Belfort-Montbeliard

____________


This project's goal, in a research perspective, is to reconstruct the environment of a vehicule that was scanned by sensors, such as IBEO Lux ou Velodyne. The data generated by the sensors will be then converted into a point cloud which will then be used to reconstruct the surface previously scanned. This project will be composed of two distincts applications

The first one will be using Unity, and will simulate a vehicule and the sensors on it. The sensors, having the same specs as the original ones, will accumulate data that will then be exported in a .PCD file, the data structure used by the Point Cloud Library (PCL) API.

The second application will use PCL to reconstruct the surface using the PCD file that was generated by the simulator. Many methods will be available, such as :
- Normals computation using the native method of PCL, or using the OpenMP method which take advantage of the GPU.
- Moving Least Squares (MLS), to perform (up)sampling on the point cloud and compute its normals.
- Greedy reconstruction, fast but (very) aggressive regarding noise and holes.
- Poisson reconstruction, slower but fill holes (potentially in a bad way if the provided normals are bad).
- Marching Cubes reconstruction, faster than Poisson and more accurate than Greedy, but does not fill holes most of the time.

The generated mesh will then be saved in a .PLY file, an open source and known type of data structure for 3D models.


IBEO Lux data sheet : http://www.autonomoustuff.com/uploads/9/6/0/5/9605198/ibeo_lux_as.pdf
Velodyne LiDAR Puck data sheet : http://www.autonomoustuff.com/uploads/9/6/0/5/9605198/vlt-16_data_sheet.pdf


____________


# PCL Installation

Simply follow the steps in this link : http://pointclouds.org/documentation/tutorials/compiling_pcl_windows.php

Then build the project with CMake, open the .sln that was generated then build the ALL_BUILD project.


____________


# PCLEngine How-To

To reconstruct a surface with the PCLEngine, a few things have to be respected, like the structure of the command line, which has to look like this:

PCLEngine.exe [name_of_my_file].pcd --list --of --arguments

then the type of normals computation and/or surface reconstruction you want to use. Here are a few possibilities :

PCLEngine.exe [name_of_my_file].pcd --no==false --fvp --nc --po

This command line will take a PCD file without normals as an input (as you specified with --no=false, this is very important), activate the viewpoint flip (--fvp), compute the normals (--nc) then reconstruction the surface with the Poisson method (--po)

PCLEngine.exe [name_of_my_file].pcd --no==true --mar

This command line will take a PCD file with normals as an input (--no=true) and reconstruct the surface with the Marching Cubes method.


____________


Here is the arguments list :

* --no=true/false 	--> Specify if the input cloud point contains normals or not, in order to use the appropriate datatype
* --fvp 	--> Will flip the viewpoint of the normals in case they are computed
* --omp 	--> Will use the OpenMP version of the normals computation method
* --up 	--> Will upsample the cloud point if MLS method is used
* --nc 	--> Will compute the normals
* --mls 	--> Will compute the normals/upsample the cloud point using the Moving Least Squares method
* --gr 	--> Reconstruct the surface with the Greedy method
* --po 	--> Reconstruct the surface with the Poisson method
* --mar 	--> Reconstruct the surface with the Marching Cubes method
