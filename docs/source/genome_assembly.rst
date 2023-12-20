Genome Assembly
===============

.. _Genome Assembly:

Genome Assembly is `hard <https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.2005894>`_ . Even now with long read sequencing, `there is still a long ways to go <https://www.nature.com/articles/s41592-021-01057-y>`_. In 1999, the human genome sequence was announced. In 2021, a telomere to telomere genome assembly, with no gaps, was announced. Even with the advances in technology and reduction in cost, many genomes cannot realistically be sequenced and assembled without a significant investment. For example, most salamander and newt species have genomes that are estimated to be 100 gigabase. The largest genome sequenced so far was the `lungfish <https://www.nature.com/articles/s41586-021-03198-8?#Sec2>`_, with a genome size of 43 gigabases. Lungfish have a diploid genome, which is fairly normal for a large animal. But plants and many other organisms are not diploid and require extra caution when assembling. One notable example is `wheat <https://www.sciencedirect.com/science/article/pii/S1672022920300590>`_ which has chromosomes from three different genomes and is an `allohexaploid <https://www.nature.com/articles/s41586-020-2961-x0>`_, meaning it has six sets of two chromsomes. Most genome assembler would not know what to do with this and would likely collapse the three sets in a psuedohaploid assembly, loosing a great deal of information in the process. 

Genome assembly approaches largely depend on the type and quality of data you have available to you. Sanger sequencing used to be the golden standard. Then Illumina shotgun sequencing took over for awhile. Now we have long read sequencing using either PacBio or Oxford Nanopore Technologies platforms. Additional wet lab techniques such as `Hi-C <https://en.wikipedia.org/wiki/Chromosome_conformation_capture#Hi-C_(all-vs-all)>`_ or `optical mapping <https://en.wikipedia.org/wiki/Optical_mapping>`_ can further bring the assembly pieces together into chromosome length scaffolds. Any combination of the above can be found in most current genome assembly papers. However, if Illumina is all you have and genomes of closely related species are available, then reference assisted genome assembly is a possibility, but with caveats. 

.. note:: `Mind the Gap <https://www.sciencedirect.com/science/article/abs/pii/S1087184515300220>`_ reasons to produce gapless genome assemblies
.. note:: `Errors in long-read assemblies can critically affect protein prediction <https://www.nature.com/articles/s41587-018-0004-z>`_ for why it is critical to check assembly quality and fix errors
.. note:: `A comprehensive review of scaffolding methods in genome assembly <https://academic.oup.com/bib/article-abstract/22/5/bbab033/6149347?redirectedFrom=fulltext>`_ on ways to improve assemblies scaffolding

Short Read
----------

Illumina sequencing is cheap, relatively speaking. In my case, a mere $1200 got me 100X coverage of the *Apostichopus californicus* genome. While Illumina data won't get you chromosome length scaffolds, what it will get you is access to the majority of the gene space in a genome. With some caveats of course. You likely won't be seeing a complete `Titin <https://en.wikipedia.org/wiki/Titin>`_ gene and gene counts will likely be inaccurate as a result of gene fragmentation. For example, in my short read sea cucumber genome, the telomerase gene was split into two separate genes. However, if you're interested in shorter genes such as `Mortalin <https://en.wikipedia.org/wiki/HSPA9>`_ or highly conserved genes such as `Survivin <https://en.wikipedia.org/wiki/Survivin>`_ then you'll probably be just fine with a short read assembly. If you are interested studying gene regulation and how repetitive elements influence it, you will want chromosome length scaffolds. 

It has been close to two decades since Illumina short read sequencing became accessible and as a result there are innumerable short read assemblers. In recent years though, significant progress has been made in assembly algorithms and the result has been some blazing fast and accurate assembly tools. See below for two popular short read assemblers.

.. note:: :doc:`Short read assembly using ABySS and SPAdes <assembly/short_read_assembly>`

In my case, I am working with organisms that are highly heterozygous. What this means is there is enough variation between the two chromosomes in each chromosome pair to cause problems for a genome assembler that doesn't take this into account. Platanus-Allee was specifically designed to deal with that. The author also created another tool for genomes that have less variation (such as humans) called `Platanus <http://platanus.bio.titech.ac.jp/platanus-assembler>`_. On the Platanus-Allee webpage they state that for low heterozygous species genomes with heterozygousity < 1.0%, Platanus assembler would mark better performance than Platanus-allee. Another genome assembler for heterozygous genomes is `MaSuRCA <https://academic.oup.com/bioinformatics/article/29/21/2669/195975?login=true>`_. Both Platanus-allee and MaSuRCA are hybrid genome assemblers and I go into more detail in the Hybrid genome assembly section. See below for two very brief examples of using these two genome assemblers with just short read data. 


.. note:: :doc:`Short-read assembly using Platanus-Allee and MaSURCA <assembly/heterozygous_short_read_assembly>`

Long Read
---------

With the advent of "long-read" sequencing technology, the genome sequencing landscape dramatically changed. It became possible to acquire chromosome long scaffolds with repetitive elements mostly resolved. While certain regions of chromosome were still difficult, such as telomeres and centromeres, the vast majority of the genome was now available for assembly.

While the current state of genetics leaves most researchers interested in the "gene space" of the genome, it has becomes increasingly obvious that the regulation of these genes and consequently the expression and phenotype are controlled not by the genes themselves but by promoter and enhancer sequences present in the hard to resolve areas of the genomes. In the last few years `modeling <https://www.nature.com/articles/s41467-021-25875-y>`_ and `experiments <https://www.sciencedirect.com/science/article/abs/pii/S0959437X2030037X>`_ have also revealed the critical role chromatin structure plays in gene expression. This is all to say, it's really useful and important to have a contiguous genome and this is now achievable using long read sequencing technology. 

Currently, there are long read sequencing technologies that are duking it out: Pacbio and Oxford Nanopore Technologies. There are dozens of papers arguing about the virtues and sins of each and I won't go into it here. However, `this <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-1935-5>`_ paper does offer a good introduction and insight into the pros and cons of both. 

Now on to long read sequence genome assembly. Because the reads are longer, but contain more errors, genome assemblers use slightly different approaches to handling the "noisy" reads. But the results are usually better than anything achievable using short read data and most genome assembly pipelines include a few rounds of "polishing" post genome assembly. I go into more detail on this further down this page. See the link below for re-assembly of a sea cucumber genome using three different long read assemblers. 

.. note:: :doc:`Long read assembly of Chiridota heheva using Raven, Flye, and Shasta assemblers <assembly/long_read_assembly>`

Hybrid
------

Hybrid genome assembly is the predominate approach these days. The majority of recent genome publications use a mix of long read, short read, and hi-c. See below for some examples. 


.. note:: :doc:`Hybrid genome assembly of Apostichopus japonicus using Platanus-allee and MaSURCA <assembly/hybrid_assembly>`
.. warning:: :doc:`Using Hi-C data to achieve chromosome scale scaffolds using salsa, yahs, 3d-dna and hirise for the Stichopus chloronotus genome <assembly/hi-c_hybrid_assembly>`

Phased Genome assembly
----------------------

.. note:: :doc:`Phasing Genome Assemblies <assembly/phased_assembly>`


Organelle Genome
----------------


Mitochondria and chloroplasts

