# R study notes/tips

## R profile file

- At startup, R will source the **Rprofile.site** file. It will then look for a .**Rprofile** file to source in the following order:

  - **current working directory**: `getwd()`
  -  **user's home director**y: `path.expand('~')`
  - R home directory (where R is installed): `R.home()`

- There are two special functions you can place in these files. `.First( )` will be run at the start of the R session and `.Last( )` will be run at the end of the session.

- To edit the .Rprofile file:

  ```R
  file.edit(file.path("~", ".Rprofile")) # edit .Rprofile in HOME
  file.edit(".Rprofile") # edit project specific .Rprofile   
  ```

  - Can use the following to get a list of directories where .Rprofile may exist

    ```R
    candidates <- c( Sys.getenv("R_PROFILE"),
                     file.path(Sys.getenv("R_HOME"), "etc", "Rprofile.site"),
                     Sys.getenv("R_PROFILE_USER"),
                     file.path(getwd(), ".Rprofile"))
    
    Filter(file.exists, candidates)
    ```

    

- Sample .Rprofile file

  ```R
  # Sample Rprofile.site file
  
  # Things you might want to change
  # options(papersize="a4")
  # options(editor="notepad")
  # options(pager="internal")
  
  # R interactive prompt
  # options(prompt="> ")
  # options(continue="+ ")
  
  # to prefer Compiled HTML
  help options(chmhelp=TRUE)
  # to prefer HTML help
  # options(htmlhelp=TRUE)
  
  # General options
  options(tab.width = 2)
  options(width = 130)
  options(graphics.record=TRUE)
  
  .First <- function(){
   library(pacman)
   p_load(tidyvers, data.table, arsenal, Hmisc, haven)
   cat("\nWelcome at", date(), "\n")
  }
  
  .Last <- function(){
   cat("\nGoodbye at ", date(), "\n")
  }
  ```

- More info about this topic can be found from `?Rprofile` and `?Startup`

## Useful packages/functions

1. [Compute derivatives of simple expressions, symbolically in R](https://stat.ethz.ch/R-manual/R-devel/library/stats/html/deriv.html)

   - Functions: `deriv /D /deriv3`
   - Also see [here](http://www.theresearchkitchen.com/archives/642) about how we can use these functions.

2. [Use C-style String Formatting Commands in R](https://stat.ethz.ch/R-manual/R-devel/library/base/html/sprintf.html)

   - Functions: `sprintf/gettextf`    

     ```R
     sprintf("%.3f", pi)
     sprintf("%s is %f feet tall\n", "Sven", 7.1)
     ```

3. [Create Names for Temporary Files in R](http://127.0.0.1:13234/library/base/html/tempfile.html)

   - Functions: `tempfile` / `tempdir` - assigns a temporary directory

4. `switch{base}`: tests an expression against elements of a list. If the value evaluated from the expression matches item from the list, the corresponding value is returned.

   ```R
   switch(2,"red","green","blue")
   [1] "green"
   
   switch("color", "color" = "red", "shape" = "square", "length" = 5)
   [1] "red"
   ```

5. [`icount{iterators}`:](https://cran.r-project.org/web/packages/iterators/iterators.pdf) Returns an iterator that counts starting from one.

6. use `tools::compactPDF` to resize large pdf file while maintain the quality of the graph. This is dependent on another software for manipulating large PDF file, which is gostscript or qpdf.

## Install R on Linux

1. cd to a directory to save the R install file

2. download R install file from web using wget

3. untar the install file using tar xzvf

4. cd to the newly create folder created in step 3

5. `configure './configure'`

   ​    `./configure --prefix=/home1/02784/username/R3.1.2`

   ​    `./configure --prefix=/work/02784/username/R-3.2.0`

7. type 'make' to install

8. do 'make check' to check installation status

9. export the PATH: export PATH=/thepath_to_bin:${PATH}

10. check:  cd to home directory; vim .bashrc

```bash
source: http://galton.uchicago.edu/~eichler/stat24600/Handouts/Rlinux.html

./configure --prefix=/home/username/tmux1.9
wget http://lib.stat.cmu.edu/R/CRAN/src/base/R-3/R-3.2.0.tar.gz
install.packages("INLA", repos="http://www.math.ntnu.no/inla/R/stable")
Install rjags package in R on TACC
wget https://cran.r-project.org/src/contrib/rjags_4-4.tar.gz
tar xzvf rjags_4-4.tar.gz
```

The configure script has an option `--enable-rpath` for hard-coding the location of the JAGS library into the rjags package. It is not portable, however, so may not work on your Operating System. Also, you do not appear to be passing the configure options correctly. The correct syntax is to put the configure options inside `--configure-args`, e.g.

`export JHOME=/home/h03/hadsx/extremes/JAGS_BUGS/jags`

`R  CMD INSTALL rjags_4-4.tar.gz  --configure-args='--with-jags-include=/work/02784/username/softwares/TACC_JAGS/JAGS-3.4.0/include/JAGS --with-jags-lib=/work/02784/username/softwares/TACC_JAGS/JAGS-3.4.0/lib --enable-rpath'`

`install.packages("rjags", configure.args="--with-jags-include=/work/02784/username/softwares/TACC_JAGS/JAGS-3.4.0/include/JAGS --with-jags-lib=/work/02784/username/softwares/TACC_JAGS/JAGS-3.4.0/lib --with-jags-modules=/work/02784/username/softwares/TACC_JAGS/JAGS-3.4.0/lib/JAGS/modules-3")`

