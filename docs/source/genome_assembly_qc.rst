Genome Assembly QC
===============

.. _Genome Assembly QC:

Genome assembly quality control is arguably the most critical step of any genome sequencing project. If this is skipped or executed poorly, the resulting gene model prediction and genome analyses will contain errors and scientists will make erroneous conclusions. The `Earth Biogenome Project <https://www.earthbiogenome.org/report-on-assembly-standards>`_, an ambitious initiative aimed at sequencing, cataloging, and characterizing the genomes of all eukaryotic species, has set forth comprehensive quality control standards for genome assemblies. According to these standards, detailed in their report on assembly standards, genome assemblies should meet the following criteria:

* A false duplication rate of less than 5%.
* A K-mer completeness of over 90%.
* Over 90% of the sequence assigned to candidate chromosomal sequences.
* More than 90% of single-copy conserved genes, such as those identified by BUSCO, should be complete and single copy.
* Above 90% of transcripts from the same organism should be mappable.
* The elimination of biological contamination.
* Clear separation and identification of organelle genomes.
* Phasing, which involves the explicit identification of a primary assembly and an alternate haplotype assembly.

.. figure:: assembly_qc/images/graphviz/genome_quality.png
   :align: left
   :width: 80%
   :figwidth: 40%


These standards are underscored by the 2021 publication "`Towards complete and error-free genome assemblies of all vertebrate species <https://www.nature.com/articles/s41586-021-03451-0>`_" which delves into the complexities and challenges encountered in sequencing and assembling six vertebrate genomes to the exacting standards set by the international Genome 10K (G10K) consortium.

Further insights into the intricate processes and methodologies required to achieve high-quality genome assemblies, such as the telomere-to-telomere human genome assembly, are explored in the 2022 paper "`Chasing perfection: validation and polishing strategies for telomere-to-telomere genome assemblies <https://www.nature.com/articles/s41592-022-01440-3>`_" This paper discusses the pioneering effort to achieve a complete human genome assembly, which revealed an additional 151 megabases of sequence information. This newly included sequence data, predominantly from pericentromeric and subtelomeric regions, recent segmental duplications, ampliconic gene arrays, and ribosomal DNA (rDNA) arrays, sheds light on areas of the human genome previously challenging to assemble due to their repetitive content. These regions are not only complex but also harbor sequences and genes crucial to genomic function. For a more detailed exploration of this topic, the paper "`The complete sequence of a human genome <https://www.science.org/doi/full/10.1126/science.abj6987>`_" offers in-depth information.

In summary, the pursuit of a high-quality genome assembly requires significant due diligence, often making it the most time-intensive and resource-demanding aspect of any genome sequencing project. However, this meticulous approach is essential; neglecting it would fall into the realm of poor scientific practice. Regrettably, there is a non-trivial segment within the biological research community that prioritizes rapid publication and the associated prestige over rigorous methodology. This haste can lead to the release of substandard genome assemblies, undermining the integrity and value of the research. Such practices highlight the importance of adhering to established standards and methodologies to ensure the reliability and utility of genomic data. 

It's important to distinguish between the lack of rigorous methodology and the constraints of limited resources and funding in genome assembly projects. In scenarios where researchers are constrained by budget and resources, it may not be feasible to achieve a high-quality genome assembly. In such cases, if there is no existing assembly, it remains a sound scientific practice to publish the available assembly, provided it is accompanied by appropriate caveats. This approach ensures that even with limitations, the research contributes valuable data to the scientific community, while clearly communicating the potential limitations of the assembly due to resource constraints. This transparency is crucial for maintaining scientific integrity and for guiding future research that may build upon or refine these preliminary assemblies.

Summary Statistics
-------------------

The N50 metric is a commonly referenced standard for assessing genome contiguity, but it is often misconstrued to as an "average" contig length. `wikipedia <https://en.wikipedia.org/wiki/N50,_L50,_and_related_statistics#N50>`_ get's it right, but unfortunately the explanation may be a little confusing. To succinctly describe N50 without delving into the details, it is essential to understand that this metric does imply that the majority of the contigs (sequences) in an assembly are shorter than the N50 value. The N50 represents a length such that 50% (half) of the total length of the assembly is contained in sequences that are shorter than the N50 value. Conversely, this means that half the genome length is contained in contigs/scaffolds that are longer than the N50 value. 

For example, an N50 value of 9kb suggests that half of the assembled genome is in fragments that are 9kb or longer. This metric is crucial for gene prediction, as a higher N50 often indicates better contiguity, facilitating the identification of complete genes. However, genes that are longer and more complex, like `titin <https://en.wikipedia.org/wiki/Titin>`_, will likely be fragmented in an assembly with an N50 of this size. 

An essential aspect to consider in genome assembly evaluation is the distinction between contig N50 and scaffold N50 metrics. Contigs are contiguous sequences of DNA, while scaffolds represent a higher-order assembly, comprising multiple contigs linked by gaps. These gaps, often represented by a series of 'N' characters in the sequence, indicate unknown or unsequenced regions between contigs. The assembly tool determines the order and orientation of these contigs but leaves the gaps to signify areas of uncertainty.

In summary statistics of genome assemblies, both contig N50 and scaffold N50 values are usually presented. However, the contig N50 metric is particularly crucial, as it provides a more direct measure of the assembly's quality in terms of gene model predictions. Higher contig N50 indicates a larger number of longer contigs and, consequently, a higher likelihood of capturing complete genes. In contrast, scaffold N50, while useful for understanding the broader structure of the genome, may overestimate the assembly quality due to the inclusion of gap regions. Therefore, contig N50 serves as a more reliable indicator of the potential accuracy and completeness of gene predictions in a given genome assembly.

* :doc:`Summary Statistics Via a Variety of Tools <assembly_qc/summary-stats>`

Checking for Complete Conserved Genes
------------------------

Checking `BUSCO <https://busco.ezlab.org/>`_ (Benchmarking Universal Single-Copy Orthologs) scores is an effective method to gauge the level of fragmentation in a genome assembly and to set expectations for gene prediction accuracy. The principle behind BUSCO is grounded in the identification of a set of genes that are highly conserved across various kingdoms and phyla of life and usually only one copy of the gene is present. Due to their conserved nature, it's a reasonable assumption that these genes should be present in the genome of most organisms. Therefore, the presence, completeness, and duplication status of these genes in a genome assembly can be a strong indicator of the overall assembly quality. If a significant proportion of these conserved genes are found intact and unduplicated in your assembly, it suggests that a similar percentage of the entire genome is likely well-assembled and reliable for further analysis.

However, it's important to recognize that BUSCO scores are not representative of a 'best case scenario' but should be considered as a targeted, yet somewhat random, sample of the assemblies gene content. This means while BUSCO scores provide valuable insights into the assembly quality, they do not necessarily account for all possible structural gene variations or complexities. Therefore, while BUSCO scores are a valuable tool in genomic analysis, they should be interpreted within the broader context of the genome's overall characteristics and other quality metrics. However, if BUSCO scores are low, it is `highly indicative of problems with the genome assembly <https://onlinelibrary.wiley.com/doi/abs/10.1111/1755-0998.13364>`_. 

* :doc:`Assembly Quality Assessment using BUSCO Analysis <assembly_qc/assembly_busco>`

Assembly Contamination and Quality
----------------------------------
In addition to examining summary statistics and assessing BUSCO scores, it is crucial to screen for potential contamination in genome assemblies. Contamination can occur during the sequencing process, where DNA from other organisms inadvertently gets included in the sample (e.g., epibionts on gill tissue of king crabs or endophytes that live within plant tissue). Identifying and addressing such contamination is vital for the integrity of the genome assembly.

One common approach to detecting contamination involves using protein databases like UniProt or RefSeq. By conducting a BLAST (Basic Local Alignment Search Tool) analysis against the assembly with these databases, you can identify the organisms that correspond to the highest number of sequence matches. If the top hits are from organisms closely related to your species of interest, the assembly is likely accurate. Because the organism that is having its genome sequenced likely does not have sequences in UniProt or RefSeq, it is expected that closely related species will be present. However, if there are significant matches to distantly related organisms, this could indicate contamination. In such cases, it might be necessary to revisit the preprocessing and filtering stages of the raw sequencing data before assembly or simply remove the contigs containing the potential contamination.

Additionally, k-mer analysis can be used to filter out potential contamination in the raw sequence data and genome assembly. A k-mer is a sequence string of 'k' consecutive nucleotides. In the context of DNA sequencing, it refers to all the possible subsequences (of length 'k') that can be derived from a DNA sequence. For example, in a given DNA sequence, if 'k' is set to 3 (thus making it a 3-mer), and the sequence is 'ATGCA', the 3-mers would be 'ATG', 'TGC', and 'GCA'. For detecting potential contamination, k-mer analysis looks for k-mers that have unusual GC content. This is especially effective if there are multiple sequence datasets from different tissues, which include whole-genome data using both short and long reads. Comparing these datasets should reveal any unexpected reads which can then be removed prior to assembly or after assembly. 

Recent years have also seen the development of quality value scores (QV) which vary depending on which tool is used. `Merqury <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02134-9>`_ calculates a QV score which represents a "log-scaled probability of error per a base in the assembly". In the case of `Inspector <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02527-4>`_, QV is "calculated based on the identified structural and small-scale errors scaled by the total base pairs of the assemblies ". While the method for calculating these two different QV scores are different, they do correlate with each other. 

* :doc:`Assembly Contamination and Quality Analysis <assembly_qc/contamination>`
* :doc:`Calculating Genome Assembly Quality Value Scores <assembly_qc/genome_quality>`

Polishing and Gap Closing
-------------------------

Once you have an assembly that is as good as it'll get, it might be possible to squeeze a little more out of your data using gap closing and polishing tools. However, just like with read trimming, doing either gap closing or polishing can result in an assembly that was worse than what you started with. I also want to add that overzealous use of gap closing or polishing can result in poor assemblies. This is a huge problem when these assemblies are then uploaded into NCBI and used as references genomes for other projects. Most researchers do not have the skill, knowledge, or time to check that the assembly or genes from assemblies are trustworthy, potentially resulting in a lot of frustration and wasted time and money. So proceed with caution. 

Gap Closing
~~~~~~~~~~~

The most common method for generating scaffolds is through the use of Hi-C to phase and orient contigs into chromosome scale scaffolds. Gaps are created in the assembly composed of Ns which represent the unknown bases and the number of Ns representing the distance between contigs (ideally). `Here <https://academic.oup.com/bib/article/22/5/bbab033/6149347>`_ is a review that goes into detail regarding various scaffolding approaches and the caveats associated with each. These gaps can potentially be filled using gap closing tools. This may require new long read data, and in some cases ultra-long reads sequenced using nanopore sequencing. Doesn't hurt to try using either previously used reads though. 

Polishing
~~~~~~~~~~~

Polishing is probably one of the most overlooked and underappreciated steps of genome assembly. As a result, what appear to be high quality genomes and gene models are published that contain numerous errors. Do not skip this step. Also, have a list of gene models to manually check for gene models errors as this will be more revealing than output summary statistics from the polishing tools. Polishing removes insertions, deletions, and adapter contamination that may have crept into the genome assembly. Examples of what this looks like can be found in the paper `Chasing perfection: validation and polishing strategies for telomere-to-telomere genome assemblies <https://www.nature.com/articles/s41592-022-01440-3>`_. Polishing can be accomplished using either long read or short read data. Short read data has a much higher accuracy and as such can correct short sequences. However, long reads can fix longer, structural errors. Assembly polishing is typically an iterative process requiring anywhere from three to six rounds of polishing. However, this is a subjective number and the correct number of polishing rounds should be based on manual inspection of genes and sequence alignments. It should also be noted that under or over polishing can significantly impact the assembly quality. Under polishing, intuitively, fails to correct as many errors as possible. On the other hand, some polishing tools such as Racon are notorious for over polishing, meaning the more polishing rounds are done, the more likely the tool will start introducing errors into the assembly. 

* :doc:`Genome assembly polishing <assembly_qc/polishing>`

