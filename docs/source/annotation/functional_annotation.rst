gallery: - url:
/assets/images/interproscan/blast2go_statistics_20200212_0156.png
image_path:
/assets/images/interproscan/blast2go_statistics_20200212_0156.png title:
“blast2go graph of braker2 de novo gene predictions with interproscan
and without interproscan predictions” - url:
/assets/images/interproscan/thumbnail_image001.png image_path:
/assets/images/interproscan/thumbnail_image001.png title: “Interproscan
annotations by database” - url:
/assets/images/interproscan/thumbnail_image002.png image_path:
/assets/images/interproscan/thumbnail_image002.png title: “Interproscan
annotations by protein length” - url:
/assets/images/interproscan/thumbnail_image003.png image_path:
/assets/images/interproscan/thumbnail_image003.png title: “Breakdown of
annotation types of the longest annotated protein”

InterProScan
============

Functional annotation of the genome

Background
----------

Excerpt from their github
`page <https://github.com/ebi-pf-team/interproscan/wiki>`__

InterPro is a database which integrates together predictive information
about proteins’ function from a number of partner resources, giving an
overview of the families that a protein belongs to and the domains and
sites it contains.

Users who have novel nucleotide or protein sequences that they wish to
functionally characterise can use the software package InterProScan to
run the scanning algorithms from the InterPro database in an integrated
way. Sequences are submitted in FASTA format. Matches are then
calculated against all of the required member database’s signatures and
the results are then output in a variety of formats.

InterPro website can be found `here <https://www.ebi.ac.uk/interpro/>`__

InterPro provides functional analysis of proteins by classifying them
into families and predicting domains and important sites. To classify
proteins in this way, InterPro uses predictive models, known as
signatures, provided by several different databases (referred to as
member databases) that make up the InterPro consortium. We combine
protein signatures from these member databases into a single searchable
resource, capitalising on their individual strengths to produce a
powerful integrated database and diagnostic tool.

Installation
------------

There is no conda package for interproscan. It can be downloaded
interproscan from their github page or
`website <https://www.ebi.ac.uk/interpro/download/>`__. The package
requires uncompressing.

Commands
--------

It was run on the three braker2 gene predictions sets. Specifically the
augustus.hints.aa

   InterProScan will throw an error if there is a \* at the end of each
   fasta sequence. I used the find and replace all function in the text
   editor sublime to remove them

de novo gene predictions from braker2

::

   ./interproscan.sh -cpu 40 -d /home/jon/Working_Files/interproscan -f TSV,XML,JSON,GFF3 -goterms -i /home/jon/Working_Files/braker2/braker_denovo/augustus.ab_initio.aa -iprlookup -pa -d /home/jon/Working_Files/interproscan

braker2 gene predictions using rna-seq

::

   ./interproscan.sh -cpu 75 -d /home/jon/Working_Files/interproscan/ -f TSV,XML,JSON,GFF3 -goterms -i /home/jon/Working_Files/braker2/braker_rna_pcali_platredun/rna-mode.augustus.hints.aa -iprlookup -pa 

braker2 gene predictions using rna-seq and *Apostichopus japonicus*
protein

::

   ./interproscan.sh -cpu 75 -d /home/jon/Working_Files/interproscan/ -f TSV,XML,JSON,GFF3 -goterms -i /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/pcali-rna.ajap-prot.augustus.hints.aa -iprlookup -pa 

Results
-------

I have not done much analysis of the interproscan results. But I did
load the results into blast2go for a quick summary graph and worked some
excel magic briefly. See charts below

{% include gallery %}
