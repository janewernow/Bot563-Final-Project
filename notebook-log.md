# Data Set: Beaver Phylogeny
I found my data set from a paper titled "The Beavers Phylogenetic Lineage Illuminated by Retroposon Reads." 
This data helped researchers conclude that beavers (Castoridae) are more closely related to Kangaroo rats (Geomyoidea) within a mouse-related clade than squirrels (suborder Sciuromorpha) which were previously thought to be their closest phylogenetic relatives. 
I believe this data set has been through the QC steps; I have not personally made any alterations to the data set from the paper.
https://static-content.springer.com/esm/art%3A10.1038%2Fsrep43562/MediaObjects/41598_2017_BFsrep43562_MOESM37_ESM.pdf
Data set original files are stored in *Bot563-Final-Project* in *Beaver_Data* in *Castorimorphia_Clade* in *Marker_Original*

# Multiple Sequence Alignments
## Installing programs: ClustalW and MAFFT
I downloaded miniconda (https://docs.conda.io/en/latest/miniconda.html) in order to download ClustalW to my mac **conda install -c bioconda clustalw**.
 I also downloaded MAFFT (https://mafft.cbrc.jp/alignment/software/). Both programs run in the terminal by just typing their name in the command line. Make sure to cd into the folder where the data you want to run is stored before starting the program. 

## Formatting Data for Alignment
I pasted the data set for each gene marker into a visual studio code (any text editor will do) and saved each one as a text file. It is in *Bot563-Final-Project* in *Beaver_Data* in *Castorimorphia_Clade* in *Marker_txt_files*. All data files MUST be saved as a .txt. Make sure to delete any unwanted page numbers or headings from the data that may accidentally get copied into the data when pasting into a text editor.

## Aligning 
### Aligning Data with ClutalW
Make sure you are in the directory of the files you want to align. Run **clustalw** on the terminal and select 1 to inpust sequence data ((Marker_42657.txt)). Then select 2 to run a multiple sequence alignment of the inputted data. Name the output file (Marker_42657_aligned_clustalw.txt) The output will be in phyllip format.
Aligned output data can be found in *Bot563-Final-Project* in *Beaver_Data* in *Castorimorphia_Clade* in *Aligned_ClustalW* in *Phyllip_files_clustalw*

### Aligning Data With MAFFT
Make sure you are in the directory of the files you want to align. Run **mafft** on the terminal. Input the file you want to sequence (Marker_42657.txt). Name the output file (Marker_42657_aligned.txt). Select 1 for clustal format. Select 1 again for "auto" as the strategy. Leave blank for additonal arguments and run for the multiple sequence alignment of the inputted data. The output will be in phyllip format.
Aligned output data can be found in *Bot563-Final-Project* in *Beaver_Data* in *Castorimorphia_Clade* in *Aligned_MAFFT* in *Phyllip_files_mafft*

# Converting Phyllip Files to Other Formats
I used a format converter (http://phylogeny.lirmm.fr/phylo_cgi/data_converter.cgi) to convert my aligned phyllip files to both Fasta and Nexus files. 
Aligned mafft fasta files are in *Bot563-Final-Project* in *Beaver_Data* in *Castorimorphia_Clade* in *Aligned_MAFFT* in *Fasta_files_mafft*
Aligned mafft nexus files are in *Bot563-Final-Project* in *Beaver_Data* in *Castorimorphia_Clade* in *Aligned_MAFFT* in *Nexus_files_mafft*

# Distance and Parsimony
## Installing R Studio + packages
I downloaded R studio and R (https://posit.co/download/rstudio-desktop/). Then in R Studio I installed the necessary packages with **install.packages("adegenet", dep=TRUE)**
**install.packages("phangorn", dep=TRUE)**. Then I loaded the packages using **library(ape)**, **library(adegenet)**, **library(phangorn)**.

## Creating a tree on R
Make sure data file is in FASTA format.  **dna <- fasta2DNAbin(file="NAME OF FILE")**
Computing the genetic distances: here we use the Tamura and Nei 1993 model which allows for different rates of transitions and transversions, heterogeneous base frequencies, and between-site variation of the substitution rate **D <- dist.dna(dna, model="TN93")**
Get the NJ tree **tre <- nj(D)**
We use the ladderize function before plotting which reorganizes the internal structure of the tree to get the ladderized effect when plotted **tre <- ladderize(tre)**
Plot the tree 
**plot(tre, cex=.6)**
**title("A simple NJ tree")** 

# Maximum Liklihood
## Installing Programs: RaX-ML and iqtree
I downloaded both RaX-ML and iqtree, both can be run by typing their name in the command line of the terminal. 

## Creating a Maximum Liklihood tree with iqtree
Make sure you are in the directory of the file you want to make into a tree. Make sure the file is a fasta file. Run **iqtree** in the command line. Run command **iqtree -s NAME OF FILE -bb 1000 -nt AUTO** This command selects the file (-s Marker_42657_aligned_formated.fasta), specifies 1000 replicates for the ultrafast bootstrap (-bb 1000), and determines the best number of CPU cores to speed up the analysis (-nt AUTO). 
This command will output a bunch of files. To view the tree, run **library(ape)** in R. Then run **myTree <- read.tree(file="NAME OF FILE")** and **plot(myTree)** to view the ML tree. To select an outgroup, **myTree <- root(myTree, outgroup="NAME OF OUTGROUP")** and **plot(myTree)**

# Bayesian Inference
## Installing MrBayes 
I downloaded Homebrew **/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"** in order to install MrBayes.
To download and install MrBayes, I used the commands **brew tap brewsci/bio** and 
**brew install mrbayes**

## Creating trees using MrBayes
Make sure the data file is in Nexus format. I converted my fasta files to nexus files using an online converter (http://phylogeny.lirmm.fr/phylo_cgi/data_converter.cgi).
I made an additional text file of my "MrBayes Block" which essentially holds the instructions for MrBayes to run on the dataset (priors, outgroup, taxon info, etc) called "MrBayes_block".
This block should be customized (priors, presets, etc) to your data set, not one block fits all datasets.
Make sure you are in the directory of the nexus file you want to run in MrBayes.
First, combine the data set file and your customized MrBayes block in the terminal using **cat NAMEOFDATAFILE.nex NAMEOFMRBAYESBLOCK.txt > NAMEOFOUTPUTCOMBINEDFILE.nex**
Next, run MrBayes using the command **mb NAMEOFOUTPUTCOMBINEDFILE.nex**
If you run MrBayes without the dataset first, use the command **execute NAMEOFOUTPUTCOMBINEDFILE.nex** and it will do the same thing. 


