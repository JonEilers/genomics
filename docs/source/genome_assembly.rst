Genome Assembly
===============

.. _Genome Assembly:

In 1995, the first genome was published, the virus `Haemophilus influenzae <https://www.science.org/doi/10.1126/science.7542800>`_. The first `draft human assembly <https://www.nature.com/articles/35057062>`_ was published in 2001. 2022 saw the publication of the first complete, `telomere to telomere human genome assembly <https://www.science.org/doi/10.1126/science.abj6987>`_. But while the tools and methods for achieving highly contiguous genome assemblies have greatly progressed in recent years, even `telomere to telomere length chromsome-scale scaffolds <https://scholar.google.com/scholar?as_ylo=2023&q=telomere+to+telomere&hl=en&as_sdt=0,1>`_ for many species, genome assembly is still hard. This is due to biological complexity, species specific genome characteristics, and technological constraints. 

Evolution is messy and a genome represents the four billion years of evolution from the beginning of life to modern organisms. Because of this, genomes themselves are complex quagmires of nucleic acid strings. On paper, assembling a genome is as simple as overlapping sequences of ATGC, but the reality is that the geonme contains a huge diversity of sequences with a wide range of functions from the repetitive telomeric sequences at the end of eukaryotic chromosomes to the introns of coding genes. Adding to this complexity are the genomic parasites called transposons and their fossilized remains which are scattered throughout the genome. An additional layer of complexity is that of `ploidy <https://en.wikipedia.org/wiki/Polyploidy>`_. Humans have a rather boring genome which contains two sets of 23 chromosomes, each set inherited from one parent and hypothetically identical. Meaning, that if a genome assembly is to accurately capture the genetics of an individual, then there should be two sets of each chromosome-scale scaffold, aka diploid. Achieving this is known as genome assembly phasing. 

Biology is never so straightforward though and many organisms, plants being a good example, have multiple sets. For example, the `cultivated strawberry is octoploid <https://academic.oup.com/dnaresearch/article/21/2/169/404005>`_. In some strange cases such as the `hybrid cultivated duran wheat <https://www.sciencedirect.com/science/article/pii/S1672022920300590>`_, the offspring inherited both diploid chromosome sets. The Both parents were 2n with a total of 14 chromosome. Duran wheat is 28 with 2n from one parent and 2n from the other. On the other end of the ploidy spectrum is an `ant <https://www.science.org/doi/10.1126/science.231.4743.1278>`_ with only one set of chromosomes and on the extremely high end is the fern `Ophioglossum reticulatum <https://academic.oup.com/botlinnean/article-abstract/102/3/205/2633396>`_ with a total chromosome count of 1440. Heterozygousity can also be a significant challenge in genome assembly. Humans have low levels of heterozygosity, meaning variation between chromosome is relatively low. However, in some species such as sea cucumbers or fruit trees heterozygousity is high. This makes it is difficult for genome assemblers to figure out if slightly different sequences are `alleles <https://en.wikipedia.org/wiki/Allele>`_ or `duplications <https://en.wikipedia.org/wiki/Repeated_sequence_(DNA)>`_ in a sequence. 

In addition to the strange ploidy levels, wide range in number of chromosome, and heterozygousity there is also large variation in genome size and gene structure. The largest estimated genome size is that of the `amoeba Polychaos dubium <https://www.sciencedirect.com/science/article/abs/pii/0010406X68903149>`_, with a size of 670gb. The c-value for this was determined in 1968 and modern techniques have not been applied. The smallest genome goes to the parasite Encephalitozoon intestinalis with a `genome size of 2.25mb <https://www.nature.com/articles/ncomms1082>`_. A good overview of genomesize and the c-value enigma can be found on `wikipedia <https://en.wikipedia.org/wiki/Genome_size#>`_. Gene structure is also widely different between major organisnmal groups, for example plants and echinoderms tend to have introns that span a few hundred to a few thousand bases whereas introns in mammals tend to significantly larger, often in the tens of thousands of bases. Another complication is that plants are more resilient to duplication events, meaning they tend to have a higher number of duplicated regions in the genome. Duplication events can create repetitive sequences which increase the difficulty of genome assembly. 

The previously mentioned characteristics of a genome can create difficulties in assembly. For example, while the cost of genome sequencing has significnatly decreased in recents years, particularly large genomes can be prohibitively expensive. The largest genome sequenced as of writing this (January 2024) is the European Mistletoe which has a haploid size of 94gb. With current costs, this likely represent hundreds of thousands of dollars of sequencing. Ploidy also represent a challenge as until recently, most genome assemblers output a merged haploid assembly. With improvements in Hi-C scaffolding and optical genome mapping, new genome assemblers take in account ploidy, but most are only designed for diploid genomes.  

A telomere to telomere genome assembly can be achieved using 50X coverage short read, 30X coverage long read, 10X coverage ultra-long read, and 30X coverage short read Hi-C. Although this is going to be highly dependent on the characteristics of genome. Long reads are assembled into contigs. Hi-C is used to scaffold and orient the contigs into long chromosome-scale sequences. Ultra-long reads are used to fix assembly errors and fill in gaps in the scaffolds, and short reads are used to further polish the assembly, removing any erroneous indels and correcting base errors. 

Draft assemblies can be acquired using long reads or short reads. In the case of long reads, these assemblies often contain several thousand long contigs which capture most the genes in the genome but are missing structural and repetitive content such as telomeres, centromeres, transposons, and other repetitive elements. Short read assemblies are of a much lower quality, often containing more than 100,000 contigs and having an N50 less than 10kb, meaning that if your lucky the majority of genes will be present, albeit possibly fragmented. 

Short Read Assemblies
----------

Illumina short read sequencing is cheap, relatively speaking. For example, 110X coverage of a 900mb sea cucumber genome cost about $1200 in 2019. While Illumina data won't get you chromosome length scaffolds, what it will "hopefully" get you is access to the majority of the gene space in a genome. With some caveats of course. You likely won't be seeing a complete `Titin <https://en.wikipedia.org/wiki/Titin>`_ gene and gene counts will likely be inaccurate as a result of gene fragmentation. For example, in a short read sea cucumber genome, the telomerase gene was split into two separate genes. However, if you're interested in shorter genes such as `Mortalin <https://en.wikipedia.org/wiki/HSPA9>`_ or highly conserved genes such as `Survivin <https://en.wikipedia.org/wiki/Survivin>`_ then you'll probably be just fine with a short read assembly. If you are interested studying gene regulation and how repetitive elements influence it, you will want chromosome length scaffolds and will need at least long read sequencing.  

Long Read
---------

With the advent of "long-read" sequencing technology, the genome sequencing landscape dramatically changed. It became possible to acquire chromosome long scaffolds with repetitive elements mostly resolved. While certain regions of chromosome were still difficult, such as telomeres and centromeres, the vast majority of the genome was now available for assembly.

While the current state of genetics leaves most researchers interested in the "gene space" of the genome, it has becomes increasingly obvious that the regulation of these genes and consequently the expression and phenotype are controlled not by the genes themselves but by promoter and enhancer sequences present in the hard to resolve areas of the genomes. In the last few years `modeling <https://www.nature.com/articles/s41467-021-25875-y>`_ and `experiments <https://www.sciencedirect.com/science/article/abs/pii/S0959437X2030037X>`_ have also revealed the critical role chromatin structure plays in gene expression. This is all to say, it's really useful and important to have a contiguous genome and this is now achievable using long read sequencing technology. 

Long read

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

