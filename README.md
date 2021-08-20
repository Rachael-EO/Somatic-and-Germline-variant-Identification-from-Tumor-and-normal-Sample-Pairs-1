# Identification of somatic and germline variants from tumor and normal sample pairs

## Introduction




![Graphical Abstract](Graphic_Abstract-Genomics-Two-A.png)

This tutorial was reproduced on the Galaxy platform and with Linux Pipeline

# Section one:  `GALAXY WORKFLOW`

<Lets add the galaxy sections here>

 
 
 
 
 
 
 
## Adding genetic and clinical evidence_based annotation : Creating a GEMINI database for variants 
 
The next step after adding functional annotation to the called variants was to add "genetic and clinical based annotations"
The processes of the step will help observe more information on the variants like: the clinical and genetic aspects, prevalence in the population and the frequency of occurency.
Firstly, the output from function annotation was loaded into the GEMINI database so as to create a Gemini database where further annotations can be effectively carried 
out.
The following optional contents of the variant were loaded as well: GERP scores- these are scores gotten from the constrained elements in multiple alignments by 
quantifying the substitution deficits, CAAD scores, [N:B-high CAAD and GERP scores were observed in all pathogenicity components], gene tables, sample genotypes and 
variant INFO fields. 
These loaded variants in the gemini database are then annoatated by GEMINI annotate; this tool adds more explicit information on the output of VarScan that could not be indentified by Gemini load.
 
## Making variant call statistics accessible
 
Hence, we used Gemini annotate to extract three values : Somatic Status(SS), Germline p-value (GPV) and Somatic p-value(SPV) from the info generated by VarScan and added them to the Gemini database.
In order to crosscheck if all information extracted by the GEMINI database are in relation to variants observed in the population, we decided to annote by adding more information from the Single Nucleotide Polymorphism Database(dbSNP), also from Cancer Hotspots, links to CIViC database as well as more information from the Cancer Genome Interpreter (CGI)
 
For dbSNP: the last output from Gemini annotate was annotated with the imported dbSNP using the Gemini annotate tool. This process extrated dbSNP SNP Allele Origin (SAO) and adds it as "rs_ss" column to the existing database.
 
For Cancerhotspots: the last ouput generated from annotating dbSNP information was then annotated using GEMINI annotate tool, using the imported cancer hotspots as annotation source to extract "q-values" of overlapping cancerhotspots and add them as "hs_qvalue" column to the existing database.
 
Links to CIVic: the output of the last Gemini annoate for cancerhotspots was annotated using the imported CIViC bed as annotation source. We extracted 4 elements from this source and again added them as a list of "overlapping_civic_urls" to the existing Gemini database. 
 
For the Cancer Genome Interpreter: the last output with the extracted infomation linking to CIViC was further annotated using the tool Gemini annotate with the imported CGI variants as an annotation source. the information extracted was recorded in the Gemini database as "in_cgidb" being used as the column name.
 
 
 
 
 
 
 
## Adding additional Annotation to the Gene-Centered Report
The aim of including extra annotations to the GEMINI-generated gene report (that is, the output of the last GEMINI query) is to make interpreting the final output easier. While GEMINI-annotate allowed us to add specific columns to the table of the database we created, it does not allow us to include additional annotations into the tabular gene report.
 
By simply using the Join two files tools on Galaxy, this task can be achieved. After which, irrelevant columns are removed by specifying the columns that are needed. Three step wise process are involved here: One, pull the annotations found in Uniprot cancer genes dataset; second, using the output of the last Join operation, annotate the newly formed gene-centered report with the CGI biomarkers datasets; and three, using the output of the second Join operation, add the Gene Summaries dataset. Lastly, run Column arrange by header name to rearrange the fully-annotated gene-centered report and eliminate unspecified columns.

The last output of the Join operation must be select in the “file to arrange” section. The columns to be specified by name are: gene, chrom, synonym, hgnc_id, entrez_id, rvis_pct, is_TS, in_cgi_biomarkers, clinvar_gene_phenotype, gene_civic_url, and description. The result will be a tabular gene rport that is easy to understand and interpret.
 
 


 
 
 
 
# Section Two: `Linux Pipeline`

<Lets add the Linux Section here>
 
 ## Mapped read postprocessing
 After mapping of the sample sequences against the reference genome with the aim of determining the most likey source of the observed sequencing read. A sAM(sequence   alignment/map)format output is generated. The file has a single unified format for storing read alignments to a reference genome.
 
### Conversion of the SAM file to BAM file
A BAM(Binary Alignment/Map) format is an equivalent to sam but its developed for fast processing and indexing. It stores every read base, base quality and uses a single conventional technique for all types of data.
The commands : **samtools view -O BAM SLGFSK35.sam -o SLGFSK55.bam and samtools view -O BAM SLGFSK36.sam -o SLGFSK56.bam was used to produce the BAM file output for subsequent step analysis.**
 
### Sorting and indexing of the BAM file
The produced BAM files were sorted by read name and indexing was done for faster or rapid  retrieval.
The commands: **samtools sort -T temp -O bam -o SLGFSK35.sorted.bam SLGFSK35.bam and samtools sort -T temp -O bam -o SLGFSK36.sorted.bam SLGFSK36.bam** to perform sorting. Indexing of the sorted files was done using the **samtools index  SLGFSK35.sorted.bam  and SLGFSK36.sorted.bam.**  At the end of the every BAM file,  a special end of file (EOF) marker is usually written, the samtools index command also checks for this and produces an error message if its not found.

### Mapped reads filtering
To filter for the mapped reads the command **samtools view -b -F SLGFSK35.bam >SLGFSK35.bam and samtools view -b -F SLGFSK36.bam >SLGFSK36.bam.**
Their were 21200965 mapped reads for  SLGFSK35.bam  and 32643682 mapped reads for SLGFSK36.bam

### Duplicates removal
During library construction sometimes there's introductio of PCR (Polymerase Chain Reaction) duplicates, these duplicates usually can result in false SNPs (Single Nucleotide Polymorphisms), whereby the can manifest themselves as high read depth support. A low number of duplicates (<5%) in good libraries is considered standard.
The commands **samtools rmdup SLGFSK35.sorted.bam  SLGFSK35.rdup and samtools rmdup SLGFSK36.sorted.bam  SLGFSK36.rdup.**  



--- 
##  List of team members according to the environment used:

1. Galaxy Workflow:
- @Rachael 
- @Mercy
- @Orinda
- @Heshica
- @Kauthar
- @VioletNwoke
- @AmaraA
- **@Amarachukwu - Gemini annotate(CGI) and Gemini query**
- @Mallika
- @Olamide ``
- @NadaaHussienn
- @Christabel
- @Marvellous  
 

2. Linux Workflow
- @Praise 
- @Fredrick
- @RuthMoraa
- @Kauthar
- @Gladys
- @Nanje


