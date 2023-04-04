# Cisplatin project

## Contribute

This is an update of the cisplatin 2020 project to bring it up to
date with R4.2 and associated packages.

**So** do not install as listed below.
The content here is a placeholder/reminder of things to do.


Once you are setup, before you edit anything, create a new branch!
In RStudio, go to the "Git" tab and click on "New Branch".
Name it something like `bob`, or `gael`.
Please do not commit to the master branch.
We will all work on our own branches, and manage the master branch through pull
requests on Github.

Now for the real work!
So, what goes in all these directories?

- `analysis` --
  This is the starting point of the project.
  This directory contains rmarkdown reports that record data processing steps,
  draft figures, draft tables, thoughts, hypotheses, conclusions.
  The exact structure of these reports is flexible, just be sure to prepend the
  report name with a number larger than any reports that it requires.
  For example, all reports will depend on the data imported in
  `00-import-external-data.Rmd`, so all reports should be prepended with at
  least `01`.
  You can have the analysis update automatically with each file change by
  running `rothsteinlab.2020.cisplatin::view_analysis()`
  
  I do have a few requests:

  - Analyses should not modify the source data.
  - Intermediate results that might be usefull for another analysis should be
    saved as a new file in `data`, but _NOT_ committed to version control!
  - File paths should be written relative to the project directory.
    Please consider using `here::here("anaylsis", "path/to/file")` to construct
    file paths.

- `manuscript` --
  this contains all files related to the manuscript including final figures,
  supplementary tables, and references.
  This directory also contains a `reports` directory that holds reports
  written for this project by previous students.
  You can use
  `rothsteinlab.2020.cisplatin::ref_add_pmids()`
  to add entries to the reference library.
  Be sure to check new entries, and wrap words in the title that should have 
  their case preserved in curly braces (e.g. `{RAD5}`).
  
  Another request:
  
  - When writing prose in markdown, please start sentences on a new line.

- `package` --
  This is an R package
  (`rothsteinlab.2020.cisplatin`) which is used to specify the project's R
  package requirements (in `DESCRIPTION`), and as a means to compartmentalize
  our analysis code for documentation and testing purposes.
  
  If you have a report with a _lot_ of code, please refactor your code into a
  function with the following naming convention
  (name the file after the function):
                  
  - `data_*` if it imports data into the project
  - `analysis_*` if it generates an intermediate dataset
  - `figure_*` if it generates a figure
                  
  This funcion should be placed into `package/R` and documented
  in the package with a roxygen block (see other files here for examples).
  Don't forget to add an `#' @export` roxygen tag to make the function available
  to users, and re-install the package to use this function in your reports!


## Workflow

In shorter terms, the workflow goes something like this
(not to be taken as an entirely linear process):


### Analysis

1. I have an idea for an analysis that will address a **single question**.
2. I add a new rmarkdown file to `analysis`
3. I describe my thought process, motivation or hypothesis.
4. I perform my experiment, or program my analysis, and iterate on this
   report until I have addressed my question.
5. I refactor my code to make it extremely simple---perhaps only a few function
   calls, or even just one!
6. I edit my prose to clarify my thoughts.
7. I share my analysis with the group for review (submit a pull request).


### Manuscript

1. I have a collection of analyses
2. I outline a narrative of results from these analyses
3. I spot a hole in the narrative (return to analysis workflow)
4. I let the group know when I am working on a particular section, so we can
   avoid modifying the same text.
5. I make my changes and submit them to the group for review
   (submit a pull request)
6. At any given time, I may want to work on adding methods, or doing literature
   review for the introduction etc.
7. When the results are essentially complete (save for some pollish), revisit
   abstract, introduction, and discussion to make sure everything maintains the
   same narrative flow.
8. Final pollish will include any manual adjusting of figures
   (e.g. adjust layout, things that are difficult to do programmatically).
   Avoid at all cost doing extensive figure pollish until everything else is
   nice.

Final note: numbers quoted in the text should be pre-computed and included via
rmarkdown (no copy-pasting numbers if at all possible).
These numbers should be included as data in the project R package.
Even a number quoted from another paper, should ideally be recorded in a table
with reference information and accompanying text for context.
This will help us quickly fact-check ourselves, when we ask "where did this 
number come from???."


## Setup

### Short version

1. Install [R 3.6.2](https://cloud.r-project.org),
2. Install [RStudio 1.2.5019](https://rstudio.com/products/rstudio/download/#download)
3. Install [R package development prerequisites](https://support.rstudio.com/hc/en-us/articles/200486498-Package-Development-Prerequisites)
   - **Mac OS X**:  `xcode-select --install`
   - **Windows**: Install [Rtools](https://cran.r-project.org/bin/windows/Rtools/)
   - **Debian/Ubuntu**: `sudo apt-get install build-essential`
4. Clone project `git clone https://github.com/EricEdwardBryant/rothsteinlab-2020-cisplatin.git`
5. Open project in RStudio
6. Run `source("install.R")`
7. Run `source("make.R")`

### Long version

First things first!
Lets make sure we are all using the same software.
I know, the instructions below are long, but it only needs to be done once,
and will keep our work consistent and robust
(maybe not future proof, but certainly future resistant!).
I recommend keeping installers in the `renv/installers` directory of this
project in case you need to revert to these versions in the future.

1. [R 3.6.2](https://cloud.r-project.org)
   (released on 2019-12-12) --
   (_Required_) --
   This is the last patch release of R 3.6 before R 4.0, so it is a good fixed
   version to use for this project.
   (See [here](https://gist.github.com/EricEdwardBryant/6e997a183606354e88df4e16e9c53ab9)
   for instructions to manage multiple R versions on macOS)
2. [RStudio 1.2.5019](https://rstudio.com/products/rstudio/download/#download)
   (released on 2019-10-24) -- 
   (_Recommended_) --
   It is not essential to use this exact version of RStudio, but it is bundled
   with the required version of `pandoc` (2.3.1) and `pandoc-citeproc` (0.14.4).
3. [R package development prerequisites](https://support.rstudio.com/hc/en-us/articles/200486498-Package-Development-Prerequisites)
   (_Required_) --
   - **Mac OS X**:
     Install [Xcode](https://developer.apple.com/xcode/) developer tools
     (in terminal: `xcode-select --install`).
   - **Windows**:
     Install [Rtools](https://cran.r-project.org/bin/windows/Rtools/).
   - **Debian/Ubuntu**:
     Install the build-essential package
     (in shell: `sudo apt-get install build-essential`).

Ok, now your system is looking good, you have everything you need to run R
and develop R packages like a pro!
Let's get you set up with this project!

1. Clone the project from Github with:
   `git clone https://github.com/EricEdwardBryant/rothsteinlab-2020-cisplatin.git`
2. Open the project in RStudio.
   The [renv](https://rstudio.github.io/renv/articles/renv.html)
   package will be installed if missing.
   The console should have a message along the lines of
   `* Project ... loaded. [renv 0.9.2]`
3. Install this project's R package dependencies using the install script:

   ```r
   source("install.R")
   ```
   
   You will be shown the list of packages to be installed and prompted to 
   consent to their installation (type `y` press `enter` to consent).
   Packages will be downloaded and installed.
   The `renv` package does a great job of saving time and space on downloads
   and installation by using a global package cache and taking advantage of
   any previously installed packages in your default libraries.
   If you already have the proper versions installed they will be copied over
   into the `renv` cache.
   If the proper versions are already in the cache, then they will just be
   linked in `renv/library`.
   This project is configured to use packages from `2019-12-12` and
   Bioconductor `3.10` (see `getOption("repos")`).
4. Build the manuscript.

   ```r
   source("make.R")
   ```

Let me know if you saw any messages or errors!

### But what about pandoc? :man_facepalming:

The `rmarkdown` package searches for pandoc versions and uses the most recent
version available
(either the system install, or a version bundled with RStudio).
This can cause problems if you upgrade RStudio, and a new version of pandoc
gets used that has breaking changes.
**Before you upgrade RStudio, you may want to configure this project to use
project specific binaries for bundled software or other command-line programs.**

This is my current solution:

1. Copy the RStudio binaries into `renv/bin`.
   For convenience, you can run `rothsteinlab.2020.cisplatin::pandoc_copy()`
2. Create a `.Rprofile.user` text file and add the following code:

   ```r
   # Add ./renv/bin to PATH, and override RStudio pandoc
   local({
     bin <- normalizePath("renv/bin")
     Sys.setenv(
       PATH = paste(bin, Sys.getenv("PATH"), sep = ":"),
       RSTUDIO_PANDOC = bin
     )
   }) 
   ```
   
   On windows, theoretically (I can't test this), this should work if you change
   sep to `";"` (at least according to
   [this](https://stackoverflow.com/questions/24622725/how-to-set-path-on-windows-through-r-shell-command)).
3. Restart R to enable `.Rprofile.user`
4. You can check that these environment variables are configured as expected 
   with e.g. `Sys.getenv("PATH")`
4. You can check if rmarkdown is able to find the right versions of pandoc by
   running `rothsteinlab.2020.cisplatin::pandoc_find()`.





