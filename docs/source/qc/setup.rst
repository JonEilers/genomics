
Setup
================
Bioinformatics and computational biology have a low entry cost. But it comes with a very very steep learning curve. All you need is a computer, preferably one with a lot of CPUs and RAM or access to a server/aws. The cost is going to be a very very steep learning curve. Some might call it `logarithmic <https://en.wikipedia.org/wiki/Logarithm>`_.

Hardware
--------
Working with large sequence datasets or on genomes requires some computational power. Most laptops or even desktop computers with their configurations maxed will not be able to handle the majority of genome bioinformatics. This is why most biology departments that are serious about incorporating bioinformatics into their program have access to a school computer cluster or purchase computational time on AWS. However, if you are freelance bioinformatician like me, neither of those are good options. My secret is that I realized servers have been around for awhile now and data centers replace them every so many years which means someone somewhere is surplusing them for cheap. For example 
`Newegg <https://www.newegg.com/p/pl?N=100852105%204016%20600031341>`_ has a lot and they are quite cheap. Cheaper than buying a new shiny workstation or laptop.

So I bought a barebones dell poweredge r910 for a few hundred and then wandered over to ebay where I bought four 10 core cpus for 20 dollars, sixteen 600gb hard drives for 10 dollars per a hard drive, and 362gb of ram for less than a dollar per a 1g of ram. Let me tell you, this beast purrrrrs and also heats my apartment in the winter. For awhile the server was sitting on top of a dressor, but I upgraded to a rack eventually and also bought a refurbished UPS ebay to smooth out power flucuations. Total cost? probably sitting at close to $2k at this point, but I have a machine that can do genome bioinformatics for a fraction of the cost of what most people pay. 

Software
--------
The majority of bioinformatics tools that are readily available are intended to be used in a linux environment using a command line terminal such as bash. There is no way to get around that. Some companies have developed nice graphical user interfaces, but those tools are expensive and also lack the flexibility you will have when working from the command line.

Learning how to use linux and navigating the commandline is a very steep learning curve. But thankfully there is a wealth of information and tutorials available on the internet. The best skill you will develop while learning and doing bioinformatics is how to search for information on the internet. No joke - you will spend copius amounts of time rummaging around for answers to why something isn't working or how to do such and such in python. A great search engine for this is `DuckDuckGo <https://duckduckgo.com/>`_. This is relevant as microsoft owns github and bing/duckdcukgo seem to prioritize searching through github where most bioinformatic tools are kept/developed. Google doesn't seem to do this very well. 

Bash
~~~~
In case you are wondering: I use `Ubuntu Desktop <https://ubuntu.com/download/desktop>`_ installed on my server.  It's easy to install and feels similar enough to using windows or macOS that it isn't too painful to learn. Built into ubuntu is something call bash. It is a commandline tool for running scripts and executing tools (among many other uses) and is the backbone of all bioinformatics. **You will have to become familiar with bash.**  

Typing commands into the command terminal is how I spend most of my time interfacing with bioinformatic tools. You will need to be proficient in the basics of bash. There are numerous introduction tutorials scattered throughout the internet. I don't know which is a good one, but for now I will drop this `Introduction to Bash <https://www.javatpoint.com/bash-introduction>`_ to one that looked promising at a glance. 

Package Management Software
~~~~~~~~~~~~~~~~~~~~~~~~~~~
Once you have Ubuntu set up and a few bash commands, install some flavor of `Anaconda <https://www.anaconda.com/>`_. This will make life so easy when installing and managing bioinformatics tools. Anaconda comes with a tool called 'conda' which will allow you to easily install bioinformatic tools. 

For example::

    # creating an environment which will minimize package dependency conflict
    conda create -n sratools

    # activating the environment
    conda activate sratools

    # installing sratoolkit into the environment. Sratoolkit can be used to download data from NCBI
    conda install sratoolkit

    # you can also create an environment and install directly into it
    conda create -n sratools sratoolkit

There is a GUI for conda, but I suggest you run it from the command line. 

Recently I have been running into issues with conda. A bit of poking around pulled up an alternative called `mamba <https://mamba.readthedocs.io/en/latest/index.html>`_. It is intended to be a replacement for conda which is faster and more robust with some added features.::

    # installing - be sure to only install into the base conda environment
    conda install mamba -n base -c conda-forge

    # creating environments and installing packages
    mamba create -n sratools sratoolkit 

    # you will still want to use conda for activating and deactivating environments
    conda activate sratools

Containerization Tools
~~~~~~~~~~~~~~~~~~~~~~
Sometimes you will find you are unable to get a tool setup and working. Maybe because you don't have permissions, maybe you are going through dependency hell, or maybe compiling the tool is resulting in red error text everywhere. Conda can't always solve this and you will need to take it to the next level and use containers. There are two primary tools for this: `Docker <https://www.docker.com/ >`_ and `Singularity <https://sylabs.io/guides/3.5/user-guide/introduction.html>`_. 

They are both a little more involved than using a package manager, but they can be very useful in cases where there are a lot of dependencies and conda can't handle it. You download the container image, mount it on a directory, and run it. Viola! Everything is working. 

One example of a useful container set up can be found in `TeTools <https://github.com/Dfam-consortium/TETools>`_. If you don't want to deal with the headache of installing all the dependencies, this is the solution for you. They even created a nice bash script.::

    # downloading the bash script
    curl -sSLO https://github.com/Dfam-consortium/TETools/raw/master/dfam-tetools.sh

    # making it executable
    chmod +x dfam-tetools.sh

    # running the script which will download the image and create a container. I suggest you use singularity as your container tool
    ./dfam-tetools.sh --singularity

And now you have a suite  of repeat identification, annotation, and masking tools at your finger tips. 

Text Editors
~~~~~~~~~~~~
You will want to keep track of everything you do, what commands you use and what options you tweaked etc. A great text file editor for that is called `sublime <https://www.sublimetext.com/>`_. Another great tool for that has the added benefit of directory visualization and roughly the same functionality of sublime is `visual studio <https://code.visualstudio.com/>`_. While both tools are intended for writing and editing code, they are also great for keeping notes on tools and commands you are using.

I organize work directory by project, for example *Apostichopus californicus* genome project. Within that directory I will either create folders specific each tool I am using or create folders that are purpose specific. For example 'genome assembly' will then have 'masurca' and 'platanus-allee' inside of it. 

Pipeline Tools
~~~~~~~~~~~~~
In recent years there has been a great deal of discussion around the reproducibility of studies. One aspect that is how people will write one off scripts to run their analysis and never published them so their is no way to validate their data analysis. Or if they do publish, they never state what version of a tool they used. 

That is where pipeline tools such as `snakemake <https://snakemake.readthedocs.io/en/stable/>`_  and `nextflow <https://www.nextflow.io/>`_ are useful. They force a standard way of writing scripts, include version numbers, and allow for complete reproducibililty of analysis results. 

One example can be found in the tool called `blobtoolkit <https://blobtoolkit.genomehubs.org/pipeline/>`_. Instead of manually entering oneoff commands into the bash, you enter all the information into a configeration file and run it using snakemake. An example for blobtoolkit can be found `here <https://blobtoolkit.genomehubs.org/pipeline/pipeline-tutorials/configuring-the-pipeline/>`_. 

A more complex example could be creating an genome assembly and quality assessment pipeline. Nextflow Examples can be found `here <https://nf-co.re/pipelines?q=genome-assembly>`_.

Concluding
-------
Once you have those tools installed you are basically ready to go! Ready to get your hands dirty learning how to use the command line that is. It is a very steep learning curve, but once you get a handle of the basics you will be the cool kid in town and all the other biologists in your department will be coming to you for help. Shoot, you will even end up on more publications cause everyone needs a bioinformatician these days. 
