# NeurodevelopmentalCNVCalling

This pipeline was designed to perform CNV calling and quality control using PennCNV, identify known neurodevelopmental CNVs and output the results in a standardised and easily interpretable format. The pipeline requires you to have working versions of penn cnv and perl. A single output file .txt from Genome Studio is required to run the pipeline. Please see Penn CNV documentation for how to export data in the appropriate format. 

# Pipeline Operations  
 
1) Penn CNV splits up a single txt file from genome studio into individual sub-files and creates a list of ids.
2) CNV calling is performed using the array specific pfb file and gc model file. You will likely need to create this yourself.  
3) CNVs called close together are merged if the the distance between CNVs is less than 50% of their combined length. 
4) QC files and a filtered cnv list is then produced. 
5) QC metrics plots are produced 
6) After removing individuals and CNVs that fail specified QC parameters (LRR standard deviation, wave factor, N CNVs), remaining high quality CNV calls are compared against a list of known neurodevelopmental CNVs (see [Kendall el al, 2019](https://jamanetwork.com/journals/jamapsychiatry/fullarticle/2730725)).
7) Each neurodevelopmental CNV has specific criteria that must be met. This may be overlapping a certain % of the critical region, hitting specific genes / exons or must be a certain size. For CNVs within each ND CNV locus, these criteria are automatically tested. 
8) All CNVs that are found at a ND CNV locus are reported and saved to file, and are flagged for whether they pass or fail on the locus specific criteria.
9) A final html report is generated, with information about the sample, QC plots, ND CNVs called in the sample, and trace plots (B-allele freq, LRR). Trace plots are generated for each ND CNV that passes QC and satisfies the locus specific criteria and must be manually inspected.    

# Required R libraries:

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
  
# Step 2: Modify HAWK.Psychmed.CNVQC.V4.3.Rmd for your own parameters 
Lines 48-60:  
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

LRR_SD.thres=0.2  
NCNV.thres=100  
WF.thres=0.03  


# Run the pipeline.  

The script is written in R Markdown (Rmd). You wil require a working copy of Pandoc (version 2.5 or later). 

These core lines will work on a server based environment. Please ensure that your genome studio output file for CNV calling is within a directory of the same name: e.g Sample1/Sample1.txt, and change this within the sh file provided. The sh and Rmd files must be located within your Sample1/ directory. If you need to rerun the script, for example changing QC parameters, you must delete all files within this directory apart from your initial genome studio text file, Rmd and sh files.  

module load R  
export R_LIBS_USER=/home/YOURUSERNAME/R/x86_64-pc-linux-gnu-library/3.5  
export RSTUDIO_PANDOC="/home/YOURUSERNAME/Software/pandoc-2.5/bin"  
sed s/"NAMEOFDATASET"/NAMEOFDATASET/g HAWK.Psychmed.CNVQC.V4.3.Rmd > HAWK.Psychmed.CNVQC.V4.3.NAMEOFDATASET_CNV.Rmd  
Rscript -e "rmarkdown::render(\"HAWK.Psychmed.CNVQC.V4.3.NAMEOFDATASET_CNV.Rmd\", params = list(DATASET = \"NAMEOFDATASET\"))"  

