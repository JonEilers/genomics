KAT
---

Kmer Analysis of Raw Sequence Data

Background
~~~~~~~~~~

The below exerpt is from the KAT
`website <https://kat.readthedocs.io/en/latest/>`__

KAT provides a suite of tools that, through the use of k-mer counts,
help the user address or identify issues such as determining sequencing
completeness for assembly, assessing sequencing bias, identifying
contaminants, validating genomic assemblies and filtering content. KAT
is geared primarily to work with high-coverage genomic reads from
Illumina devices, although can work with any fasta or fastq sequence
file.

At it’s core KAT exploits the concept of k-mer spectra (histograms
plotting number of distinct k-mers at each frequency). By studying
properties of the k-mer spectra it’s possible to discover important
information about the data quality (level of errors, sequencing biases,
completeness of sequencing coverage and potential contamination) and
genomic complexity (size, karyotype, levels of heterozygosity and repeat
content). Further information can be gleaned through pairwise comparison
of spectra, making KAT useful for WGS library comparisons and assembly
validation.

Installion information
~~~~~~~~~~~~~~~~~~~~~~

KAT was installed using conda. Due to dependency conflicts, KAT required
creating a separate conda environment prior to installation. See below
for KAT package details

::

   kat                       2.4.2            py36he1b856b_0    bioconda

To install

.. code:: bash

   conda create -name kat
   conda activate kat
   conda install -c bioconda kat 

New tool:
`Merqury <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02134-9>`__
github: https://github.com/marbl/merqury

Commands
~~~~~~~~

KAT commands

.. code:: bash

   kat comp -t 70 -n -o P_cali_katcomp ../P_cali_g_1.fq  ../P_cali_g_2.fq
   kat plot spectra-mx -i -x 200 -o P_cali_kat_spectra.png P_cali_katcomp-main.mx
   kat gcp -t 70 -m 32 ../P_cali_g_1.fq  ../P_cali_g_2.fq

Figures
~~~~~~~

{% include gallery %}

The first figure has GC read content on the Y axis and 32mer frequency
on the X axis. It essentially shows how frequently each 32mer appeared
and the gc content of those 32mers. Important to note there is a band of
yellow off to the far left - those likely represent kmers that appear
only once in the dataset and are artifacts/errors. There are two peaks
in the plot which represent the heterozygosity that is expected in a sea
cucumber genome.

The second figure depicts a very similar pattern, except on a diagonal.
The plot has the paired-end data 27mers plotted against eachother.
Bottom corner is showing kmers that appear only a few times and
represent contamination or kmers found in reads containing sequencing
errors.

The third figure is a graph that also appears when using genomescope. In
this graph though we get to compare the k-mer mulitiplicity between the
forward and reversed reads of paired-end Illumina data. They should be
basically identical, and so they appear. The bumpy lines are showing
k-mers that are shared between the two files and the almost vertical
line is showing k-mers that are most likely from reads containing errors
or contamination.
