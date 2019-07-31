# Parse Pfam

## Purpose

Get query alignment from the Pfam Stockholm Alignment files

## Motivation 

In Pfam website, some domain family alignments are not downloadable. e.g The [FN3 domain](https://pfam.xfam.org/family/fn3#tabview=tab3) using NCBI sequence database . Thus we need to retrieve this data from the big files from the [Pfam ftp site](ftp://ftp.ebi.ac.uk/pub/databases/Pfam/current_release/database_files/). 

However, there is not such an existing tool to extract all the alignments given a domain ID from the Pfam multiple concatenated alignments files. The most relevant tool Biopython supports parsing Stockholm alignment format but does not support output alignment given a query domain accession ID.    

Here, a handy script is written to extract alignments from Pfam file in Stockholm format given a domain ID from Pfam. 

## Usage

Input 1: The Pfam file that contains full alignments of all Pfam-A families e.g. Pfam-A.full.ncbi.gz 
Input 2: Pfam Domain ID e.g PF00041
Output: Alignment file with format as following

| Sequence_ID_1 | Aligned_Sequence_1|
| Sequence_ID_2 | Aligned_Sequence_2|


Python parse_pfam_stockholm.py Pfam-A.full.ncbi.gz PF00041 > output.file 




