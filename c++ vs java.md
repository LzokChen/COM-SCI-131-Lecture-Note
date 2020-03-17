#### C++ vs Java

| Language | platform |  | operator overloading | multiple inheritance | class file management |
|---|---|---|---|---|---|
| C++ | dependent | uses compiler only | yes | yes | header file |
| Java | independent | complier + interpreter | no | no | use import keyword |

* **memory management**
  * **C++**: managed by developer using pointers, supports structures and union
  * **java**: Controlled by system, does not use pointer, supports threads and interfaces
* **Inheritance**
  * **C++**: Provide single and multiple inheritance both
  * **java**: not support multiple inheritance, use “interface”
* **Runtime error detection mechanism**
  * **C++**: programmer's responsibility
  * **java**: system's responsibility
* **Libraries**
  * **C++**: comparatively available with low-level functionalities.
  * **java**: Provide wide range of classes for various high-level services
* **Program Handling**
  * **C++**: methods and data can reside outside classes. Concept of global file, namespace scopes available
  * **java**: All methods and data reside in class itself
* **Type Semantics**
  * **C++**: Supports consistent support between primitive and object types
  * **java**: Different for primitive and object types.
* **Portability**
  * **C++**: Platform dependent as source code must be recompiled for different platform
  * **java**: Uses concept of byte code which is platform independent and can be used with **platform specific** JVM.
* **Polymorphism**
  * **C++**: Explicit for methods, supports mixed hierarchies.
  * **java**: Automatic, uses static and dynamic binding.
-------
* C/C++ will be better than Java in these cases:
  * Footprint (ex: embedded controllers)
  * Reboot time (ex: pacemakers)
  * Arrays reshaping is hard to do in Java
  * Value types
  * Direct machine access (ex: device drivers, FPS games)
  * Direct code generation
  * Destructors versus finalizers
  * Destructors versus try/finally
* On the other hand, Java will beat C/C++ in these cases:
  * Profiling
  * Very large programs
  * Garbage collection
  * Multi-threading
  * Tools for parallel coding and debugging
-------------------