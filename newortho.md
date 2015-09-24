 Load necessary modules
 
 ```module load orthomcl
 
 module load ncbi_blast python perl```
 
 Create a new directory
 
 ```mkdir compliantFasta
 
 cd compliantFasta/ ```
 
 Create compliant files
 
 ```orthomclAdjustFasta Pbuckler ../protein_buckler2009.fasta 4
 
 orthomclAdjustFasta Pflorid ../protein440_floriddb_08102015.fasta 1
 
 orthomclAdjustFasta Phiggins ../protein_higgins2010.fasta 1
 
 orthomclAdjustFasta Prefseq ../protein_flowering_refseq_greenplants.fasta 4
 
 orthomclAdjustFasta Psrikanth ../protein_srikanth2011.fasta 1 ```
 
