Genome Annotation
=====

.. _Genome Annotation:

While creating a high quality, contiguous genome assembly is critical to any genomics or genetics project, a genome assembly is practically useless without identifying and annotating the various components of a genome such as genes, transposable elements, promoters, transcription factor binding sites, etc. This is currently where the real challenge lays in most genome sequencing projects and the consequences of doing this poorly can impact future analyses. Including propagation of those `errors to other genomes <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1715-2>`_ as is `commonly seen on NCBI <https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-020-03855-1>`_ when performing blast searches for homologs and orthologs where up to an estimated 50% of genes contain errors. 

The most common example of errors in a genome assembly is simply gene content and is often seen in the change in gene content as observed in subsequent versions of a genome assembly. One such example is that of the saugaro cactus. In 2017, a `draft assembly (SGP5 v1.3) <https://www.pnas.org/doi/full/10.1073/pnas.1706367114>`_ was published with 28,292 genes. In 2023, a `pre-print <https://www.biorxiv.org/content/10.1101/2023.04.11.536419v2.full>`_  was uploaded to biorxiv on an improved saguaro cactus genome assembly (SGP5p v2) that had 34,209 protein-coding gene models. Any genome wide analysis of incomplete assemblies would inherently be missing information. Even assemblies that are almost complete can have missing content such as seen when comparing the human reference genomes HG38 and complete `telomere to telomere (T2T) assembly <https://www.science.org/doi/full/10.1126/science.abj6987>`_ in which the number of genes increased from 60,090 to 63,494 and protein coding genes increased from 19,890 to 19,969. 

Gene content is not the only challenge though, acquiring correct gene model structures has proven difficult. One such example is the `Bartlett double haploid pear genome <https://academic.oup.com/gigascience/article-abstract/8/12/giz138/5670615>`_  published in 2019. `Close analysis of several gene families <https://www.frontiersin.org/articles/10.3389/fpls.2022.975942/full>`_ using a combination of multiple sequence alignment of orthologs and RNA-seq alignment revealed a significant number of gene models were incorrect due to assembly errors producing resulting in reading frames shifts and stop codons which likely confounded the gene prediction tools. This is all to say, this is an important step and not performing due deligence in checking gene models and annotation for mistakes can have `downstream effects on others <https://academic.oup.com/nar/article/47/21/10994/5580909>`_. If the genome assembly is fragmented or "low-quality", I personally recommend only uploading the assembly to NCBI. If gene models or annotations are also uploaded they will likely contain errors that will be propagated to other assemblies and analyses. 

.. figure:: /annotation/annotation_page_assets/annotation_short_version.png
   :align: left
   :figwidth: 50%

That said, let's take a look at what it takes to predict gene models and functional annotations. The primary steps are: repetitive elements identification and masking, gene expression data mapping, protein database alignment, de novo gene prediction, and functional annotation of gene models. While this is the primary pipeline for gene prediction, the genome contains numerous other sequences of interest such as `cis-regulatory elements <https://en.wikipedia.org/wiki/Cis-regulatory_element>`_, `non-coding genes <https://en.wikipedia.org/wiki/Non-coding_DNA>`_, and organelle genomes such as `mitochondria <https://en.wikipedia.org/wiki/Mitochondrial_DNA>`_ and `chloroplasts <https://en.wikipedia.org/wiki/Chloroplast_DNA>`_, and there are a number of tools and analyses that can be done to find those. These additional steps will be covered later though. 

Repetitive Element Identification and Masking
----------

Transposons and repetitive elements (REs) are integral parts of the genome, often constituting a significant portion, in some cases up to 90%, of the genomic content in many species. These elements, historically considered "junk DNA," are now recognized for their substantial impact on genome function and evolution. Transposons, or transposable elements (TEs), are DNA sequences capable of moving within the genome, and are classified into retrotransposons and DNA transposons. They can influence the genome by causing mutations, altering gene expression, and contributing to genetic diversity. Repetitive elements, including various sequences like satellite DNA, play roles in chromosomal structure and segregation, impacting genome stability.

In the context of gene prediction and annotation, TEs and REs present both challenges and opportunities. To reduce computational load during gene prediction, these elements are often masked, focusing efforts on non-repetitive regions where protein-coding genes are more likely to be found. However, this approach can lead to challenges in annotation, as some genes are located within or near REs, particularly in regions like centromeres and subtelomeres. Overzealous masking might result in missing or misannotating these genes. Furthermore, transposons can contain gene-like sequences or influence the expression of nearby genes, adding complexity to the annotation process.

The evolutionary significance of TEs and REs is now increasingly acknowledged. These elements are instrumental in driving genomic and evolutionary processes, contributing to structural genomic variation critical for understanding genome regulation and evolution. They are involved in creating new genes, regulating existing ones, and contributing to genetic innovation and adaptation. This evolving perspective underscores the necessity of including the annotation and study of TEs and REs as a vital component of genomic research, on par with the study of protein-coding regions. 

* :doc:`An Introduction to Repetitive Elements and Transposable Elements <>`
* :doc:`Using TeTools to Identify and Mask Repetitive Elements and Transposons <>`
* :doc:`Manual Annotation and Curation of Transposable Elements <>`

Expression Data Mapping and Protein Database Alignment
----------

Following the genome assembly masking, the subsequent phases involve the integration of gene expression data, typically obtained through RNA sequencing (RNA-Seq), and aligning protein sequences from established protein databases, such as UniProt and RefSeq. RNA-Seq mapping is critical for pinpointing gene locations within the genome and elucidating gene structures, including the identification of exons and introns.

In addition to RNA-Seq data, leveraging protein databases is also necessary. These databases host millions of protein sequences that serve as references for start and stop codons, and splice sites which might be missed due to noisy gene expression data. This approach is particularly beneficial in instances where RNA-Seq data may not encompass all genes. Gene expression is often tissue-specific, and without a comprehensive atlas covering all tissue types and developmental stages, certain genes might lack corresponding expression data. In such cases, protein databases offer a complementary resource.

Moreover, these protein databases often include manually curated gene models. This manual curation involves thorough verification and correction of gene structures based on available evidence, enhancing the accuracy of gene predictions. By integrating RNA-Seq data with information from protein databases, researchers can achieve a more complete and precise understanding of the genomic landscape, even in areas where gene expression data is limited or absent.

* :doc:`Mapping Gene Expression Data to the Genome Assembly <>`
* :doc:`Aligning Protein Databases to the Genome Assembly <>`

Gene Model Prediction
----------

Gene prediction, the process of identifying regions of a genome that encode genes, is a fundamental aspect of genomic analysis. Modern gene prediction tools like Braker and Maker utilize a masked genome, where repetitive elements are hidden, and incorporate mapped extrinsic data such as RNA-seq and protein alignments. These tools also rely on intrinsic knowledge of gene structure, including start and stop codons, splice sites, and untranslated regions, to construct putative gene models. However, achieving accurate gene prediction remains a complex challenge. One primary difficulty lies in assembly errors, which can lead to reading frame shifts and inaccurately identified stop codons and splice sites. Additionally, the gene content and structure can vary significantly between species due to factors like transposons, recombination, and genome duplication, which rearrange genomes. Furthermore, a lack of comprehensive gene expression data can hinder the ability to identify all genes accurately.

Despite these challenges, automatic methods have become the mainstream approach for initial gene prediction due to the vast size of most genomes and the enormous number of genes they contain. These computational tools provide a foundation for gene annotation but often fall short of the accuracy achieved by manual curation. Manual curation, considered the gold standard, involves a thorough examination of gene expression and protein alignment data to assess and refine gene models. This process is time-consuming and labor-intensive, requiring expert knowledge and a detailed analysis of genomic data. As a result, few projects undertake comprehensive manual curation. However, in recent years, there has been a significant push to manually curate the complete gene sets of specific organisms and particular gene subsets, such as olfactory genes in mice and humans.

* :doc:`Gene Prediction using Braker <>`
* :doc:`Gene Prediction using Maker and Augustus <>`
* :doc:`Combining Evidence using EvidenceModler or Tserba <>`
* :doc:`Visualizing and Editing Gene Models <>`

Functional Annotation and Analysis
----------

Functional annotation in genome analysis is a crucial process that involves assigning biological functions to gene products, such as proteins or RNAs. This step is essential for transforming raw sequence data into meaningful biological information. The significance of functional annotation lies in its ability to facilitate comparative studies across different species, enable functional enrichment analyses, and aid in understanding the evolutionary and functional dynamics of genomes. By providing insights into the roles of genes in various biological processes and conditions, functional annotations enrich our understanding of gene functionality within the broader context of cellular and organismal biology.

The process of functional annotation employs several methods and tools. Gene Ontology (GO) prediction is a primary approach, where GO terms describing biological processes, cellular components, and molecular functions are assigned to genes based on sequence similarity to known genes. Tools like Blast2GO or InterProScan are commonly used for this purpose. Another approach is the identification of protein domains using tools such as Pfam and SMART. These tools detect specific structural motifs within proteins that are often associated with particular functions. Homology-based methods, notably BLAST searches against comprehensive databases like NCBI’s non-redundant database or UniProt, are also widely used. These methods find orthologs or closely related sequences, allowing researchers to infer the functions of newly annotated genes based on their known relatives.

However, functional annotation is not without its challenges. A significant issue is the propagation of errors; if an initial annotation in a database is incorrect, this error can be perpetuated and magnified across subsequent studies and databases. This problem is particularly acute with homology-based methods that rely heavily on existing annotations. To counter this, manual verification of gene annotations is strongly recommended, especially for critical research areas. This involves a detailed comparison of the gene with known orthologs, examination of literature, and consideration of experimental data, ensuring a more accurate and reliable annotation.

In practice, a hierarchical approach is often employed in functional annotation. Typically, researchers first attempt to assign GO terms. If this is not possible, they then look for known protein domains. When both these approaches yield no results, homology-based methods are used. If there is still no clear annotation, genes are often labeled as “uncharacterized” or “hypothetical proteins.” This cautious, step-wise approach helps in mitigating the risk of propagating errors.

The field of functional annotation, while rich in tools and methods, lacks a unified standard, leading to a variety of approaches with varying levels of accuracy and reliability. Researchers must judiciously choose tools and methods, balancing the need for comprehensive annotation with the risk of error. This careful and considered approach to functional annotation is vital for the accurate interpretation of genomic data, ultimately advancing our understanding of biological systems and their implications in health and disease. The continuous development and refinement of annotation tools, coupled with rigorous validation practices, are essential to ensure the reliability and utility of genomic annotations in scientific research.

* :doc:`Functional Annotation using Gene Ontology <>`
* :doc:`Protein Domain Annotation using InterProScan and EggNOG-mapper <>`
* :doc:`Ortholog search using Blast and NCBI <>`
* :doc:`Evalauting functional annotations <>`


Non-Coding RNA
--------------

The human genome, a vast and intricate blueprint of life, is predicted to contain over 60,000 genes, yet less than 20,000 of these are believed to be protein-coding. This disparity raises an intriguing question: what functions do the remaining genes serve? The answer lies in the realm of non-coding RNAs (ncRNAs), which play critical roles in the molecular and cellular machinery of organisms. Non-coding RNAs come in various forms – from long non-coding RNAs (lncRNAs) involved in transcriptional regulation to microRNAs (miRNAs) and small interfering RNAs (siRNAs) that modulate gene expression post-transcriptionally. 

In addition to their role in gene regulation, ncRNAs are pivotal in `epigenetic modulation <https://www.nature.com/articles/s41580-022-00566-8>`_, altering gene expression patterns over time through mechanisms like DNA methylation and histone modification. This function is crucial during development and differentiation in multicellular organisms. Structurally, ncRNAs such as ribosomal RNA (rRNA) and transfer RNA (tRNA) are fundamental components of the protein synthesis machinery. They are also involved in essential cellular processes including cell cycle regulation, apoptosis, and stress responses. The versatility and indispensability of ncRNAs in biological systems are thus evident.

Moreover, the significance of ncRNAs extends to disease contexts, with their dysregulation being linked to various diseases including `cancers, neurological disorders, and heart diseases <https://link.springer.com/article/10.1007/s10142-022-00947-4>`_. This association offers insights into disease mechanisms and potential therapeutic targets. Additionally, ncRNAs facilitate intercellular communication, often found in exosomes and influencing neighboring or distant cells. Their conservation across species underscores their evolutionary importance. Therefore, identifying and properly annotating ncRNAs in the genome assembly is not just a matter of cataloging; it's a crucial step in unraveling the complex orchestration of life at the molecular level, revealing intricate mechanisms fundamental to both health and disease. The ongoing discovery and study of ncRNAs continue to illuminate the vast, uncharted territories of non-protein-coding genes, offering profound insights into the complexities of `genetic regulation and function <https://www.sciencedirect.com/science/article/abs/pii/S1874939919302160>`_.

Because of the difficulty in differentiating ncRNA from protein-coding RNA, a combination of bioinformatic tools and ncRNA databases are utilized to identify and annotate ncRNA. Databases include: `NONCODE <http://www.noncode.org/>`_, `RNAcentral <https://rnacentral.org/>`_, `FANTOM <https://fantom.gsc.riken.jp/>`_, `RFAM <https://rfam.org/>`_, etc. Tools for identifying and annotating ncRNA are also numerous, popular ones include `Infernal <http://eddylab.org/infernal/>`_ and ncRNA type-specific tools such as for `tRNA <http://gtrnadb.ucsc.edu/>`_, `lncRNA <https://academic.oup.com/nar/article/45/8/e57/2798184?login=false>`_, `miRs <https://tools4mirs.org/>`_, the list goes on (`piRNA, tsRNA, rRNA, snoRNA, sRNA, etc <https://pubmed.ncbi.nlm.nih.gov/29730207/>`_)

* :doc:`An attempt at finding all the ncRNAs in an assembly <>`

