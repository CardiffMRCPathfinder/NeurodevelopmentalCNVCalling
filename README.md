# NeurodevelopmentalCNVCalling

This pipeline was designed to perform CNV calling and quality control using PennCNV, identify known neurodevelopmental CNVs and output the results in a standardised and easily interpretable format.

***Step 1: Download required R libraries:

library("ggplot2")  
library("gridExtra")  
library("tidyverse") # Package version: 1.2.1 
library("ggthemes").  # Package version: 4.0.1. 
library("scales") # Package version 1.0.0.  
library("RColorBrewer") # Package version 1.1-2.   
library("data.table") # Package version 1.12.0
library("rvest") # Package version 0.3.2
library("data.table")
library("bioconductor")
library("limma")

PennCNV and instalation instructions can be found here: https://github.com/WGLab/PennCNV
Please use this resource for generating your own .pfb files
  
***Step 2: Modify the Rmd script for your own parameters (lines 48-56).  

Please change the required parameters to mirror that of your own paths for Penn CNV and perl. 

miscfolder="~/Pathfinder/CNVCallingPipeExtra/" 
DATASET="Dataset" 
PENNCNV="~/Pathfinder/CNVCallingPipeExtra/PennCNV-1.0.5/" 
PERL="~/Pathfinder/CNVCallingPipeExtra/perl-5.14.2/perl" 
outpath="/scratch/username" 
outpath=paste(outpath,"/",DATASET,"/",sep="")  
if(dir.exists("NeuroDevelopmentalPlots")==F){  
dir.create("NeuroDevelopmentalPlots")  
}  




***Step 3: Run the pipeline.  

The script is written in R Markdown (Rmd). You wil require a working copy of Pandoc (version 2.5 or later). 

These core lines will work on a server based environment. Please ensure that your genome studio output file for CNV calling is within a directory of the same name: e.g Sample1/Sample1.txt, and change this within the sh file provided. 

module load R
export R_LIBS_USER=/home/YOURUSERNAME/R/x86_64-pc-linux-gnu-library/3.5
export RSTUDIO_PANDOC="/home/YOURUSERNAME/Software/pandoc-2.5/bin"
sed s/"NAMEOFDATASET"/NAMEOFDATASET/g HAWK.Psychmed.CNVQC.V4.3.Rmd > HAWK.Psychmed.CNVQC.V4.3.NAMEOFDATASET_CNV.Rmd
Rscript -e "rmarkdown::render(\"HAWK.Psychmed.CNVQC.Beta.V4.3.NAMEOFDATASET_CNV.Rmd\", params = list(DATASET = \"NAMEOFDATASET\"))"

