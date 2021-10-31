<h1 align="center"><img src="https://user-images.githubusercontent.com/88287926/137457473-432503b6-1da7-4abf-93b1-b0f2aeae7a37.jpg" width="400" height="200"></h1>
HackBio Internship provides a platform for young Life Science Research Aspirants to collaborate and harness their potentials effectively in this genomic revolution using Bioinformatics and Computational Biology. The HackBio Internship (Thanos 2.0) 2021 is a Rigorous five week Internship focusing on Scientific Research, Programming, Bioinformatics Tools, Data Analysis, Networking and Social Activities.
 
 ## About The Team
<h2 align="center"> 👨‍🔬💻 TEAM GENOMICS ONE 💻👩‍🔬 </h2>
Genomics is an interdisciplinary field of biology focusing on the structure, function, evolution, mapping, and editing of genomes. It employs recombinant DNA, DNA sequencing methods, and bioinformatics to sequence, assemble, and analyse the structure and function of genomes. A genome is an organism's complete set of DNA, including all of its genes as well as its hierarchical, three-dimensional structural configuration. 

Our Team Genomics One is a diverse community of students, researchers and professionals from all over the globe working together to hone our skills in Bioinformatics and Computational biology. 

<p align="center">
 <br>
 <img align="center" src="https://www.nursingcenter.com/getattachment/d8003813-38ac-4105-a4c7-bd3e4676e748/Your-Health-in-the-Hands-of-Genetics-and-Genomics.aspx"  width="400px">
<br>
	<br>
	</p>
	

## Stage 2 Task 🧐🧐
- Reproduce a bioinformatics tutorial that correlates with our biostack (Genomics) 
- Create a comprehensive markdown of the steps followed 
- Prepare a Project Proposal Obliging the Teams Biostack

<br>

## Project Title: "Exome Sequencing Data Analysis for Diagnosing Genetic Disorders" :computer:
Have a look at the ***New state-of-the-art molecular diagnostic genetic test-"Exome Sequencing"***

There are around 180,000 exons in humans, with a total length of approximately 30 million base pairs (30 Mb). Thus, while accounting for only 1% of the human genome, the exome is thought to include up to 85% of all disease-causing mutations. **Exome sequencing**, as an alternative to whole-genome sequencing in the detection of genetic diseases, is less expensive, yet covers significantly more potential disease-causing variant sites than genotyping arrays. This is especially important in the case of rare genetic conditions, since the causal variations may present in the human population at too low a frequency to be included on genotyping arrays.

Our Team Genomics One will investigate :detective: exome sequencing data from a family trio :family_man_woman_boy: in which the boy child has a rare genetic disorder but both parents, who are consanguineous, are unaffected. Our lab’s goal is to figure out which genetic variant is causing the condition.

<br>

## Workflow ✍️

<p align="center">
   <br>
<img align="center" alt="" src="https://user-images.githubusercontent.com/88287926/139530679-912adc62-bd0b-4451-afdd-31433b7af113.png" width="650">
 <br>
<br>
</p>

### Step 1: Data preparation
Retrieve sequenced reads of father, mother and proband  in fastq format from [Zenodo](https://zenodo.org/record/3054169) and import the datasets into the https://usegalaxy.org/ or https://usegalaxy.eu/

```
- Check if the datatypes were assigned correctly in the format, fastqsanger.gz 
- Rename the datasets and add tags (#father, #mother, #proband) to six of the datasets accordingly. 
- Obtain the ‘hg19’ version of the human chromosome 8 sequence as the reference genome.
- Ensure the data types are specified in fasta format.

```

### Step 2: Quality control
In the FastQ format each read, representing a fragment of the library, is encoded by 4 lines:

Description

Line 1 ->  Always begins with @ followed by the information about the read

Line 2 -> The actual nucleic sequence

Line 3 -> Always begins with a + and contains sometimes the same info in line 1

Line 4 -> Has a string of characters which represent the quality scores associated with each base of the nucleic sequence; must have the same number of characters as line 2

- Run the **FastQC tool** on six of the fastq datasets with the following parameter:
```
    - “Short read data from your current history”: all 6 FASTQ datasets selected with Multiple datasets
    - Then Execute
```
- Use the **MultiQC tool** by setting the following parameter to aggregate the raw FastQC data of all input datasets into one report:

```
     - In “Results”
     - “Which tool was used generate logs?”: FastQC
     - In “FastQC output”
     - “Type of FastQC output?”: Raw data
     - “FastQC output”: all six RawData outputs of FastQC 
     - Then Execute
     - Inspect the webpage output produced by the tool to check if trimming/filtering is necessary before mapping the reads.

```
The quality score for each sequence is a string of characters, one for each base of the nucleic sequence, used to characterize the probability of mis-identification of each base. The score is encoded using the ASCII character table.
According to our quality check no further trimming or filtering is needed.

-All samples show a non-normal GC content distribution as it is a characteristic feature of many exome capture methods.


### Step 3: Adapter Trimming
- Use **Trimmomatic tool** on the all the fastq datasets to trim adapters and keep the settings By-default and Execute.


### Step 4: Read Mapping
- Use the Map with **BWA-MEM tool** to map the reads from the ‘Father’, ‘Mother’, and the ‘Proband’ samples to the reference genome, respectively.
- Set the following parameters:
```
      . “Will you select a reference genome from your history or use a built-in index?”: Use a built-in genome index
      . “Using reference genome”: Human:hg19
      . “Single or Paired-end reads”: Paired
      . “Select first set of reads”: the forward reads (R1) dataset of the father sample
      . “Select second set of reads”: the reverse reads (R2) dataset of the father sample
      . “Set read groups information?”: Set read groups (SAM/BAM specification)
      . “Auto-assign”: No
      . “Read group identifier (ID)”: 000
      . “Auto-assign”: No
      . “Read group sample name (SM)”: father
      . Then Execute
```   

- Perform the read mapping for ‘Mother’ and ‘Proband’ samples with the same parameters mentioned in the previous step with the following changes:

```     
       Mother Sample:
      . “Read group identifier (ID)”: 001
      . “Read group sample name (SM)”: mother
      
       Proband Sample:
      . “Read group identifier (ID)”: 002
      . “Read group sample name (SM)”: proband
      
```
      
### Step 5: Mapped Reads Post-processing

#### ⨀ Filtering Mapped reads
Filter the mapped reads by selecting the tool, **Filter SAM or BAM, output SAM or BAM**, and set the following parameters:

```
  - “SAM or BAM file to filter”: all 3 mapped reads datasets of the family trio, outputs of Map with BWA-MEM tool
  - “Filter on bitwise flag”: yes
  - “Only output alignments with all of these flag bits set”: Do not select anything here!
  - “Skip alignments with any of these flag bits set”:
         ✅ “The read is unmapped”
         ✅ “The mate is unmapped”
  - Then Execute
  - Ensure if three new datasets are produced with one for each of the samples.

```
#### ⨀ Removing duplicate reads
Select **RmDup tool** and set the following parameters:
```
   - “BAM file”: all 3 filtered reads datasets; the outputs of Filter SAM or BAM
   - “Is this paired-end or single end data”: BAM is paired end
   - “Treat as single-end”: No
   - Then Execute
```
Ensure if three more new datasets are produced after this step.


### Step 6: Variant Calling 
- Select the **FreeBayes tool** and set the parameters:
```
     - “Choose the source for the reference genome”:
     - “Run in batch mode?”: Merge output VCFs
     - “BAM dataset(s)”:
     - “Using reference genome”: Human: hg19
     - “Limit variant calling to a set of regions?”: Do not limit
     - “Choose parameter selection level”: 1. Simple diploid calling
     - Then Execute
```
Inspect the VCF outputs produced by FreeBayes.

### Step 7: FreeBayes Post-processing
- Use the **bcftools norm tool** and set the following parameters:
 ```
     - “VCF/BCF Data”: the VCF output of FreeBayes tool
     - “Choose the source for the reference genome”: Use a built-in genome
     - “Reference genome”: Human: hg19
     - “When any REF allele does not match the reference genome base”: Ignore the problem (-w)
     - “Left-align and normalize indels?”: Yes
     - “Perform deduplication for the following types of variant records”: do not deduplicate any records.
     - “~multiallelics”: split multiallelic sites into biallelic records (-)
     - “split the following variant types”: both
     - “output_type”: uncompressed VCF
     - Then Execute
```
Look out for the output listing the total number of variant lines processed, along with the number of splits, realigned, and skipped records.

### Step 8: Variant annotation
- Use the **SnpEff Download** to download genome annotation database  hg19
- Create a PED-formatted pedigree dataset describing the single-family sample trio in the following format:
```
      #family_id    name     paternal_id    maternal_id    sex    phenotype
        FAM         father    0              0              1      1
        FAM         mother    0              0              2      1
        FAM         proband   father         mother         1      2
```
- Use the **SnpEff eff tool** and set the following parameters:
```
    - “Sequence changes (SNPs, MNPs, InDels)”: the output of bcftools norm tool
    - “Input format”: VCF
    - “Output format”: VCF (only if input is VCF)
    - “Genome source”: Locally installed reference genome
    - “Genome”: Homo sapiens: hg19 (or a similarly named option)
    - “Produce Summary Stats”: Yes
    - Then Execute
```
- Use the **SnpSift Variant type tool** and select output of SnpEff and execute.

### Step 9: Generate GEMINI Database
- Use the **GEMINI load tool** and set the following parameters:

```
    - “VCF dataset to be loaded in the GEMINI database”: the output of SnpEff eff tool
    - “The variants in this input are”: annotated with snpEff
    - “This input comes with genotype calls for its samples”: Yes
    
   Sample genotypes were called by Freebayes for us.
    - “Choose a gemini annotation source”: select the latest available annotations snapshot (most likely, there will be only one)
    - “Sample and family information in PED format”: the pedigree file prepared above
    - “Load the following optional content into the database”
        ✅ “GERP scores”
        ✅ “CADD scores”
        ✅“Gene tables”
        ✅“Sample genotypes”
        ✅“variant INFO field”
    
   Leave unchecked the following:
     - “Genotype likelihoods (sample PLs)”
     - “only variants that passed all filters”
     - Then Execute
```

### Step 10: Candidate Variant Detection
- Use the **GEMINI inheritance pattern tool** and set the following parameters:
```
- “GEMINI database”: the GEMINI database of annotated variants; output of GEMINI load tool
- “Your assumption about the inheritance pattern of the phenotype of interest”: Autosomal recessive
        >“Additional constraints on variants”
        >“Additional constraints expressed in SQL syntax”: impact_severity != 'LOW'
        >“Include hits with less convincing inheritance patterns”: No
        >“Report candidates shared by unaffected samples”: No
- “Family-wise criteria for variant selection”: keep default settings
- In “Output - included information”
    .“Set of columns to include in the variant report table”: Custom (report user-specified columns)
    .“Choose columns to include in the report”:
        >“alternative allele frequency (max_aaf_all)”
    .“Additional columns (comma-separated)”: chrom, start, ref, alt, impact, gene, clinvar_sig, clinvar_disease_name, clinvar_gene_phenotype, rs_ids
- Then Execute
```

## References :scroll:
Wolfgang Maier, Bérénice Batut, Torsten Houwaart, Anika Erxleben, Björn Grüning, 2021 Exome sequencing data analysis for diagnosing a genetic disease (Galaxy Training Materials). https://training.galaxyproject.org/training-material/topics/variant-analysis/tutorials/exome-seq/tutorial.html 

Batut et al., 2018 Community-Driven Data Analysis Training for Biology Cell Systems https://doi.org/10.1016%2Fj.cels.2018.05.012



## Contributors 🤝
Get to know our awesome team members and their contributions 👩‍💻👨‍💻

|     Team members  	|      @Slack username        |       Contributions      |
| ------- | --- | --- |

| <h3 align="center"> Sooraj Shivakumar  </h3> |  @Sooraj  | Validation of Complete Pipeline, Worked on Graphic Representation, Prepared the GitHub Repository and Markdown File https://in.linkedin.com/in/sooraj-s-71756510a  | 
| ------- | --- | --- |
| ------- | --- | --- |
| <h3 align="center"> Negar Khalili  </h3> |  @negkhalili |  Performed Complete Pipeline, Contributed for Workflow in Repo And Worked on Project Proposal https://www.linkedin.com/in/negarkhalili/  |
| ------- | --- | --- |
| <h3 align="center"> Esther Opone </h3> |  @thevalueadder |  Performed Complete Pipeline and Worked on Project Proposal  https://www.linkedin.com/in/estheropone/  |
| ------- | --- | --- |
| <h3 align="center"> Margaret Adedayo Opeoluwa  </h3> |  @LadyMarg | Performed Complete Pipeline, Contributed for Workflow in Repo and Worked on Project Proposal https://www.linkedin.com/in/margaret-adedayo-adeogun-b6a9117a/ |
| ------- | --- | --- |
| <h3 align="center"> Habeneheir Hailu Tickheir  </h3> | @HabenTick | Performed Complete Pipeline, Worked on Project Proposal and Contributed for Workflow in Repo www.linkedin.com/in/habeneheir-h-tickheir |
| ------- | --- | --- |
| <h3 align="center"> Namrata  </h3> |  @Namrata  | Performed Complete Pipeline, Contributed for Workflow in Repo and Worked on Project Proposalhttps://www.linkedin.com/in/namrata-sharma-186097174/  | 
| ------- | --- | --- |
| <h3 align="center"> Shalaka More </h3> | @shalaka | Validation of Complete Pipeline and Worked on Project Proposal https://www.linkedin.com/in/shalaka-more-03277913b |
| ------- | --- | --- |
| <h3 align="center"> Samson Folami </h3> | @Samson | Validation of Complete Pipeline and Worked on Project Proposal https://www.linkedin.com/in/samson-folami-54b4b41a1/ |
| ------- | --- | --- |
| <h3 align="center"> Shruti Chowdhury  </h3> | @Shruti2754 | Validation of Complete Pipeline and Worked on Project Proposal www.linkedin.com/in/shruti-chowdhury-8b876921a  | 
| ------- | --- | --- |
| <h3 align="center"> Filani Bisola  </h3> | @bisola |  Project Proposal and Performed Quality Check and Mapping in Pipeline https://www.instagram.com/filani.bisola/  |
| ------- | --- | --- |
| <h3 align="center"> Janefrances Okeke  </h3> | @Janefrances |  Performed Complete Pipeline https://www.linkedin.com/in/janefrances-okeke-547416223  |
| ------- | --- | --- |
| <h3 align="center"> Joy Egbejale </h3> | @joyomonighoo |   |
| ------- | --- | --- |


## Thank you for your Time and Patience :relaxed: 
 <p align="center">
   <br>
    <br>
<img align="center" alt="gif" src="https://github.com/arsentieva/arsentieva/blob/main/code.gif?raw=true" width="500" height="320" />
   <br>
</p>

