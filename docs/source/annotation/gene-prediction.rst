Braker2
=======

Gene Prediction

Background
----------

Excerpt from the braker2 github
`page <https://github.com/Gaius-Augustus/BRAKER>`__

The rapidly growing number of sequenced genomes requires fully automated
methods for accurate gene structure annotation. With this goal in mind,
we have developed BRAKER1, a combination of GeneMark-ET and AUGUSTUS
that uses genomic and RNA-Seq data to automatically generate full gene
structure annotations in novel genome.

However, the quality of RNA-Seq data that is available for annotating a
novel genome is variable, and in some cases, RNA-Seq data is not
available, at all.

BRAKER2 is an extension of BRAKER1 which allows for fully automated
training of the gene prediction tools GeneMark-EX, and AUGUSTUS from
RNA-Seq and/or protein homology information, and that integrates the
extrinsic evidence from RNA-Seq and protein homology information into
the prediction.

In contrast to other available methods that rely on protein homology
information, BRAKER2 reaches high gene prediction accuracy even in the
absence of the annotation of very closely related species and in the
absence of RNA-Seq data.

Installation
------------

braker2 conda versions

.. code:: bash

   braker2                   2.1.4                         0    bioconda
   braker2                   2.1.5                         0    bioconda

.. code:: bash

    conda install -c bioconda braker2 

genemark-ES was downloaded from the genemark
`website <http://exon.gatech.edu/GeneMark/>`__. It was unpacked and
added to $PATH

.. code:: bash

   GENEMARK_PATH=/home/jon/genemark 
   export GENEMARK_PATH
   export PATH

Also, there is a weird bug in the filterGenesIn_mRNAname.pl which
requires the below fix

.. code:: bash

   $_ =~ m/transcript_id \"(.*)\"
   # to
   $_ =~ m/transcript_id \"(.*?)\"

Commands
--------

braker2 *ab initio*

.. code:: bash

   braker.pl --genome=/home/jon/Working_Files/repeats/repeatmasking_output/pcali_ajap.redundans.platanus.fa.masked \
      --species=Apostichopus_californicus \
      --esmode \
      --makehub \
      --gff3 \
      --softmasking \
      --cores 8 \
      --email jon.eilers@wallawalla.edu

braker2 run with rna-seq

.. code:: bash

    braker.pl --species=Apostichopus californicus \
     --genome=/home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
     --bam=/home/jon/Working_Files/star/Aligned.sortedByCoord.out.bam \
     --softmasking \
     --cores 75 \
     --makehub \
     --gff3 \
     --email jon.eilers@wallawalla.edu \
     --useexisting

braker2 run with rna-seq and protein data from Apostichopus japonicus

.. code:: bash

   braker.pl --species=Apostichopus_californicus \
     --genome=/home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
     --bam=/home/jon/Working_Files/star/Aligned.sortedByCoord.out.bam  \
     --softmasking \
     --cores 75 \
     --makehub \
     --gff3 \
     --email jon.eilers@wallawalla.edu \
     --prot_seq=/home/jon/Working_Files/Apostichopus_japonicus/ajap_protein_uniprot.fasta \
     --prg=gth
