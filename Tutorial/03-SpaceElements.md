Spaces and elements {#TutorialSpaces}
============================



You've learned how to discretize the space you want to compute on.
You now have to learn how to define and use function spaces and elements of functions spaces.

The source code is available in `myfunctionspace.cpp`
(The listing is given at the end).

  


# Constructing a function space {#fs}   
- Loading a Mesh in 2D   
  ``` auto mesh = loadMesh(_mesh=new Mesh<Simplex<2>>);```

- For basic function spaces, we have predetermined constructors:   
  ```auto Xh = Pch<2>( mesh );```    

- Defining an element   
  ```auto u = Xh->element( "u" );```   
  ```auto w = Xh->element( "w" );```

 
!CODEFILE "code/myfunctionspace.cpp" 

One can also use :
- `Pdh<ORDER>(mesh)` : Polynomial Discontinuous
- `Pvh<ORDER>(mesh)` : Polynomial Continuous Vectorial
- `Pdhv<ORDER>(mesh)` : Polynomial Discontinuous Vectorial
- `Pchm<ORDER>(mesh)` : Polynomial Continuous Matrix
- `Ned1h<ORDER>(mesh)` : Nedelec function spaces   
 
# Code with other features {#code}   
all   
!CODEFILE "code/myfunctionspace.cpp" 
