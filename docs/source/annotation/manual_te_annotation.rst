======================================================
Manually Curating and Annotating Transposable Elements
======================================================


.. contents::
   :depth: 3
..

Introduction
============

Insert about manual curation of transposable element library

The below set of protocols are taken from `A beginner’s guide to manual
curation of transposable
elements <https://mobilednajournal.biomedcentral.com/articles/10.1186/s13100-021-00259-7>`__
with some minor modifications

Summary of Approach
===================

a) 

The first step is to reduce the number of sequences progressing to
manual curation based on their sequence identity. To this end, we
recommend using the program ‘cd-hit-est’ (Protocol 2). The output is a
FASTA file with non-redundant sequences and a “cluster” with records of
which sequences were clustered together. b)

We can prioritise sequences based on the presence and number of protein
domains (Protocol 3a and b). Autonomous TEs from most canonical
orders/superfamilies have an expected set of proteins encoded in their
sequence. We can predict conserved protein domains using profile-HMMs
available from the Pfam database [23] and use this list to include or
exclude sequences depending on the HMM match. For example, de novo TE
predictions tools may include large families of closely related genes,
typically recent expansions and gene duplications, that are not TEs but
appear “as” TEs due to their copy number and similarity
(i.e. repetitiveness). In these cases, sequences may have a non-TE Pfam
HMM / or homology (blast) match and can be removed from the priority
list.

iii) 

Another parameter for prioritising families for manual curation is the
length of the prospective family (Protocol 4). In general, most
organisms have TEs that range from a couple of hundred (e.g. SINEs) to
many thousands of bases (~ 10–20 kb. e.g. Mavericks), with protein
coding autonomous TEs in general longer than non-autonomous ones. We
suggest that prospective families up to 10 kb should be prioritised for
manual curation but longer families should not be ignored. The 10 kb
suggested limit is a guideline that can make more sense when taken
together with the number of copies found in the genome (see below). iv)

The number of copies in the genome can be another good tool to
prioritise the prospective families, in particular when it is used in
conjunction with other parameters such as the length of the family. It
is possible to quickly perform a count of family members or best hits in
the genome by running a homology search with the program ‘blastn’ [20]
using the set of putative TE families as queries and the genome as a
database (Protocol 5).

Show at least 1 TE-related conserved protein domain

Their sequence length is within the limits of an expected TE family,
that is approximately between 1 and 10 kb.

Have at least 10 good quality ‘blast’ hits in the genome.

-  dot plots are a good way to visualize LTRs and TIRs

installing packages in a conda environment

.. code:: bash

   mamba create -n te_annot -c bioconda cd-hit blast bedtools mafft muscle emboss pfam_scan gepard hmmer ucsc-fasplit r-base t-coffee

downloading github

.. code:: bash

   git clone https://github.com/annaprotasio/TE_ManAnnot
   git clone https://github.com/clemgoub/TE-Aid 

downloading pfam database

.. code:: bash

   mkdir Pfam_db
   cd Pfam_db

   wget ftp://ftp.ebi.ac.uk/pub/databases/Pfam/current_release/Pfam-A.hmm.gz
   wget ftp://ftp.ebi.ac.uk/pub/databases/Pfam/current_release/Pfam-A.hmm.dat.gz
   wget ftp://ftp.ebi.ac.uk/pub/databases/Pfam/current_release/relnotes.txt

   # unpacking files
   gunzip Pfam-A.hmm.gz
   gunzip Pfam-A.hmm.dat.gz

   # building hmm library
   hmmpress Pfam-A.hmm

Protocol 0
==========

Produce a priority table for predicted families

.. code:: bash


   bash /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/TE_ManAnnot/bin/generate_priority_list_from_RM2.sh \
       /home/jon/Working_Files/japonicus_genome_project/repeats/tetools/database/ajapmasurca-families.fa \
       /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
       /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/Pfam_db \
       /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/TE_ManAnnot

Protocol 1
==========

Extract data from RM2 fasta headers

.. code:: bash

   # pulling out the header information from the repeatmodeler output
   perl /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/TE_ManAnnot/bin/rm2_fams2table.pl \
       /home/jon/Working_Files/japonicus_genome_project/repeats/tetools/database/ajapmasurca-families.fa

Protocol 2
==========

Reduce redundancy with cd-hit

.. code:: bash

   # clustering repeatmodeler output into groups with >80% identity
   cd-hit-est \
       -i /home/jon/Working_Files/japonicus_genome_project/repeats/tetools/database/ajapmasurca-families.fa \
       -o reduced.fa \
       -d 0 \
       -aS 0.8 \
       -c 0.8 \
       -G 0 \
       -g 1 \
       -b 500

   # pulling out the header info from the reduced/clustered repeatmodelor output
   perl /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/TE_ManAnnot/bin/rm2_fams2table.pl \
       /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/reduced.fa    

Protocol 3
==========

Predict conserved protein domains

.. code:: bash

   # finding open reading frames in the transposable element sequences
   getorf \
       -sequence /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/reduced.fa \
       -outseq reduced.orf \
       -minsize 300

   # searching for conserved domains by comparing the open reading frames against pfam database
   pfam_scan.pl \
       -fasta /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/reduced.orf \
       -dir /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/Pfam_db \
       -outfile out.orf.pf

   # 
   pfam_scan.pl \
       -fasta /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/protocol_3/reduced.orf \
       -dir /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/Pfam_db \
       -outfile reduced.pfam

   # something something
   cat /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/out.orf.pf | grep -v "^\#" | head -5

   # counting domains per a family
   awk '{if ($6~/^PF/) {print $1}}' /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/protocol_3/reduced.pfam | sed 's/\#/ /1' | awk '{print $1}' | sort | uniq -c | sort > pf.domains.count

   # a complete list domains including TEs without any pfam hits
   grep '>' /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/reduced.fa | sed 's/\#/ /1;s/^>//g' | awk '{print $1}' > all.families.names

   # something something
   awk '{if ($6~/^PF/) {print $1}}' /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/protocol_3/reduced.pfam  | sed 's/\#/ /1' | awk '{print $1}' > pf.domains.names

   cat pf.domains.names all.families.names | sort | uniq -c | awk '{print $2,$1-1}' | sort -k 2 > pf.domains.with0.count

   # translating the domains to proteins
   transeq \
       -sequence /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/protocol_2/reduced.fa \
       -outseq reduced.transeq.pep \
       -frame 6 \
       -clean

   # running transeq results through pfam
   pfam_scan.pl \
       -fasta /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/protocol_3/reduced.transeq.pep \
       -dir /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/Pfam_db \
       -outfile reduced.transeq.pfam

   # using blastx for a similar approach

   # downloading the repeatpeps database
   curl -o TE_peptide_database.lib https://raw.githubusercontent.com/rmhubley/RepeatMasker/master/Libraries/RepeatPeps.lib

   # building database
   makeblastdb \
       -in TE_peptide_database.lib \
       -dbtype prot

   # blastx of reduced.fa against repeatpeps database
   blastx \
       -query /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/protocol_2/reduced.fa \
       -db TE_peptide_database.lib \
       -outfmt 6 \
       -num_threads 30 \
       -mt_mode 1 \
       > TE_peptide_blastx_reduced-fa.out

probably should have added thread info

Protocol 4
==========

Obtain the length in nucleotides for each sequence

.. code:: bash

   # get nucleotide length for each sequence
   sed 's/\#/ /g' /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/reduced.fa | awk -v OFS='\t' '{print $1}' > reduced_short_header.fa


   bash /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/TE_ManAnnot/bin/get_fasta_length.sh /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/reduced_short_header.fa

Protocol 5
==========

Run a homology search and count the number of hits with BLAST

.. code:: bash

   # making database of the genome assembly - this was down earlier?
   makeblastdb -in /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta -dbtype nucl

   # querying the database
   blastn \
       -query /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/reduced.fa \
       -db /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
       -outfmt "6 qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore qlen" \
       -out red_vs_genome_blast.o

   # something something
   awk '{OFS="\t"; if ($3 >= 80 && (($4/$13) > 0.5 )) {print $0,$4/$13}}' red_vs_genome_blast.o > red_vs_genome_good_blast.o

   # something something
   cat reduced_short_header.fa.len red_vs_genome_good_blast.o | sed 's/\#/ /g' | awk '{print $1}' | sort | uniq -c | awk '{print $2,$1-1}' | sort > red_vs_genome_good_blast.count

Protocol 6
==========

Collect all available data into one table with UNIX commands

.. code:: bash

   wc -l reduced_short_header.fa.len red_vs_genome_good_blast.count rm2header2table.tab pf.domains.with0.count

     2210 reduced_short_header.fa.len
     2210 red_vs_genome_good_blast.count
     2210 rm2header2table.tab
     2210 pf.domains.with0.count
     8840 total

   awk 'NR==FNR{a[$1]=$2;next} {print $1,$2, a[$1]}' red_vs_genome_good_blast.count reduced_short_header.fa.len > cols_len_blast

   awk 'NR==FNR{a[$1]=$2;next} {print $1,$2,$3,$4, a[$1]}' pf.domains.with0.count rm2header2table.tab > cols_head_pf

   awk -v OFS='\t' 'NR==FNR{a[$1]=$2;b[$1]=$3;next} {print $1,$2,$3,$4,a[$1],b[$1],$5}' cols_len_blast cols_head_pf  > final_priority.table

Protocol 7B
===========

Bulk-extract individual sequences into a directory

.. code:: bash


   # making directory
   mkdir ind_seq

   # running into libssl1.0.0 error sigh, creating a new conda env for faSplit 
   mamba create -n fasplit -c bioconda ucsc-fasplit
   conda activate fasplit

   # remove '/' from the fasta header
   sed "s/\// /g" /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/reduced.fa > reduced_namechg.fasta

   # running script that splits the fasta
   faSplit byname /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/reduced_namechg.fasta ind_seq/

this takes the reduced repeatmodeler fasta and slices/dices it into
individual fasta files that will then be used in protocol 8

Note: the reduced.fa file is the repeatmodeler families that have been
filtered to try ensure they are actually unique families and not
redundant families. So they should be used.

TE_aid I think does some filtering later on down the road too.

Protocol 8
==========

Find family members in the reference genome

.. code:: bash


   # switching back to the conda te_annot environment
   conda activate te_annot

   # taking genome blast result and making a fasta from it - note bash loop over all .fa in directory ind_seq

   mkdir reference_genome_fam_mem
   cd reference_genome_fam_mem

   for file in /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/ind_seq/*.fa
   do
       bash /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/TE_ManAnnot/bin/make_fasta_from_blast.sh \
           /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
           $file \
           0 \
           500
   done

this goes through the genome assembly and pulls everything that aligns
well plus a little on either side. This can then be tossed into a MSA
for visualization and manual curation

Protocol 9
==========

Generating and curating multiple sequence alignments (MSA).

.. code:: bash

   cd ..
   mkdir family_msa
   cd family_msa

   for file in /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/reference_genome_fam_mem/*.fa
   do
       base=$(basename "$file")
       mafft \
           --thread 40 \
           $file \
           > $base.maf
   done

   # removing gaps generated during the alignment that aren't helpful 
   # Note tcoffee likes to through PID errors, installing using conda was the only thing that worked for me
   mamba create -n tcoffee -c bioconda t-coffee
   conda activate tcoffee

   cd ..
   mkdir tcoffee_gap_removal
   cd tcoffee_gap_removal

   for msa in /home/jon/Working_Files/japonicus_genome_project/repeats/te_annot/family_msa/*.maf
   do
       base=$(basename "$msa")
       t_coffee \
           -other_pg seq_reformat \
           -in $msa \
           -action +rm_gap 80 \
           > $base.aln
   done

take the results from protocol 8 and create a MSA and then view it.

.. code:: bash

   for fasta in *.aln
   do
   grep ">" $fasta | wc -l >> family_count.txt
   done 


   grep ">" ltr-1_family-1#LTR.fa.blast.bed.fa.maf.aln | wc -l

ltr-1_family-1#LTR.fa.blast.bed.fa.maf.aln

.. code:: bash

   # Protocol 10 
   Dealing with very large alignments.

.. code:: bash

   # Protocol 11 
   Families and subfamilies.

some more clustering to look for families/subfamilies. I am not entirely
clear on this

.. code:: bash

   # Protocol 12 
   Generating consensus sequences and profile-HMM from sequence alignments. 

getting it ready for dfam submission essentially.

.. code:: bash

   # Protocol 13 
   Running TE-Aid

running TE-aid to visualize MSA/hmm stuff

.. code:: bash

   # Protocol 14 
   Visualisation of structural characteristics of TE sequences (dot-plots)

stopped at protocol 5 section of tutorial

`A beginner’s guide to manual curation of transposable
elements <https://mobilednajournal.biomedcentral.com/articles/10.1186/s13100-021-00259-7>`__

`Curation guidelines for de novo generated transposable element
families <https://dfam.org/assets/publications/2021-Current_Protocols-Curation_guidelines.pdf>`__

`TE-aid <https://github.com/clemgoub/TE-Aid>`__ `TE
Hub <https://tehub.org/>`__

`Ten things you should know about transposable
elements <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-018-1577-z>`__

`Transposable element annotation in non-model species: The benefits of
species-specific repeat libraries using semi-automated EDTA and DeepTE
de novo pipelines <https://pubmed.ncbi.nlm.nih.gov/34407282/>`__

`A Field Guide to Eukaryotic Transposable
Elements <https://www.annualreviews.org/doi/10.1146/annurev-genet-040620-022145>`__

`Structural and sequence diversity of eukaryotic transposable
elements <https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article>`__

`Accurate Transposable Element Annotation Is Vital When Analyzing New
Genome
Assemblies <https://academic.oup.com/gbe/article/8/2/403/2574076?login=false>`__
