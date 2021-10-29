# TEAM-GENOMICS-ONE
##STEP 4- Mapped reads post processing 
   Important process to get an accurate picture of the variant spectrum found in the dataset and consists of two steps:
**A.Filtering on mapped reads properties**
  To produce new filtered BAM datasets with only mapped reads the mate of which is also mapped:
   Use "Filter SAM or BAM, output SAM or BAM tool" with the following parameters (leaving non-mentioned ones at their defaults):
   o “SAM or BAM file to filter”: all 3 mapped reads datasets of the family trio, outputs of "Map with BWA-MEM tool"
   o “Filter on bitwise flag”: yes
   o “Only output alignments with all of these flag bits set”: Do not select anything here!
   o “Skip alignments with any of these flag bits set”:
       Parameter-check “The read is unmapped”
       Parameter-check “The mate is unmapped”
   This step will produce three filtered bam files of mapped reads for each dataset(mother, father and child).
**B.Removing duplicate reads** 
  To remove duplicated reads formed due to PCR overamplification in the sequencing library preparation process:
  Use "RmDup tool" with the following parameters:
  o “BAM file”: all 3 filtered reads datasets; the outputs of "Filter SAM or BAM, output SAM or BAM tool"
  o “Is this paired-end or single end data”: BAM is paired-end
  o “Treat as single-end”: No
  The above step will again result in three deduplicated dataset, one for each member of the family trio.
