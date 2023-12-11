Data Cleaning
=====

.. _installation:

Introduction
============

Each sequencing technology introduces its own unique artifacts and inaccuracies into the sequencing data. Identifying, removing the artifacts, and then analyzing the raw data are the first steps in preparing the sequence data for assembly. If this is the first time dipping your toes into bioinformatics, check out this introduction to getting set up for genome bioinformatics.

`A brief introduction to what you will need if you are serious about getting into bioinformatics <setup/>`_

Checking Data Integrity
=======================

Let's start from the beginning. When you first acquire your data, whether it is via a sketchy flash drive or downloaded from the internet, you will want to check the data integrity. The easiest way is to perform something called a checksum. This essentially checks that the calculated hash value for the copied data is the same as the calculated hash value for the original data, verifying that the copied data has not been corrupted. The popular algorithm to use is md5 and is fairly easy. See the link below for a more in-depth look at how to do this.

`Using Md5sum to verify data integrity <checksum/>`_

Summary Statistics and Data Quality
===================================

Once you can have confidence in the data integrity, it's time to get some rough ideas about what the data looks like. [A few things need to be checked...]

`Checking Illumina data quality using FastQC <short_read_quality/>`_    
`Checking Long read data quality using Nanoplot and SequelTools <long_read_quality/>`_   
`(To Do) Checking Hi-C data quality using pairtools, qc3C, and HiCExplorer <hic_read_quality/>`_   

Trimming, Filtering, and Error Correcting Raw Reads
===================================================

In some cases, there may be problems with the raw sequence data and it needs some cleaning. [This can be due to excessive adapter contamination...]

`Raw sequence data trimming and filtering <trimming/>`_
`(To Do) Error correcting long read data <long_read_error_correction/>`_
`(To Do) Trimming and filtering Hi-C data <hic_data_trimming/>`_

K-mer Analysis of Reads
=======================

Now the data is looking good, all the reads have high phred scores and there appears to be little to no adapter contamination. [It is time to take a closer look at the raw sequence data...]

`Estimating Best Genome Assembly K-mer size using Kmergenie <kmergenie/>`_

[However, some genome assemblers...]

`Estimating genome characteristics using Jellyfish and GenomeScope <genomescope/>`_

[Additionally, k-mer analysis can be used to check for various types of contamination in the raw sequence data...]

`K-mer Analysis using KAT <kat/>`_

If everything looks good or has been cleaned up, the next step is `genome assembly <assembly/>`_!

