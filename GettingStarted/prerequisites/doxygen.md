##DOXYGEN
Doxygen is a tool for auto-generating API documentation. It generates documentation from annotated sources. The main advantage of Doxygen is that you can write documentation directly within the comments of your source code. Doxygen searches for source code in your tree and generates API documentation for it.   

**So, what's the value to Doxygen?** 

Some of Doxygen's output is extracted from the semantics of the source programs. Other parts are from special comments you embed in your code. The entire process requires a special configuration file that specifies exactly what you want to do. Just as your development directory usually has a *Makefile* that controls the build process, you also have a *Doxyfile* that contains the options used by Doxygen.So Doxygen automatically generates an example Doxyfile for you on request.

The source comments can use any of three formats:   
 1. Special comments can mimic a Javadoc comment by using an extra asterisk in a multiline comment
 
 ```
     /**
      *@brief     operation.
      *@date      june 19, 2015
      *@author:   kyoshe winstone
      *@version   1.1
      */
     ```
 2. You can also use an exclamation point instead of the second asterisk 
  
 ```
      /*!
       *x is variable
       *y is variable too
       */
       ```   
 3. Two or more single line comments (//) that have at least one extra slash or exclamation point following the comment marker also form a special comment.   
```
///
/// @return the x value
///
```
 
**Tags**:

Special comments can contain Doxygen tags that let you specify particulars about your program. By default, comments refer to the next lexical element, for example:   
```
    /**
     * @brief, which provides a brief description of the function.
     * @param, which identifies a single parameter to the function (you may have more than one param tag).
     * @return, which describes the function's return value.
     * @date, which describes the date of documentation
     * @todo
     */
```

We can configure our cmake system to enable Doxygen to automatically generate an API documentation when we run ```make```   commands.    
To generate a documentation for our simple simple project using doxygen, add to our CMakeLists.txt the following:

```sh
# add a target to generate API documentation with Doxygen
#set OFF if you don't want Doxygen to generate the documentation
option(BUILD_DOCUMENTATION "Use Doxygen to create the HTML based API documentation" ON)
if(BUILD_DOCUMENTATION)
find_package(Doxygen)
  if(NOT DOXYGEN_FOUND)
    message(FATAL_ERROR
      "Doxygen is needed to build the documentation. Please install it correctly")
  endif()
  configure_file(${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile.in 
                       ${PROJECT_BINARY_DIR}/Doxyfile @ONLY)
  add_custom_target(doc ALL
             COMMAND ${DOXYGEN_EXECUTABLE} ${PROJECT_BINARY_DIR}/Doxyfile
                  COMMENT "Generating API documentation with Doxygen" VERBATIM)
endif()

```   
To generate the documentation, we have to set   
```option(BUID_DOCUMENTATION = ON)``` and ```OFF```  if we don't desire a documentation.

*NOTE* : To reset the option's initial value,we use the cmake command:   
 - For our project   
  ```sh
cmake -D BUID_DOCUMENTATION=ON/OFF
```   
 - GENERALLY:   
```sh
cmake -DOPTION_NAME=NEW_VALUE
```

The base situation is like this :   

After a  CMake run, you can type “make doc” to have CMake run Doxygen. To keep the source tree clean in out-of-source builds, the documentation is generated in the corresponding build directory.   
 - To view the online html online documentation, we use the following command line in the source directory:   
```open html/index.html```   
 - To generate the pdf format of the documentation from the Latek files created, go to the Latex  directory and then use the command line:
  ```sh
    pdflatex   
    pdflatex fileName
    ```


For detailed information about doxygen, please consult [the online doxygen documentation](http://www.stack.nl/~dimitri/doxygen/manual/index.html) and [Doxygen quick reference](http://www.digilife.be/quickreferences/QRC/Doxygen%20Quick%20Reference.pdf)




