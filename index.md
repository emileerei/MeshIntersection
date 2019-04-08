---
layout: default
---
# PARALLEL 3D MESH INTERSECTIONS	

This is the documentation/notes I have taken while learning about 3D(tetrahedral) meshes during my independent study. I used the program GMSH to create the meshes, and used Salles Viana Gomes de Magalhaes's, [MeshIntersection](https://github.com/sallesviana/MeshIntersection) program to find the intersections between two meshes.

## Testing out Various Mesh Creation Programs
_February 9, 2019_

After testing out Gmsh and ONELAB, ONELAB seems to just be an extension of Gmsh with added capabilities for finite element research.

So far I have watched a few basic Gmsh tutorial videos and have been reading more of the documentation from the Gmsh website. The program seems easy to use and is good for more than just creating meshes. It’s also good for basic geometry creation and post-processing on meshes. The basic process of creating a simple mesh is first by creating geometry—add points, connect these points by lines, make plane surfaces from the lines, and possibly add volume surfaces—for the mesh. 

![Image](/assets/screenshot1.png)
The image on the left is a simple 2D mesh (surface meshing) I created after following the simple tutorial videos.
There are also two types of geometries, elementary entities and physical groups. Elementary entities are for anything related to geometry and physical groups are related to the boundary conditions for the geometry created. 

I have also found that there are many options for viewing the geometry/mesh in useful ways such as changing the color of the mesh by vertex valence and viewing the object on a grid to see how large the object is. I will be trying to create some interesting 3D meshes (volume meshing) tomorrow.

## Learning how to Use GMSH
_February 11, 2019_

I followed the the rest of the GMSH tutorial provided by the Applied Modeling and Computation Group (AMCG), which gave a good overview of various ways to create and edit geometry and meshes in GMSH.  And was able to create this 3D mesh of a cylinder-like object with a hold down the middle of it. This was easily created by using the GMSH extrude tool on two 2D rectangles which were extruded around an axis. I also did basic functions, such as creating simple geometry and creating surfaces, which connect the geometry, in the script that describes each GMSH file. The different colors on the cylinder represent different groups of geometry, which is what occurs when extruding or transforming the mesh in various ways.
![Cylinder Mesh](/assets/screenshot2.png)
![Solid Cylinder](/assets/screenshot3.png)

I was also playing around with a simple rectangular prism and the “Prescribed mesh size at point” which basically changes the size of the point. By making this number small, 0.1, when creating a 3D mesh there are more tetrahedra than if I make this number larger, 1. Below are the comparisons between making the size = 0.1 and size = 1. There is another way to achieve this effect, but instead of changing the actual geometry, it just change each of the element sizes with respect to the element size that was defined when creating the points. From the few tutorials I have followed it has been advised against, but I’m not completely sure why.

Both photos below are 2D meshes with a point size of 1(top) and 0.1(bottom), and on the right are 3D meshes with a point size of 1(top) and 0.1(bottom). The triangle elements are colored red and the tetrahedral elements are a dark blue.
![2D Meshes](/assets/screenshot4.png)
