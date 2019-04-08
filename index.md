## PARALLEL 3D MESH INTERSECTIONS	


Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/emileerei/MeshIntersection/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.


### Testing out Various Mesh Creation Programs
_February 9, 2019_
After testing out Gmsh and ONELAB, ONELAB seems to just be an extension of Gmsh with added capabilities for finite element research.

So far I have watched a few basic Gmsh tutorial videos and have been reading more of the documentation from the Gmsh website. The program seems easy to use and is good for more than just creating meshes. It’s also good for basic geometry creation and post-processing on meshes. The basic process of creating a simple mesh is first by creating geometry—add points, connect these points by lines, make plane surfaces from the lines, and possibly add volume surfaces—for the mesh. 

![Image](/assets/screenshot1.png)
The image on the left is a simple 2D mesh (surface meshing) I created after following the simple tutorial videos.
There are also two types of geometries, elementary entities and physical groups. Elementary entities are for anything related to geometry and physical groups are related to the boundary conditions for the geometry created. 

I have also found that there are many options for viewing the geometry/mesh in useful ways such as changing the color of the mesh by vertex valence and viewing the object on a grid to see how large the object is. I will be trying to create some interesting 3D meshes (volume meshing) tomorrow.
