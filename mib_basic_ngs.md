# Introduction to basic High Throughput Sequencing data analysis: a case of Virus Detection

### Table of content
* [Introduction](#introduction)
* [Softwares Required for this Tutorial](#softwares-required-for-this-tutorial)
* [Downloading the Dataset](#downloading-the-dataset)
* [Quality control](#quality-control)
* [De novo assembly](#de-novo-assembly)
* [Alignment of the contigs to a reference genome](#alignment-of-the-contigs-to-a-reference-genome)


### Introduction

We will proceed in several steps to analyse an Illumina MiSeq dataset, sequenced from Pig samples.
# -> Oskar add info here

On this metagenomic dataset, we will perform:
- quality control
- taxonomic classification of the reads
# -> do we do tax class before assembly or not?
- de novo assembly of the trimmed reads
- alignment of the obtained contigs to the reference genome of the detected virus of interest
- mapping of all the dataset's reads back to the viral contigs to close the gaps
- functional annotation of the obtained viral genome (at least a few genes)


### Softwares required for this tutorial

The ones that we will use on line:

Taxonomic classification:
* [Kaiju]()


The ones that we will use on our server:

Quality control:
* [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
* [Sickle]()

Assembly:
* [SPAdes]()

Alignment / Mapping:
* [Abacas]()
* [Bowtie2]()

The one that you install on your machine:
* [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)

### Downloading the dataset

We will login to a training server called eBioKit by typing the following command in the terminal (replacing the X by the number that we give to you):
```
ssh studentX@77.235.253.122
```
This server will be use for running some tools during all this tutorial.

# TODO: can we copy data from Github here?

## Quality control

### FastQC
To check the quality of the sequence data we will use a tool called FastQC. With this you can check things like read length distribution, quality distribution across the read length, sequencing artifacts and much more.

FastQC has a graphical interface and can be downloaded and run on a Windows or Linux computer without installation. It is available [here](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/).

However, FastQC is also available as a command line utility on the training server you are using. You can load the module and execute the program as follows:

```
fastqc $read1 $read2
```

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
-s trimmed_singles_file.fastq -q 25
```
What did the trimming do to the per-base sequence quality, the per sequence quality scores and the sequence length distribution? Run FastQC again to find out.

## Taxonomic classification with Kaiju

We will run a taxonomic classification of our good quality reads at the [Kaiju server](http://kaiju.binf.ku.dk)

Go to the WebServer tab, fill in the needed information, upload your reads files and select the database to use: NCBI nr Bacteria, Archaea, Viruses

Once the analysis is done, visualize the results with Krona on the Kaiju server.

Which kind of interesting virus do you find?

## De novo assembly

In order to get longer sequences from the detected virus, and hopefully to obtain its complete genome, we will be using the SPAdes assembler to assemble our metagenomic dataset.

# TODO: check the SPAdes command for metagenomic (and the time)
```
spades.py -k 21,33,55,77,99 --careful --meta -1 read_1.fastq -2 read_2.fastq -o output
```

This will produce a series of outputs. The scaffolds will be in fasta format.

## Alignment of the contigs to a reference genome

You will find [here](http://www.ebi.ac.uk/ena/data/view/JF713713) the reference genome to use
