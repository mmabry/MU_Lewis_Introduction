# 1. Logging into Lewis

http://docs.rnet.missouri.edu/getting-started

To log into Lewis, type the following in a local terminal:

```bash
ssh <username>@lewis.rnet.missouri.edu
```

# 2. Basic Linux Commands

http://docs.rnet.missouri.edu/Linux/basic-commands

### Basic commands

`--help`	Used with a command to get info on syntax/options. Ex: ls --help

`man`	Gives access to the 'man pages' or 'manual pages'. Ex: man ls

`pwd`	Prints the full pathname of the file or directory

`up arrow`	Hitting the up arrow on your keyboard will display the last run command

`history`	Outputs all previously used commands

`tab complete`	To complete a command without typing the full command hit tab

`control+c`	To kill the foreground process hit control+c

`clear` Refresh screen, also can use control+l

### Files

`cat`	Used to make files, redirect files, and to read the contents of a file

`ls`	Used to list files and directories Ex: ls -l gives you more info, ls -lh makes it human readable size numbers

`cp`	Used to copy

`mv`	Used to move files

`rm`	Used to remove files

`mkdir`	Used to make new directories

`less`	Used to paginate text. Ex: less job_output.txt

# 3. SLURM Overview

http://docs.rnet.missouri.edu/SLURM/slurm

### Example Batch SLURM job

```bash
nano saving_the_world.sh
````

```bash
#! /bin/bash

#SBATCH -p Lewis  # use the Lewis partition
#SBATCH -J saving_the_world  # give the job a custom name
#SBATCH -o results-%j.out  # give the job output a custom name
#SBATCH -t 0-02:00  # two hour time limit

#SBATCH -N 1  # number of nodes (you will most likely never need more than 1 node)
#SBATCH -n 2  # number of cores (AKA tasks)

# Commands here run only on the first core
echo "$(hostname), reporting for duty."

# Commands with srun will run on all cores in the allocation
srun echo "Let's save the world!"
srun hostname
```

```bash
sbatch saving_the_world.sh
```

check out your results file!

### checking job status

`sacct` or `squeue` with flags `-u <username>` or `-j <job id>` 

`scancel` cancels jobs with flags `-u <username>` or `-j <job id>`

# 4. Rstudio on Lewis

http://docs.rnet.missouri.edu/Software/r-studio

#### log out of lewis and log back in using adding `-YC` after `ssh`

  - macOS X - Download and Install XQuartz and then add the -YC flags to your ssh command when connecting to Lewis
  - Windows 10 - MobaXterm will provide X11 support by default

```bash
srun -p Interactive --mem 4G --pty /bin/bash
module load rstudio/rstudio-desktop-1.1.456
rstudio
```

https://www.tidyverse.org/packages/

```r
install.packages("tidyverse")
library(tidyverse)
```

## Iris Dataset Visualization

#### Modified  from : https://www.kaggle.com/leolcling/visualizing-iris-datasets-with-r-ggplot2

#### A. load the iris dataset that is already loaded in R. we can use the `head()` command to look at the first five columns

```r
data("iris")
head(iris)
```

```r
> head(iris)
  Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1          5.1         3.5          1.4         0.2  setosa
2          4.9         3.0          1.4         0.2  setosa
3          4.7         3.2          1.3         0.2  setosa
4          4.6         3.1          1.5         0.2  setosa
5          5.0         3.6          1.4         0.2  setosa
6          5.4         3.9          1.7         0.4  setosa
```

#### B. Using `summary()` we can check the statistic summary of our iris data

```r
summary(iris)
```

```r
> summary(iris)
  Sepal.Length    Sepal.Width     Petal.Length    Petal.Width   
 Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.100  
 1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600   1st Qu.:0.300  
 Median :5.800   Median :3.000   Median :4.350   Median :1.300  
 Mean   :5.843   Mean   :3.057   Mean   :3.758   Mean   :1.199  
 3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.800  
 Max.   :7.900   Max.   :4.400   Max.   :6.900   Max.   :2.500  
       Species  
 setosa    :50  
 versicolor:50  
 virginica :50
 ```
 
#### C. Let's plot our data to see the relation between these features using the `ggplot()` function 
  - First we could check the relation between Sepal length and Speal width 
  - we use `theme_minimal()` here to adjust the appearance of the plots 
  
 ```r
 ggplot(data=iris,aes(x=Sepal.Width, y=Sepal.Length)) + 
    geom_point() + 
    theme_minimal()
 ```
 
#### we could add title, change the xy axis labels by adding `ggtitle("your title here")`, `xlab("label for x axis)` & `ylab("label for y axis)`.

#### D. Now we would like to see this relation by each species. To do this we can pass a additional aesthetic to our ggplot, which marked the species by different colors

```r
ggplot(data=iris,aes(x=Sepal.Width, y=Sepal.Length,color=Species)) + 
    geom_point() + 
    theme_minimal() 
```
