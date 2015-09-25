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

To specify the structure for the orthomcl database that we just created, we will use the following command:

```orthomclInstallSchema mysql.config mysql.log ```

Now, the database is ready to up load the similarSeqeunces.txt file

UPLOAD DATA INTO THE DATABASE

```orthomclLoadBlast mysql.config similarSequences.txt```

Once uploaded we can call pairs (potential orthologs, co-orthologs and in-paralogs).

```orthomclPairs mysql.config pairs.log cleanup=no```

This is a computationally intensive step that finds protein pairs looking in to the BLAST results that was
uploaded. This program executes a series of 20 internal steps, each creating an intermediate database
table or index. Finally, it populates the three output tables: Orthologs, InParalogs and CoOrthologs.
cleanup=no, will make sure that all intermediary tables in the database are kept so that, if one of those
steps fails, you can restart from the step it failed. You can also use options such as yes (not to keep
intermediate tables), only (drops table if all steps are successful), all (retains only final 3 tables).

GETTING RESULTS

To get the results back from the database made by orthomclPairs, orthomclDumpPairsFiles command can be used.

```orthomclDumpPairsFiles mysql.config```

The output will be a directory (called pairs) and a file (called mclinput). The pairs directory, will
contain three files: orthologs.txt, coorthologs.txt, inparalogs.txt. Each of these files describes
pair-wise relationships between proteins. They have three columns: Protein 1, Protein 2 and
normalized similarity score between them. The mclinput file contains the identical information as
the three files in pairs directory but merged as a single file and in a format accepted by the mcl program.

MCL program (Dongen 2000) will be used to cluster the pairs extracted in the previous steps to determine ortholog groups.

```mcl mclInput --abc -I 1.5 -o groups_1.5.txt```

Here, --abc refers to the input format (tab delimited, 3 fields format), -I refers to inflation value and -o
refers to output file name. Inflation value will determine how tight the clusters will be. It can range from 1
to 6, but most publications use values between 1.2 -1.5 for detecting orthologous groups.

The final step is to name the groups called by mcl program.

```orthomclMclToGroups OG1.5_ 1000 < groups_1.5.txt > named_groups_1.5.txt```

Here, OG1.5_ is the prefix we use to name the ortholog group, 1000 is the starting number for the
ortholog group and last 2 fields are input and output file name respectively

FORMATTING RESULTS

Once we have the Group numbers and IDs (named_groups_1.5.txt), we can generate a summary table showing number of genes present in each genome for each ortholog group as well as extract ortholog groups that have single copy gene in each species. We will use 2 custom scripts for this purpose:

```scripts/CopyNumberGen.sh named_groups_1.5.txt > named_groups_1.5_frequency.txt ```

```scripts/ExtractSCOs.sh named_groups_1.5_frequency.txt > scos_list.txt ```

The scos_list.txt generated will have all ortholog groups that have only one copy gene in all species. We will use this list to generate a file similar to named_groups_1.5.txt but with only ortholog groups that are present in scos_list.txt

```cut -f 1 scos_list.txt > ids.txt ``` #cut first field from the file

```while read line; do grep -w "$line" named_groups_1.5.txt; done<ids.txt > named_sco_groups_1.5.txt``` #filtering group file

Extract sequence

```scripts/ExtractSeq.sh -o sequences named_sco_groups_1.5.txt goodProteins.fasta ```
