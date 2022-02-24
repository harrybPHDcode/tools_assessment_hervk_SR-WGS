# tools_assessmnent_hervk_SR-WGS

This repo contains all of the code and supplementary data used for the paper: An assessment of bioinformatics tools for the detection of human endogenous retroviral insertions in short-read genome sequencing data. 

The supplementary files contain HERV loci used for comparison against results given by each of the tools when applied to 50 SR-WGS samples. S1 = 40 polymorphic HERV-Ks, S2 = UCSC Repeatmasker HERV-KS, S3 = UCSC Repeatmasker all ERV/LTR elements.

The simulation file contains the bash script for generating simulated genome data with 19 known, non-reference HERV-K (HML-6) insertions. This script requires a hg19 reference genome and a bed file containing locations of known HERV-K integration sites. It might be possible to use the hg38 reference if the HERV-K loci are lifted over to hg38. 
The LTR3B and LTR3A files contain the locations in bed format, together they give the total HML-6 loci file.


