# Binding
* At the vary basic, binding is assigning one thing to another, usually, values to variables either by **explicit declaration** or **implicit declaration**.
*  Explicit declaration simply implies a statement that defines the variable name and it's type e.g `public int i;`
*  while in implicit, the default convention is implemented e.g `int i;`
   *  it assumes it is public.
   *   Binding `int a, b = 0;` can occur at run-time or at compilation time, when it happens at run-time, the `current state` of the variable is changed in that a value is assigned to it. For instance
        ```C
        int a, b = 0, c;
        for(c = 0; c < 10; c++){
            b+=c;
        }
        ```  
        * the state of `c` changes as values are been added as the `for` loop is being executed.

## Types of Binding
* **Dynamic Binding** : Variable are bound to a type depending on the value assigned this happens at **run-time**.
* **Static Binding**: This happens at **compile time** where the compiler figures out what variable to assign to what method or function. A perfect example is method overloading where two methods have same name but different number or type of parameter. If two parameters are passed in a method `A` (that has two parameters), the compiler figures out that the method to return is `A`.


## static scoping vs dynamic scoping**
* https://www.geeksforgeeks.org/static-and-dynamic-scoping/
* **static scoping**
  * a.k.a lexical scoping
  * the scope a property of the program text, it is defined in compile time
  * a variable always refers to its top level environment
  * much easier to make a modular code as programmer can figure out the scope just by looking at the code. (why it is dominant in most mof programming)
  * C, C++ and Java
  * the compiler first searches in the current block, then in global variables, then in successively smaller scopes.
* **dynamic scoping**  
  * the scope is defined in run time (from the stack)
  * dynamic scope requires the programmer to anticipate all possible dynamic contexts.
  * a global identifier refers to the identifier associated with the most recent environment
  * each identifier has a global stack of bindings and the occurrence of a identifier is searched in the most recent binding.
  * the compiler first searches the current block and then successively all the calling functions.
  * Dynamic scoping does not care how the code is written, but instead how it executes. Each time a new function is executed, a new scope is pushed onto the stack.

*   ```C
    int main()
    {
    printf(g());
    return 0;
    }

    int x = 10; //global

    int f()
    {
    return x;
    }

    int g()
    {
    int x = 20; //local
    return f();
    }
    //static scoping -> return 10, the value returned by f() is not dependent on who is calling it
    //dynamic scoping -> return 20
    ```

* Perl supports both dynamic ans static scoping. Perl’s keyword “my” defines a statically scoped local variable, while the keyword “local” defines dynamically scoped local variable.
