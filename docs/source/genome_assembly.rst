Genome Assembly
===============

.. _Genome Assembly:

In 1995, the first genome was published, the virus `Haemophilus influenzae <https://www.science.org/doi/10.1126/science.7542800>`_. The first `draft human assembly <https://www.nature.com/articles/35057062>`_ was published in 2001. 2022 saw the publication of the first complete, `telomere to telomere human genome assembly <https://www.science.org/doi/10.1126/science.abj6987>`_. But while the tools and methods for achieving highly contiguous genome assemblies have greatly progressed in recent years, even `telomere to telomere length chromsome-scale scaffolds <https://scholar.google.com/scholar?as_ylo=2023&q=telomere+to+telomere&hl=en&as_sdt=0,1>`_ for many species, genome assembly is still hard. This is due to biological complexity, species specific genome characteristics, and technological constraints. 

Evolution is messy and a genome represents the four billion years of evolution from the beginning of life to modern organisms. Because of this, genomes themselves are complex quagmires of nucleic acid strings. On paper, assembling a genome is as simple as overlapping sequences of ATGC, but the reality is that the geonme contains a huge diversity of sequences with a wide range of functions from the repetitive telomeric sequences at the end of eukaryotic chromosomes to the introns of coding genes. Adding to this complexity are the genomic parasites called transposons and their fossilized remains which are scattered throughout the genome. An additional layer of complexity is that of `ploidy <https://en.wikipedia.org/wiki/Polyploidy>`_. Humans have a rather boring genome which contains two sets of 23 chromosomes, each set inherited from one parent and hypothetically identical. Meaning, that if a genome assembly is to accurately capture the genetics of an individual, then there should be two sets of each chromosome-scale scaffold, aka diploid. Achieving this is known as genome assembly phasing. 

Biology is never so straightforward though and many organisms, plants being a good example, have multiple sets. For example, the `cultivated strawberry is octoploid <https://academic.oup.com/dnaresearch/article/21/2/169/404005>`_. In some strange cases such as the `hybrid cultivated duran wheat <https://www.sciencedirect.com/science/article/pii/S1672022920300590>`_, the offspring inherited both diploid chromosome sets. The Both parents were 2n with a total of 14 chromosome. Duran wheat is 28 with 2n from one parent and 2n from the other. On the other end of the ploidy spectrum is an `ant <https://www.science.org/doi/10.1126/science.231.4743.1278>`_ with only one set of chromosomes and on the extremely high end is the fern `Ophioglossum reticulatum <https://academic.oup.com/botlinnean/article-abstract/102/3/205/2633396>`_ with a total chromosome count of 1440. 

In addition to the strange ploidy levels and wide range in number of chromosome, there is also large variation in genome size and gene structure. The largest estimated genome size is that of the `amoeba Polychaos dubium <https://www.sciencedirect.com/science/article/abs/pii/0010406X68903149>`_, with a size of 670gb. The c-value for this was determined in 1968 and modern techniques have not been applied. The smallest genome goes to the parasite Encephalitozoon intestinalis with a `genome size of 2.25mb <https://www.nature.com/articles/ncomms1082>`_. A good overview of genomesize and the c-value enigma can be found on `wikipedia <https://en.wikipedia.org/wiki/Genome_size#>`_. Gene structure is also widely different between major organisnmal groups, for example plants and echinoderms tend to have introns that span a few hundred to a few thousand bases whereas introns in mammals tend to significantly larger, often in the tens of thousands of bases. Another complication is that plants are more resilient to duplication events, meaning they tend to have a higher number of duplicated regions in the genome. Duplication events can create repetitive sequences which increase the difficulty of genome assembly. 


Short Read
----------

Illumina sequencing is cheap, relatively speaking. In my case, a mere $1200 got me 100X coverage of the *Apostichopus californicus* genome. While Illumina data won't get you chromosome length scaffolds, what it will get you is access to the majority of the gene space in a genome. With some caveats of course. You likely won't be seeing a complete `Titin <https://en.wikipedia.org/wiki/Titin>`_ gene and gene counts will likely be inaccurate as a result of gene fragmentation. For example, in my short read sea cucumber genome, the telomerase gene was split into two separate genes. However, if you're interested in shorter genes such as `Mortalin <https://en.wikipedia.org/wiki/HSPA9>`_ or highly conserved genes such as `Survivin <https://en.wikipedia.org/wiki/Survivin>`_ then you'll probably be just fine with a short read assembly. If you are interested studying gene regulation and how repetitive elements influence it, you will want chromosome length scaffolds. 

It has been close to two decades since Illumina short read sequencing became accessible and as a result there are innumerable short read assemblers. In recent years though, significant progress has been made in assembly algorithms and the result has been some blazing fast and accurate assembly tools. See below for two popular short read assemblers.

* :doc:`Short read assembly using ABySS and SPAdes <assembly/short_read_assembly>`

In my case, I am working with organisms that are highly heterozygous. What this means is there is enough variation between the two chromosomes in each chromosome pair to cause problems for a genome assembler that doesn't take this into account. Platanus-Allee was specifically designed to deal with that. The author also created another tool for genomes that have less variation (such as humans) called `Platanus <http://platanus.bio.titech.ac.jp/platanus-assembler>`_. On the Platanus-Allee webpage they state that for low heterozygous species genomes with heterozygousity < 1.0%, Platanus assembler would mark better performance than Platanus-allee. Another genome assembler for heterozygous genomes is `MaSuRCA <https://academic.oup.com/bioinformatics/article/29/21/2669/195975?login=true>`_. Both Platanus-allee and MaSuRCA are hybrid genome assemblers and I go into more detail in the Hybrid genome assembly section. See below for two very brief examples of using these two genome assemblers with just short read data. 


* :doc:`Short-read Assembly <assembly/heterozygous_short_read_assembly>`

Long Read
---------

With the advent of "long-read" sequencing technology, the genome sequencing landscape dramatically changed. It became possible to acquire chromosome long scaffolds with repetitive elements mostly resolved. While certain regions of chromosome were still difficult, such as telomeres and centromeres, the vast majority of the genome was now available for assembly.

While the current state of genetics leaves most researchers interested in the "gene space" of the genome, it has becomes increasingly obvious that the regulation of these genes and consequently the expression and phenotype are controlled not by the genes themselves but by promoter and enhancer sequences present in the hard to resolve areas of the genomes. In the last few years `modeling <https://www.nature.com/articles/s41467-021-25875-y>`_ and `experiments <https://www.sciencedirect.com/science/article/abs/pii/S0959437X2030037X>`_ have also revealed the critical role chromatin structure plays in gene expression. This is all to say, it's really useful and important to have a contiguous genome and this is now achievable using long read sequencing technology. 

Currently, there are long read sequencing technologies that are duking it out: Pacbio and Oxford Nanopore Technologies. There are dozens of papers arguing about the virtues and sins of each and I won't go into it here. However, `this <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-1935-5>`_ paper does offer a good introduction and insight into the pros and cons of both. 

Now on to long read sequence genome assembly. Because the reads are longer, but contain more errors, genome assemblers use slightly different approaches to handling the "noisy" reads. But the results are usually better than anything achievable using short read data and most genome assembly pipelines include a few rounds of "polishing" post genome assembly. I go into more detail on this further down this page. See the link below for re-assembly of a sea cucumber genome using three different long read assemblers. 

* :doc:`Assembling Contigs using Long Read Data <assembly/long_read_assembly>`

Scaffolding using Hi-C and Optical Genome mapping
----------------------

* :doc:`Scaffolding Long Read Contigs using Hi-C and Optical Genome Mapping <assembly/long_read_assembly>`


Assembly Phasing
----------------------

* :doc:`Phasing Genome Assemblies <assembly/phased_assembly>`


Organelle Genome
----------------


Mitochondria and chloroplasts

