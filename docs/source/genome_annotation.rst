Genome Annotation
=====

.. _Genome Annotation:

While creating a high quality, contiguous genome assembly is critical to any genomics or genetics project, a genome assembly is practically useless without identifying and annotating the various components of a genome such as genes, transposable elements, promoters, transcription factor binding sites, etc. This is currently where the real challenge lays in most genome sequencing projects and the consequences of doing this poorly can impact future analyses. Including propagation of those `errors to other genomes <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1715-2>`_ as is `commonly seen on NCBI <https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-020-03855-1>`_ when performing blast searches for homologs and orthologs where up to an estimated 50% of genes contain errors. 

The most common example of errors in a genome assembly is simply gene content and is often seen in the change in gene content as observed in subsequent versions of a genome assembly. One such example is that of the saugaro cactus. In 2017, a `draft assembly (SGP5 v1.3) <https://www.pnas.org/doi/full/10.1073/pnas.1706367114>`_ was published with 28,292 genes. In 2023, a `pre-print <https://www.biorxiv.org/content/10.1101/2023.04.11.536419v2.full>`_  was uploaded to biorxiv on an improved saguaro cactus genome assembly (SGP5p v2) that had 34,209 protein-coding gene models. Any genome wide analysis of incomplete assemblies would inherently be missing information. Even assemblies that are almost complete can have missing content such as seen when comparing the human reference genomes HG38 and complete `telomere to telomere (T2T) assembly <https://www.science.org/doi/full/10.1126/science.abj6987>`_ in which the number of genes increased from 60,090 to 63,494 and protein coding genes increased from 19,890 to 19,969. 

Gene content is not the only challenge though, acquiring correct gene model structures has proven difficult. One such example is the `Bartlett double haploid pear genome <https://academic.oup.com/gigascience/article-abstract/8/12/giz138/5670615>`_  published in 2019. `Close analysis of several gene families <https://www.frontiersin.org/articles/10.3389/fpls.2022.975942/full>`_ using a combination of multiple sequence alignment of orthologs and RNA-seq alignment revealed a significant number of gene models were incorrect due to assembly errors producing resulting in reading frames shifts and stop codons which likely confounded the gene prediction tools. This is all to say, this is an important step and not performing due deligence in checking gene models and annotation for mistakes can have downstream effects on others. If the genome assembly is fragmented or "low-quality", I personally recommend only uploading the assembly to NCBI. If gene models or annotations are also uploaded they will likely contain errors that will be propagated to other assemblies and analyses. 

That said, let's take a look at what it takes to predict gene models and functional annotations. The primary steps are: repetitive elements identification and masking, gene expression data mapping, protein database alignment, de novo gene prediction, and functional annotation of gene models. While this is the primary pipeline for gene prediction, the genome contains numerous other sequences of interest such as `cis-regulatory elements <https://en.wikipedia.org/wiki/Cis-regulatory_element>`_, `non-coding genes <https://en.wikipedia.org/wiki/Non-coding_DNA>`_, and organelle genomes such as `mitochondria <https://en.wikipedia.org/wiki/Mitochondrial_DNA>`_ and `chloroplasts <https://en.wikipedia.org/wiki/Chloroplast_DNA>`_, and there are a number of tools and analyses that can be done to find those. These additional steps will be covered later though. 

The first step is identifying and masking `repetitive elements <https://en.wikipedia.org/wiki/Repeated_sequence_(DNA)>`_ (RE) and `transposons <https://en.wikipedia.org/wiki/Transposable_element>`_ (TE) in the genome assembly. This is mostly to reduce the computational load during gene prediction as genomes can be up to two-thirds TEs and REs. By masking them, the gene prediction tools only have to search a much smaller sequence space for genes. However, as noted later, TEs have genes and genes are sometimes located in RE such as in the `centromeres <https://www.science.org/doi/full/10.1126/science.abl4178>`_ and `subtelomeres <https://www.sciencedirect.com/science/article/pii/S0022283620300905>`_ of chromosomes. So this step should not be considered lightly and the result should be analyzed with care and an eye for mis-masked regions. 

The next two steps are gene expression data (`rna-seq <https://en.wikipedia.org/wiki/RNA-Seq>`_) mapping and protein database alignment(e.g. `Uniprot <https://en.wikipedia.org/wiki/UniProt>`_ and `Refseq <https://en.wikipedia.org/wiki/RefSeq>`_). Mapping rna-seq data represents gene expression and is key data for finding where genes are located in a genome and what the structure of the gene is (i.e. `exons <https://en.wikipedia.org/wiki/Exon>`_ and `introns <https://en.wikipedia.org/wiki/Intron>`_). Additionally, publicly available protein databases contain millions of protein sequences which can be used to inform where start, stop, and splice sites are located. This can be particularly useful as gene expression data does not always contain information on every gene. This is due to gene expression being tissue specific and if there is no gene expression atlas of all tissue types and developmental stages, then some genes may not have expression data to corroborate their existence. Additionaly, protein databases can contain manually curated gene models where someone has checked the evidence for the gene stucture and corrected any errors.

Genome Annotation has three parts to it. First is called ab initio gene prediction, algorithms search through the genome assembly for sequences that are known to be part of a gene's structure such as start, stop codons, and intron/exon splice sites. Additionally, further gene prediction can be accomplished using what is called homology-based prediction in which genes from closely related species are used to find homologous genes in the genome assembly. The second step is called gene annotation, the predicted gene sequences are compared to databases of known genes and assigned function and identities. The third step is a process called manual curation in which scientists look at the predicted genes and verify the gene structure and annotation is correct and has evidence such as gene expression data to support the gene model.

The first two parts are relatively easy to do with a computer and software. The last part takes time, requires detailed knowledge of the genes of interest, and is considered the most difficult. For my thesis I will attempt to do the first two parts and do the third part on a few select genes that I am interested in.

Expression Data Mapping
----------

The central dogma of molecular biology states that the flow of information in the cell is DNA to RNA to Protein. While not exactly true, it is true that if we want to know what proteins are present in the cell we can look at what rna is present. Additionally, if we want to what genes are "turned on" we can look at rna. We can also use this gene expression data as a way to identify where genes are located in the genome assembly and determine their structure. 

Identifying genes and their structure in the genome assembly is a difficult task. Having some gene expression data increases the confidence in those gene models significantly. 

Currently the majority of gene expression data is in the form of Illumina rna-seq. In the past few years both Oxford Nanopore Technologies and Pacbio have released their own version of long read rna sequencing. In the case of ONT, they directly sequence the rna. There is no need to turn it into DNA or fragment it before sequencing. 

Not all genes are being expressed at the same time and in the same tissue. Thus, when generating gene models for an assembly, it is a good idea to have gene expression data for as many different tissue types and life stages as possible. In the example I used data from all the developmental stages of sea cucumber in addition to a number of different tissue types. 

Not only will the mapped reads be used in gene model creation, but the mapping rates and indel rates will tell me something about the quality of the genome assembly. 

.. note:: :doc:`RNA-Seq mapping <annotation/rna-seq_mapping>`


Protein Database Alignment
--------------------------

.. note:: :doc:`Aligning Proteins from a database to the assembly <annotation/protein_database_alignment>`

Repetitive Element Identification and Masking
----------

Genomes are a quagmire of nucleic acids strung together. Over the billions of year of genome evolution, genomes have gone through multiple rounds of duplication events. Through duplication, all sequences are multiplied, then as evolution takes it course, those sequences mutate or are lost. Additionally, within the genome are sequences which can jump around and duplicate themselves, resulting in further repeats and in some cases, greatly increased genome size. `Structural and sequence diversity of eukaryotic
transposable elements <https://www.jstage.jst.go.jp/article/ggs/advpub/0/advpub_18-00024/_pdf/-char/ja>`_ is a great review of the diversity of these "jumping genes'.

In recent years repetitive elements, once termed "junk dna", have been found to play important roles in gene regulation. Transposable elements, for example, can contain sequences that recruit transcription factors. A paper titled "`Roles of transposable elements in the regulation of mammalian transcription <https://www.nature.com/articles/s41580-022-00457-y>`_" goes into detail regarding the impact TEs can have on gene regulation. A different paper titled `Repetitive Elements in Humans <https://www.mdpi.com/1422-0067/22/4/2072/htm>`_ offers a more detailed review of repetitive elements in humans. `Centromere Repeats: Hidden Gems of the Genome <https://www.mdpi.com/2073-4425/10/3/223>`_

Identifying repetitive elements in genome assemblies is an underappreciated task which is often performed in order to get to the "interesting" parts of the genome such as genes. However, it is looking more and more like genome assembly and annotation projects should also invest time into identifying and annotating the repetitive elements of the genome too. This paper - "`A Roadmap for Understanding the Evolutionary Significance of Structural Genomic Variation <https://www.sciencedirect.com/science/article/abs/pii/S0169534720300768>`_ outlines the importance of studying structural genome variation which includes incorporating repetitive elements into a broader understanding of genome regulation and evolution. 

For genome projects, the goal is identifying and "masking" repetitive elements in the assembly. The first reason is so that the gene modeling tool doesn't have to go search through the 50%-90% of an assembly that is repetitive sequences. The second reason is that some repetitive elements such as transposable elements contain genes within them which are specific to their ability to "jump" around the genome or create duplicates of themselves. Those genes are not particularly interesting to most researchers and shouldn't be included in the gene prediction process. Due to the repetitive nature of transposable elements, these genes may have numerous copies which could impact downstream analysis. 

Below is a link to a page in which I have used two different pipelines to identify and annotate the repetitive elements in a sea cucumber genome. 


.. note:: :doc:`Repetitive Element Identification and Masking <annotation/repetitive_elements>`
.. note:: :doc:`Manual Curation and Annotation of Transposable Elements <annotation/manual_te_annotation>`


Gene Model Prediction
----------

The crux of any genome project is predicting gene models. A `gene model <https://en.wikipedia.org/wiki/Gene_structure>`_ is composed of a 5' untranslated region, start site, an open reading frame containing introns and exons, stop codon, and 3' untranslated region. While we know the general structure of genes, there is enough variation and rule breaking that achieving high confidence gene models has proven to be a difficult task. Tools such as Maker and Braker have attempted to create automated pipelines to simplify the process, but the results have been less than perfect. Recently the Braker group published a tool called `Tserba <https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-021-04482-0>`_ to combine gene predictions using different sets of evidence. One group recently published a paper titled `Foster thy young: enhanced prediction of orphan genes in assembled genomes <https://academic.oup.com/nar/article/50/7/e37/6470686?login=true>`_ in which they found combining the two pipelines (Maker and Braker) improves the identification of "orphan genes" or genes that have evolved recently.

These tools use two different types of data often termed intrinsic and extrinsic. Intrinsic data is going to be data gleaned directly from the organism such as gene expression data or protein sequence data. Extrinsic data can be found in protein databases such as uniprot or orthoDB which have sequences of known structure and function which can be used to "infer" the sequence and structure in your organism of choice. See below for examples of using both datatypes and pipelines 


Additionally, some databases such as Echinobase are trying to systematize their workflow, insure quality, and increase reproducibility by requiring all echinoderm genome projects to go through NCBI's `gene prediction pipeline <https://www.ncbi.nlm.nih.gov/genome/annotation_euk/process/>`_ before allowing the genome, gene models, and annotations to be hosted on echinobase. 

Non-Protein Coding and Other Functional Element Prediction
----------

While most scientists are focused on the glamor of genes. The genome contains numerous other functional elements that need to be identified and annotated before a genome can be said to be to "annotated'. I previously talked about repetitive elements, but there are also non-coding RNAs, organelle genomes, promotors, enhancers, chromatin binding sites, transcription binding sites, etc. 

Non-Coding RNA
--------------

tfam, dfam, rfam

`Integrative rDNAomics—Importance of the Oldest Repetitive Fraction of the Eukaryote Genome <https://www.mdpi.com/2073-4425/10/5/345>`_

`Long Noncoding RNAs and Repetitive Elements: Junk or Intimate Evolutionary Partners? <https://www.sciencedirect.com/science/article/pii/S0168952519301933>`_



Organelle Genome
----------------

Mitochondria and chloroplasts



Transcription and Chromatin Binding Sites
-----------------------------------------



Enhancers and Promoters
-----------------------



Functional Annotation
----------

Discussion about the tools used for functional annotation, like InterProScan, EggNOG-mapper, Panzzer2, etc.

Visualizing and Editing Gene Models and Annotations
----------

Information about tools like Apollo, JBrowse2 for visualizing and editing gene models and annotations...

