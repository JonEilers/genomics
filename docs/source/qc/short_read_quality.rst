Introduction
------------

You have your data, you’ve checked the file integrity and are confident
no data was corrupted during the transfer process. It’s now time to take
a quick look to see what you’ll be working with. Things such as average
read length, duplications, base and sequence quality, GC content,
sequence length distribution, overrepresented sequences, adapter
contamination, the list goes on. Thankfully, someone created a really
good tool for checking all those things. It’s sort of a one stop shop
for read quality analysis and it’s called
`FastQC <https://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`__.

Installing Fastqc
-----------------

You can download it from the website, or you can go use the package
manager called
`conda <https://docs.conda.io/en/latest/miniconda.html>`__ to install
it.

Installing using conda

.. code:: bash

    # creating conda environment
    conda create -n fastqc

   # installing fastqc
   conda install -c bioconda fastqc 

I created a separate conda environment for FastQC ast his helps to keep
software dependencies isolated from eachother when using a lot of
different bioinformatic tools. An easy example is with the tools that
use python 2.7 rather then python 3. They won’t be able to be installed
in the environment.

Running fastQC
--------------

Fastqc can be used via command line or via its GUI

.. code:: bash

   # activating the fastqc conda environment
   conda activate fastqc

   # running fastqc from the command line
   fastqc \
       -o QC \
       --noextract \
       -t 5 \
       -a adapters.txt \
       P_cali_g_USD16092980L_HKG7CDSXX_L1_1.fq.gz \
       P_cali_g_USD16092980L_HKG7CDSXX_L1_2.fq.gz 

   # This will open the GUI
   fastqc

If FastQC looks like it is having difficulty importing data, it could be
due to `hardcoded memory
limits <https://githubmemory.com/repo/nf-core/eager/issues/650>`__. An
easy fix is adding more threads using the -t followed by the number of
cpu threads you want to use.

FastQC also works with long read data generated using PacBio or Oxford
Nanopore Technology platforms. So long as the read files are in FastQ
format, it should work.

Results
-------

Results can be found `here </fastqc1/>`__ and `here </fastqc2/>`__ The
quality of the data looks pretty darn good. There might be some PCR
amplication represented by the repeating ‘G’. The beginning of the reads
could be smoother, but nothing to write home about. In fact, I would say
this data doesn’t need any read filtering or trimming.

Novogene report
---------------

Novogene also provided a quality report which agrees with what FastQC
shows. Here is the `Report </data-cleaning/Novogene/>`__

Oxford Nanopore Technologies MinION sequencing results
------------------------------------------------------

I also had the opportunity to try sequencing using ONT’s MinION
sequencing platform. I find the technology absolutely incredible. So
let’s take a look at the results.

Fastqc can be used for this.
`Here <https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwi_-NCwzp_zAhUGip4KHYSJApYQFnoECBoQAQ&url=https%3A%2F%2Fzenodo.org%2Frecord%2F1442930%2Ffiles%2FDay_2_practicals_LRDW.pdf%3Fdownload%3D1&usg=AOvVaw2Mz3-BVI3AbVmQR9ge1vMf>`__
is a great walk through on working with long read data. Section 3
contains a brief bit on fastqc and nanopore data.
`Here </nanopore-fastqc/>`__ is the report and oh boy. Whelp, it was
good experience and I can put ONT MinION sequencing on my resume right?
So it is all really low quality data, GC peak is roughly where it should
be, but the shape of the curve is not what it should be. Sequence
content is all over the place and the bulk of the reads are not very
long. Will have to try fixing this later with a little trimming. Another
really neat tool for looking at ONT data is called
`nanoplot <https://github.com/wdecoster/NanoPlot>`__.

`Nanoplot results </nanoplot/>`__

To install Nanoplot

.. code:: bash

   conda create -n nanoplot
   conda install -c bioconda nanoplot

I did not use the command line version of nanoplot so I don’t have a
command to share. However, if you are feeling lazy like I was and have
less than 100 mb file size, you can use their webservice availabe
`here <http://nanoplot.bioinf.be/>`__

Resources
---------

I could go into details about what each of the FastQC graphs mean and
how to read them, but others have done a great job. Here is a a good
walk through of the different fastqc outputs and their meaning in
addition to quality control processing: `FastQC info by open
repository <https://openrepository.aut.ac.nz/bitstream/handle/10292/5170/FASTQC%20analysis%20guide.pdf?sequence=5&isAllowed=y>`__

Excellent comparisons of bad versus good quality sequence data and the
graphs fastqc produces: `Fastqc tutorial and faq by
MSU <https://rtsf.natsci.msu.edu/genomics/tech-notes/fastqc-tutorial-and-faq/>`__

Also contains excellent comparisons of bad versus good quality sequence
data along with some processing steps: `Bioinformatics Core
Training <https://bioinformatics-core-shared-training.github.io/cruk-autumn-school-2017/Introduction/SS_DB/Materials/Lectures/Lecture2_qualityControl_artefactRemoval_DB.pdf>`__
