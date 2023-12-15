Genome Annotation
=====

.. _Genome Annotation:

While creating a high quality, contigious genome assembly is critical to any genomics or genetics project, a genome assembly is practically useless without identifying and characterizing the structural components of a genome such as genes, transposable elements, promoters, transcription factor binding sites, etc. This is currently where the real challange lays in most genome sequencing projects and the consequences of doing this part poorly can have dramatic affects on future projects. 

There are several parts to genome annotation: repetitive elements identification and masking, gene expression data mapping, de novo gene prediction, gene annotation, transcription site identification, and various post annotation analysis such as gene regulatory network analysis looking at gene interaction or tissue specific expression. 

The first two steps to do are gene expression data (rna-seq) mapping and repetetitive element masking. Mapping rna-seq data to the genome assembly is important as it provides evidence that indeed a gene can be found here. Additionally, transcriptome assembly can be performed and the results mapped to the genome with much the same result but different caveats. Repetitive element masking first identifies repeats and transposable elements in the genome assembly and then tags them so that the gene prediction software doesn't confuse them with genes. Transposable elements have open reading frames and genes within them, but these genes are not of interest. Once these two things have been done, the next step is genome prediction and annotation. 

Genome Annotation has three parts to it. First is called ab initio gene prediction, algorithms search through the genome assembly for sequences that are known to be part of a gene's structure such as start, stop codons, and intron/exon splice sites. Additionally, further gene prediction can be accomplished using what is called homology-based prediction in which genes from closely related species are used to find homologous genes in the genome assembly. The second step is called gene annotation, the predicted gene sequences are compared to databases of known genes and assigned function and identities. The third step is a process called manual curation in which scientists look at the predicted genes and verify the gene structure and annotation is correct and has evidence such as gene expression data to support the gene model.

The first two parts are relatively easy to do with a computer and software. The last part takes time, requires detailed knowledge of the genes of interest, and is considered the most difficult. For my thesis I will attempt to do the first two parts and do the third part on a few select genes that I am interested in.

Expression Data Mapping
----------

The central dogma of molecular biology states that the flow of information in the cell is DNA to RNA to Protein. While not exactly true, it is true that if we want to know what proteins are present in the cell we can look at what rna is present. Additionally, if we want to what genes are "turned on" we can look at rna. We can also use this gene expression data as a way to identify where genes are located in the genome assembly and determine their structure. 

Identifying genes and their structure in the genome assembly is a difficult task. Having some gene expression data increases the confidence in those gene models significantly. 

Currently the majority of gene expression data is in the form of Illumina rna-seq. In the past few years both Oxford Nanopore Technologies and Pacbio have released their own version of long read rna sequencing. In the case of ONT, they directly sequence the rna. There is no need to turn it into DNA or fragment it before sequencing. 

Not all genes are being expressed at the same time and in the same tissue. Thus, when generating gene models for an assembly, it is a good idea to have gene expression data for as many different tissue types and life stages as possible. In the example I used data from all the developmental stages of sea cucumber in addition to a number of different tissue types. 

Not only will the mapped reads be used in gene model creation, but the mapping rates and indel rates will tell me something about the quality of the genome assembly. 

.. warning:: :doc:`In Progress - rna-seq mapping <annotation/rna-seq_mapping>`


Protein Database Alignment
--------------------------

.. warning:: :doc:`In Progress - Aligning Proteins from a database to the assembly <annotation/protein_database_alignment>`

Repetitive Element Identification and Masking
----------

Genomes are a quagmire of nucleic acids strung together. Over the billions of year of genome evolution, genomes have gone through multiple rounds of duplication events. Through duplication, all sequences are multiplied, then as evolution takes it course, those sequences mutate or are lost. Additionally, within the genome are sequences which can jump around and duplicate themselves, resulting in further repeats and in some cases, greatly increased genome size. `Structural and sequence diversity of eukaryotic
transposable elements <https://www.jstage.jst.go.jp/article/ggs/advpub/0/advpub_18-00024/_pdf/-char/ja>`_ is a great review of the diversity of these "jumping genes'.

In recent years repetitive elements, once termed "junk dna", have been found to play important roles in gene regulation. Transposable elements, for example, can contain sequences that recruit transcription factors. A paper titled "`Roles of transposable elements in the regulation of mammalian transcription <https://www.nature.com/articles/s41580-022-00457-y>`_" goes into detail regarding the impact TEs can have on gene regulation. A different paper titled `Repetitive Elements in Humans <https://www.mdpi.com/1422-0067/22/4/2072/htm>`_ offers a more detailed review of repetitive elements in humans. `Centromere Repeats: Hidden Gems of the Genome <https://www.mdpi.com/2073-4425/10/3/223>`_

Identifying repetitive elements in genome assemblies is an underappreciated task which is often performed in order to get to the "interesting" parts of the genome such as genes. However, it is looking more and more like genome assembly and annotation projects should also invest time into identifying and annotating the repetitive elements of the genome too. This paper - "`A Roadmap for Understanding the Evolutionary Significance of Structural Genomic Variation <https://www.sciencedirect.com/science/article/abs/pii/S0169534720300768>`_ outlines the importance of studying structural genome variation which includes incorporating repetitive elements into a broader understanding of genome regulation and evolution. 

For genome projects, the goal is identifying and "masking" repetitive elements in the assembly. The first reason is so that the gene modeling tool doesn't have to go search through the 50%-90% of an assembly that is repetitive sequences. The second reason is that some repetitive elements such as transposable elements contain genes within them which are specific to their ability to "jump" around the genome or create duplicates of themselves. Those genes are not particularly interesting to most researchers and shouldn't be included in the gene prediction process. Due to the repetitive nature of transposable elements, these genes may have numerous copies which could impact downstream analysis. 

Below is a link to a page in which I have used two different pipelines to identify and annotate the repetitive elements in a sea cucumber genome. 


.. warning:: :doc:`In Progress - Repetitive Element Identification and Masking <annotation/repeats>`
.. warning:: :doc:`In Progress - Manual Curation and Annotation of Transposable Elements <annotation/manual_te_annotation>`


Gene Model Prediction
----------

The crux of any genome project is predicting gene models. A `gene model <https://en.wikipedia.org/wiki/Gene_structure>`_ is composed of a 5' untranslated region, start site, an open reading frame containing introns and exons, stop codon, and 3' untranslated region. While we know the general structure of genes, there is enough variation and rule breaking that achieving high confidence gene models has proven to be a difficult task. Tools such as Maker and Braker have attempted to create automated pipelines to simplify the process, but the results have been less than perfect. Recently the Braker group published a tool called `Tserba <https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-021-04482-0>`_ to combine gene predictions using different sets of evidence. One group recently published a paper titled `Foster thy young: enhanced prediction of orphan genes in assembled genomes <https://academic.oup.com/nar/article/50/7/e37/6470686?login=true>`_ in which they found combining the two pipelines (Maker and Braker) improves the identification of "orphan genes" or genes that have evolved recently.

These tools use two different types of data often termed intrinsic and extrinsic. Intrinsic data is going to be data gleaned directly from the organism such as gene expression data or protein sequence data. Extrinsic data can be found in protein databases such as uniprot or orthoDB which have sequences of known structure and function which can be used to "infer" the sequence and structure in your organism of choice. See below for examples of using both datatypes and pipelines 

.. warning:: :doc:`To do - Gene Prediction <annotation/gene-prediction>`

Additionally, some databases such as Echinobase are trying to systematize their workflow, insure quality, and increase reproducibility by requiring all echinoderm genome projects to go through NCBI's `gene prediction pipeline <https://www.ncbi.nlm.nih.gov/genome/annotation_euk/process/>`_ before allowing the genome, gene models, and annotations to be hosted on echinobase. 

Non-Protein Coding and Other Functional Element Prediction
----------

While most scientists are focused on the glamor of genes. The genome contains numerous other functional elements that need to be identified and annotated before a genome can be said to be to "annotated'. I previously talked about repetitive elements, but there are also non-coding RNAs, organelle genomes, promotors, enhancers, chromatin binding sites, transcription binding sites, etc. 

Non-Coding RNA
--------------

tfam, dfam, rfam

`Integrative rDNAomics—Importance of the Oldest Repetitive Fraction of the Eukaryote Genome <https://www.mdpi.com/2073-4425/10/5/345>`_

`Long Noncoding RNAs and Repetitive Elements: Junk or Intimate Evolutionary Partners? <https://www.sciencedirect.com/science/article/pii/S0168952519301933>`_


.. warning:: :doc:`To do - Non-coding RNA Identification and Annotation <annotation/rfam_infernal>`

Organelle Genome
----------------

Mitochondria and chloroplasts

.. warning:: `To do - Organelle Gene Prediction <Organelle/>`_

Transcription and Chromatin Binding Sites
-----------------------------------------

.. warning:: `To do - Transcription and Chromatin Binding Sites <binding-sites/>`_

Enhancers and Promoters
-----------------------

.. warning:: `To do - Enhancer and Promoter Identification <functional-elements/>`_

Functional Annotation
----------

Discussion about the tools used for functional annotation, like InterProScan, EggNOG-mapper, Panzzer2, etc.

Visualizing and Editing Gene Models and Annotations
----------

Information about tools like Apollo, JBrowse2 for visualizing and editing gene models and annotations...

