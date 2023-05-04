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
Make sure you are in the directory of the files you want to align. Run **clustalw** on the terminal and select 1 to inpust sequence data (Marker_42657.txt). Then select 2 to run a multiple sequence alignment of the inputted data. Select 1 to Do complete multiple alignment now Slow/Accurate. Name the output file (Marker_42657_aligned_clustalw.txt) The output will be in phyllip format.
Aligned output data can be found in *Bot563-Final-Project* in *Beaver_Data* in *Castorimorphia_Clade* in *Aligned_ClustalW* in *Phyllip_files_clustalw*

### Aligning Data With MAFFT
Make sure you are in the directory of the files you want to align. Run **mafft** on the terminal. Input the file you want to sequence (Marker_42657.txt). Name the output file (Marker_42657_aligned.txt). Select 1 for clustal format. Select 1 again for "auto" as the strategy. Leave blank for additonal arguments and run for the multiple sequence alignment of the inputted data. The output will be in phyllip format.
Aligned output data can be found in *Bot563-Final-Project* in *Beaver_Data* in *Castorimorphia_Clade* in *Aligned_MAFFT* in *Phyllip_files_mafft*

# Converting Phyllip Files to Other Formats
I used a format converter (http://phylogeny.lirmm.fr/phylo_cgi/data_converter.cgi) to convert my aligned phyllip files to both Fasta and Nexus files. 
Aligned mafft fasta files are in *Bot563-Final-Project* in *Beaver_Data* in *Castorimorphia_Clade* in *Aligned_MAFFT* in *Fasta_files_mafft*
Aligned mafft nexus files are in *Bot563-Final-Project* in *Beaver_Data* in *Castorimorphia_Clade* in *Aligned_MAFFT* in *Nexus_files_mafft*


# Maximum Liklihood
## Installing Programs: RaX-ML and iqtree
I downloaded both RaX-ML and iqtree, both can be run by typing their name in the command line of the terminal. 

## Creating a Maximum Liklihood tree with iqtree
Make sure you are in the directory of the file you want to make into a tree. Make sure the file is a fasta file. Run **iqtree** in the command line. Run command **iqtree -s Marker_42657_aligned_fasta.txt -bb 1000 -nt AUTO** This command selects the file (-s Marker_42657_aligned_formated.fasta), specifies 1000 replicates for the ultrafast bootstrap (-bb 1000), and determines the best number of CPU cores to speed up the analysis (-nt AUTO). 
This command will output a bunch of files. To view the tree, run **library(ape)** in R. Then run **myTree <- read.tree(file="Marker_42657_aligned_formated.fasta.iqtree")** and **plot(myTree)** to view the ML tree. To select an outgroup, **myTree <- root(myTree, outgroup="PB1D10")** and **plot(myTree)**

Alternatively, to view the tree you can use the command **tre <- read.tree(text="(PB1D10:0.1228098586,Castor:0.0969025868,((Dipodomys:0.0628382842,Chaetodipus:0.1551797055)100:0.2228923674,(((((((Mus:0.0452138931,Rattus:0.0597236617)58:0.0000023788,Apodemus:0.0409490516)100:0.0510547586,(Peromyscus:0.0434392075,Microtus:0.0928499895)89:0.0174962841)100:0.1143766892,Nannospalax:0.1397668019)82:0.0444066204,(Ictidomys:0.1231925847,Cavia:0.3697410752)94:0.0723348322)50:0.0120420420,Jaculus:0.1616687609)64:0.0269756813,Anomalurus:0.1998679010)100:0.1209404318)74:0.0266045506);")** if you want to use the text of the tree found in the file released  by iqtree. This text is found in the file *"Marker_42657_aligned_formated.fasta.iqtree"* but I chose to use this method beacuse the file had too much information which caused R to crash. 


#### Reasoning
I chose to use IQtree because of its fast and efficient computing abilities. It supports a wide range of models for molecular evolution, including nucleotide, amino acid, codon, and morphological models. It allows you to set specific parameters like substitution models and branch lengths. It is important to note that with sparse data, IQtree may not always converge to the correct tree! 



# Bayesian Inference
## Installing MrBayes 
I downloaded Homebrew **/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"** in order to install MrBayes.
To download and install MrBayes, I used the commands **brew tap brewsci/bio** and 
**brew install mrbayes**

## Creating trees using MrBayes
Make sure the data file is in Nexus format. 
I made an additional text file of my "MrBayes Block" which essentially holds the instructions for MrBayes to run on the dataset (priors, outgroup, taxon info, etc) called "MrBayes_block".
This block should be customized (priors, presets, etc) to your data set, not one block fits all datasets.
Make sure you are in the directory of the nexus file you want to run in MrBayes.
First, combine the data set file and your customized MrBayes block in the terminal using **cat Marker_42657_aligned_nexus.nex MrBayes_block.txt > Marker_42657_aligned_nexus_mb.nex**
Next, run MrBayes using the command **mb Marker_42657_aligned_nexus_mb.nex**
If you run MrBayes without the dataset first, use the command **execute Marker_42657_aligned_nexus_mb.nex** and it will do the same thing. 

To view the tree, run **library(ape)** in R. Then run **tre <- read.nexus(file="Marker_42657_aligned_nexus_mb.nex.con.tre", tree.names = NULL, force.multi = FALSE)** and **plot(tre)** to view the consensus tree. 

##### Reasoning
I chose to use MrBayes because it implements a Bayesian approach to phylogenetics, which is a powerful and flexible framework for inference that can incorporate a wide range of models and priors. It is relatively fast compared to other methods of Bayesian inference due to its use of MCMC algorithms. MrBayes can be computationally expensive for large data sets. The results are also extremely influenced by the priors, complexity of the model, and amount of data in the dataset. Therefore it is very important to run multiple MCMC chains with the priors to ensure good mixing and convergence of chains. 


