Introduction
------------

While my Illumina data was high quality, not all sequence data is.
Sometimes this can be alleviated by removing (filtering) low quality
reads or bases or trimming the ends of the reads where most low quality
bases are. Additionally, there can be artificats from the sequencing
platform used in the form of “adapters”.

Normally, during the sequencing process, the adapters are not sequenced,
but once in awhile a small portion of the adapter is sequenced. If this
happens enough and the contamination is not removed, it can result in
misalignments or misassemblies.
`Here <https://www.ecseq.com/support/ngs/trimming-adapter-sequences-is-it-necessary>`__
is a better explanation with regards to Illumina sequencing.

Trimming and Filtering Illumina Data
------------------------------------

While my data was high quality, that is not always the case.
`Here </badrna1/>`__ and `here </badrna2/>`__ are examples of some low
quality rna-seq data that I used for de novo gene prediction, but first
I cleaned it up a bit.

There are a lot of tools for sequence trimming and filtering. Probably
the tool I see used most often is
`Trimmomatic <http://www.usadellab.org/cms/?page=trimmomatic>`__. Their
github page is `here <https://github.com/usadellab/Trimmomatic>`__. I
personally don’t use it. It is an old tool and I’ve always found it a
pain to use. That’s not to say that it isn’t good though. There are also
numerous publications that have compared trimming tools and consequences
of trimming.
`Here’s <https://link.springer.com/article/10.1186/s12859-016-0956-2>`__
one that looks at rna-seq data trimming and its impact on downstream
analysis such as gene expression.

For trimming this dataset I used
`BBduk <https://jgi.doe.gov/data-and-tools/bbtools/bb-tools-user-guide/bbduk-guide/>`__

.. code:: bash

   # creating environment and installing
   conda create -n bbmap
   conda install -c bioconda bbmap 

bbmap is another tool in the BBtools suite. In this case, bbduk is
bundled with the bbmap conda package and the package is regularly
updated.

.. code:: bash

   # running BBduk
   bbduk.sh \
       in1=SRR1695477_1.fq \
       in2=SRR1695477_2.fq \
       out=clean5.fq \
       out2=clean6.fq \
       ref=adapters.fa \
       ktrim=r \
       k=23 \
       mink=11 \
       hdist=1 \
       tpe \
       tbo \
       maq=10

   # changing the quality cut off to see if it changes anything
   bbduk.sh \
       in1=SRR1695477_1.fq \
       in2=SRR1695477_2.fq \
       out=clean3.fq \
       out2=clean4.fq \
       ref=adapters.fa \
       ktrim=r \
       k=23 \
       mink=11 \
       hdist=1 \
       tpe \
       tbo \
       maq=15

Results
-------

Let’s take a look to see how this changed the fastqc analysis.
`Here </cleaned_mapq10/>`__ is the data set with a phred cutoff of 10
and `here </cleaned_mapq15/>`__ is the data with a phred cutoff of 15.
The data is definitely a lot cleaner and good enough to be used for de
novo gene prediction and possibly validating gene models later.
Unfortunately, the consequences of the aggressive trimming is that
almost 70% of the reads were removed.

Below is an example of what the command line output looks like

.. code:: bash

   java -ea -Xmx63784m -Xms63784m -cp /home/jon/anaconda3/envs/biotools/opt/bbmap-38.73-0/current/ jgi.BBDuk in1=SRR1695477_1.fq in2=SRR1695477_2.fq out=clean3.fq out2=clean4.fq ref=adapters.fa ktrim=r k=23 mink=11 hdist=1 tpe tbo maq=15
   Executing jgi.BBDuk [in1=SRR1695477_1.fq, in2=SRR1695477_2.fq, out=clean3.fq, out2=clean4.fq, ref=adapters.fa, ktrim=r, k=23, mink=11, hdist=1, tpe, tbo, maq=15]
   Version 38.73

   maskMiddle was disabled because useShortKmers=true
   0.050 seconds.
   Initial:
   Memory: max=66890m, total=66890m, free=66848m, used=42m

   Added 17879 kmers; time:    0.243 seconds.
   Memory: max=66890m, total=66890m, free=66773m, used=117m

   Input is being processed as paired
   Started output streams: 0.014 seconds.
   Processing time:        54.031 seconds.

   Input:                      52003372 reads      5200337200 bases.
   KTrimmed:                   32461380 reads (62.42%)     3222085994 bases (61.96%)
   Trimmed by overlap:         249026 reads (0.48%)    1215970 bases (0.02%)
   Low quality discards:       3868494 reads (7.44%)   376310720 bases (7.24%)
   Total Removed:              35723132 reads (68.69%)     3599612684 bases (69.22%)
   Result:                     16280240 reads (31.31%)     1600724516 bases (30.78%)

   Time:                           54.294 seconds.
   Reads Processed:      52003k    957.81k reads/sec
   Bases Processed:       5200m    95.78m bases/sec
