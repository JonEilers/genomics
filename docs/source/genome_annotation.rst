Genome Annotation
=====

.. _Genome Annotation:

While creating a high quality, contiguous genome assembly is critical to any genomics or genetics project, a genome assembly is practically useless without identifying and annotating the various components of a genome such as genes, transposable elements, promoters, transcription factor binding sites, etc. This is currently where the real challenge lays in most genome sequencing projects and the consequences of doing this poorly can impact future analyses. Including propagation of those `errors to other genomes <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1715-2>`_ as is `commonly seen on NCBI <https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-020-03855-1>`_ when performing blast searches for homologs and orthologs where up to an estimated 50% of genes contain errors. 

The most common example of errors in a genome assembly is simply gene content and is often seen in the change in gene content as observed in subsequent versions of a genome assembly. One such example is that of the saugaro cactus. In 2017, a `draft assembly (SGP5 v1.3) <https://www.pnas.org/doi/full/10.1073/pnas.1706367114>`_ was published with 28,292 genes. In 2023, a `pre-print <https://www.biorxiv.org/content/10.1101/2023.04.11.536419v2.full>`_  was uploaded to biorxiv on an improved saguaro cactus genome assembly (SGP5p v2) that had 34,209 protein-coding gene models. Any genome wide analysis of incomplete assemblies would inherently be missing information. Even assemblies that are almost complete can have missing content such as seen when comparing the human reference genomes HG38 and complete `telomere to telomere (T2T) assembly <https://www.science.org/doi/full/10.1126/science.abj6987>`_ in which the number of genes increased from 60,090 to 63,494 and protein coding genes increased from 19,890 to 19,969. 

Gene content is not the only challenge though, acquiring correct gene model structures has proven difficult. One such example is the `Bartlett double haploid pear genome <https://academic.oup.com/gigascience/article-abstract/8/12/giz138/5670615>`_  published in 2019. `Close analysis of several gene families <https://www.frontiersin.org/articles/10.3389/fpls.2022.975942/full>`_ using a combination of multiple sequence alignment of orthologs and RNA-seq alignment revealed a significant number of gene models were incorrect due to assembly errors producing resulting in reading frames shifts and stop codons which likely confounded the gene prediction tools. This is all to say, this is an important step and not performing due deligence in checking gene models and annotation for mistakes can have downstream effects on others. If the genome assembly is fragmented or "low-quality", I personally recommend only uploading the assembly to NCBI. If gene models or annotations are also uploaded they will likely contain errors that will be propagated to other assemblies and analyses. 

.. figure:: /annotation/annotation_page_assets/annotation_short_version.png
   :align: left
   :figwidth: 50%

That said, let's take a look at what it takes to predict gene models and functional annotations. The primary steps are: repetitive elements identification and masking, gene expression data mapping, protein database alignment, de novo gene prediction, and functional annotation of gene models. While this is the primary pipeline for gene prediction, the genome contains numerous other sequences of interest such as `cis-regulatory elements <https://en.wikipedia.org/wiki/Cis-regulatory_element>`_, `non-coding genes <https://en.wikipedia.org/wiki/Non-coding_DNA>`_, and organelle genomes such as `mitochondria <https://en.wikipedia.org/wiki/Mitochondrial_DNA>`_ and `chloroplasts <https://en.wikipedia.org/wiki/Chloroplast_DNA>`_, and there are a number of tools and analyses that can be done to find those. These additional steps will be covered later though. 

Repetitive Element Identification and Masking
----------

The first step is identifying and masking `repetitive elements <https://en.wikipedia.org/wiki/Repeated_sequence_(DNA)>`_ (RE) and `transposons <https://en.wikipedia.org/wiki/Transposable_element>`_ (TE) in the genome assembly. This is mostly to reduce the computational load during gene prediction as genomes can be up to two-thirds or more TEs and REs. By masking them, the gene prediction tools only have to search a much smaller sequence space for genes. However, as noted later, TEs have genes and genes are sometimes located in RE such as in the `centromeres <https://www.science.org/doi/full/10.1126/science.abl4178>`_ and `subtelomeres <https://www.sciencedirect.com/science/article/pii/S0022283620300905>`_ of chromosomes. So this step should not be considered lightly and the result should be analyzed with care and an eye for mis-masked regions. 

Identifying repetitive elements in genome assemblies is an underappreciated task which is often performed in order to get to the "interesting" parts of the genome such as genes. However, it is looking more and more like genome assembly and annotation projects should also invest time into identifying and annotating the repetitive elements of the genome too. This paper - "`A Roadmap for Understanding the Evolutionary Significance of Structural Genomic Variation <https://www.sciencedirect.com/science/article/abs/pii/S0169534720300768>`_ outlines the importance of studying structural genome variation which includes incorporating repetitive elements into a broader understanding of genome regulation and evolution. 

* :doc:`An Introduction to Repetitive Elements and Transposable Elements </annotation/repetitive_elements>`
* :doc:`Using TeTools to Identify and Mask Repetitive Elements and Transposons </annotation/tetools>`
* :doc:`Manual Annotation and Curation of Transposable Elements </annotation/manual_te_annotation>`

Expression Data Mapping and Protein Database Alignment
----------

The next two steps are gene expression data (`rna-seq <https://en.wikipedia.org/wiki/RNA-Seq>`_) mapping and protein database alignment(e.g. `Uniprot <https://en.wikipedia.org/wiki/UniProt>`_ and `Refseq <https://en.wikipedia.org/wiki/RefSeq>`_). Mapping rna-seq data represents gene expression and is key data for finding where genes are located in a genome and what the structure of the gene is (i.e. `exons <https://en.wikipedia.org/wiki/Exon>`_ and `introns <https://en.wikipedia.org/wiki/Intron>`_). Additionally, publicly available protein databases contain millions of protein sequences which can be used to inform where start, stop, and splice sites are located. This can be particularly useful as gene expression data does not always contain information on every gene. This is due to gene expression being tissue specific and if there is no gene expression atlas of all tissue types and developmental stages, then some genes may not have expression data to corroborate their existence. Additionaly, protein databases can contain manually curated gene models where someone has checked the evidence for the gene stucture and corrected any errors.

* :doc:`Mapping Gene Expression Data to the Genome Assembly </annotation/rna-seq_mapping>`
* :doc:`Aligning Protein Databases to the Genome Assembly </annotation/protein_database_alignment>`

Gene Model Prediction
----------

Finally, gene prediction tolls utilize the masked genome along with the mapped extrinsic data and intrisic knowledge of how genes are structured (start and stop codons, splices sites, etc) to find putative genes and predict the structure of the gene, aka gene model. Once complete, the final step is to associate functional annotations to the predicted genes. There are two approaches to this, the first is uses homology to known genes to assign either a name or function to the gene. The second is to look at the amino acid seqeunce, compare to known protein domains, and predict what that protein does. This last step is arguably the least accurate of all the steps as errors from low-quality genomes and incomplete download of protein fucntional domains tends to result in questionable annotations. However, it is a starting point for manual curation and analyse. 

Manual curation of gene models is considered the gold standard for producing high-confidence gene models and annotations. It also happens to be very time consuming as it requires examining gene expression data and protein alignment data to assess the gene model and then manually modify and submit the change. Suffice to say very few projects do this. However, recent years have seen significant effort to do this for the complete gene set of organisms such as `C. briggsae <https://link.springer.com/article/10.1186/s12864-023-09582-0>`_ and in some cases for subsets such as `olfactory genes of mice and men <https://bmcgenomics.biomedcentral.com/articles/10.1186/s12864-020-6583-3>`_.

A `gene model <https://en.wikipedia.org/wiki/Gene_structure>`_ is composed of a 5' untranslated region, start site, an open reading frame containing introns and exons, stop codon, and 3' untranslated region. While we know the general structure of genes, there is enough variation and rule breaking that achieving high confidence gene models has proven to be a difficult task. Tools such as Maker and Braker have attempted to create automated pipelines to simplify the process, but the results have been less than perfect. Recently the Braker group published a tool called `Tserba <https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-021-04482-0>`_ to combine gene predictions using different sets of evidence. One group recently published a paper titled `Foster thy young: enhanced prediction of orphan genes in assembled genomes <https://academic.oup.com/nar/article/50/7/e37/6470686?login=true>`_ in which they found combining the two pipelines (Maker and Braker) improves the identification of "orphan genes" or genes that have evolved recently.

* :doc:`Gene Prediction using Braker <annotation/braker_gene_prediction>`
* :doc:`Gene Prediction using Maker and Augustus <annotation/maker_gene_prediction>`
* :doc:`Combining Evidence using EvidenceModler or Tserba <annotation/combining_evidence>`
* :doc:`Visualizing and Editing Gene Models <annotation/manual_curation>`

Functional Annotation and Analysis
----------

Once there the gene models look good, it is important to assign some functional annotation to each one. There are two reasons for this, the first is that it can save a lot of time if someone is looking for an ortholog. The second is that these functional annotation can then be fed into various tools to look for enrichment of specific functions, study how the genome and genes have evolved, or identify changes in function. However, there is a very large caveat to these annotations, they are likely not accurate. As noted multiple times throughout the annotation section, erroneaous annotations from previous projects have been propagated by automatic annotation tools to other genome annotations and have thus become established in databases as "truth". It is necessary to manually inspect the gene and compare it to known orthologs in order to trust it. This is all to say, use functional annotations as a starting point, but don't put your career on the line without verifying the truth of those annotations. 

There are essentially three different approaches to functional annotation: use Gene Ontology to prediction whole gene function, use protein domain based methods to predict what parts of the protein do, and blast against a database of genes to find orthologs and assign the same name. The last is how annotation errors survive and thrive. Whereas gene model prediction methods and tools are starting to mature, functional annotation is still something of a wild west with new tools and methods published frequently and no real standard has been established regarding quality. However, what can be said is that Gene Ontology names are assigned first, if there is no confident GO, then functional domains are used, then blast results, and finally if there is no ortholog then "unmamed protein".

* :doc:`Functional Annotation using Gene Ontology <annotation/gene_ontology>`
* :doc:`Protein Domain Annotation using InterProScan and EggNOG-mapper <annotation/protein_domain_annotation>`
* :doc:`Ortholog search using Blast and NCBI <annotation/annotation_via_ortholog>`
* :doc:`Evalauting functional annotations <annotation/functional_evaluation>`

Non-Protein Coding and Other Functional Element Prediction
----------

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

