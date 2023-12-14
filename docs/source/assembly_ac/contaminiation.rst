=============================
Genome Assembly Contamination
=============================


.. contents::
   :depth: 3
..

Introduction
============

`Blobtoolkit <https://blobtoolkit.genomehubs.org/>`__ is an amazing
suite of tools for analyzing and producing publication grade figures of
genome assemblies. This includes snail plots for visulizing genome
summary statistics, blobplots of contamination and taxonomy hits, and
cumulative assembly span plots of the multiple genome assemblies. Their
2020
`paper <https://academic.oup.com/g3journal/article/10/4/1361/6026202>`__
contains examples and explanations of the various figures and analysis’
that can be performed.

Installing
==========

There are a number of options for how to install blobtoolkit. There is a
`docker
image <https://blobtoolkit.genomehubs.org/install/use-docker/>`__
available. Additionally, you can install it directly by following these
`instruction <https://blobtoolkit.genomehubs.org/install/>`__, but as
they note you can make your life easier by installing most of the
dependencies using conda.

I have used both the docker image and installing directly. Both are a
bit of a pain to do. Thankfully, they introduced an alternative recently
- using `pip <https://pypi.org/project/pip/>`__. pip is a package
installer for python. It is akin to using conda to install stuff. At the
time of writing this I suggest you use pip.

Installing blobtoolkit
----------------------

Installing using pip

.. code:: bash

   pip install blobtoolkit

That was easy and painless. Love it.

Downloading and setting up databases
------------------------------------

To fully utilize blobtoolkit you will need to download a few databases.
They go over how to do this in detail on their
`website <https://blobtoolkit.genomehubs.org/install/>`__. But in case
you don’t feel like clicking on the above link I have also copied and
pasted their code below.

downloading the taxonomy database

.. code:: bash

   mkdir -p taxdump;
   cd taxdump;
   curl -L ftp://ftp.ncbi.nih.gov/pub/taxonomy/new_taxdump/new_taxdump.tar.gz | tar xzf -;
   cd ..;

The NCBI `taxonomy
database <https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?mode=Root>`__
is just that. It contains a list of organism names, unique ID numbers,
and how they are all related to the best of our knowledge. This will be
used in blobtoolkit when looking for contamination in the genome
assembly.

downloading the ncbi nucleotide database

.. code:: bash

   # NCBI nucleotide database
   mkdir -p nt
   wget "ftp://ftp.ncbi.nlm.nih.gov/blast/db/nt.??.tar.gz" -P nt/ && \
           for file in nt/*.tar.gz; \
               do tar xf $file -C nt && rm $file; \
           done

The NCBI `nucleotide databse <https://www.ncbi.nlm.nih.gov/nuccore>`__
aka ncbi nt database contains “entries from all traditional divisions of
GenBank, EMBL, and DDBJ; excluding bulk divisions (gss, sts, pat, est,
htg) and wgs entries. Partially non-redundant” - per the
`readme <https://ftp.ncbi.nlm.nih.gov/blast/db/README>`__ file on the
ftp webpage. This will be used to search the genome assembly for
sequences that might not be sea cuke sequences.

Downloading the uniprot protein database

.. code:: bash


   # before you download and format the uniprot database you will need to
   # install the sequence aligner called diamond.
   conda create -n uniprot
   conda activate uniprot
   conda install -c bioconda diamond 


   # Uniprot database
   mkdir -p uniprot
   wget -q -O uniprot/reference_proteomes.tar.gz \
    ftp.ebi.ac.uk/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/$(curl \
        -vs ftp.ebi.ac.uk/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/ 2>&1 | \
        awk '/tar.gz/ {print $9}')
   cd uniprot
   tar xf reference_proteomes.tar.gz

   # successfully downloaded and extract prokaryota and archea, but got at truncated
   # tried the above command a few times and got the same truncated results.
   # trying a different command

   aria2c https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/Reference_Proteomes_2022_01.tar.gz

   tar xf reference_proteomes.tar.gz

   touch reference_proteomes.fasta.gz
   find . -mindepth 2 | grep "fasta.gz" | grep -v 'DNA' | grep -v 'additional' | xargs cat >> reference_proteomes.fasta.gz

   echo -e "accession\taccession.version\ttaxid\tgi" > reference_proteomes.taxid_map
   zcat */*/*.idmapping.gz | grep "NCBI_TaxID" | awk '{print $1 "\t" $1 "\t" $3 "\t" 0}' >> reference_proteomes.taxid_map

   diamond makedb -p 60 --in reference_proteomes.fasta.gz --taxonmap reference_proteomes.taxid_map --taxonnodes ../taxdump/nodes.dmp -d reference_proteomes.dmnd
   cd -

and finished setting up the uniprot database. Now to delete the large
proteome files yeesh.

.. code:: bash

   Database sequences                   59528643
   Database letters                     22057035815
   Accessions in database               59528643
   Entries in accession to taxid file   64078951
   Database accessions mapped to taxid  59528643
   Database sequences mapped to taxid   59528643
   Database hash                        2791a2558388fd6af017a4e0b85f6fc3
   Total time                           746s

“The Universal Protein Resource (UniProt), a collaboration between the
European Bioinformatics Institute (EBI), the SIB Swiss Institute of
Bioinformatics, and the Protein Information Resource (PIR), is comprised
of three databases, each optimized for different uses. The UniProt
Knowledgebase (UniProtKB) is the central access point for extensively
curated protein information, including function, classification and
cross-references. The UniProt Reference Clusters (UniRef) combine
closely related sequences into a single record to speed up sequence
similarity searches. The UniProt Archive (UniParc) is a comprehensive
repository of all protein sequences, consisting only of unique
identifiers and sequences.”

The above is an excerpt taken from the
`readme <https://ftp.ebi.ac.uk/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/>`__
file on their ftp server. In this case we are using the reference
proteomes which includes Archaea, Bacteria, Eukaryota and Viruses

Running
=======

Note: the creators of blobtoolkit also have tutorials and information on
their website. For looking for contamination - their tutorial can be
found
`here <https://blobtoolkit.genomehubs.org/blobtools2/blobtools2-tutorials/adding-data-to-a-dataset/adding-hits/>`__

.. code:: bash

   blobtools create \
     --fasta /home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta \
     --taxid 307972 \
     --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
     /home/jon/Working_Files/blobtoolkit/ajap

   blobtools create \
     --fasta /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
     --taxid 307972 \
     --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
     /home/jon/Working_Files/blobtoolkit/ajap-masurca

   blobtools create \
     --fasta /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/scaffolds.ref.fa \
     --taxid 307972 \
     --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
     /home/jon/Working_Files/blobtoolkit/ajap-masurca-scaffolds.ref.fa

running minimap for generating coverage data. There is no raw read data
for the original assembly, so this will only be for the two masurca
assemblies. Am also only using the Pacbio data as there is far too much
short read illumina data.

this will require minimap2 and samtools. I suggest you make a conda
environment and install both using conda. Also note - I am using the
commands from the blobtools website.

.. code:: bash

   conda create -n blobtools
   conda activate blobtools
   conda install -c bioconda minimap2 
   conda install -c bioconda samtools

   minimap2 -ax map-pb \
            -t 20 \
            /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
            /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fasta \
   | samtools sort -@20 -O BAM -o ajap-masurca -

   minimap2 -ax map-pb \
            -t 20 \
            /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/scaffolds.ref.fa \
            /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fasta \
   | samtools sort -@20 -O BAM -o Aajap-masurca-scaffolds.ref.bam -

   minimap2 -ax map-pb \
            -t 30 \
            /home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta \
            /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
   | samtools sort -@30 -O BAM -o Ajap.bam -

blasting NCBI nucleotide database against the genomes

.. code:: bash

   # install the blast package to your blobtools conda environment
   conda install -c blast

   # run blastn on the three assemblies
   blastn \
     -db nt \
     -query /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
     -outfmt "6 qseqid staxids bitscore std" \
     -max_target_seqs 10 \
     -max_hsps 1 \
     -evalue 1e-25 \
     -num_threads 16 \
     -out ajap-masurca.ncbi.blastn.out

   blastn -db nt \
          -query /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/scaffolds.ref.fa \
          -outfmt "6 qseqid staxids bitscore std" \
          -max_target_seqs 10 \
          -max_hsps 1 \
          -evalue 1e-25 \
          -num_threads 16 \
          -out ajap-masurca.scaffolds.ref.ncbi.blastn.out

   blastn -db nt \
          -query /home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta \
          -outfmt "6 qseqid staxids bitscore std" \
          -max_target_seqs 10 \
          -max_hsps 1 \
          -evalue 1e-25 \
          -num_threads 16 \
          -out ajap.ncbi.blastn.out

blasting the diamond uniprot database against the assemblies

.. code:: bash

   conda create -n diamond
   conda activate diamond
   conda install -c bioconda diamond 

   diamond blastx \
           --query /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
           --db /home/jon/Working_Files/blobtoolkit/uniprot/reference_proteomes.dmnd \
           --outfmt 6 qseqid staxids bitscore qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore \
           --sensitive \
           --max-target-seqs 1 \
           --evalue 1e-25 \
           --threads 60 \
           > ajap-masurca.diamond.blastx.out


   diamond blastx \
           --query /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/scaffolds.ref.fa \
           --db /home/jon/Working_Files/blobtoolkit/uniprot/reference_proteomes.dmnd \
           --outfmt 6 qseqid staxids bitscore qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore \
           --sensitive \
           --max-target-seqs 1 \
           --evalue 1e-25 \
           --threads 60 \
           > ajap-masurca.scaffolds.ref.diamond.blastx.out

   diamond blastx \
           --query /home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta \
           --db /home/jon/Working_Files/blobtoolkit/uniprot/reference_proteomes.dmnd \
           --outfmt 6 qseqid staxids bitscore qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore \
           --sensitive \
           --max-target-seqs 1 \
           --evalue 1e-25 \
           --threads 60 \
           > ajap.ncbi.diamond.blastx.out        

Adding coverage, blastn, and blastx hits for each assembly

.. code:: bash

   #ajap
   blobtools add \
       --hits /home/jon/Working_Files/blobtoolkit/blastn_results/ajap.ncbi.blastn.out \
       --hits /home/jon/Working_Files/blobtoolkit/diamond_results/ajap.ncbi.diamond.blastx.out \
       --cov /home/jon/Working_Files/blobtoolkit/minimap_results/Ajap.bam
       --taxrule bestsum \ 
       --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \ 
       /home/jon/Working_Files/blobtoolkit/datasets/ajap            


   blobtools add --hits /home/jon/Working_Files/blobtoolkit/blastn_results/ajap.ncbi.blastn.out --hits /home/jon/Working_Files/blobtoolkit/diamond_results/ajap.ncbi.diamond.blastx.out --cov /home/jon/Working_Files/blobtoolkit/minimap_results/Ajap.bam --taxrule bestsum --taxdump /home/jon/Working_Files/blobtoolkit/taxdump --replace /home/jon/Working_Files/blobtoolkit/datasets/ajap  

   #ajap-masurca.scaffolds.ref
   blobtools add \
     --hits /home/jon/Working_Files/blobtoolkit/blastn_results/ajap-masurca.scaffolds.ref.ncbi.blastn.out \
     --hits /home/jon/Working_Files/blobtoolkit/diamond_results/ajap-masurca.scaffolds.ref.diamond.blastx.out \
     --cov /home/jon/Working_Files/blobtoolkit/minimap_results/ajap-masurca-scaffolds.ref.bam \
     --taxrule bestsum \ 
     --taxdump /taxdump \ 
     /home/jon/Working_Files/blobtoolkit/datasets/ajap-masurca.scaffolds.ref

   blobtools add --hits /home/jon/Working_Files/blobtoolkit/blastn_results/ajap-masurca.scaffolds.ref.ncbi.blastn.out --hits /home/jon/Working_Files/blobtoolkit/diamond_results/ajap-masurca.scaffolds.ref.diamond.blastx.out --cov /home/jon/Working_Files/blobtoolkit/minimap_results/ajap-masurca-scaffolds.ref.bam --taxrule bestsum --threads 40 --taxdump /home/jon/Working_Files/blobtoolkit/taxdump /home/jon/Working_Files/blobtoolkit/datasets/ajapmasurcascaffoldsref

   #ajap-masurca
   blobtools add \
       --hits /home/jon/Working_Files/blobtoolkit/blastn_results/ajap-masurca.ncbi.blastn.out \
       --hits /home/jon/Working_Files/blobtoolkit/diamond_results/ajap-masurca.diamond.blastx.out \
       --cov /home/jon/Working_Files/blobtoolkit/minimap_results/ajap-masurca.bam \
       --taxrule bestsum \ 
       --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \ 
       /home/jon/Working_Files/blobtoolkit/ajap-masurca 


   blobtools add --hits /home/jon/Working_Files/blobtoolkit/blastn_results/ajap-masurca.ncbi.blastn.out --hits /home/jon/Working_Files/blobtoolkit/diamond_results/ajap-masurca.diamond.blastx.out --cov /home/jon/Working_Files/blobtoolkit/minimap_results/ajap-masurca.bam --threads 20 --taxrule bestsum --taxdump /home/jon/Working_Files/blobtoolkit/taxdump /home/jon/Working_Files/blobtoolkit/datasets/ajapmasurca 

Note: the blob directory datasets can not have periods or hyphens in the
dataset name.

viewing the results locally

.. code:: bash

   blobtools view --local /home/jon/Working_Files/blobtoolkit/datasets/

Blobtoolkit Examples
====================

We have results for coverage and blast hits from the NCBI nucleotide
database and uniprot. Which is awesome. Now let’s dive into how to
understand blobplots. They are an incredibly powerful method of
investigating the quality of a genome assembly. In an ideal work I would
host an interactive version such as is available on the blobtoolkit
`website <https://blobtoolkit.genomehubs.org/view/>`__. The interactive
format allows you to quickly and easily change parameters. However, this
website is not set up for that so we’ll have to do with images, but I do
encourage you to either set up a local instance of blobtoolkit or
explore the website.

Before looking at my results, let’s look at some blobplots of good, bad,
and suspicious genome assemblies.

High Quality Genome Examples
----------------------------

{% include gallery id=“gallery_blobtoolkit_best_snail”
caption=“Blobtoolkit plots of the best echinoderm genome assemblies” %}

The three blobtoolkit plot sets are from left to right: *Patiria
miniata* - bat star, *Asterias rubens* - common sea star, and
*Marthasterias glacialis* - spiny starfish.

First, note that the snail plots have what appear to be steps in them.
This represents scaffolds that are very long. In a lower quality
assembly with scaffolds shorter than a megabase you won’t see that. The
first assembly has clearly filtered out any short reads as shown by the
lack of a steep drop off at the end of the snail plot. The other two
assemblies do have this.

Also notice that the GC content in the outer blue ring is consistent. As
in there is not a jagged wiggly line in the center of the blue which
represents changes in the GC content. Additionally, there is little to
no “N” content in the scaffolds, so these scaffolds are highly
contigious with little to no gap in them.

{% include gallery id=“gallery_blobtoolkit_best_blob”
caption=“Blobtoolkit plots of the best echinoderm genome assemblies” %}

The blobplots also tell a story of high quality. The first assembly has
almost exclusively echinodermta blast hits with the one exception being
a scaffold with a majority of the hits being mollusca. This scaffold
does have a slightly higher GC content and lower coverage than the
majority of the scaffolds, but it is still very similar to the others.
The scale has been adjusted so that it looks like everything is spread
out. But when we look at the the other two, the scale is very different.
The other two assemblies have all the large reads clustered together and
a few scaffolds have majority hits from chordata. but by and large they
are from echinodermata.

{% include gallery id=“gallery_blobtoolkit_best_length”
caption=“Blobtoolkit plots of the best echinoderm genome assemblies” %}

The tiny blobs that we see scattered around the large scaffolds are
shorter scaffolds. When the Y-axis is switched from coverage to read
length we see these small blobs at the bottom of the graph. We also see
a pyramid shape that is reflecting how shorter reads tend to have a
wider range of gc content which is to be expected. The “weight” of the
scaffolds are always at the top of graph though, which is expected when
looking at assemblies with chromosome scale scaffolds.

Medium Quality Genome Examples
------------------------------

{% include gallery id=“gallery_blobtoolkit_medium_snail” layout=“half”
caption=“Blobtoolkit plots of medium quality echinoderm genome
assemblies” %}

Here we have blobtoolkit plots of *Acanthaster planci* - sunflower star
and *Strongylocentrotus purpuratus* - purple sea urchin. The scaffold
counts are floating around a thousand. We start to see the snail plots
get less step like and more smooth. There is a higher “N” count, meaning
more gaps in the asssembly that were filled with place holders. We also
start to see the shortest reads in the genome get “weird”. I have
noticed the tail end of snail plots often have GC contents and “N”s all
over the place. Overall though, these genomes are still pretty darn
good. Good enough for most genome analysis.

{% include gallery id=“gallery_blobtoolkit_medium_blob” layout=“half”
caption=“Blobtoolkit plots of medium quality echinoderm genome
assemblies” %}

Coverage in these blobplots is a bit more spread out. Also a wider range
in the GC content. If the reads are short and the GC content is wild,
that doesn’t necessarily mean it is contamination. It could be
repetetive sequences. If the scaffold is a bit longer (or larger circle
in this case), then it should be re-examined. Additionally, if the
majority of the blast hits show something not related to the species of
interest. In the sunflower star case we have another mollusca scaffold
that has a GC content that is quite a bit different from the species of
interest. This is a little suspicious and would merit looking into. The
purple sea urchin coverage vs gc looks beautiful. We are still getting
some weird blast hits, but the GC content is dead on. The coverage could
be a bit higher, but 10ish isn’t the worst.

{% include gallery id=“gallery_blobtoolkit_medium_length” layout=“half”
caption=“Blobtoolkit plots of medium quality echinoderm genome
assemblies” %}

When filtering by length we see some interesting stuff. I have noticed
assemblers will do some strange things. Regarding the sunflower star,
there are essentially two levels in the shorter scaffolds. This strikes
me as an artifact, but without looking into I don’t know. It might be
interesting to change the X axis from GC to coverage and see what falls
out. I suspect the second level with longer scaffolds might have greater
coverage and the wide spread is a product of shorter scaffolds having
more variance in gc content.

The greater variety of blast hits could be from the databases not having
a lot of echinoderm sequence data in it so non-echinoderm sequences end
up with the majority of best hits. Hard to tell without looking at some
of those hits and teasing out what happened. It could also be due to
assembly error introducing indels which may through off blast searches.
Unless the GC content is wildly off in a large scaffold it isn’t too
alarming to me to see other blast hits. But I could be wrong too.

Suspicious Genome Examples
--------------------------

Lower quality genome assemblies are essentially worse versions of these
two. So smoother snail plot, more “N”s. Lots more blast hits that are
all over the place. More variance in the gc content, etc. But generally
they follow a similar pattern. However, suspicious genomes? heh, let’s
take a look.

In my experience, people who get “creative” with their assembly process
tend to get some weird assemblies. That’s fine if they keep it to
themselves, but when they publish it to NCBI then it becomes a problem
because few people actually check the quality of an assembly before
trying to use it.

{% include gallery id=“gallery_blobtoolkit_sus_snail” layout=“half”
caption=“Blobtoolkit snail plots of suspicious quality echinoderm genome
assemblies” %}

These two examples are from the assemblies: *Apostichopus leukothele* a
sea cucumber and *Ophionereis fasciata* a brittle star. Wow these are
bad. From a biological viewpoint - The sea cucumber one should not have
a single scaffold that is 110 megabases long. Their genome is ~900 mb
and have a chromosome haplotype of ~22. A chromosome of 110mb would
leave 800 megabases split between 21 chromosome. Surprisingly the GC
content is pretty close to what is expected for this species. But notice
how the scaffold length quickly drops from ~100 megabases to a N90 of
2.5 kilobases. That makes no sense for a genome assembly. I haven’t
talked about Buscos, but suffice to say if you have scaffolds that are
in the 100 megabase range you should not be getting a busco percentage
less than 90%, let alone below 50%.

The brittle star assembly is more like what I’d expect to see for a
highly fragmented genome. As in something with a very large number of
scaffolds and low N50. However, if your N50 is 500 base pairs, you
shouldn’t be publishing it. That’s not useful for anything. Also notice
a “N” of 12.4% which is really high for such a fragmented genome. If
something is this fragmented, there probably shouldn’t be any gaps. Also
strange is the GC content in the largest scaffolds, but maybe that is to
be expected for short scaffolds. The busco score is atrocious. Again,
probably shouldn’t be publishing something that has a busco score less
than 70% or so, let alone 7%. This means single copy genes that are
found in almost all organisms are either fragmented or missing from your
assembly, which means your assembly is garbage.

{% include gallery id=“gallery_blobtoolkit_sus_length” layout=“half”
caption=“Blobtoolkit blob plots of suspicious quality echinoderm genome
assemblies” %}

Looking at the length vs gc content we see some weird stuff in the sea
cucumber assembly. This is an example of someone getting creative with
their assembly process and it has created a strange pattern. The blast
hits are weird though. I can’t tell, but it sorta looks like one of the
two largest scaffolds has a majority hits from flat worms. That’s not
great when your whole assembly is essentially two huge scaffolds. The
second problem is the layering that is obvious, this looks like an
assembly artifact. There isn’t anything obviously wrong with it, but it
makes me suspicious as I don’t usually see this pattern in even low
quality assemblies.

The brittle star assembly has a lot wrong in the blob plot. First, if
you ever see anything other than eukaryota blast hits, you probably have
contamination in your assembly and that should be removed. Three of the
top blast results are non-eukaryota. Secondly, it is interesting how the
GC content at the top roughly lines up with what I’d expect to see in
echinoderms, which is ~37ish%. However, it starts skewing to the right
and by the time we are the bottom the GC content looks to be more around
40%-50%. This isn’t necessarily bad as shorter reads do have higher
variance. Normally, I would except to see a triangle or pyramid shape as
scaffolds get shorter. However, this is a low quality assembly that is
mostly scaffolds shorter than 10kbp so essentially all the reads are
going to have high variance and the pyramid shape is lost.

Results
=======

A medium quality genome of *Apostichopus japonicus* was published in
2017. While working on my masters thesis I stumbled across some gene
model problems. Upon closer inspection using mapped rna-seq data and
Apollo it became apparent that there were numerous insertion and
deletion errors in the assembly itself. The original raw sequencing data
was not made publicly available which prevented a standard genome
assembly polishing to fix the errors. Thankfully, another group uploaded
their sequencing data for a different project and I was able to create a
genome assembly using the assembler Masurca. See below for a comparison
between the two assemblies.

Masurca creates several assemblies one which includes lower confidence
scaffolds. I have included it here because it spans the predicted length
of the genome and because the high confidence assembly is shorter than
the published genome assembly. Including both provides an interesting
insight into both the tool and the assemblies.

{% include gallery id=“gallery_blobtoolkit_ajap_snail”
caption=“Blobtoolkit snail plots of *Apostichopus japonicus* genome
assemblies” %}

Comparing the snail plots of the published and the masurca assembly, one
might think the orginal has longer scaffolds based off the thickness of
the orange and grey lines. However, looking at the legend in the center
of the circle shows that the masurca assembly has much longer scaffolds.
In fact, the masurca assembly has a largest scaffold that is almost
three times as long and an N50 that is almost twice that of the
published assembly. But the published version has another 130 megabases
fo assembly in it then the masurca assembly.

The masurca assembly has roughly 250 megabases of scaffolds that are
longer than one megabase whereas the published version has about 100
megabases of scaffolds longer than one megabase. Amusingly, when looking
at the low confidence masurca assembly, it’s summary stats are very
similar to the published version, however, it has an additional 160
megabases of scaffolds included in its assembly. It is also worth noting
the GC content of the low confidence assembly gets highly variable when
scaffolds are shorter than 100kb.

{% include gallery id=“gallery_blobtoolkit_ajap_blob”
caption=“Blobtoolkit snail plots of *Apostichopus japonicus* genome
assemblies” %}

There is some weird stuff in the published version. There is 6.5
megabases of sequences in the published assembly that have the majority
of blast hits coming from streptophyta. The only reason plant sequences
should be getting blast hits against an animal genome is if there is
contamination. There may have been some algae that the sea cuke was
eating or on its body that found itself in the dna extraction process
and into the assembly.

I will note- for the blob plots of the published assembly I had to use
long read sequence data from a different sea cucumber project as there
was no sequence data available from the original project. However, the
only thing that should be impact is the coverage metrics. The blast hits
are from uniprot and ncbi databases against the original genome.

The masurca assemblies are interesting. There is some skewing to a
higher GC content in shorter scaffolds. This becomes especially apparent
in the low confidence assembly where there are vertical lines of blobs
at specific GC values. I have no idea what that means. There appears to
be one at ~50%, ~55%, and ~45% if you squint. The majority are no-hits
and echinodermata. It could be assemnbly artifacts or repetetive
elements.

{% include gallery id=“gallery_blobtoolkit_ajap_length”
caption=“Blobtoolkit snail plots of *Apostichopus japonicus* genome
assemblies” %}

The streptophyta in the published assembly really pops out when looking
at scaffold length vs GC content. The mean GC content for those
scaffolds appears to be a little lower than the overall mean GC content.
This could suggest those are contamination.

When comparing the masurca and low confidence masura assemblies, those
lines I mentioned earliar appear to be primarily in shorter scaffolds.
While the masurca assembly is cleaner, there are enough scaffolds in the
low confidence assembly that could contain genes that I would be
hesitant to toss it. I would also say that based off the blobtoolkit
results, I have higher confidence in the masurca assemblies than the
published assembly. So with that said - let’s go look at the BUSCO
results to get a sense of how fragmented our gene models would be
between the three.
