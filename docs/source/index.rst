Genome Assembly and Annotation: A Gentle Introduction
===================================

In the upcoming years the number of species with genome assemblies is going to explode. There are an estimated 8.7 million species according to a `2011 study <https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1001127&utm_campaign=Au%20fil%20des%20lectures&utm_medium=email&utm_source=Revue%20newsletter>`_ and each species will need multiple assemblies to generate a `pangenome <https://library.oapen.org/bitstream/handle/20.500.12657/37707/1/2020_Book_ThePangenome.pdf#page=98>`_. Recent years have seen genome sequencing projects become more common and accessible. This will only become more true as long read sequencing becomes cheaper. However, the tools and knowledge to assemble and annotate a genome are scattered across scientific papers, github repositories, and academic institutions. 

.. figure:: /front_page_assets/short_version.png
   :align: left
   :width: 80%
   :figwidth: 40%

My goal is to distill the basics into one website that is understandable and accessible to encourage scientists and the curious that genome sequencing projects are acheivable without needing wads of cash and a team of scientists. It is my hope that someday sequencing technology and assembly are so accessible that a child or intrepid explorer stumbling across an interesting bug or novel plant phenotype could take it home and submit it for genome sequencing without needing to sell an arm and leg or brave the ivory tower that academics hole up in. 

On the surface, genome sequencing projects are fairly straightforwad. It begins with checking the quality of your raw sequene data, assembling the raw reads into contiguous sequences, and then predicting where genes are and what those genes do. However, nothing is ever that simple. Genome sequencing requires several different data types, each step has numerous sub-steps and quality control steps, and the final product will need to be manually inspected to ensure that the assembly, gene models, and annotations are correct. 

.. figure:: /front_page_assets/data_types.png
   :align: center
   :width: 100%

.. raw:: html

   <div style="margin-bottom:20px;"></div>

In order to achieve the highest quality and most contigious genome assembly (preferably chromsome length scaffolds or telomere to telomere) several different data types are required. The first and most important are long reads, these are sequences that are 10,000 bases to 100,000 bases long. Without these your assembly will be very fragmented. The second most important data type is what's called Hi-C. This is a technique to capture the chromatin conformation within a nucleus. It also happens to allow for chromsome scale scaffolding because sequences that are close to eachother in the nucleus also tend to be from the same chromosome. Current long read sequencing isn't quite accurate enough, so there will likely be assembly errors. Short reads can correct the majority of these. For larger assembly errors, optical genome mapping does the trick. Neither of these are necessary for a genome sequencing project, but they greatly improve the quality of the assembly. Finally, assemblies are great, but the reality is they are just a wall of ATGC text and most people are interested in what that wall of text contains - genes. Unless the genome has previously been assembled and there are quality gene models, it will be necessary to predict where genes are and the gene structure is. That is where rna-seq data comes in handy and is arguably the best type of data for that. 

Data Quality control
~~~~~~~~~~~~~~~~~~~~~

.. figure:: /front_page_assets/data_cleaning.png
   :align: right
   :figwidth: 50%

Now to get a little into the weeds. The first step checking the quality of the raw sequence data. As mentioned previously there are a number of different data types, all of which have specific types of errors that can creep into the data and wreck havoc on later steps in the pipeline. A common one is not removing adaptor contamination. Not dealing with this can result in fragmented genes models and incorrectly assembled sequences. Low quality reads can also impact assembly by introducing incorrect bases and the assembly tool being unable to resolve alignment conflicts in sequences resulting in a fragmented assembly. 

An additional aspect of doing data quality control prior to assembly is something called K-mer analysis. Exactly what this is will be delved into later. However, doing this kind of analysis can reveal a number of characteristics about the data and the organism. This includes genome size, heterozygousity, ideal k-mer size for genome assembly, potential biological contamination, and raw data quality. These data can inform what you should expect to see in your assembly. 

.. toctree::

   data_cleaning

Genome Assembly and Quality control
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Let's say the data looks great, adapters have been removed, low quality reads filtered from the datasets, etc. It's time to do some assembly. There are numerous assembly tools and pipelines. It is considered good practice to try a few variations of each as every dataset has unique charactistics that may work better with one tool compared to others. Generally, this happens at every step of the assembly process. IE, try three different contig assemblers, check the quality of the resulting assemblies, pick one, polish the assembly using short reads, and move onto scaffolding. Same applies to the Hi-C scaffolding step. After Hi-C scaffolding, there are often structural errors that can then be corrected using optical genome mapping. However, recent years have seen successful chromosome scale assembly without OGM so there has been a move away from using it unless the genome is large and particularly complex. 

.. figure:: /front_page_assets/assembly.png
   :align: center
   :width: 100%

.. raw:: html

   <div style="margin-bottom:20px;"></div>

There are a number of quality control metrics and analyses that can be done to ensure the assembly is approaching high-quality. One is using summary statistics such as N50, number of contigs and scaffolds, genome size, heterozygousity, and QV. Additionally, there are tools that check for biological contamination and use K-mer analysis look for signals of adapter and error contamination in the assembly relative to the raw data. Finally, polishing a genome using highly accurate reads is almost a must if the assembly is going to have a high QV score and minimal errors. This is often check using BUSCOs, which are a set of highly conserved genes found in all organisms. High-quality genomes should have above 90% of the BUSCOs present. It is always a huge red flag when a "chromosome-scale" assembly has complete BUSCOs below 90%. 

.. toctree::

   genome_assembly
   genome_assembly_qc

Genome Gene Prediction and Annotation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The assembly is chromsome scale, maybe telomere to telomere, gapless, polished, and biological contamination removed. That's a nice looking genome assembly. To find the interesting bits inside that string of letters requires a bit of work though. First is mapping gene expression data to the assembly. This tells you where genes are and possibly what the structure is. However, unless you have gene expression data from every tissue type and developmental stage, there is a good chance some genes will not have expression data to guide gene model prediction. Aligning protein sequences from NCBI's refseq and Uniprot, possibly using proteins from closely related species, is a considered standard practice. To get the rest of way though you need a tool which can predict gene models using what's called "intrinsic" information. These are the start codons, splice sites, and stop codons which tell the algorithm where coding sequence starts and ends. 

.. figure:: /front_page_assets/annotation.png
   :align: center
   :figwidth: 100%

.. raw:: html

   <div style="margin-bottom:20px;"></div>

Before predicting gene models though, it is important to do something called "masking" where repetetitive elements such as transposons and low complexity regions are identified in the genome assembly and annotated so the gene prediction tool ignores those areas. This significantly reduces the computational time and effort it takes to predict gene models in the assembly. 

Once that's done and gene models predicted, it is time to get some summary statistics on how many genes, intron to exon ratio, average length, etc. If all looks good, functional annotation is accomplished using those a handful of tools and databases that have functions assigned to genes. This is not the most accurate step, but having some functional annotation is better than none. With the functional annotation, identifying genes you are interested in can be simple. Which leads to the final and arguably most important step - checking the quality of the assembly and gene models. Doing this has revealed numerous assembly and gene model prediction errors during my own research. If more genome sequencing projects did this there would likely be higher quality assemblies and gene models available. 

.. figure:: /front_page_assets/other_annotations.png
   :align: right
   :figwidth: 50%
 
But wait, that's not all. There is a slew of additional non-coding sequence content in the genome that should be identified and annotated. A notable one being the repetetitive elements that were masked earlier. These play a huge role in genome evolution including messing with cis-regulatory elements that can result in chances in phenotype. Good examples including the origins of mammary glands and placentas in placental mammals. Additionally Organelle genomes should be checked for completeness and gene content as those are rather important to life and shouldn't be overlooked during the larger genome assembly process. There are many other interesting parts of the genome to include, some of which are listed to the left. Some genome sequencing projects put in the effort to identify and annotate these features, but this is time consuming and most projects do not. 

.. toctree::

   genome_annotation



