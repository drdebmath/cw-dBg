# cw-dBg: the compressed weighted de Bruijn graph

Author: Nicola Prezza. Joint work with Giuseppe Italiano, Blerina Sinaimeri, Rossano Venturini

_Modified by Debasish Pattanayak to add external sorting_

### Description

**Warning: experimental code. dBg construction uses external sorting. See [results.md](https://github.com/drdebmath/cw-dBg/blob/main/results.md) for estimated RAM usage.**

This library builds a compressed representation of the weighted de Bruijn graph. The underlying graph topology is stored using the BOSS representation, while the weights are differentially encoded and sampled on a spanning tree of the graph chosen to minimize the total bit-size of the structure. Results show that on a 20x-covered dataset with 27M distinct kmers (700Mbases in total), the whole structure takes 5.44 bits per kmer (just 18 MB in total).

### Download

To clone the repository, run:

> git clone http://github.com/drdebmath/cw-dBg

> cd cw-dBg

> git clone https://github.com/stxxl/stxxl.git

### Compile

The library has been tested under linux using gcc 9.2.1. You need the SDSL library installed on your system (https://github.com/simongog/sdsl-lite).

We use cmake to generate the Makefile. Create a build folder in the main cw-dBg folder:

> mkdir build

run cmake:

> cd build; cmake ..

and compile:

> make

To run stxxl, set a system disk with desired file size in a file .stxxl in the build directory

> disk=/var/tmp/stxxl,500GiB,syscall unlink

### Run

After compiling, run 

>  cw-dBg-build [-l nlines] [-a] [-s srate] input k

to build the compressed weighted de Bruijn graph of order k on the file input (a fastq file by default, or a fasta file if option -a is specified). if option -l nlines is specified, build the graph using only the first nlines sequences from the input file. If option -s srate is specified, sample one out of srate weights (default: srate=64).

The tool cw-dBg-check allows to benchmark the data structure previously built as follows:  

>  cw-dBg-check [options] <input_index> <input_fastx>  

Options:  
&nbsp;&nbsp;&nbsp;&nbsp;-q <arg>            Extract and test the structure on the first maximum <arg> k-mers in the dataset. Default: 1000000  
&nbsp;&nbsp;&nbsp;&nbsp;-a                  The input file is fasta. If not specified, it is assumed that the input file is fastq.  
&nbsp;&nbsp;&nbsp;&nbsp;-c                  Check correctness of the structure against a classic hash (space-consuming!!). Default: false.  
&nbsp;&nbsp;&nbsp;&nbsp;<input_index>       Input index built with cw-dbg-build. Mandatory.  
&nbsp;&nbsp;&nbsp;&nbsp;<input_fastx>       Fasta/fastq file from which test kmers will be extracted. Must be the same on which the index was built.  
