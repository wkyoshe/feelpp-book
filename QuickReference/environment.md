Environment
===========


In this section, we present some tools to initialize and manipulate Feel++ environment. For more information, see  [Environment](environment.md).

# Initialize Feel++

Environment class is necessary to initialize your application, as seen in FirstApp. Interface is as follows:
```cpp
Environment env( _argc, _argv, _desc, _about );
```
None of those parameters are required but it is highly recommended to use the minimal declaration:
```cpp
  Environment env( _argc=argc, _argv=argv,
                   _desc=feel_option(),
                   _about=about(_name="name_of_your_app",
                                _author="your_name",
                                _email="your_email_adress") );
```

* `_argc` and `_argv`` are the arguments of your main function.
* `_desc` is a description of your options.
* `_about` is a brief description of your application.

# Options Feel++ Options Description
## Options_Custom Add Options
`feel_option()`  returns a list of default options used in Feel++.<br>

You can create a personal list of options as seen in \ref FirstApp.

You can also add a list of options, using a routine as follows:
```cpp
  using namespace Feel;


  inline
  po::options_description
  makeOptions()
  {
    po::options_description myappOptions( "My app options" );
    myappOptions.add_options()
      ( "option1", po::value<type1>()->default_value( value1 ), "description1" )
      ( "option2", po::value<type2>()->default_value( value2 ), "description2" )
      ( "option3", po::value<type3>()->default_value( value3 ), "description3" )
      ;
    return myappOptions.add( feel_options() ); // Add the default feel options to your list
  }
```
* `makeOptions`  is the usual name of this routine but you can change it
* `myappOptions`  the name of you options list
* `option` #: the name of parameter #
* `type` #: the type parameter #
* `value` #: the default value of parameter #
* `description` #: the description of parameter #

The data returned is typically used as an argument of a Feel::Application subclass.

This routine has to be declared before your `main`  function. Then you can use it to initialize Feel++ Environment:
```cpp
  Environment env( _argc=argc, _argv=argv,
                   _desc=makeOptions(),
                   _about=about(_name="myapp",
                                _author="myname",
                                _email="my@email.com") );
```

So you can change this parameter when you execute your app:
```cpp
  ./myapp --option1=alpha --option2=beta --option3=gama
```


**Example:**<br>
From `/doc/manual/laplacian/laplacian.cpp`
```cpp
  using namespace Feel;

  inline
  po::options_description
  makeOptions()
  {
      po::options_description laplacianoptions( "Laplacian options" );
      laplacianoptions.add_options()
          ( "hsize", po::value<double>()->default_value( 0.2 ), "mesh size" )
          ( "shape", Feel::po::value<std::string>()->default_value( "hypercube" ), "shape of the domain (either simplex or hypercube)" )
          ( "nu", po::value<double>()->default_value( 1 ), "grad.grad coefficient" )
          ( "weakdir", po::value<int>()->default_value( 1 ), "use weak Dirichlet condition" )
          ( "penaldir", Feel::po::value<double>()->default_value( 10 ),
          "penalisation parameter for the weak boundary Dirichlet formulation" )
          ( "exact1D", po::value<std::string>()->default_value( "sin(2*Pi*x)" ), "exact 1D solution" )
          ( "exact2D", po::value<std::string>()->default_value( "sin(2*Pi*x)*cos(2*Pi*y)" ), "exact 2D solution" )
          ( "exact3D", po::value<std::string>()->default_value( "sin(2*Pi*x)*cos(2*Pi*y)*cos(2*Pi*z)" ), "exact 3D solution" )
          ( "rhs1D", po::value<std::string>()->default_value( "" ), "right hand side 1D" )
          ( "rhs2D", po::value<std::string>()->default_value( "" ), "right hand side 2D" )
          ( "rhs3D", po::value<std::string>()->default_value( "" ), "right hand side 3D" )
          ;
      return laplacianoptions.add( Feel::feel_options() );
  }
```


## Options_Accessors Accessors
**Options Description:**<br>
```cpp Environment::optionsDescription();```
Returns options description data structure (`po` options_description).<br>


**Variable map**<br>
You can access to the parameters of your application environment using the following function:
```cpp
  Environment::vm(_name);
```
`_name`  is the name of the parameter as seen in the previous paragraph.<br>
This function returns a `po` variable_value .<br>
Use template methode to cast the parameter into the appropriate type.<br>

**Examples:**<br>
From `doc/manual/solid/beam.cpp`
```cpp
  const double E = Environment::vm(_name="E").template as<double>();
  const double nu = Environment::vm(_name="nu").template as<double>();
```
From `doc/manual/fd/penalisation.cpp`
```cpp
  Tfinal =  Environment::vm( _name="test" ).template as<int>()*dt;
```


# Repository Repository
## Change changeRepository
You can change the default repository.

Interface
```cpp
void changeRepository( _directory, _subdir, _filename );
```
Required Parameters:
* `_directory`: new directory

Optional Parameters:
* `_subdir`: Default = `true`
* `_filename`: Default = `"logfile"`

You can use `boost` format to customize the path. <br>
**Example:**<br>
From `doc/manual/laplacian/laplacian.cpp`
```cpp
    Environment::changeRepository( boost::format( "doc/manual/laplacian/%1%/%2%-%3%/P%4%/h_%5%/" )
                                   % this->about().appName()
                                   % shape
                                   % Dim
                                   % Order
                                   % meshSize );
```
Then results will be store in: "/doc/manual/laplacian/<appName>/<shape>-<Dim>/P<Order>/h_<meshSize>/"


## Find findFile

Interface
```cpp
std::string findFile( std::string const& filename );```
Returns the string containing the filename path.

The lookup is as follows:
* look into current path
* look into paths that went through changeRepository(), it means that we look for example into the path from which the executable was run

If the file has an extension .geo or .msh, try also to
* look into `localGeoRepository()`  which is usually $HOME/feel/geo
* look into `systemGeoRepository()`  which is usually $FEELPP_DIR/share/feel/geo

If `filename` is not found, then the empty string is returned.


## SetLogs setLogs
**Interface***
```cpp
void setLogs( std::string const& prefix );
```
Required Parameters:
* `prefix`  prefix for log filenames.
