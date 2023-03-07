# Data Set: Beaver Phylogeny
I found my data set from a paper titled "The Beavers Phylogenetic Lineage Illuminated by Retroposon Reads." 
This data helped researchers conclude that beavers (Castoridae) are more closely related to Kangaroo rats (Geomyoidea) within a mouse-related clade than squirrels (suborder Sciuromorpha) which were previously thought to be their closest phylogenetic relatives. 
I believe this data set has been through the QC steps; I have not personally made any alterations to the data set from the paper.
https://static-content.springer.com/esm/art%3A10.1038%2Fsrep43562/MediaObjects/41598_2017_BFsrep43562_MOESM37_ESM.pdf

# Installing my programs: ClustalW and Mafft
I downloaded miniconda in order to download ClustalW to my mac. I also downloaded Mafft. Both programs run in the terminal by just typing their name in the command line. Make sure to cd into the folder where the data you want to run is stored before starting the program. 

# Formatting Data for Alignment
I pasted the data set for each gene marker into a visual studio code (any text editor will do but this worked the best for me) and saved each one as a text file. I put it in my Bot563-Final-Project folder in the Beaver_Data folder. All data files MUST be saved as a .txt. Make sure to delete any unwanted page numbers or headings from the data that may accidentally get copied into the data when pasting into a text editor.

# Installing R Studio + packages
I downloaded R studio and R. Then in R Studio I installed the necessary packages with **install.packages("adegenet", dep=TRUE)**
**install.packages("phangorn", dep=TRUE)**. Then I loaded the packages using **library(ape)**, **library(adegenet)**, **library(phangorn)**.

# Creating a tree on R
Make sure data file is in FASTA format.  **dna <- fasta2DNAbin(file="http://adegenet.r-forge.r-project.org/files/usflu.fasta")**
Computing the genetic distances: here we use the Tamura and Nei 1993 model which allows for different rates of transitions and transversions, heterogeneous base frequencies, and between-site variation of the substitution rate **D <- dist.dna(dna, model="TN93")**
Get the NJ tree **tre <- nj(D)**
We use the ladderize function before plotting which reorganizes the internal structure of the tree to get the ladderized effect when plotted **tre <- ladderize(tre)**
Plot the tree 
**plot(tre, cex=.6)**
**title("A simple NJ tree")** 