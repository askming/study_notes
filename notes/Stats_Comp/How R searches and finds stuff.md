# [How R searches and finds stuff](https://blog.obeautifulcode.com/R/How-R-Searches-And-Finds-Stuff/)
by *Suraj Gupta*
<hr>

- Everything in R lives in an **environment**.
- Ultimately, we're trying to figure out which environment something lives in, and how we get there (or how we got there)
- An environment, like everything else in R is an object. Objects hold stuff. Environments are specialized, they can only hold two things:
  - A **frame**
    - This is just a collection of named objects. Since everything in R lives in an environment and the frame is the place where an environment holds objects
  - **Enclosing environment** (environment's owner)
    - This is just a reference to another environment (parent environment).
    
    - The chain of enclosing environments stops at a special environment called the **Empty Environment**, which can be accessed with `emptyenv()`. 
    
      ![environment-structure](https://raw.githubusercontent.com/askming/picgo/master/environment-structure.png)
    
    - Given an environment object, you can query the object for the two things that matter: the environment’s owner and the objects in the frame.

## Play time with Environemnts

- Create a new environment

  ```R
  > myEnvironment = new.env() 
  > myEnvironment 
  <environment: 0x0000000006ce0920>
  ```

- Every environment (except R_EmptyEnv) has an enclosure

  ```R
  > parent.env(myEnvironment) 
  <environment: R_GlobalEnv>
  ```

- Who's R_GlobalEnv's enclosing environment?

  ```R
  > parent.env( parent.env( myEnvironment ) ) 
  <environment: package:stats>
  attr(,"name")
  [1] "package:stats"
  attr(,"path")
  [1] "C:/R/R-2.14.1/library/stats"
  ```

- The `R_GlobalEnv` must be special if it can be retrieved using the identifier `.GlobalEnv` AND a function `globalenv()`

- The empty environment is accessed using `emptyenv()`

  ```R
  > emptyenv()
  <environment: R_EmptyEnv>
  ```

- `<environment: 0x0000000006ce0920>` is the location of the environment in memory. We can add a friendly name by assigning a "name" attribute.

  ```R
  > attr( myEnvironment , "name" ) = "Cool Name" 
  > myEnvironment 
  <environment: 0x0000000006ce0920>
  attr(,"name")
  [1] "Cool Name"
  > environmentName( myEnvironment ) 
  [1] "Cool Name"
  ```

- Unless you try hard, when you create an object it is automatically placed in the "current" or "local" environment, accessible using `environment()`; And we can query an environment for all objects in the frame using `ls()`.

- We can override the default behavior and create an object in an environment other than the local environment using the `assign()` function.

  ```R
  > ls( envir = myEnvironment )
  character(0)
  > assign( "myLogical" , c( FALSE , TRUE ) , envir = myEnvironment )
  > ls( envir = myEnvironment )
  [1] "myLogical"
  ```

- We can retrieve any named object from any given environment using the `get()` function

  ```R
  > get( "myLogical" , envir = myEnvironment )
  [1] FALSE  TRUE
  ```

- R uses the local environment as the default value. You can change an environment's enclosure using the replacement form of `parent.env()`.

  ```R
  > myEnvironment2 = new.env()
  > parent.env( myEnvironment2 )
  <environment: R_GlobalEnv>
  > parent.env( myEnvironment2 ) = myEnvironment
  > parent.env( myEnvironment2 )
  <environment: 0x0000000006ce0920>
  attr(,"name")
  [1] "Cool Name"
  ```

- Another way to understand the "current" or "local" environment: We create a function that calls `environment()` to query for the local environment. When R executes a function it automatically creates a new environment for that function. This is useful - variables/objects created inside the function will live in the new local environment.

- We call `Test()` to verify this. We can see that `Test()` does NOT print `R_GlobalEnv`. We didn't create any objects within `Test()`. If we had, they would live in the `0x0000000006ce9b58` environment while `Test()` is running. When the function completes executing, the environment dies. 

  ```R
  > Test = function() { print( environment() ) } 
  > environment()
  <environment: R_GlobalEnv>
  > Test() 
  <environment: 0x0000000006ce9b58>
  ```

## Short Answer: How R Searches and Finds stuff

- It seems that environments are not just static repositories of objects. When R executes an expression, there is always one *local* or *current* environment. Maybe its easier to think of this as the *active* environment, in contrast to the other environments which are *inactive*. Or perhaps its more intuitive to think of an expression executing “within” a particular environment. The point here is that at any moment R can ask “hey, what’s the local environment.” R asks this questions a lot. In fact, it asks this question every time it needs to find a named object. We saw that R creates a new local environments every time it runs a function. So when we run any decently involved piece of code, functions call other function and environments spawn and die.
- When R goes searching for the names in that expression, it first looks at the objects within the local environment. If the object is not found by name in that environment, then R searches the enclosing environment of the local environment. If the object is not in the enclosure, then R searches the enclosure’s enclosure, and so on. That’s how R searches and finds stuff; it traverses the enclosing environments and stops at the first environment that contains the named object.

## Map of the World (follow the purple line road)

![map-of-the-world](https://raw.githubusercontent.com/askming/picgo/master/map-of-the-world.png)

This graphic shows the state of all environments when you first startup R. Each box represents a unique environment. The solid purple line represents the enclosing environment relationship. I’ll explain the dotted purple line in a bit. For now, consider it a relationship that’s similar to the enclosing environment.

## The Global Environment

- The global environment is precisely the environment that you start at when you launch R. It is your *current* or *local* environment when R launches. If you make an assignment at the prompt, the named object is stored in `R_GlobalEnv`.
- Be careful with the `environment()` function. It might seem wrong that this returns `NULL` but if you read the documentation you'll see that `environment()` takes a function as input. `myVariable` is not a function, its a numeric.  The purpose of `environment()` is not to tell you an object's owner.

## The Search List

- In R, the “search list” is the chain of enclosing environments starting with `R_GlobalEnv` and ending with `R_EmptyEnv`. I like to think of it as the main highway on our map. This is the highway that R drives down when we start in `R_GlobalEnv`. All roads in our world eventually lead to this highway. You can obtain the search list by typing `search()` at the prompt:

## Package v. Namespace v. Imports Environments

![package-namespace-imports](https://raw.githubusercontent.com/askming/picgo/master/package-namespace-imports.png)

1. **package environment**

   - This is where a package’s *exported* objects go.
   - Simply put, these are the objects that the package author wants you to see. These are most likely functions. 
   - In traditional Object Oriented Programming (OOP), this is analogous to a “public” class or method.

2. **namespace environment**

   ```{margin}
   [^1]: The latter, the “hidden” objects (they are not really hidden, you can access them if you’d like) facilitate the “visible” ones. 
   ```
   - This is where *all* objects in a package go. This includes objects the package author wants you see. It also includes objects that are not meant to be accessed by the end-user[^1]. In OOP this is analogous to a “private” or “internal” class or method.
   - You might be thinking “wait, so objects the author wants me to see are in BOTH the package environment and the namespace environment?” Yes and No. Yes, both environments have a frame that lists objects of the same name, but no there is not two copies. Both environments have [pointers to the same function](http://stackoverflow.com/questions/9278715/value-reference-equality-for-same-named-function-in-package-namespace-environmen).

3. **imports environment**

   - This environment contains objects from other packages that are explicitly stated requirements for a package to work properly. Most packages published on CRAN are not islands; they build on functionality provided in other packages. The `imports:package` environment contains all objects in the imported packages.

## Imports v Depends

- The `Depends` section *also* lists packages that a package requires.The difference between `Imports` and `Depends` is where the requirement is placed on our map of the world. Because our map specifies the path R takes to find objects, there are consequences to specifying a requirement in `Imports` versus `Depends` in terms of how R finds the dependency.

- If the package is specified in `Imports`, then the package contents will go into the “imports” environment. 

  - For example, In the case of ggplot2, the objects in the `plyr` package will appear in the `imports:ggplot2` environment. Notice also that `plyr` does not have a package environment. Its nicely tucked away inside the environment `imports:ggplot2`. 

    ![imports-depends-plyr](https://raw.githubusercontent.com/askming/picgo/master/imports-depends-plyr.png)

    

- If a package is specified in `Depends` (i.e. `reshape` package), then the package is loaded as it would be if you called `library()` or `require()` from the R prompt. That is, the package, namespace, and imports environments are created for the dependency and placed on our map. The reshape package is attached *before* `ggplot2` and the `package:reshape` environment becomes `package:ggplot2`’s enclosing environment.

  ![imports-depends-reshape](https://raw.githubusercontent.com/askming/picgo/master/imports-depends-reshape.png)

  

- Is the choice between Depends and Imports arbitrary? Its not. The `library()` command (or generally attaching a library) places the package environment under `R_Global`. More precisely, the package environment becomes `R_Global`’s enclosing environment. `R_Global`’s old enclosure now encloses the package environment. 

  ![reshape-reshape2-cast-conflict](https://raw.githubusercontent.com/askming/picgo/master/reshape-reshape2-cast-conflict.png)

  

  - Both `reshape` and `reshape2` contain the function `cast`. Lets say (I’m making this up) that `ggplot2` has a function called `FunctionThatCallsCast()`. As you can guess, this function calls the `cast()` function. Without knowing any details of how R finds stuff, lets just follow the “purple line road”. We travel from to 1 and 2 and find `FunctionThatCallsCast()`. Remember, the package and namespace environments both reference a package’s public-facing functions. We execute that function and now we need to find `cast`. We travel from 3 to 5 searching for `cast`. We find `cast` at 6 and stop. But this is the *wrong* `cast`. This is `cast` in package `reshape2`, but `ggplot` depends on the `cast` in `reshape`. This could have dire consequences depending on the differences between `cast` in `reshape` and `reshape2`.
  - The better solution would have been to stuff `reshape`’s `cast()` function into `imports:ggplot2` using the `Imports` feature. In that case, we would have travelled from 2 to 3 and stopped. Now you can see why the choice between `Imports` and `Depends` is not arbitrary. With so many packages on CRAN and so many of us working in related disciplines its no surprise that same-named functions appear in multiple packages. `Depends` is less safe. `Depends` makes a package vulnerable to whatever other packages are loaded by the user.

## namespace:base

- All `imports:<name>` environments have `namespace:base` as their enclosure. Think of this a freebie for creating a package. Since the base functions are used frequently, they are most likely a dependency for any package (or a package’s imports). Without `namespace:base` where it is, R would have to go hunting quite far to find `package:base`. There’s a big risk that another package has a function of the same name as a base function.
- A package author cannot know a-prior when you intend to attach her package nor that you have decided to write your own version of a base function. So do as you like, a package author can expect that R will find the base functions immediately after `Imports`. There’s no chance of corruption.

## The Curveball (the dotted purple lines)

- Functions, like all objects, are housed inside environments. However, functions themselves have a property which is a pointer to the environment in which they should run. When you create a function, that property is automatically set to the environment in which the function was created. So the environment that houses a function and the environment that the function will run in is one and the same.

  ![environment-property](https://raw.githubusercontent.com/askming/picgo/master/environment-property.png)

  

- *"The environment that a function will run in"*: executing a function creates a new environment specifically for that function and the enclosing environment function’s new environment is what is specified by the function’s environment property. This is “the environment that a function will run in.” We can get a function object’s environment property using the `environment()` function.

  ```R
  > MyFunction = function() {} 
  > environment( MyFunction ) 
  <environment: R_GlobalEnv>
  ```

- And when we run `MyFunction()` and R is executing lines of codes inside that function, the environments look like this:

  ![environment-execution](https://raw.githubusercontent.com/askming/picgo/master/environment-execution.png)

  

- By default, R sets a function’s environment property equal to the environment where the function was created (the environment that owns the function). However, its not necessary that a function’s executing environment and the environment that owns the function are one and the same. In fact, we can change the environment to our liking:

  ```R
  # notice how environment(MyFunction) no longer returns R_GlobalEnv
  > MyFunction = function() { } 
  > newEnvironment = new.env() 
  > environment( MyFunction ) = newEnvironment 
  > environment( MyFunction ) 
  <environment: 0x000000000e895628>
  ```

- Another way to see a function's environment property is to just print the function. The environment will appear at the bottom of the printed function.

- How R searches for variables in a function call: find the "cloest" one to use first if not otherwise specifically specified for its enclosing environment

  ```R
  > age = 32 
  > MyFunction = function() 
  + { 
  +     age = 22 
  +     FromLocal = function() { print( age + 1 ) } 
  +     FromGlobal = function() { print( age + 1 ) } 
  +     NoSearch =  function() { age = 11; print( age + 1 ) } 
  +     environment( FromGlobal ) = .GlobalEnv 
  +     FromLocal() 
  +     FromGlobal() 
  +     NoSearch() 
  + } 
  > MyFunction() 
  [1] 23 
  [1] 33 
  [1] 12
  ```

- This explains the dotted purple lines in our map. If you inspect the environment property of the functions within the `package:<name>` environments you’ll see that they all point to the `namespace:<name> environment`. So in essence, the package environment is just a pass-thru to the namespace environment. 

  ```R
  > statsPackageEnv = as.environment( "package:stats" ) 
  > sdFunc = get( "sd" , envir = statsPackageEnv ) 
  > environment( sdFunc ) 
  <environment: namespace:stats> 
  > statsNamespaceEnv = environment( sdFunc ) 
  > sdFunc2 = get( "sd" , envir = statsNamespaceEnv ) 
  > environment( sdFunc2 ) 
  <environment: namespace:stats>
  
  # An easier way to get a namespace environment
  > statsNamespaceEnv = asNamespace( "stats" ) 
  > statsNamespaceEnv 
  <environment: namespace:stats>
  ```

- The package environment says “I don’t know what to do, ask my functions”. And when we ask the functions they all say “when you execute us create a new environment whose enclosure is the namespace environment.” More precisely, the functions are just offering up their environment property . We might as well make those dotted lines solid:

  ![map-of-the-world-complete](https://raw.githubusercontent.com/askming/picgo/master/JP2bBA_2021_07_07.jpg)

  

- Incidentally, this is another explanation for why there’s no easy way to query an object for the environment that owns it. When we are executing in an environment, we are interested in the objects it owns because we might be looking for one of them. When we find a function we need to know which environment to execute it within. But its not important in our workflow to identify an arbitrary object’s owning environment.

## Passing Functions

-  If a function `FunctionA( someOtherFunction )` takes another function `someOtherFunction` as a parameter then `FunctionA` must have some variability in the way it runs. That variability is governed by the implementation of `someOtherFunction`. When we construct `someOtherFunction`, we expect it to run in a particular way. `someOtherFunction` should have access to the objects in the environment in which it was constructed. That expectation doesn’t change when the function is handed-off to `FunctionA` . But R creates a new environment for `FunctionA`. Thankfully that’s not a problem. When `someOtherFunction` is finally run R looks to the function’s environment property and executes within *that* environment, not within `FunctionA`’s environment. So the integrity of our expectation is upheld. In fact, `FunctionA` can pass `someOtherFunction` to `FunctionB` which in turn can pass the function to `FunctionC` and it has no consequence on how `someOtherFunction` will run. That’s the magic of a function’s environment property.

## That Creepy Caller

- The call stack is the sequence of function calls that has gotten you to wherever you currently are in the calculation. For example, `FunctionA` calls `FunctionB` which in turn calls `FunctionC`. The call stack just places each of those functions on top of one another in the order in which they were called. Lets say `FunctionC` needs to execute `FunctionD`. The *wrong* way to think about the search mechanism is to follow the callers.
- Unfortunately, the call stack is more intuitive than the chain of enclosing environments. Just remember, whenever R is evaluating a statement the system is simultaneously at the top (or bottom if it’s easier to visualize that way) of *two* important chains of environments.
  - One is the **chain of enclosing environments** which is involved in the task of scoping (i.e. where to look next for variable names not found in the frame of the current environment). *This is the chain we care about.* 
  - The other **chain is the call stack**, which is produced by the sequence of function calls. *You can ignore this chain*.
  - There *are* scenarios where it’s necessary to look for a variable via the call stack, but to accomplish that you have to use some special functions in R.
- **A word of caution:** R (and some R literature) uses the term “parent” in context of both chains. There’s the function `parent.env()` which we already know and `parent.frame()` which is used to interrogate the call stack. This is certainly confusing and its a historic slip-up. The term “parent” should *not* be used as a substitute for enclosing environments. It should only be used with the call stack.

## Finally, How R searches and finds stuff

- **R just follows the purple line road** in our map above.
- If `ggplot` is not in the global environment, then it must be in a package. So R travels down the search list looking for `ggplot`. This is simply the chain of enclosing environments starting with `R_Global`. R ultimately find the function in one of the package environments. Although `ggplot` is found within the `package` environment, R executes `ggplot` within the `namespace` environment as described in the prior section. In this case, we’ve found `ggplot` in `package:ggplot2` and we execute the function within `namespace:ggplot2`.
- Lets say `ggplot` calls another function `MyFunction`. A few things can happen:
  1. If `MyFunction` is defined within `ggplot`, then we find it immediately since R checks the local environment first. In this case the local environment is the environment created to run `ggplot`
  2. If not found, then R looks to the enclosing environment of `MyFunction`’s executing environment which is `namespace:ggplot2`. If we find `MyFunction` here, then it’s a case of a package function calling another function in the same package.
  3. If `MyFunction` is not in the `namespace:ggplot2`, then R checks the enclosing environment of the namespace environment which is the **imports environment**. This gives `ggplot` an opportunity to find `MyFunction` within a set of explicitly defined package dependencies. This is like ggplot finding a plyr function in our example above.
  4. If `MyFunction` is not in the imports environment, then we check the enclosing environment of the imports environment which is `namespace:base`. A base function (i.e. sd() for standard deviation) would be found here and the search would be complete.
  5. If `MyFunction` is not found in `namespace:base`, then we are back to the search list. We start by checking `R_GlobalEnv`. Its unlikely that `MyFunction` is in `R_GlobalEnv`. It would be poor practice for a package to expect the user to define some function in the global environment. However, the user could take this as an opportunity to intercept the search by defining her own version of `MyFunction` in the global environment.
  6. If `MyFunction` is within a package that’s a dependency of `ggplot2` and that dependency is specified in` Depends` rather than `Imports`, then the search list is where we would find `MyFunction`.

## Qualitative Comments

- If we are executing outside of a package (as in `R_GlobalEnv`) it enables us to find functions inside packages. If we are inside a package it allows the package functions to find the specified dependencies. If we are inside a package or a package’s imports (dependencies), then we have a buffer of base functions before we plunging into the search list. Also, the design ensures that we terminate at `R_EmptyEnv` if a named object cannot be found, no matter where on the map we are.

## Skip the search-and-find

- f you know exactly which package contains the object desired then you can reference it directly using the `::` operator. Simply place the package name before the operator and the name of the object after the operator to retrieve it.

- If the object is not exported or you are unsure, then you can use the `:::` operator (notice the extra colon).

  - This operator searches the namespace environment for the given object (as we discussed, non-exported objects do not appear in the package environment, only in the namespace environment). 

    ```R
    # view the ::: operator function
    > `:::` 
    function (pkg, name) 
    { 
        pkg <- as.character(substitute(pkg)) 
        name <- as.character(substitute(name)) 
        get(name, envir = asNamespace(pkg), inherits = FALSE) 
    } 
    <bytecode: 00000000073BAEA8>
    <environment: namespace:base>
    ```

    

[^1]: The latter, the “hidden” objects (they are not really hidden, you can access them if you’d like) facilitate the “visible” ones. 

