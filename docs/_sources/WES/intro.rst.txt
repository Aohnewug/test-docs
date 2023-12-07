Whole Exome Sequencing
====================

Introduction
-----------------------------------
The AIMS DNA-Seq analysis pipeline identifies somatic variants within whole genome sequencing (WGS) and whole exome sequencing (WES) data.

WGS involves sequencing the entire genome, including coding and non-coding regions. WGS offers a comprehensive view of an individual's genomic landscape, capturing variations in protein coding, regulatory, intronic, and intergenic regions. WGS is particularly useful for understanding complex genetic traits, population genetics, and uncovering structural variants.

WES specifically targets the exome, which represents only the protein-coding regions of the genome. While the exome constitutes only a small fraction of the entire genome, it contains the majority of known disease-causing mutations. WES is valuable for investigating genetic characteristics of diseases in exonic regions with much lower cost compared to WGS.

AIMS DNA-Seq pipeline detects variants on WGS or WES data. It compares allele frequencies in normal and tumor sample alignments, annotating each mutation, and aggregating mutations from multiple callers.


Pipeline Overview
-----------------------------------

workflow
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The AIMS WES pipeline consists of the following main steps:
* Preprocessing
* Genome Alignment
* Quality Score Recalibration Improving the accuracy of variant calls
* Variant Calling
* Variant Filtration and Annotation
* Mutation Aggregation
* Downstream Statistics calculation

Input data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
AIMS WES pipeline accommodates two types of input data, the raw sequencing data in FASTQ format, or aligned reads information in BAM format. If a BAM file is provided, the pipeline will automatically bypass the preprocessing and genome alignment steps.

Tools and Software
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The pipeline relies on the following bioinformatics tools and internal scripts for the entire analysis process:

* BWA (version, link) for reads mapping
* GATK (version, link) for variant preprocessing and variant calling
* HaplotypeCaller (GATK) for germline variant calling
* Mutect2 (GATK) for somatic variant calling
* Strelka2 (GATK) for somatic variant calling
* VEP (version, link) for variant annotation
* Perl scripts for variant aggregation and somatic mutation masking
* Python scripts for Tumor Mutation Burden (TMB)
* MSIsensor2 for Microsatellite Instability (MSI) calculation.
* MANTIS for MSI calculation

Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
AIMS WES pipeline provides the following configuration for users to control the variant calling process.
* Version
* Data type
* Downsample
* Caller

Output
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The outputs of the AIMS WES pipeline comprise sample level 1) quality control assessment result, 2) variant call results (VCF) for each selected variant caller, 3) aggregated masked somatic mutation annotation file (MAF), and dataset-level 4) sample-by-gene mutation feature matrix.

Pipeline workflow
-----------------------------------

Pre-alignment processing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Raw sequencing reads undergo quality control using FastQC to assess read quality, identify adapter contamination, and evaluate other sequencing metrics.
* Command Line
```FastQC <fastq_1.fq.gz> <fastq_2.fq.gz>```

Alignment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Align reads to the reference genome. Supported reference genomes include Human (hg38), Mouse (mm39), Fruit fly (dm6), SARS-CoV-2.
* Command Line
```Bwa mem <reference> <fastq_1.fq.gz> <fastq_2.fq.gz>```

Pre-alignment processing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Subsampling
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Base (Quality Score) Recalibration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* GATK BaseRecalibrator
* GATK ApplyBQSR

Quality control
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* samtools stats
* Qualimap bamqc

Pipeline run summary
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* MultiQC

Variant calling
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
There are many different variant calling tools, each has strengths and weaknesses, and their performance can vary based on factors such as sequencing technology, read depth, and the type of variants being analyzed. There is currently no scientific consensus on the best variant calling pipeline. AIMS includes three variant calling tools, users can choose the pipeline(s) most appropriate for the data.

Germline variant calling
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Germline variant calling involves identifying genetic variations present in the germline cells of an individual, which are inherited from their parents and are present in every cell of the body. AIMS call germline variant using HaplotypeCaller and Strelka2.
* Command Line
```HaplotypeCaller <bam>```
```Strelka2 <bam>```

Somatic variant calling
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
A somatic mutation is a genetic alteration that occurs in the non-reproductive cells (somatic cells) of an organism. Unlike germline mutations, which are inherited and present in every cell of an individual's body, somatic mutations arise during an individual's lifetime and are specific to certain cells or tissues. Somatic variant calling is to detect alterations in tumor samples compared to matched normal tissue samples.. AIMS call somatic mutation using Mutect2 and Strelka2
* Command Line
```Mutect2 <normal_bam> <tumor_bam>```
```Strelka2 <normal_bam> <tumor_bam>```

Variant filtration and annotation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Tumor mutation burden estimation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Tumor Mutation Burden (TMB) is a measure of the total number of mutations present in the genomic DNA of a tumor cell. It quantifies the extent of genetic alterations within a tumor and is often expressed as the number of mutations per sample. AIMS call internal script to calculate the TMB of a sample.

Microsatellite instability detection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Microsatellite instability (MSI) refers to the condition where the number of short tandem repeats, known as microsatellites, in the DNA of a cell differs from what is considered normal.

Validation
-----------------------------------
Germline variant calling validation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Report given

Somatic variant calling validation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Report given

TMB calculation validation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Report given

MSI calculation validation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Report given

Version update history
-----------------------------------





