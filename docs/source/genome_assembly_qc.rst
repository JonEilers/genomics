Genome Assembly QC
===============

.. _Genome Assembly QC:

Once a few assemblies are complete, it's time to determine which one is best and if any further steps need to be taken. There are a few approaches to this and each approach has a great tool for it. 

Additionally, the Earth Bio Genome project has published genome assembly standards to strive towards: More information can be found `here <https://www.earthbiogenome.org/assembly-standards>`_ . I have listed a few of them below. 

* < 5% false duplications 
* > 90% kmer completeness
* > 90% sequence assigned to candidate chromosomal sequences
* > 90% single copy conserved genes e.g. BUSCO>`_ complete and single copy
* > 90% transcripts from the same organism mappable

A paper titled `Towards complete and error-free genome assemblies of all vertebrate species <https://www.nature.com/articles/s41586-021-03451-0>`_ was published in 2021 and offers more information on the difficulties and challenges they faced while sequencing and assembling six vertebrate genomes to the level of quality set by the international Genome 10K G10K consortium. 

Summary Statistics
-------------------

A standard metric for genome contiguity is the N50 value. N50 is a tricky beast to understand and I've seen more blogs and descriptions get it wrong than right. Thankfully, `wikipedia <https://en.wikipedia.org/wiki/N50,_L50,_and_related_statistics#N50>`_ gets it right. Without getting into the details on it, the thing that matters when considering the N50 is that the majority of reads in an assembly will be shorter than the N50 value. If you have an N50 of 9kb, than the majority of the assembly will be scaffolds or contigs shorter than 9kb. Having an N50 of 9kb consequently means gene prediction will likely capture the bulk of the genes, but there will be a large number of fragmented genes such as `titin <https://en.wikipedia.org/wiki/Titin>`_. While there are a number of tools for acquiring this metric, probably my favorite way to visualize it is the snail plot produced via `Blobtoolkit <https://www.g3journal.org/content/10/4/1361>`_. `Here <https://blobtoolkit.genomehubs.org/>`_ is a link to their website. See the link below for an example. 


.. note:: `Summary statistics via Blobtoolkit <summary-stats/>`_

Assembly QC with BUSCOs
------------------------

An easy way to capture the level of fragmentation and also get idea of what to expect when predicting genes is to check the `BUSCOs <https://pubmed.ncbi.nlm.nih.gov/26059717/>`_. You'll hear BUSCOs thrown around a lot in genome papers and among scientists involved in genome sequencing. It's treated like a holy metric for how good your assembly is and it is a reasonable way to check. In essence, someone took the time to find a number of genes that are highly conserved across the kingdoms and phylum of life. Because of their conserved nature, it is a reasonable assumption that the genome of your organism likely contains these genes. So if the majority of these genes can be found in your assembly and they are not fragmented or unexpectedly duplicated, then it is reasonable to assume that a similar percentage of genes in the genome will likewise be in good shape. It is important to understand though, that BUSCO results do not represent a best case scenario but rather a targeted random sample of the genome assembly. See below for an example


.. note:: `Assembly quality assessment using BUSCO analysis <busco/>`_

Assembly Contamination and Quality
----------------------------------
In addition to looking at summary statistics and checking BUSCOs, it is also wise to check for contamination. During the sequencing process, DNA from other organisms may be in the sample and it's important to know if that has found its way into the genome assembly. A common method for checking this is to download a uniprot or refseq protein database and blast it against your assembly then check to see what organisms had the highest hit. If those organisms are closely related to the organism of interest, then it is safe to say that's probably solid, but if there are a lot of hits for distantly related organisms, then it might be a good idea to consider preprocessing and filtering the raw data before assembling. `Blobtoolkit <https://www.g3journal.org/content/10/4/1361>`_ produces two different types of graphs, the blobplot and the Cumulative assembly span plot, for visualizing possible contamination. Additionally, `KAT <https://academic.oup.com/bioinformatics/article/33/4/574/2664339?login=true>`_ can be used to check for k-mer contamination in an assembly. 

.. note:: `Assembly contamination and quality via Blobtoolkit <contamination/>`_

Assembly Evaluation using K-mers and Long Reads
-----------------------------------------------

It is important to get a quantifiable picture of the assembly quality. One way to check assembly quality is using K-mers to see how many unique K-mers are found in both the assembly and the raw data then visualize it using a `K-mer spectra graph <https://academic.oup.com/view-large/figure/118668344/btw663f1.tif>`_. A great tool for this is `Merqury <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02134-9>`_. Another tool for assembly evaluation is called `Inspector <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02527-4>`_ which uses long reads to identify, quantify, and correct errors. 

.. note:: `Assembly Quality Assessment using Inspector and Merqury <genome_quality/>`_

Polishing and Gap Closing
-------------------------

Once you have an assembly that is as good as it'll get, it might be possible to squeeze a little more out of your data using gap closing and polishing tools. However, just like with read trimming, doing either gap closing or polishing can result in an assembly that was worse than what you started with. I also want to add that overzealous use of gap closing or polishing can result in poor assemblies. This is a huge problem when these assemblies are then uploaded into NCBI and used as references genomes for other projects. Most researchers do not have the skill, knowledge, or time to check that the assembly or genes from assemblies are trustworthy, potentially resulting in a lot of frustration and wasted time and money. So proceed with caution. 

Gap Closing
-----------

Using long read data, it is now possible to close gaps that are produced by genome assembler with a high degree of confidence. This is significantly different from previous tools such as `SSpace <https://academic.oup.com/bioinformatics/article/27/4/578/197626>`_ that relied on paired end short reads to close gaps or extend contigs. There are two problems with this approach, the obvious one is that the reads are too short to accurately span repetitive elements. The second problem is that these tools are haplo-type insensitive, meaning they can't tell if they are actually extending a real contig or just stringing alleles together creating inaccurate duplications. 

.. warning:: To-do: Gap closing using Dentist, SAMBA and Longstitch

Polishing
---------

It's also important to remove any insertions, deletions, and adapter contamination that may have crept into the genome assembly. This can be accomplished using either long read or short read data. Short read data has a much higher accuracy, but long read data, while full of errors, can produce a consensus sequence that is highly accurate which can allow it to correct longer mistakes than short read data. 

.. note:: `Genome assembly polishing using hapo-g <genome_polishing/>`_

Manual Assembly Correction
--------------------------

Recently, researcher who completed the first 100% complete genome assembly uploaded a `pre-print <https://www.biorxiv.org/content/10.1101/2021.07.02.450803v1>`_ to biorxiv detailing their process and advocating for a more manual gap closing process. This is a neat paper and their process will likely feature in many future efforts to create 100% complete telomere to telomere genome assemblies. Although, this process is likely to primarily be used on genome assemblies that are already near completion. Sea cucumber genomes may be waiting awhile before they get this level of treatment. The paper is titled `chasing perfection <https://www.biorxiv.org/content/10.1101/2021.07.02.450803v1>`_. 

.. note:: `Significantly improving the quality of genome assemblies through curation <https://academic.oup.com/gigascience/article/10/1/giaa153/6072294>`_

.. note:: Hmmm, maybe play with `AGB <https://github.com/almiheenko/AGB>`_ for visualizing long read assembly?

.. note:: Note to self: Consider future genome analysis and reassembly for `Australostichopus mollis <https://www.ncbi.nlm.nih.gov/bioproject/PRJEB10682/>`, `Apostichopus leukothele <https://www.ncbi.nlm.nih.gov/sra/SRX8086344`accn <>`, and `Actinopyga echinites <https://www.ncbi.nlm.nih.gov/assembly/GCA_010015985.1#/st>` genomes.

