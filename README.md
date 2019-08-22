# .Rprofile
Rprofile file and other default settings for R

R has features to allow you to customize the start-up protocols. These are often used to automatically load packages or custom functions that you use regularly, or to change default settings. These settings can be saved in a .Rprofile file that is saved in your R directory.

For some examples, check out:

 https://www.gettinggeneticsdone.com/2013/07/customize-rprofile.html
 
 https://www.datascienceriot.com/r/pimp-rprofile/
 
 https://stackoverflow.com/questions/1189759/expert-r-users-whats-in-your-rprofile
 
 https://csgillespie.github.io/efficientR/3-3-r-startup.html#r-startup
 
  

# Overview R CRAN
By default R looks for and runs .Rprofile files in the three locations described above, in a specific order. .Rprofile files are simply R scripts that run each time R runs and they can be found within R_HOME, HOME and the project’s home directory, found with the R command :
```r
getwd()

if(!file.exists("~/.Rprofile")) # only create if not already there
  file.create("~/.Rprofile")    # (don't overwrite it)
file.edit("~/.Rprofile")

```



# Overview RStudio-Desktop
RStudio Desktop stores your custom settings and options in a hidden directory called RStudio-Desktop. 

## Accessing the RStudio-Desktop Directory
### Windows
You can open an Explorer window into the RStudio-Desktop directory by typing the following command into Start -> Run:

For Windows 7, 8 and 10:

```r
%localappdata%\RStudio-Desktop
```

You can then rename this directory to backup-rstudio-desktop and send it with your support request.

## Resetting Other Preferences
### Windows
You can open an Explorer window into the RStudio preferences directory by typing the following command into Start -> Run:

For Windows 7, 8 and 10:

```r
%appdata%\RStudio
```

You can then rename this directory to backup-rstudio and send it with your support request.
