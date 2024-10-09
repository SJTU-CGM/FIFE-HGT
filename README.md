# FIFE-HGT: a Fast Identifier For Eukaryotes of Horizontal Gene Transfers
FIFE-HGT is a fast identification method for HGTs in eukaryotes by combining sequence composition filtering and sequence alignment to reduce both the false-positive rate and computational cost. Moreover, it could identify some novel HGTs because it broke the restriction of identifying HGTs involving protein coding regions only.

## Installation
### Requirements

- Perl 5.26.3 or up
- Software needed: lastz (1.04.00), RepeatMasker (4.0.7), TRF (4.09), cd-hit-est (4.6.6), Bowtie2 (2.2.4), BLAST (2.12.0), MAFFT (7.520), trimAl (1.4.rev15), IQ-TREE (2.2.2.3).

### Installation procedures
#### 1. Download the FIFE-HGT toolbox from github
```
$ git clone https://github.com/SJTU-CGM/FIFEHGT.git
$ tar zxvf FIFE-HGT.tar.gz
```
#### 2. Add bin/ to PATH and add lib/ to LD_LIBRARY_PATH
```
$ export PATH=$PATH:/path/to/FIFEHGT/:
$ export PERL5LIB=$PERL5LIB:/path/to/FIFEHGT/lib/:
$ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/path/to/FIFEHGT/lib/:
```
#### 3. Test if FIFEHGT toolbox is installed successfully: `FIFEHGT`. If you see the following content, congratulations! HUPAN toolbox is successfully installed. If not, see if all the requirements are satisfied.
```
Usage: FIFEHGT <command> ...

Available commands:
	kmerFilter  Select most different fragments with target genome by calculating the distance of kmer frequecies
	splitDB     Split the genomic database into three groups: SG, CRG and DRG
	seqAlign	  Sequence aligment using LASTZ
	screenHGT	  Screen potential HGTs using sequence alignment results
	WGSValidate	Validate potential HGTs using WGS datasets
	conPhyTree	Construct sequence phylogenetic tree to validate HGTs
```
The usage information for each command can be shown with the --help or -h option after each command name. 

## Quick start
### Example data
We use the identification of HGTs in one chromosome of yeast with some bacteria as the example data presenting at `path/to/FIFEHGT/data/`. 
The dataset includes 
-  `path/to/FIFEHGT/example/data/fa/sacCer3_NC_001144.5.fa`: sequence of one chromosome of yeast
-  `path/to/FIFEHGT/example/data/db/refseq/*`: genome databases including 10 fungi and 13 bacteria
-  `path/to/FIFEHGT/example/data/db/*txt`: taxonomic information of all organisms in genome datasets
-  `path/to/FIFEHGT/example/data/db/WGSdata/*`:  Whole genomic sequencing datasets including several bacteria
-  `path/to/FIFEHGT/example/data/mitochondria_chloroplast/*`: mitochondrial sequence of yeast
-  `path/to/FIFEHGT/example/data/rmsk/*`: repeat sequence annotation file of yeast
-  `path/to/FIFEHGT/example/data/singlecopy/*`: single copy genes common in all eukaryotes.
### Manual (using example data mentioned above)
#### 1. Select most different fragments with target genome by calculating the distance of kmer frequecies
```
$ FIFEHGT kmerFilter --fasta path/to/FIFEHGT/example/data/fa/sacCer3_NC_001144.5.fa
```
#### 2. Split the genomic database into three groups: SG, CRG and DRG
```
$ FIFEHGT splitDB --id GCF_000146045.2 --taxo Fungi,Ascomycota,Saccharomycetes,Saccharomycetales --taxoes path/to/FIFEHGT/example/data/db/all_range.txt --type path/to/FIFEHGT/example/data/db/all_type.txt
```
#### 3. Sequence aligment using LASTZ
```
$ FIFEHGT seqAlign --db path/to/FIFEHGT/example/data/db/refseq
```
#### 4. Screen potential HGTs using sequence alignment results (strict mode was used)
```
$ FIFEHGT screenHGT --mode Strict --repeat path/to/FIFEHGT/example/data/rmsk/rmsk.txt --singlecopy path/to/FIFEHGT/example/data/singlecopy/eukaryota.faa --mitChl path/to/FIFEHGT/example/data/mitochondria_chloroplast/mitochondria.fa --taxo Fungi,Ascomycota,Saccharomycetes,Saccharomycetales
```
#### 5. Validate potential HGTs using WGS datasets
```
$ FIFEHGT WGSValidate --dbdir path/to/FIFEHGT/example/data/db/refseq/ --mode Strict --dbInfo path/to/FIFEHGT/example/data/db/all_info.txt --taxo Fungi,Ascomycota,Saccharomycetes,Saccharomycetales --dbWGSDir path/to/FIFEHGT/example/data/db/WGSdata/
```
#### 6. Construct sequence phylogenetic tree to validate HGTs
```
$ FIFEHGT conPhyTree --mode Strict --genomeId GCF_000146045.2 --fullName Saccharomyces_cerevisiae_S288C --dbId path/to/FIFEHGT/example/data/db/all_id.txt --dbInfo path/to/FIFEHGT/example/data/db/all_info.txt
```
The final HGTs were saved in path/to/FIFEHGT/example/WGSValidate/afterWGS/HGT*
