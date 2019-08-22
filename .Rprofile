## See http:// ... customize-rprofile.html
## @PTardif 2019-08-22

## Create a new invisible environment for all the functions to go in so it doesn't clutter your workspace.
.env <- new.env()

## Returns a logical vector TRUE for elements of X not in Y
.env$"%nin%" <- function(x, y) !(x %in% y) 

## Returns names(df) in single column, numbered matrix format.
.env$ndf <- function(df) matrix(names(df)) 

## List objects and classes (from @_inundata, mod by ateucher)
.env$lsa <- function() {
    obj_type <- function(x) class(get(x, envir = .GlobalEnv)) # define environment
    foo = data.frame(sapply(ls(envir = .GlobalEnv), obj_type))
    foo$object_name = rownames(foo)
    names(foo)[1] = "class"
    names(foo)[2] = "object"
    return(unrowname(foo))
}
  
## List all functions in a package (also from @_inundata)
.env$lsp <-function(package, all.names = FALSE, pattern) {
  package <- deparse(substitute(package))
  ls(
    pos = paste("package", package, sep = ":"),
    all.names = all.names,
    pattern = pattern
  )
}

## Read data on clipboard.
.env$read.cb <- function(...) {
  ismac <- Sys.info()[1]=="Darwin"
  if (!ismac) read.table(file="clipboard", ...)
  else read.table(pipe("pbpaste"), ...)
}

## De-windowsified path - change \ inside Windows path to / for R compatibility, 
##  then paste into current cursor position - shortkey : shift+Ctrl+v
.env$flip_windows_path  <- function() {
  iswin <- Sys.info()[1]=="Windows"
  if (iswin) {
    win_path <- normalizePath(utils::readClipboard(), winslash = "/")
    # win_path <- stringr::str_replace_all(utils::readClipboard(), "\\\\", "/")
    writeClipboard(win_path)
    context <- rstudioapi::getActiveDocumentContext()
    rstudioapi::insertText(context$selection[[1]]$range, win_path, id = context$id)
  }
}

## Open Finder to the current directory
.env$winopen <- function() {
  system("open .")
  ## suppressWarnings(shell(paste("explorer",  gsub('/', '\\\\', getwd()))))
}
.env$.o <- function() { system("open .") }

## load library in one call  .library(pkgName1, pkgName2, ...)
.env$.library <- function(...){
  # .library is load inside an .RProfile, an .function make it invisible in the list of Global Env. 
  #  - install and load multiple R packages, quoting the names in the list arg 3-dots ... is not require.
  # usage: .library(pkgName1, pkgName2, ...)
  
  # Sanitise user input
  if (length(eval(substitute(alist(...)))) <= 0) {
    return(character(0))
  }
  
  # deparse args ... for quote the unquote args of .library
  ls_pkg <-  sapply( substitute(list(...)), deparse)[-1]
  
  # ipak function: install and load multiple R packages.
  # check to see if packages are installed. Install them if they are not, then load them into the R session.
  ipak <- function(pkg){
    new.pkg <- pkg[!(pkg %in% installed.packages()[, "Package"])]
    if (length(new.pkg)) 
      install.packages(new.pkg, dependencies = TRUE)
    sapply(pkg, base::library, character.only = TRUE)
  }
  
  #load the list ls_pkg
  ipak(ls_pkg)
  
}

## Attach all the variables above
attach(.env)

## .First() run at the start of every R session. 
## Use to load commonly used packages? 
.First <- function() {
  # library(ggplot2) - preload library here
  
  # Append dcf Addins to devtool package
  dcf_addins <- data.frame(
    Name =".RProfile (PTardif) - Flip Windows path",
    Description = 'Convert \"\\\" in clipboard to \"/\", then paste into current cursor position.',
    Binding = "flip_windows_path",
    Interactive = "false")
  dcf_file_dev <- file.path(.libPaths(),"devtools/rstudio/addins.dcf")
  if (file.exists(dcf_file_dev)) {
    dcf_binding <- read.dcf(dcf_file_dev, fields = "Binding")
    if (! "flip_windows_path" %in% dcf_binding) {
      cat(file=dcf_file_dev, append = TRUE, "\n") #separating empty line
      write.dcf(dcf_addins, file = dcf_file_dev, append = TRUE, width = 135)
    }
  }
  
  # Append shortkey for Addins in RStudio
  json_file_key <- file.path(getwd(), ".R/rstudio/keybindings/addins.json")
  struct_file_key <- jsonlite::read_json(json_file_key)
  if (!exists('devtools::flip_windows_path', where = struct_file_key)) {
    if (length(struct_file_key)>0) {
      f_midle <- "    \"devtools::flip_windows_path\" : \"Ctrl+Shift+V\","
    } else {
      f_midle <- "    \"devtools::flip_windows_path\" : \"Ctrl+Shift+V\""
    }
    f_lns <- readr::read_lines(json_file_key)
    readr::write_lines(c(utils::head(f_lns,n = 1), f_midle, utils::tail(f_lns,n = -1)), 
                       json_file_key, sep = "\n", append = F)
  }

  cat("\nSuccessfully loaded .Rprofile of PTardif at", date(), "\n")
}

## .Last() run at the end of the session
.Last <- function() {
  # save command history here?
  cat("\nGoodbye at ", date(), "\n")
}