# NeurodevelopmentalCNVCalling

This pipeline was designed to perform CNV calling and quality control using PennCNV, identify known neurodevelopmental CNVs and output the results in a standardised and easilly interpretable format.

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
  
***Step 2: Modify the Rmd script for your own parameters.  

Please inspect the venn-diagram to see which metric individuals fail quality control. You may need to be more/less stringent depending on the array.  

***Step 3: Run the pipeline.  

The script is written in R Markdown (Rmd). You wil require 
