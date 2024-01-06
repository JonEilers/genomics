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

Illumina short read sequencing is cheap, relatively speaking. For example, 110X coverage of a 900mb sea cucumber genome cost about $1200 in 2019 and it is set to get a lot cheaper with the Illumina patents starting to expire. While short read data won't get you chromosome length scaffolds, what it will "hopefully" get you is access to the majority of the gene space in a genome. With some caveats of course. You likely won't be seeing a complete `Titin <https://en.wikipedia.org/wiki/Titin>`_ gene and gene counts will likely be inaccurate as a result of gene fragmentation. For example, in a short read sea cucumber genome, the telomerase gene was split into two separate genes. However, if you're interested in shorter genes such as `Mortalin <https://en.wikipedia.org/wiki/HSPA9>`_ or highly conserved genes such as `Survivin <https://en.wikipedia.org/wiki/Survivin>`_ then you'll probably be just fine with a short read assembly. If you are interested studying gene regulation or repetitive elements, you will want chromosome length scaffolds and will need at least long read sequencing.  

Genome assembly using short reads is unfortunately extremely computationally expensive relative to long read genome assembly. However, it has had two decades of algorithm refinement and current tools are designed to make the most out of the data and resources available. 

* :doc:`Short-read Genome Assembly <>`

Long Read Assemblies
---------

With the advent of "long-read" sequencing technologies offered by Pacbio and Oxford Nanopore Technologies, the genome sequencing landscape dramatically changed. It became possible to acquire chromosome long scaffolds with repetitive elements mostly resolved. While certain regions of chromosome are still difficult, such as telomeres and centromeres, the vast majority of the genome is now available for assembly. A great summary of how long reads have changed genomics can be found `here <https://www.nature.com/articles/s41592-022-01730-w>`_.

While the current state of genetics leaves most researchers interested in the "gene space" of the genome, it has becomes increasingly clear that the regulation of these genes and consequently the expression and phenotype are controlled by numerous factors including `cis-regulatory sequences <https://www.sciencedirect.com/science/article/abs/pii/S0958166921001208>`_, `methylation <https://link.springer.com/article/10.1007/s13237-021-00367-y>`_, and the `3-dimensional folding of chromatin <https://www.sciencedirect.com/science/article/pii/S0959437X23000126>`_. In order to be able to capture a complete picture of gene regulation and how it contributes to development and phenotype, a chromosome-scale genome assembly is needed. The bare minimum necessary to achieve this is copious amounts of long read sequence data. 

The caveat of current long read sequencing technologies is the error rate per a base, often denoted as a `Phred Score <https://en.wikipedia.org/wiki/Phred_quality_score>`_. Early versions of the long read technology had high rates of errors ranging from an error every 10 to 100 bases. Current versions for Pacbio HiFI are expected to have an error every 1,000 bases and for the most accurate ONT chemistry and nanopores it is five to ten errors every 1,000 bases. Both technologies have strengths and weakness. For example, ONT sequencing can produce what are called Ultra-long reads ranging in length from 100kb to several megabases long. This is especially useful for assembling extremely repetetiive genomic regions such as telomeres and centromeres. However, this comes at a cost to the "throughput" as there is a large decrease in the number of reads sequenced. Normal read length for Pacbio HiFi is limited to 15kb-20kb, whereas the range for a standard ONT run is typically 10kb-100kb. Both sequencing platforms have other caveats, a deeper review on this can be found `here <https://www.annualreviews.org/doi/full/10.1146/annurev-genom-101722-103045>`_. That being said, the more long reads you have, the closer and easier it will be to get a chromosome-scale gapless genome assembly. 

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

