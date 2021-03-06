Loading a Mesh 
==============


The next step is to load a mesh. The source code is available in `mymesh.cpp.` 

The `loadMesh` function has a `_name` option set by default as the default value of the `--gmsh.filename` option that point either to a `.geo`, either to a `.msh`, or a ` .h5` file.

```c++
#include <feel/feel.hpp>

...

auto mesh=loadMesh( _mesh=new Mesh<Simplex<2>> );
```

## Exporting the Mesh for visualisation 

See this [section](05-VisualizingFunctions.md) for more details about
exporting and visualizing meshes.


# Complete code

!CODEFILE "code/mesh.cpp"
