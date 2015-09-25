 Load necessary modules
 
 ```module load orthomcl```
 
 ```module load ncbi_blast python perl```
 
 Create a new directory
 
 ```mkdir compliantFasta```
 
 ```cd compliantFasta/ ```
 
 Create compliant files
 
```orthomclAdjustFasta Pbuckler ../protein_buckler2009.fasta 4```
 
```orthomclAdjustFasta Pflorid ../protein440_floriddb_08102015.fasta 1```
 
```orthomclAdjustFasta Phiggins ../protein_higgins2010.fasta 1```
 
```orthomclAdjustFasta Prefseq ../protein_flowering_refseq_greenplants.fasta 4```
 
``` orthomclAdjustFasta Psrikanth ../protein_srikanth2011.fasta 1 ```
 
Create orthomcl database

```hpc_create_orthomcl_database orthomcl_dpaudel_newortho```

Check if database is created

```hpc_list_orthomcl_databases ```

Give permission to sh file

```chmod +x *.*```

Extract sequence

```scripts/ExtractSeq.sh -o sequences named_sco_groups_1.5.txt goodProteins.fasta ```
