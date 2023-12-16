Welcome to Analytical Genomics' Tutorials!
===================================

In the upcoming years the number of species with genome assemblies is going to explode. There are an estimated 8.7 million species according to a `2011 study <https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1001127&utm_campaign=Au%20fil%20des%20lectures&utm_medium=email&utm_source=Revue%20newsletter>`_ and each species will need multiple assemblies to generate a `pangenome <https://library.oapen.org/bitstream/handle/20.500.12657/37707/1/2020_Book_ThePangenome.pdf#page=98>`_. Recent years have seen genome sequencing projects become more common and accessible. This will only become more true as long read sequencing becomes cheaper. However, the tools and knowledge to assemble and annotate a genome are scattered across scientific papers, github repositories, and academic institutions. 

My goal is to distill the basics into one website that is understandable and accessible to encourage scientists and the curious that genome sequencing projects are acheivable without needing wads of cash and a team of scientists. It is my hope that someday sequencing technology and assembly are so accessible that a child or intrepid explorer stumbling across an interesting bug or novel plant phenotype could take it home and submit it for genome sequencing without needing to sell an arm or leg or brave the ivory tower that academics hole up in. 
.. figure:: /front_page_assets/short_version.png
   :align: left
   :width: 30%
On the surface, genome sequencing projects are fairly straightforwad. It begins with checking the quality of your raw sequene data, assembling the raw reads into contiguous sequences, and then predicting where genes are and what those genes do. However, nothing is ever that simple.

.. figure:: /front_page_assets/data_types.png
   :align: center
   :width: 100%

In order to achieve the highest quality and most contigious genome assembly (preferably chromsome length scaffolds or telomere to telomere) several different data types are required. The first and most important are long reads, these are sequences that are 10,000 bases to 100,000 bases long. Without these your assembly will be very fragmented. The second most important data type is what's called Hi-C. This is a technique to capture the chromatin conformation within a nucleus. It also happens to allow for chromsome scale scaffolding because sequences that are close to eachother in the nucleus also tend to be from the same chromosome. Current long read sequencing isn't quite accurate enough, so there will likely be assembly errors. Short reads can correct the majority of these. For larger assembly errors, optical genome mapping does the trick. Neither of these are necessary for a genome sequencing project, but they greatly improve the quality of the assembly. Finally, assemblies are great, but the reality is they are just a wall of ATGC text and most people are interested in what that wall of text contains - genes. Unless the genome has previously been assembled and there are quality gene models, it will be necessary to predict where genes are and the gene structure is. That is where rna-seq data comes in handy and is arguably the best type of data for that. 

.. figure:: /front_page_assets/data_cleaning.png
   :align: right
   :width: 60%

.. figure:: /front_page_assets/assembly.png
   :align: center
   :width: 100%

.. figure:: /front_page_assets/annotation.png
   :align: center
   :width: 100%

.. figure:: /front_page_assets/other_annotations.png
   :align: right
   :width: 50%
 
Here I have detailed the tools I’ve used and the process I go through to create genome assemblies, predict genes, and analyze results. When reading these walk-throughs, keep in mind that it’s a rare day when everything works the first time. More time is spent troubleshooting installation problems and learning how to use a tool than is spent actually using the tool. So if you find yourself struggling to get things working, rest assured, that’s a battle we all fight everyday.

.. warning:: This page is currently under construction with new content and mistakes added regularly.

Contents
--------

.. toctree::

   data_cleaning
   genome_assembly
   genome_assembly_qc
   genome_annotation
 
