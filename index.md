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

## Compiling MeshIntersection Code
_February 19, 2019_

I created a simple mesh that has about 600,000 tetrahedrons. I have been trying to compile Salles’s code from https://github.com/sallesviana/MeshIntersection but so have been running into trouble with either libraries missing or something similar. I have solved some of these issues, and after many updates to gcc and other libraries that are used in the program, I am still missing at least one. The library I am having most trouble with is gnu_parallel. I think the issue might be that when I am attempting to compile, the compiler cannot find where the files related to the library is stored. I might have a different issue regarding one of the compiler flags, -ltcmalloc.

I have gotten the program to compile, and my problem was that I did not have gperftools installed on my computer. Once that was solved I was able to compile and run the program with the sample meshes that were given on the MeshIntersection README.md file.

## Additive Manufacturing Reading
_February 27, 2019_

Today I looked over a good amount of the paper “The status, challenges, and future of additive manufacturing in engineering,” and I thought it was pretty interesting. I did not finish the entire paper but from what I could tell it seems to be a great comprehensive overview of additive manufacturing and the various challenges that need to be overcome to become more widely used.

The biggest issue that seems to be facing AM is that there is not really any type of standardization with regards to all aspects of AM, from design principles, when creating models, to the actual production/manufacturing of the model.
I was surprised as to how many different types of materials can be used now, from various types of metal, plastic, to even ceramic. I searched various AM companies that take a 3D model and print the model for you, I was amazed as to how many types of materials that were available. From highschool I remember people in my general computer science/3D printing/hardware meets software class would try and print out complicated models they made on TinkerCAD. These models resulted in many supports to be created and the amount of time it took to remove these supports without breaking the actual model seemed almost not worth it. I cannot imagine how this problem scales when printing much larger models where accuracy is necessary and either more brittle or more durable materials are utilized.

The general AM process is pretty simple, “successive cross-sectional layers,” each layer “created by selective deposition of material.” These layers allow for complex geometries to be produced, but this also results in a cost of time and energy, especially if the model is quite sizeable. I was curious as to whether AM could be a “greener” way to manufacture products if down the line, after much research, we were able to speed up the process or somehow mass-produce products using AM. But as expected, there just is not enough research in this to really come to a solid conclusion on if this is the way companies should be going when manufacturing. The fact that created large models takes a lot of time and energy to constantly keep the nozzle and bed at very high temperatures (depending on the material, this temperature can be extremely high, i.e. metals). I do think AM would probably be beneficial in the long run as it creates much less waste than the subtractive manufacturing used today, and the flexibility of creating multi-material and geometrically complicated products, which would normally take the assembly of multiple parts, that additive manufacturing gives rise to.

At least from what I have read so far of this overview paper, I think that the additive manufacturing process and various problems facing it are one of the more interesting, things I have learned about.

## "QuickCSG" Reading
_February 28, 2019_

Notes from the “QuickCSG: Arbitrary and Faster Boolean Combinations of N Solids”:
* CSG = Constructive Solid Geometry
  - Representation of solid geometry through binary trees
    - Nodes => boolean operations
    - Leaves => solid primitives
  - Used in solid modeling
    - Emphasis on physical accuracy
    - Geometric + Solid Modeling = 3D CAD
* Problem with Computer Graphics and Computational Geometry: solid modeling using boolean operations
  - Boundary Representation (BREP, B-Rep)
    - Representation of shapes using their limits
    - Collection of connected surface elements (topology & geometry)
    - Boolean, extrusion (sweeping), chamfer, blending, etc. types of operations
  - B-Rep merging written mostly for two solids
  - Computation of boolean operation usually divided into 3 stages:
    1. Subdivision - each object’s boundaries split into 2 groups
    2. Classification - classify which group belongs inside or outside other object
    3. Reconstruction - primitives gathered and connected to build result based on boolean
  - Stages can lead to bad runtime through construction of spatial decomposition structures (hierarchical)
  - Instead of computing unnecessary intermediate results, just compute the entire problem at once
    - Perform subdivision and classification simultaneously with k-d tree
    - Classify nodes once created; determine if in final boundary and subdivide tree if it is
    - Depth of tree depends on size of output
    - Parallel evaluation

## Simple Intersection
_March 12, 2019_

This first image shows the final product of an intersection between a simple 3-dimensional box mesh and a 3-dimensional torus mesh, both created in GMSH, exported as .gts.msh files and converted using Salles’s program to convert from .gts.msh files to .lium files, which is a version of an .off file, compatible with Salles’s 3D-EPUG-Overlay program. The second image is a cross-section of the intersection showing the many faces contained in the 3-dimensional mesh output. 
![Intersection](/assets/screenshot5.png)
![Cross section](/assets/screenshot6.png)

There are about 7 times as many faces as there are vertices in the resulting mesh, after some cleaning up was performed. The .off file which is output by Salles’s program combines all the vertices of the original two meshes and the resulting intersection into one file. This is the reason for the extra vertices originally. Once applying the ‘Remove Unreferenced Vertices’ and ‘Remove Duplicate Vertices’ filters in MeshLab on the output file these unnecessary vertices are removed and only the intersection-relevant geometry is left. But removing all these extra vertices does not account for the fact that there are 7 times more faces than vertices. After taking a closer look at the resulting mesh it seems as though there are many faces that have very small areas which should be able to be simplified to create a more normal mesh. I will have to try this program on smaller meshes to see if there is still a large difference between the two values to be sure it is not a result of some other issue.

Statistics/Data about each Mesh/Model used:
![Table 1](/assets/table1.png)

I am not sure what the number of tetrahedra is in the Intersection model as MeshLab (from what I can tell) does not seem to provide this information. I believe GMSH’s terminology is a bit different as it (from what I can tell) refers to vertices as nodes, and faces as elements. When looking at smaller mesh examples, the numbers make sense so I believe the number of faces above are correct, and will be how I determine the number of faces for any future meshes created in GMSH.

## More Complicated Mesh Intersections
_March 14, 2019_

After playing around with ways of creating somewhat complex meshes, I finally got two somewhat interesting meshes, one a simple couch, and the other is an extruded plus sign where each of the 4 ends have varying heights. After intersecting both meshes, the resulting object is shown in the image below. The other two images are the Couch.gts.msh and Steps.gts.msh files.
![Intersection](/assets/screenshot7.png)
![Steps](/assets/screenshot8.png)
![Couch](/assets/screenshot9.png)

To run Salles’s program on these two meshes, I used Parallel because my laptop took a very long time to run it and gave me a notification that the program was using too much disk space. Even opening the intersection between the two meshes in MeshLab takes a long time to load on my computer. When opening the out.off file, I get the notification: “Warning mesh contains 0 vertices with NAN coords and 92042 degenerated faces. Corrected.” However, the mesh still looks correct and has very many faces and vertices. The number of triangles is data from Salles’s program output when the MeshIntersection program is parsing the given meshes.
![Table 2](/assets/table2.png)
![Table 3](/assets/table3.png)

The data from the second table was output from Salles's MeshIntersection program.

## Trying out Self Intersecting Meshes
_March 18, 2019_

Today I will try and find the intersection between a the steps.gts.msh and a mesh which has self intersections. Salles’s program may crash if there any self-intersections or the mesh is invalid in other ways. I made a mesh with holes and two self-intersections.
Below is the mesh that has self intersections:
![Self Intersecting](/assets/screenshot10.png)

And below is the table of data from the two input meshes, but the program crashed before being able to create an output mesh.
![Table 4](/assets/table4.png)

“segmentation fault (core dumped)  ./meshIntersection chain.lium couch.lium 64 8 1 out.off” is what was given during the programming. This occured when triangulating polygons during retesselation. After trying to run this program twice, I received the same error both times. I suspect that this is due to the chain.gts.msh having self intersections. As noted by Salles, the program’s response to self intersecting meshes is unknown.
When trying to intersect chain.gts.msh with itself, an error also occurs. 
“terminate called after throwing an instance of 'std::bad_alloc'
    what():  std::bad_alloc
zsh: abort (core dumped)  ./meshIntersection chain.lium chain.lium 64 8 1 out.off” 
is the error thrown while in the process of detecting intersections.

## Notes from Salles's Thesis
_March 30, 2019_

__Notes while reading “Exact and Parallel Intersections of 3D Triangular Meshes” by Salles Magalhaes.__
3 Parts: (1) EPUG-Overlay — overlaying of 2D maps, (2) PinMesh — locating points in 3D meshes, and (3) 3D-EPUG-Overlay — computes intersection of two 3D meshes
_EPUG-Overlay_ = _Exact Parallel Uniform Grid Overlay_

- Problems in processing 3D models:
  - Floating point arithmetic precision error
  - Robustness
  - Efficiency
  - Complexity in algorithms for special cases
  
- Fixes in 3D-EPUG-Overlay:
  - Exact arithmetic used
  - _Simulation of Simplicity_ used for special cases
  - Parallelization used to increase efficiency
  
- Roundoff Errors:
  - Roundoff error = difference between actual value and floating-point value
    - Floating-point value is approximation of actual non-integer value
  - Can lead to failures where there should be none
  - Arithmetic filters & interval arithmetic is used in 3D-EPUG-Overlay
    - Simulation of Simplicity needs exact arithmetic
    - Can support geometrical data where coordinates are not represented using rational numbers
    - First, interval arithmetic is used, then an arithmetic filter will filter out unreliable results and recomputes them with exact arithmetic if interval arithmetic answer is unreliable
    - Exact arithmetic is always necessary when new coordinates are computed
    - Arithmetic filters utilized to accelerate the 3D mesh intersection

