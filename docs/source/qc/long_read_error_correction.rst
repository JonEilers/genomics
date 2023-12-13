Introduction
============

Long reads generated using either Pacbio or Oxford Nanopore Technologies
sequencing platforms have relatively high errors rates. These errors can
reduce assembly quality and contiguity if not corrected. There are two
approaches to this: the first is to do genome assembly and `polish the
genome </genome_polishing/>`__ afterwards. Another approach to dealing
with the high error rate is to perform error correction on the reads
before genome assembly.

Long read error correction tools fall in to two categories. The first,
and currently most common, is using short highly accurate reads such as
produced by Illumina to fix errors in the long reads. The alternative
approach is to use the long reads themselves to produce “consensus
sequences” which can be used to fix the errors in the long read data.

`A comprehensive evaluation of long read error correction
methods <https://link.springer.com/article/10.1186/s12864-020-07227-0>`__

`ELECTOR <https://github.com/kamimrcht/ELECTOR>`__ is a tool for
comparing different long read correction tools.

Both the *Stichopus chloronotus* and *Chiridota heheva* datasets contain
long reads and short Illumina reads. So these two data sets will be used
for long read error correction examples.

Data
====

On NCBI

.. code:: bash

   conda create -n sratools
   conda activate sratools
   conda install sra-tools

   # Downloading and extracting the *Chiridota heheva* data 
   prefetch

   fasterq-dump

   # Downloading and extracting the *Stichopus chloronotus* data
   fasterq-dump

I prefer to use prefetch to download the sequence read archive (.sra)
files as they are smaller so they take up less space on my harddrive
when I am not using the data. Fasterq-dump can also download and extract
the fastq afterwards, but it deletes the .sra file afterwards.

Short read correction of long reads
===================================

Seems like a lot of the long read correction using short read tools are
no longer maintained. Was fairly difficult to find any that have been
updated in the last five years.

`LoRDEC <http://www.atgc-montpellier.fr/lordec/>`__

`VeChat <https://github.com/HaploKit/vechat>`__

Long read self-correction
=========================

Use `Medaka <https://github.com/nanoporetech/medaka>`__ on the chiridota
nanopore data.

Use `consent <>`__ for apostichopus japonicus pacbio reads?

Use VeChat and Consent for polishing both the Chiridota heheva and
stichopus chloronotus reads

`VeChat <https://github.com/HaploKit/vechat>`__

.. code:: bash

   conda create -n vechat
   conda activate vechat
   # installing dependencies
   conda install -c bioconda minimap2 yacrd fpa=0.5

   # cloning git repository
   git clone https://github.com/HaploKit/vechat.git
   cd vechat
   mkdir build;cd build;cmake -DCMAKE_BUILD_TYPE=Release -Dspoa_optimize_for_portability=ON ..;make

   # exporting path
   export PATH=/home/jon/Working_Files/vechat/scripts:$PATH
     

running vechat

.. code:: bash

   vechat \
     /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
     --platform ont \
     --split \
     -t 60 \
     --overwrite

Self-correction of sequencing errors using
`consent <https://github.com/morispi/CONSENT>`__.

`Scalable long read self-correction and assembly polishing with multiple
sequence
alignment <https://www.nature.com/articles/s41598-020-80757-5>`__

.. code:: bash

   conda create -n consent
   conda activate consent
   conda install -c bioconda consent 
