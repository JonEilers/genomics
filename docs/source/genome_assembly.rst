Genome Assembly
===============

.. _Genome Assembly:

.. warning:: This page is currently under construction with new content and mistakes added regularly.

Introduction
============

Genome Assembly is `hard <https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.2005894>`_. Even now with long read sequencing, `there is still a long way to go <https://www.nature.com/articles/s41592-021-01057-y>`_. [Description of genome assembly challenges...]

.. note:: `Mind the Gap <https://www.sciencedirect.com/science/article/abs/pii/S1087184515300220>`_ reasons to produce gapless genome assemblies
.. note:: `Errors in long-read assemblies can critically affect protein prediction <https://www.nature.com/articles/s41587-018-0004-z>`_ for why it is critical to check assembly quality and fix errors
.. note:: `A comprehensive review of scaffolding methods in genome assembly <https://academic.oup.com/bib/article-abstract/22/5/bbab033/6149347?redirectedFrom=fulltext>`_ on ways to improve assemblies scaffolding

Assembly
========

Short Read
----------

[Details about Illumina sequencing...]

.. note:: `Short read assembly using ABySS and SPAdes <short-read-assembly/>`_

[Description of Platanus-Allee and MaSuRCA...]

.. note:: Short-read assembly using `Platanus-Allee <platanus-allee/>` and `MaSuRCA <masurca/>`_

Long Read
---------

[Details about long-read sequencing technology...]

.. note:: `Long read assembly of Chiridota heheva using Raven, Flye, and Shasta assemblers <longread_genome_assembly/>`_

Hybrid
------

Hybrid genome assembly is the predominant approach these days. [Further explanation...]

.. note:: `Hybrid genome assembly of Apostichopus japonicus using Platanus-allee and MaSURCA <hybrid_genome_assembly/>`_
.. warning:: To-do: Using Hi-C data to achieve chromosome scale scaffolds using salsa, yahs, 3d-dna and hirise for the Stichopus chloronotus genome

Assembly QC
===========

[Description of assembly quality control...]

Summary Statistics
-------------------

[Description about the importance of N50 and other metrics...]

.. note:: `Summary statistics via Blobtoolkit <summary-stats/>`_

Assembly QC with BUSCOs
------------------------

[Details about using BUSCOs for assembly quality assessment...]

.. note:: `Assembly quality assessment using BUSCO analysis <busco/>`_

Assembly Contamination and Quality
----------------------------------

[Discussion about checking for contamination in genome assembly...]

.. note:: `Assembly contamination and quality via Blobtoolkit <contamination/>`_

Assembly Evaluation using K-mers and Long Reads
-----------------------------------------------

[Information on using K-mers and long reads for assembly evaluation...]

.. note:: `Assembly Quality Assessment using Inspector and Merqury <genome_quality/>`_

Polishing and Gap Closing
=========================

[Description about the importance and methods of gap closing and polishing...]

Gap Closing
-----------

[Details about gap closing methods...]

.. warning:: To-do: Gap closing using Dentist, SAMBA and Longstitch

Polishing
---------

[Explanation about the polishing process...]

.. note:: `Genome assembly polishing using hapo-g <genome_polishing/>`_

Manual Assembly Correction
--------------------------

[Information about manual assembly correction...]

.. note:: `Significantly improving the quality of genome assemblies through curation <https://academic.oup.com/gigascience/article/10/1/giaa153/6072294>`_

.. note:: Hmmm, maybe play with `AGB <https://github.com/almiheenko/AGB>`_ for visualizing long read assembly?

.. note:: Note to self: Consider future genome analysis and reassembly for `Australostichopus mollis <https://www.ncbi.nlm.nih.gov/bioproject/PRJEB10682/>`, `Apostichopus leukothele <https://www.ncbi.nlm.nih.gov/sra/SRX8086344[accn]>`, and `Actinopyga echinites <https://www.ncbi.nlm.nih.gov/assembly/GCA_010015985.1#/st>` genomes.

.. autosummary::
   :toctree: generated

   [list your modules, classes, or functions here]

