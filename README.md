# tools_assessmnent_hervk_SR-WGS

This repo contains all of the code and supplementary data used for the paper: An assessment of bioinformatics tools for the detection of human endogenous retroviral insertions in short-read genome sequencing data. 

### supplementary file 1
This file contains 40 polymorphic HERV-K loci from Kahyo et al. https://bmcgenomics.biomedcentral.com/articles/10.1186/s12864-017-3872-6#Sec13 
Kahyo, T., Yamada, H., Tao, H., Kurabe, N. and Sugimura, H., 2017. Insertionally polymorphic sites of human endogenous retrovirus-K (HML-2) with long target site duplications. BMC genomics, 18(1), pp.1-8.

### supplementary file 2
This file contains reference HERV-K loci obtained from the UCSC table browser (repeatmasker track, hg19 reference). The filter terms used were: LTR5_Hs, LTR5A, LTR5B, HERV-K and HERV-K-int.

### supplementary file 3
This file contains transposable element loci (all subfamilies) which are known to contain an LTR, this was obtained using the UCSC table browser (repeatmasker track, hg19). The filter terms used were: ERV, LTR. 

### supplementary table 1
This table reports the genomic loci of the HERV-Ks detected by each tool. We report the frequency of exonic and intronic HERV-K integrations as well as the proportion of HERV-Ks located 10kb upstream of a gene. 

### supplementary_table_2
For each tool, this table shows the frequencies of each HERV-K found in a sample of 50 short read whole genome sequences.

### Simulation
The simulation_script file contains the bash script for generating simulated genome data with 19 known, non-reference HERV-K (HML-6) insertions. This script requires a hg19 reference genome and a bed file containing locations of known HERV-K integration sites. 
It might be possible to use the hg38 reference if the HERV-K loci are lifted over to hg38. 
The ####LTR3B_insertions and ####LTR3A_insertions_new files contain the locations in bed format, together they give the total HML-6 loci file.
The simulated_results_analysis is a Python script that checks the results of given tool (applied to the simulated data) against the known HERV-K locations allowing computation of sensitivity and specificity.

### Long read analysis_1 
This bash script extracts reads at predicted HERV-K loci from long read WGS data. It then merges the reads into contigs before applying repeatmasker to confirm the presence of target LTR/HERV-K elements at predicted loci. Repeatmasker can be installed following the instructions below.   

### Long read analysis_2
This python script takes long read repeatmasker results and searches each locus for LTR5_Hs (or general LTR elements) of certain lengths. The input to this script is a directory containing repeatmasker output for each long read genome from a given tool. An example filename might be 10:110876847-110891982.HG007.melt which contains the following file format (view raw):

 bit   perc perc perc  query     position in query     matching  repeat            position in repeat
score   div. del. ins.  sequence  begin end    (left)   repeat    class/family    begin  end    (left)  ID

   18   13.4  4.8  0.0  Contig1    1000  1251 (30095) C L1DA09    LINE/L1            (10)   6064   6210   1  
... 

### du time test
This bash script contains a single line of code which monitors the size of a given directory over time. Below this line of code should be the script to run a HERV-K detection tool. The directory to be monitored should be the temporary/output directory used by the tool. 

### comparison previous HERV-K
This python script takes in results from a given tool and compares the predicted HERV-K locus to the loci given in the three supplementary files. ERVcaller is used as an example but the script can be used for any tools results with very little adaptation (no changes are needed in most cases). The approach is the same in all cases, first, store the results in a dictionary (key = locus, value = frequency) and merge the keys such that loci very close to each other are grouped as a single HERV-K insertion. After merging, each key of the dictionary can be compared to the list of known HERV-K insertions to see if they fall within a few hundred base pairs of the known loci (which counts as a match). 

### Ervcaller, MELT, Retroseq, STEAK, retroseq+, Mobster
These scripts run each of these five tools on a given input bam (or sam) file. They each require a reference genome (e.g. hg19), an input SR-WGS file and an LTR target fasta which contains the sequence of the target of interest (e.g. the LTR5_Hs sequence from DFAM for the HML-2 detection). The retroseq results are given as an intermediate result from the retroseq+ script. Each tool may also have their own specific input files such as a TE database, or Mobster's 'properties' file, all of these extra files can be found 
at the tools' respective github pages/pubications:
Ervcaller: https://github.com/xunchen85/ERVcaller  
MELT: https://melt.igs.umaryland.edu/#:~:text=The%20Mobile%20Element%20Locator%20Tool,genome%20sequencing%20(WGS)%20data  
Retroseq: https://github.com/tk2/RetroSeq  
STEAK:https://github.com/applevir/STEAK  
Mobster: https://jyhehir.github.io/mobster/index.html  
The tool dependencies, and installation instructions, can be found on their resepctive github pages/publications with the exception of retroseq+. As well as the retroseq dependencies, retroseq+ requires repeatmasker and CAP3. CAP3 was installed via conda, repeatmasker (which is also needed for long read analysis) was installed as follows:

1) install HMMER: 

Download hmmer-3.1b2.tar.gz from http://hmmer.org/; unpack it: 

wget ftp://selab.janelia.org/pub/software/hmmer3/3.1b2/hmmer-3.1b2.tar.gz  
tar xf hmmer-3.1b2.tar.gz 
cd hmmer-3.1b1  

If you have downloaded a variant ‘with binaries’, the pre-compiled binaries are found in the directory ./binaries. 
You may copy these into a directory of your choosing, or simply run them from here 
for example, by adding this directory to your PATH variable. 
If you have downloaded the HMMER source code, you’ll need to compile the software with configure and make:  
(it is important that you are in the hmmer directory that contains the configure and make executable files) 

./configure  
./make  
./make check  

These instructions are directly copied from http://eddylab.org/software/hmmer3/3.1b2/Userguide.pdf  
Authors: Sean j Eddy, Travis J wheeler et al. 
Date that this method was last successfully used: 01/02/2019 

2) install TRF 
conda install TRF 

3) install Repeatmasker: 

Download a zipped .tar file from http://www.repeatmasker.org/RMDownload.html 
Copy the file into the desired directory and unpack it using the following commands: 

gunzip RepeatMasker-open-4-#-#.tar.gz 

tar xvf RepeatMasker-open-4-#-#.tar 

Optionally, you can include the repbase library of DNA repeat elements, this requires a repbase account that can take a day to be 
authorised, obtained from following site: 

https://www.girinst.org/accountservices/register.php 

From girinst.org, download the repeatmasker specific repbase data set e.g.  RepBaseRepeatMaskerEdition-20181026.tar.gz 
Copy this file into the RepeatMasker directory that was created when you unpacked RepeatMasker in the previous step.  
Now, unpack the repbase library as before.  

gunzip RepBaseRepeatMaskerEdition-########.tar.gz 
tar xvf RepBaseRepeatMaskerEdition-########.tar 
rm RepBaseRepeatMaskerEdition-########.tar 

Finally, Repeatmasker needs to be ‘configured’.  
Go to the RepeatMasker directory and run the configure executable file: 

Perl ./configure 
(This will start a configuration progress where you will be asked to specify the locations of hmmer and perl etc.)

### retroseq+ 2
The output of retroseq+ needs a final post-processing step. This is acheived by the retroseq+ 2 python script. This script takes in the repeatmasker output from the first step and reads through it to find seperate contigs with LTR5_Hs sequence - this follows the description of the pipeline in Wildschutte et al. 
