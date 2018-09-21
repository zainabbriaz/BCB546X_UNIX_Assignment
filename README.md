# BCB546X_UNIX_Assignment
##### UNIX Assignment BCB546X
 
_**DATA INSPECTION**_

Logged into hpc-class using the shortcut created in class `ssh hpc_class` 

Navigated into the UNIX Assignment folder 
`cd ~/BCB546X-Fall2018/assignments/UNIX_Assignment/`

Inspected size of all files in the UNIX_Assignment folder:
`ls -l -h *`

Inspected number of lines of all .txt files in the UNIX_Assignment folder:
`wc -l *.txt`

Inspected number of columns of both .txt files in the UNIX\_Assignment folder: 
`awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt`
`awk -F "\t" '{print NF; exit}' snp_position.txt`


_**DATA PROCESSING - MAIZE:**_

`grep -w -E "(ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt >genotypes_maize.txt`


` head -n 1 fang_et_al_genotypes.txt > header_fang.txt`


` cat header_fang.txt genotypes_maize.txt > genotypes_maize_header.txt`


`awk -f transpose.awk genotypes_maize_header.txt  > genotypes_maize_transposed.txt`


`sort -k1,1 snp_position.txt > sorted_snp_position.txt`

`sort -k1,1 genotypes_maize_transposed.txt > sorted_maize.txt`

`join -1 1 -2 1 -a 2 sorted_snp_position.txt sorted_maize.txt > joined_snp_maize.txt`

` wc -l sorted_snp_position.txt sorted_maize.txt joined_snp_maize.txt`

10 files with SNP in ascending: ` awk '$2 = 1' joined_snp_maize.txt | sort -k 3 -n > 1maize.txt`

10 files with SNP descending: `awk '$2 = 1' joined_snp_maize.txt |sort -r -n -k3 | sed 's/?/-/g' > 1r_maize.txt`

1 file with unknown positions: 
`awk '$2 = "unknown"' joined_snp_maize.txt > unknown_maize.txt`

1 file with multiple positions: `awk '$2 = "multiple"' joined_snp_maize.txt > multiple_maize.txt`

_**DATA PROCESSING - TEOSINTE:**_

`grep -w -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt >genotypes_teosinte.txt`

`cat header_fang.txt genotypes_teosinte.txt > genotypes_teosinte_header.txt`

`awk -f transpose.awk genotypes_teosinte_header.txt > genotypes_teosinte_transposed.txt`

`sort -k1,1 genotypes_teosinte_transposed.txt > sorted_teosinte.txt`

`join -1 1 -2 1 -a 2 sorted_snp_position.txt sorted_teosinte.txt > joined_snp_teosinte.txt`

10 files with SNP ascending: 
`awk '$2 = 1' joined_snp_teosinte.txt | sort -k 3 -n > 1teosinte.txt`

10 files with SNP descending:
`awk '$2 = 1' joined_snp_teosinte.txt |sort -r -n -k3 | sed 's/?/-/g' > 1r_teosinte.txt`

1 file with unknown positions: `awk '$2 = "unknown"' joined_snp_teosinte.txt > unknown_teosinte.txt`

1 file with multiple positions: `awk '$2 = "multiple"' joined_snp_teosinte.txt > multiple_teosinte.txt`


Check: `cat -n 1 1maize.txt` 
`vim 1maize.txt`
