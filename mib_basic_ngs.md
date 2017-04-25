# Introduction to basic High Throughput Sequencing data analysis: a case of Virus Detection

### Table of content
* [Introduction](#introduction)
* [Softwares Required for this Tutorial](#softwares-required-for-this-tutorial)
* [Login to the training server and find the dataset](#login-to-the-training-server-and-find-the-dataset)
* [Quality control](#quality-control)
* [Taxonomic classification with Kaiju](#taxonomic-classification-with-Kaiju)
* [De novo assembly](#de-novo-assembly)
* [Alignment of the contigs to a reference genome](#alignment-of-the-contigs-to-a-reference-genome)
* [Mapping the reads on the viral contigs](#mapping-the-reads-on-the-viral-contigs)


### Introduction

[Astroviruses](http://viralzone.expasy.org/all_by_species/27.html) within porcine hosts show a remarkable diversity and are known to cause both mild and severe gastroenteritis. During outbreaks of suspected viral gastroenteritis in 11 production pig farms in Hungary in 2011, several samples were collected and screened by routine diagnostic approaches for common viral agents of gastroenteritis, including astroviruses. Since the conventional diagnostic assays did not yield conclusive etiological results, samples were subjected to viral metagenomics to discern putative new viral agents. Viral metagenomics aims at characterizing the virome, the complete viral genomic material of a sample. By enrichment of the virome and high-throughput sequencing the genetic material of the viruses is characterized.

Twenty-one (21) intestinal samples were collected during 2011 in Hungary at nine large industrial pig production farms, which were reporting cases on diarrhoea amongst the 1-2 week piglets. Samples were collected from both piglets and weaned pigs. The specimens were initially screen at Agricultural Office Veterinary Diagnostic Directorate, Budapest. If the samples were deemed to originate from disease forms of unknown aetiology, they were sent to the OIE Collaborating Centre for the Biotechnology-based Diagnosis of Infectious Diseases in Veterinary Medicine, SLU, Uppsala, Sweden for further studying the possible aetiology by the application of metagenomics analysis.



On this metagenomic dataset, we will perform:
- quality control
- taxonomic classification of the reads
- de novo assembly of the trimmed reads
- alignment of the obtained contigs to the reference genome of the detected virus of interest
- mapping of all the dataset's reads back to the viral contigs to close the gaps
- functional annotation of the obtained viral genome (at least a few genes)


### Softwares required for this tutorial

The ones that we will use on line:

* [Kaiju](http://kaiju.binf.ku.dk)

The ones that we will use on our server:
* [Sickle](https://github.com/najoshi/sickle)
* [SPAdes](http://bioinf.spbau.ru/spades)
* [Abacas](http://abacas.sourceforge.net)
* [Bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml)

The ones that you install on your machine:
* [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
* [Ugene](http://ugene.net)

### Login to the training server and find the dataset

We will login to a training server called eBioKit by typing the following command in the terminal (replacing the X by the number that we give to you):
```
ssh studentX@77.235.253.122
```
This server will be use for running some tools during all this tutorial.
The Illumina reads files (R1 and R2) are in your home directory:
```
965_S11_L001_R1_001.fastq.gz
965_S11_L001_R2_001.fastq.gz
```


## Quality control

### FastQC
To check the quality of the sequence data we will use a tool called FastQC. With this you can check things like read length distribution, quality distribution across the read length, sequencing artifacts and much more.

FastQC has a graphical interface and can be downloaded and run on a Windows or Linux computer without installation. It is available [here](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/).

So you can copy the data on your laptop by typing this command line from the terminal of your laptop (in your working directory):

`
scp studentX@77.235.253.122:/Users/studentX/965_*.fastq.gz .
`

Then run fastqc from the terminal by typing:

`
fastqc &
`
and you will get the Graphical User Interface (GUI) in which you can open your data files.

However, FastQC is also available as a command line utility on the training server you are using. You can execute the program as follows:

`
fastqc $read1 $read2
`

which will produce both a .zip archive containing all the plots, and a html document for you to look at the result in your browser.

Open the html file with your favourite web browser, and try to interpret them.

Pay special attention to the per base sequence quality and sequence length distribution. Explanations for the various quality modules can be found [here](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/3%20Analysis%20Modules/). Also, have a look at examples of a [good](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html) and a [bad](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html) illumina read set for comparison.

### Sickle
Most modern sequencing technologies produce reads that have deteriorating quality towards the 3'-end and some towards the 5'-end as well. Incorrectly called bases in both regions negatively impact assembles, mapping, and downstream bioinformatics analyses.

We will trim each read individually down to the good quality part to keep the bad part from interfering with downstream applications.

To do so, we will use sickle. Sickle is a tool that uses sliding windows along with quality and length thresholds to determine when quality is sufficiently low to trim the 3'-end of reads and also determines when the quality is sufficiently high enough to trim the 5'-end of reads. It will also discard reads based upon a length threshold.

Sickle has two modes to work with both paired-end and single-end reads: sickle se and sickle pe.

Running sickle by itself will print the help:

`sickle`

Running sickle with either the "se" or "pe" commands will give help specific to those commands. Since we have paired end reads:

`sickle pe`

Set the quality score to 25. This means the trimmer will work its way from both ends of each read, cutting away any bases with a quality score < 25.

```
sickle pe -f input_file1.fastq -r input_file2.fastq -t sanger \
-o trimmed_output_file1.fastq -p trimmed_output_file2.fastq \
-s trimmed_singles_file.fastq -q 30
```
What did the trimming do to the per-base sequence quality, the per sequence quality scores and the sequence length distribution? Run FastQC again to find out.

You copy the trimmed files produced by Sickle on your laptop and gzip them for FastQC and for the next step.

On your laptop, in your working directory:

```
scp studentX@77.235.253.122:/Users/studentX/trimmed*.fastq .
gzip trimmed_file1.fastq
gzip trimmed_file2.fastq
```

## Taxonomic classification with Kaiju

We will run a taxonomic classification of our good quality reads at the [Kaiju server](http://kaiju.binf.ku.dk)

Go to the WebServer tab, fill in the needed information, upload your reads files and select the database to use: NCBI nr Bacteria, Archaea, Viruses

Once the analysis is done, visualize the results with Krona on the Kaiju server.

Which kind of interesting virus do you find?

## De novo assembly

In order to get longer sequences from the detected virus, and hopefully to obtain its complete genome, we will be using the SPAdes assembler to assemble our metagenomic dataset. As SPAdes is resource intensive and we are limited on time the resulting assembly is pre-processed. 

```
spades.py -o SPAdes --meta --only-assembler -k 21,33,55,77,99,127 --pe1-1 Forward_Fasta.fastq --pe1-2 Reverse_Fasta.fastq
```

This will produce a series of outputs. The scaffolds will be in fasta format. You can find the results from the SPAdes assembly here
# Add link to data on server
SPAdes output is organised as follows:
   * contigs.fasta – resulting contigs
   * scaffolds.fasta – resulting scaffolds 
   * assembly_graph.fastg – assembly graph
   * contigs.paths – contigs paths in the assembly graph
   * scaffolds.paths – scaffolds paths in the assembly graph
   * before_rr.fasta – contigs before repeat resolution

   * corrected/ – files from read error correction
   * configs/ – configuration files for read error correction
   * corrected.yaml – internal configuration file
   * Output files with corrected reads

   * params.txt – information about SPAdes parameters in this run
   * spades.log – SPAdes log
   * dataset.info – internal configuration file
   * input_dataset.yaml – internal YAML data set file
   * K<##>/ – directory containing files from the run with K=<##> 
You should look at the contigs.fasta file avilable in the top directory of the assembly, this represents the best contigs from the assembly. 

## Alignment of the contigs to a reference genome

ABACAS is intended to rapidly contiguate (align, order, orientate) , visualize and design primers to close gaps on shotgun assembled contigs based on a reference sequence. It uses MUMmer to find alignment positions and identify syntenies of assembly contigs against the reference. The output is then processed to generate a pseudomolecule taking overlaping contigs and gaps in to account. MUMmer's alignment generating programs, Nucmer and Promer are used followed by the 'delta-filter' utility function. Users could also run tblastx on contigs that are not used to generate the pseudomolecule.

```
abacas -r ref_file.fasta -q query_file.fasta -p nucmer -d -m
```

You will find [the](http://www.ebi.ac.uk/ena/data/view/JQ340310) reference genome to use. Inspect the resulting pseudo-molecule query_referens.fasta.

## Mapping the reads on the viral contigs

After producing a putative pseudo-molecule with abacas orientation of contigs we proceed to re-mapp the trimmed data (from the sickle trimming) using Bowtie2. This enables us to get a rough estimate on how well our sequencing data covers the putative new genome as well as provide other useful statistics. We will use Bowtie2 for mapping the reads back to the obtained viral contigs. Bowtie2 requires a reference genome tu build an index from, we will use the newly created pseudo molecule for that. Additionally, Bowtie2 requires fastq data, of which we have two files that are trimmed.

```
# First build an index to map towards
# Usage: bowtie2-build [options]* <reference_in> <bt2_index_base>
bowtie2-build -a -q astro_psudo.fasta astro_psudo_index

# Then map the trimmed reads towards the index
# Usage: bowtie2 [options]* -x <bt2-idx> {-1 <m1> -2 <m2> | -U <r>} [-S <sam>]
bowtie2 --local -N 1 -x astro_psudo_index -1 data/trimmed_965_S11_L001_R1_001.fastq -2 data/trimmed_965_S11_L001_R2_001.fastq  -S astro_psudo_map.sam
```
Inspect the resulting statistics. How many reads were mapped towards the psudo-molecule?

Download the .sam file and open it in UGENE. Inspect the coverage statistics.


## Genes prediction and functional annotation

We will use [GeneMark](http://exon.gatech.edu/GeneMark/) for predicting the genes from our assembled genome.
