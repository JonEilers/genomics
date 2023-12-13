Kmergenie
---------

Estimation of optimal kmer for genome assembly

Background
~~~~~~~~~~

Excerpt from `website <http://kmergenie.bx.psu.edu/>`__

KmerGenie estimates the best k-mer length for genome de novo assembly.
Given a set of reads, KmerGenie first computes the k-mer abundance
histogram for many values of k. Then, for each value of k, it predicts
the number of distinct genomic k-mers in the dataset, and returns the
k-mer length which maximizes this number. Experiments show that
KmerGenie’s choices lead to assemblies that are close to the best
possible over all k-mer lengths. KmerGenie predictions can be applied to
single-k genome assemblers (e.g. Velvet, SOAPdenovo 2, ABySS, Minia).
However, multi-k genome assemblers (e.g. SPAdes, IDBA) generally perform
better with default parameters (using multiple k values), rather than
the single best k predicted by KmerGenie.

Usage Information
~~~~~~~~~~~~~~~~~

Kmergenie has a kmer size cutoff at 127 bp. This doesn’t work well when
using reads which are significantly longer than 127 base pairs. In my
case, the read length is 150, paired end. In order to run kmergenie on
longer reads, it has to be recompiled with a the kmer size set to a
larger number. This required downloading the conda recipe used to build
the bioconda kmergenie that I used previously. I then set the max kmer
size to 300 in the bash make script and removed a few dependencies in
the yaml file which were preventing the build from completing. I then
compiled and installed the new version.

Installation and Commands
~~~~~~~~~~~~~~~~~~~~~~~~~

Kmergenie was compiled, installed, and run on a different machine that I
do not have access to at the moment. This will be updated when I am able
to get onto that computer

conda installation

.. code:: bash

   # create conda environment
   conda create -n kmergenie

   # adding conda-forge channel to conda
   conda config --add channels conda-forge

   # installing kmergenie
   conda install -c bioconda kmergenie 

I recently noticed that in new installations of miniconda conda-forge
was not automatically added to the channels that are checked for
packages. A lot of bioinformatic tools in conda require packages in
conda-forge otherwise conda won’t be able to install the tool. Easy fix
though.

commands

.. code:: bash

   # activating the kmergenie conda environment
   conda activate kmergenie

   # running kmergenie
   kmergenie \
     pcali_1.fastq \
     -t 75 \
     -o pcali_1.kmergenie

Results
~~~~~~~

The new kmergenie build was run using the illumina data with max kmer
set to 280. This was a bad choice as all the kmers that were checked
were even and this greatly increased the run time of program from a day
or two to over a week. I would include a link backing this up, but I
can’t seem to find one at the moment. Additionally, because the reads
were only 150bp long, everything over that size obviously was not
calculated, but it did generate a lot of empty files. Additionally,
kmergenie generates jpeg images for every kmer distrubition it
calculates along with a html file combining all of them. I have not
included that here, but it is available upon request.

**Predicted assembly size: 938366900 bp** **Predicted best kmer was
112**

{% include gallery %}

In an ideal result, the left graph would be a nice down-ward opening
curve as shown
`here <http://kmergenie.bx.psu.edu/sample_report.html>`__. However, as
explained in this
`link <http://kmergenie.bx.psu.edu/sample_report.html#advhelp>`__, real
world cases can be a bit messier. The result I got suggests that there
may be higher k-mer values that would could produce better results.
Unfortunately, I only have short read data with read length of 150 base
pairs which prevents analysis of longer k-mers. This result could be a
product of high heterozygosity in the data. When using the genome
assembler platanus-allee it selected a max k-mer of 110 which agrees
with kmergenie’s estimate. What this makes me think is that I should
have used long read data for this genome sequencing project.

Kmergenie predicts a genome size of 938mb. This is actually quite close
to genome estimates for *Apostichopus japonicus* which are around 900mb.
I am curious why kmergenie was able to get close to this number but
genomescope dramatically underestimated it.
