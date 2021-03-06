Forms
======


We suppose in this section that you know how to define your [Mesh](mesh.md) and your [function spaces](spaces.md). You may need integration tools too, [see  Integrals](integrals.md).

There are Feel++ tools you need to create linear and bilinear forms in order to solve variational formulation.

Notations:
* `u`  element from your trial function space (unknown function)
* `v`  element from your test function space

# Forms_Building Building Forms
##form1 form1
**Interface***
```cpp
form1(_test, _init);
```
Required Parameters:
* `_test`  test function space.

Optional Parameters:
* `_init`  Default = `false.`

By default, a new linear form is:
$$
l(v)=\int_\Omega v
$$
Then you can customize it using integration tools.

*Example*
From `mylaplacian.cpp`   
marker_form1   

```c++   
// right hand side
    auto l = form1( _test=Vh );
    l = integrate(_range=elements(mesh), _expr=id(v));
```

From `myadvection.cpp`   
marker_form1   

```c++
    // right hand side
    auto l = form1( _test=Xh );
    l+= integrate( _range=elements( mesh ), _expr=f*id( v ) );
```




Notice that \c += operator is working with linear and bilinear forms.


## form2 
**Interface***
```cpp
form2(_trial, _test, _init);
```
Required Parameters:
* `_trial`  test function space
* `_test`  trial function space

Optional Parameters:
* `_init`  Default = `false.`

By default, a new bilinear form is:
$$
a(u,v)=\int_\Omega uv
$$
Then you can custom it using integrations tools

\Example
From `mylaplacian.cpp`   
 marker_form2   
 
 ```c++
    // left hand side
    auto a = form2( _trial=Vh, _test=Vh );
    a = integrate(_range=elements(mesh),
                  _expr=gradt(u)*trans(grad(v)) );

 ```

From `mystokes.cpp`:   
marker_form2   

```c++
    // left hand side
    auto a = form2( _trial=Vh, _test=Vh );
    a = integrate(_range=elements(mesh),
                  _expr=trace(gradt(u)*trans(grad(u))) );
    a+= integrate(_range=elements(mesh),
                  _expr=-div(u)*idt(p)-divt(u)*id(p));

```

Notice that \c += operator is working with linear and bilinear forms.




# Solver 
In this section we present syntax to solve variational formulations. For more general linear problems see \ref Linear.<br>

## solve 
Once you created your linear and bilinear forms you can use the `solve()`  function on your bilinear form.<br>
The `solve()`  function presented there is a method from the class `BilinearForm.` <br>
**Interface***
```cpp
solve(_solution, _rhs, _rebuild, _name);
```
Required Parameters:
* `_solution`  the solution.
* `_rhs`  right hand side. The linear form.

Optional Parameters:
* `_rebuild`  rebuild the solver matrix. Default = `false.`
* `_name`  Default = "".

*Example*
From `laplacian.cpp`:   

 marker_solve   
 ```c++
     // solve the equation  a(u,v) = l(v)  
        a.solve(_rhs=l,_solution=u);
```

## on 
The function `on()`  allows you to add conditions to your bilinear form before using the `solve`  function.<br>
**Interface***
```cpp
on(_range, _rhs, _element, _expr);
```
Required Parameters:
* `_range`  domain concerned by this condition (see \ref Integrals ).
* `_rhs`  right hand side. The linear form.
* `_element`  element concerned.
* `_expr`  the condition.

This function is used with += operator.

*Example*
From `mylaplacian.cpp`:   
marker_on   
```c++
    // apply the boundary condition
    a+=on(_range=boundaryfaces(mesh), _rhs=l, _element=u,
          _expr=expr(soption("functions.alpha")) );
```

There we add the condition: $$ u  =  0  \text{ on }\;\partial\Omega \;$$.

From `mystokes.cpp`:   
marker_on   
```c++
    a+=on(_range=boundaryfaces(mesh), _rhs=l, _element=u,
          _expr=expr<2,1,5>(u_exact,syms));
```

You can also apply boundary conditions using :
 ```cpp
  a+=on(_range=markedfaces(mesh,"top"),_element=u[Component::Y],_rhs=l,_expr=cst(0.))
```
