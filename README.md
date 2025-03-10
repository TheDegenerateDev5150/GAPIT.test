
GAPIT [![](https://img.shields.io/badge/Issues-0%2B-brightgreen.svg)](https://github.com/jiabowang/GAPIT/issues)
===========

Genome Association and Prediction Integrated Tools Version III

![The GAPIT logo](man/figures/LOGO_WEB.png "Genome Association and Prediction Integrated Tools logo")


Citation
=====

The DOI of GAPIT GitHub repository is:
DOI: 10.5281/zenodo.7931838

If you use GAPIT and publish your analysis, please report the program version and cite the appropriate article:

  
The citation for GAPIT3 is:    
Wang J., Zhang Z., GAPIT Version 3: Boosting Power and Accuracy for Genomic Association and Prediction, Genomics, Proteomics & Bioinformatics (2021), doi: https://doi.org/10.1016/j.gpb.2021.08.005.

The citation for GAPIT2 is:    
Tang Y., Liu X., Wang J., Li M., Wang Q., et al., 2016 GAPIT Version 2: An Enhanced Integrated Tool for Genomic Association and Prediction. Plant J. 9, https://10.3835/plantgenome2015.11.0120.
  
The citation for GAPIT is:    
Lipka A. E., Tian F., Wang Q., Peiffer J., Li M., et al., 2012 GAPIT: genome association and prediction integrated tool. Bioinformatics 28: 2397–2399, https://doi.org/10.1093/bioinformatics/bts444.

The citation for cBLUP and sBLUP is:    
Wang J., Zhou Z., Zhang Z., Li H., Liu D., et al., 2018 Expanding the BLUP alphabet for genomic prediction adaptable to the genetic architectures of complex traits. Heredity https://doi.org/10.1038/s41437-018-0075-0.

The citation for BLINK is:    
Huang M, Liu X, Zhou Y, Summers RM, Zhang Z. BLINK: A package for the next level of genome-wide association studies with both individuals and markers in the millions. Gigascience. https://doi.org/10.1093/gigascience/giy154.

The citation for Farm-CPU is:    
Liu X., Huang M., Fan B., Buckler E. S., Zhang Z., 2016 Iterative Usage of Fixed and Random Effect Models for Powerful and Efficient Genome-Wide Association Studies. PLoS Genet. 12: e1005767. https://doi.org/10.1371/journal.pgen.1005767.

The citation for SUPER method is:    
Wang Q., Tian F., Pan Y., Buckler E. S., Zhang Z., 2014 A SUPER Powerful Method for Genome Wide Association Study (Y Li, Ed.). PLoS One 9: e107684, https://doi.org/10.1371/journal.pone.0107684.

The citation for P3D is:    
Zhang Z., Ersoz E., Lai C. Q., Todhunter R. J., Tiwari H. K., et al., 2010 Mixed linear model approach adapted for genome-wide association studies. Nat. Genet. 42: 355–360. https://doi.org/10.1038/ng.546.


Authors: 
-----

Jiabo Wang and Zhiwu Zhang

Contact:
-----

wangjiaboyifeng@163.com (Jiabo)

Source:
-----

[User manual](https://github.com/jiabowang/GAPIT/raw/refs/heads/master/Documents/gapit_help_document.pdf)

[Demo Data](https://github.com/jiabowang/GAPIT/raw/refs/heads/master/Documents/GAPIT_Tutorial_Data.zip)

[Source code](https://raw.githubusercontent.com/jiabowang/GAPIT/refs/heads/master/gapit_functions.txt)

Contents:
-----

   
   * [Start](#start)
      * [Installing GAPIT](#installing-gapit)
      * [Data Preparation](#data-preparation)
         * [Phenotype Data](#phenotype-data)
         * [Genotype Data](#genotype-data)
            * [Hapmap Format](#hapmap-format)
            * [Numeric Format](#numeric-format)
   * [Analysis](#analysis)
      * [GWAS](#gwas)
      * [GS](#gs)
   * [Result](#result)
   * [Example](#example)
   * [Interpretation of Parameters](#interpretation) 


Start
======


GAPIT is a package that is run in the R software environment.
R can be freely downloaded from [http://www.r-project.org](http://www.r-project.org).
We also recommend the integrated development environment RStudio which is also freely available at [http://www.rstudio.com](http://www.rstudio.com).


## Installing GAPIT

GAPIT can currently be installed in several ways.

- From source on the internet
- From GitHub
- From an archive


### Installation from source functions at ZZlab or GitHub

GAPIT can be loaded with a single funciton. 


```
R> source("http://zzlab.net/GAPIT/gapit_functions.txt")
```

Or from GitHub function.
```
R> source("https://raw.githubusercontent.com/jiabowang/GAPIT/refs/heads/master/gapit_functions.txt", encoding = "UTF-8")
```

### Installation from GitHub

Installation can also be made from GitHub when the R package `devtools` is available.

```
R> install.packages("devtools")
R> devtools::install_github("jiabowang/GAPIT", force=TRUE)
R> library(GAPIT)

or
R> install.packages("remotes")
R> remotes::install_github("jiabowang/GAPIT")
R> library(GAPIT)


```
### In some case of the BiocManager can not be installed

Installation of same packages such as multtest and biobase can not be intalled from BiocManager. These packages can be downloaded in the Bioconductor website and be installed from local source files.

The website of Bioconductor is here:

```
https://bioconductor.org/packages/3.19/bioc/
```

### Installation from an archive


GAPIT can be installed from an archive such as \*.tar.gz or \*.zip archive.
An archive can be downloaded from the "releases" page.
If you would like the latest version of GAPIT from the GitHub site you may want to clone it and then build it (this may require Rtools on Windows).

```
bash$ git clone git@github.com:jiabowang/GAPIT.git
bash$ R CMD build GAPIT
```

Once an archive has been obtained it can be installed from a shell, similar to as follows.


```
bash$ R CMD INSTALL GAPIT_3.5.0.9000.tar.gz
```

Or similarly from within R.

```
R> install.packages("GAPIT_3.5.0.9000.tar.gz", repos = NULL, type="source")
```


Data Preparation
-------


### Phenotype Data

The user has the option of performing GWAS on multiple phenotypes in GAPIT. This is achieved by including all phenotypes in the text file of phenotypic data. Taxa names should be in the first column of the phenotypic data file and the remaining columns should contain the observed phenotype from each individual. Missing data should be indicated by either “NaN” or “NA”. 

<img width="200" height="300" src="man/figures/phenotype.png">

### Genotype Data

#### Hapmap Format
Hapmap is a commonly used format for storing sequence data where SNP information is stored in the rows and taxa information is stored in the columns. This format allows the SNP information (chromosome and position) and genotype of each taxa to be stored in one file.


#### Numeric Format
GAPIT also accepts the numeric format. Homozygotes are denoted by “0” and “2” and heterozygotes are denoted by “1” in the “GD” file.  Any numeric value between “0” and “2” can represent imputed SNP genotypes. The first row is a header file with SNP names, and the first column is the taxa name.
The “GM” file contains the name and location of each SNP. The first column is the SNP id, the second column is the chromosome, and the third column is the base pair position. As seen in the example, the first row is a header file.


Analysis
======
GWAS
-----

* GLM

The GAPIT uses Least Squares to solve the model.
The GAPIT code for running a GLM is:
      
      myGAPIT_GLM <- GAPIT(
      Y=myY[,c(1,2)],
      GD=myGD,
      GM=myGM,
      model="GLM",
      PCA.total=5,
      file.output=T
      )


* MLM

EMMA method is used in GAPIT, the code of MLM is:

      myGAPIT_MLM <- GAPIT(
      Y=myY[,c(1,2)],
      GD=myGD,
      GM=myGM,
      model="MLM",
      PCA.total=5,
      file.output=T
      )


* CMLM

Compress Mixed Linear Model is published by Zhang in 2010. The code of CMLM is:

      myGAPIT_CMLM <- GAPIT(
      Y=myY[,c(1,2)],
      GD=myGD,
      GM=myGM,
      model="CMLM",
      PCA.total=5,
      file.output=T
      )

* MLMM

Multiple Loci Mixied linear Model is published by Segura in 2012. The code of MLMM in GAPIT is:

      myGAPIT_MLMM <- GAPIT(
      Y=myY[,c(1,2)],
      GD=myGD,
      GM=myGM,
      model="MLMM",
      PCA.total=5,
      file.output=T
      )

* SUPER

Settlement of MLM Under Progressively Exclusive Relation- ship is published by Qishan in 2014. The code of SUPER is:

      myGAPIT_SUPER <- GAPIT(
      Y=myY[,c(1,2)],
      GD=myGD,
      GM=myGM,
      model="SUPER",
      PCA.total=5,
      file.output=T
      )


* Farm-CPU

Fixed and random model Circulating Probability Unification (FarmCPU) is published by Xiaolei in 2016. The code of Farm-CPU in GAPIT is:

      myGAPIT_FarmCPU <- GAPIT(
      Y=myY[,c(1,2)],
      GD=myGD,
      GM=myGM,
      model="FarmCPU",
      PCA.total=5,
      file.output=T
      )



GS
-----

* gBLUP

gBLUP used marker kinship to replace the pedgree relationship matrix. The code is:

      myGAPIT_gBLUP <- GAPIT(
      Y=myY[,c(1,2)],
      GD=myGD,
      GM=myGM,
      model="gBLUP",
      PCA.total=5,
      file.output=T
      )



* cBLUP

cBLUP used group kinship to replace the individual matrix. The code is:

      myGAPIT_cBLUP <- GAPIT(
      Y=myY[,c(1,2)],
      GD=myGD,
      GM=myGM,
      model="cBLUP",
      PCA.total=5,
      file.output=T
      )

* sBLUP

sBLUP used SUPER method to build psedue QTN kinship matrix. The code is:

      myGAPIT_sBLUP <- GAPIT(
      Y=myY[,c(1,2)],
      GD=myGD,
      GM=myGM,
      model="sBLUP",
      PCA.total=5,
      file.output=T
      )

* MABLUP

MABLUP (Markers Assisted BLUP) used significant markers as fixed effect in the mixed linear model after GWAS. The GWAS method could be selected following model="". The buspred is used to predict GEBV after GWAS. The lmpred is used to select linear model or GBLUP method. The code is:

      myGAPIT_MABLUP <- GAPIT(
      Y=myY[,c(1,2)],
      GD=myGD,
      GM=myGM,
      model="BLINK",
      buspred=TRUE,
      lmpred=FALSE,
      PCA.total=5,
      file.output=TRUE
      )

Result
=====

<div align=center><img width="400" height="300" src="man/figures/Phenotype.View.png">

<div align=center><img width="400" height="300" src="man/figures/Genotype.Distance_R_Chro.png">

<div align=center><img width="400" height="200" src="man/figures/Genotype.Frequency.png">

<div align=center><img width="450" height="280" src="man/figures/Genotype.MAF_Heterozosity.png">

<div align=center><img width="450" height="400" src="man/figures/kinship_heatmap.png">

<div align=center><img width="450" height="400" src="man/figures/2D_PCA.png">

<div align=center><img width="450" height="400" src="man/figures/3D_PCA.png">

<div align=center><img width="450" height="380" src="man/figures/PCA_eigenvalue.png">

<div align=center><img width="800" height="350" src="man/figures/Manhattan.png">

<div align=center><img width="450" height="400" src="man/figures/QQ.png">

<div align=center><img width="450" height="290" src="man/figures/Optimum.png">

<div align=center><img width="450" height="400" src="man/figures/ROC.png">

<div align=center><img width="450" height="650" src="man/figures/Figure_S01.Rectangle.Manhattan.Plot.6_methods.png">

<div align=center><img width="450" height="400" src="man/figures/Figure_S02.Circular.Manhattan.Plot.6_methods.png">

<div align=center><img width="450" height="400" src="man/figures/Figure_S03.QQ.Multiple.Plot.6_methods.png">

<div align=center><img width="450" height="400" src="man/figures/Figure_S07.Kin.NJtree.fan.png">

<div align=left>


<div align=left> 


Interactive Plots (links):

- [Interactive.Manhattan](http://www.zzlab.net/GAPIT/material/Figure%20S15.Manhattan%20FarmCPU.V1.html)
- [Interactive.QQ](http://www.zzlab.net/GAPIT/material/Figure%20S21.QQ%20FarmCPU.V1.html)
- [Interactive.3D.PCAs](http://www.zzlab.net/GAPIT/material/Figure%20S09.PCA.html)
- [Interactive.GS](http://www.zzlab.net/GAPIT/material/Figure%20S23.GS.html)


   
Example
=====


```
# loading packages for GAPIT and GAPIT functions
source("http://www.zzlab.net/GAPIT/gapit_functions.txt")
# loading data set
myY=read.table(file="http://zzlab.net/GAPIT/data/mdp_traits.txt", head = TRUE)
myGD=read.table("http://zzlab.net/GAPIT/data/mdp_numeric.txt",head=T)
myGM=read.table("http://zzlab.net/GAPIT/data/mdp_SNP_information.txt",head=T)
#myG=read.table(file="http://zzlab.net/GAPIT/data/mdp_genotype_test.hmp.txt", head = FALSE)
# performing simulation phenotype
set.seed(198521)
mysimulation<-GAPIT(h2=0.7,NQTN=20,GD=myGD,GM=myGM)
myY=mysimulation$Y


myGAPIT <- GAPIT(
  Y=myY[,c(1,2)],
  GD=myGD,
  GM=myGM,
  model=c("GLM","MLM","SUPER","MLMM","FarmCPU","Blink"),# choose model
  #model=c("FarmCPU"),
  PCA.total=3,                                          # set total PCAs
  NJtree.group=4,                                       # set the number of clusting group in Njtree plot
  QTN.position=mysimulation$QTN.position,
  Inter.Plot=TRUE,                                      # perform interactive plot
  Multiple_analysis=TRUE,                               # perform multiple analysis
  PCA.3d=TRUE,                                          # plot 3d interactive PCA
  file.output=T
)
```


   
Interpretation of Parameters
======


```
  Y = NULL, #phenotype
  G = NULL, #hapmap genotype
  GD = NULL, #numeric genotype
  GM = NULL, #genotype map information
  KI = NULL, #kinship
  Z = NULL, #Z matrix for MLM, cMLM, encMLM
  CV = NULL, #corvariance matrix
  Aver.Dis=1000,
  buspred = FALSE, #Prediction option for after GWAS MABLUP
  bin.from = 10000, #SUPER 
  bin.to = 10000, #SUPER
  bin.by = 10000, #SUPER
  cutOff = 0.05, #threshold for significant
  CV.Extragenetic = 0, # the top number of no-inheritance columns in CV
  effectunit = 1, #Simulation phenotype
  file.output = TRUE, #output option
  FDRcut = FALSE, # filter pseudo QTN based on cutOff in blink
  group.from = 1000000,#MLM
  group.to = 1000000,#MLM
  group.by = 50,#MLM
  Geno.View.output = TRUE,#genotype analysis option
  h2 = NULL, #simulation phenotype heritability
  inclosure.from = 10, #SUPER
  inclosure.to = 10, #SUPER
  inclosure.by = 10, #SUPER
  Inter.Plot = FALSE, #Interactive plot option
  Inter.type = c("m","q"), #Interactive plot type for Manhattan and QQ plots
  kinship.cluster = "average", #cMLM
  kinship.group = 'Mean',#cMLM
  kinship.algorithm = "Zhang",#cMLM
  lmpred = FALSE, #option for linear model prediction or MABLUP prediction, that could be set as multiple parameters
  model = "MLM",# model or method in GWAS or GS
  maxOut = 100, # power for top number of markers in the GWAS
  memo = NULL, #label for marking
  Model.selection = FALSE,# optimum number of CV and PCAs
  Multi_iter = FALSE, #Multiple step for FarmCPU and BLink
  Major.allele.zero = FALSE, #convert hapmap file to numeric file, set major marker as 0
  Multiple_analysis = TRUE, #option for multiple Manhattan and QQ plots
  num_regwas = 10,# the max number of  markers in second GWAS
  NQTN = NULL, #Simulation phenotype, number of QTN
  N.sig=NULL, #Random.model, Number of significant markers
  NJtree.group = NULL, #NJtree set number of cluster group
  NJtree.type = c("fan","unrooted"),#NJtree type
  output.numerical = FALSE,# option for output numeric files
  output.hapmap = FALSE, # option for output hapmap files
  QTN.position = NULL, #Simulation phenotype, QTN position in the order of map file
  QTNDist = "normal",
  Random.model = TRUE, #Random.model to calculate PVE
  sangwich.top = NULL, #SUPER
  sangwich.bottom = NULL,#SUPER
  SNP.P3D = TRUE,
  SNP.effect = "Add",
  SNP.impute = "Middle",
  SNP.test = TRUE,
  SNP.MAF = 0,
  SNP.FDR = 1,  
  PCA.total = 0, # PCA number
  PCA.col = NULL, #indicater colors for individuals in PCA plot
  PCA.3d = FALSE, #3D PCA plot option
  PCA.View.output = TRUE, #option for PCA plot
  Phenotype.View= TRUE, # option for phenotype view plot
  ```