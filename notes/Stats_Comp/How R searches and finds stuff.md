# (How R searches and finds stuff)[https://blog.obeautifulcode.com/R/How-R-Searches-And-Finds-Stuff/]
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

   - This is where *all* objects in a package go. This includes objects the package author wants you see. It also includes objects that are not meant to be accessed by the end-user.[^1]

3. **imports environment**



[^1]: The latter, the “hidden” objects (they are not really hidden, you can access them if you’d like) facilitate the “visible” ones. 