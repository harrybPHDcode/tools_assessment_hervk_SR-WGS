# tools_assessmnent_hervk_SR-WGS

This repo contains all of the code and supplementary data used for the paper: An assessment of bioinformatics tools for the detection of human endogenous retroviral insertions in short-read genome sequencing data. 

### Supplementary
The supplementary files contain HERV loci used for comparison against results given by each of the tools when applied to 50 SR-WGS samples. S1 = 40 polymorphic HERV-Ks, S2 = UCSC Repeatmasker HERV-KS, S3 = UCSC Repeatmasker all ERV/LTR elements.

### Simulation
The simulation file contains the bash script for generating simulated genome data with 19 known, non-reference HERV-K (HML-6) insertions. This script requires a hg19 reference genome and a bed file containing locations of known HERV-K integration sites. 
It might be possible to use the hg38 reference if the HERV-K loci are lifted over to hg38. 
The LTR3B and LTR3A files contain the locations in bed format, together they give the total HML-6 loci file.

### Long read analysis
This python script takes long read repeatmasker results and searches each locus for LTR5_Hs (or general LTR elements) of certain lengths. The input to this script is a directory containing repeatmasker output for each long read genome from a given tool. An example filename might be 10:110876847-110891982.HG007.melt which contains the following file format (view raw):

 bit   perc perc perc  query     position in query     matching  repeat            position in repeat
score   div. del. ins.  sequence  begin end    (left)   repeat    class/family    begin  end    (left)  ID

   18   13.4  4.8  0.0  Contig1    1000  1251 (30095) C L1DA09    LINE/L1            (10)   6064   6210   1  
... 

### du time test
This bash script contains a single line of code which monitors the size of a given directory over time. Below this line of code should be the script to run a HERV-K detection tool. The directory to be monitored should be the temporary/output directory used by the tool. 

### comparison previous HERV-K
This python script takes in results from a given tool and compares the predicted HERV-K locus to the loci given in the three supplementary files. XXX is used as an example but the script can be used for any tools results with very little adaptation (no changes are needed in most cases). 

### ERVcaller, MELT, Retroseq, STEAK, retroseq+, Mobster
These scripts run each of these five tools on a given input bam (or sam) file. They each require a reference genome (e.g. hg19), an input SR-WGS file and an LTR target fasta which contains the sequence of the target of interest (e.g. the LTR5_Hs sequence from DFAM for the HML-2 detection). The retroseq results are given as an intermediate result from the retroseq+ script. 

### retroseq+ 2
The output of retroseq+ needs a final preprocessing step. This is acheived by the retroseq+ 2 python script. This script takes in the repeatmasker output from the first step and reads through it to find seperate contigs with LTR5_Hs sequence - this follows the description of the pipeline in Wildschutte et al. 
