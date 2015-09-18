# orthomcl
Candidate gene sequencing
Contains codes

cd flowering
ls */prot*
cat */prot* > allproteins.fasta
grep ">" allproteins.fasta  | wc -l
  #3581
#To create fasta files compliant with OrthoMCL.
cd complianFasta
orthomclAdjustFasta Pbuckler ../buckler2009/protein_buckler2009.fasta 4
orthomclAdjustFasta Pflorid ../florid_idb/protein440_floriddb_08102015.fasta 1
orthomclAdjustFasta Phiggins ../higgins2010/protein_higgins2010.fasta 1
orthomclAdjustFasta Prefseq ../refseq_flowering_greenplants/protein_flowering_refseq_greenplants.fasta 4
orthomclAdjustFasta Psrikanth ../srikanth2011/protein_srikanth2011.fasta 1
orthomclAdjustFasta Pyano_hd1 ../yano2001/hd1_monocots.fasta 2 #had to do this on yano file because of duplicates
orthomclAdjustFasta Pyano_hd3a ../yano2001/hd3a_monocots.fasta 2
orthomclAdjustFasta Pyano_hd6 ../yano2001/hd6_monocots.fasta 2
orthomclAdjustFasta Pyano_se5 ../yano2001/se5.fasta 2




