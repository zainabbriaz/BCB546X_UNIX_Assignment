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

Viewed the files using the `cat (filename)` `head (filename)` and `tail (filename)` commands to get an idea about the contents of the _snp_position.txt_ and _fang_et_al_genotypes.txt_ files. 

_**DATA PROCESSING - MAIZE:**_


Extracted the required Maize groups from fang_et_al_genotypes.txt using the `grep` command with "-w" and "-E" to match the group names and generated a file containing Maize genotypes

`grep -w -E "(ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt >genotypes_maize.txt`


To keep the headers from the fang file in the new Maize genotypes file, I extracted the headers using the `head` command to print the first line into an intermediate file which I then concatenated with the maize genotypes file 

` head -n 1 fang_et_al_genotypes.txt > header_fang.txt`
` cat header_fang.txt genotypes_maize.txt > genotypes_maize_header.txt`


Transposed the maize genotypes file with header using the following command in `awk` as provided in the assignment hints

`awk -f transpose.awk genotypes_maize_header.txt  > genotypes_maize_transposed.txt`


Sorted the snp position file and transposed maize file on their common first column using the following `sort` commands 

`sort -k1,1 snp_position.txt > sorted_snp_position.txt`

`sort -k1,1 genotypes_maize_transposed.txt > sorted_maize.txt`


Joined both sorted files while keeping the unpairable lines from the sorted maize file

`join -1 1 -2 1 -a 2 sorted_snp_position.txt sorted_maize.txt > joined_snp_maize.txt`


Checked joining using the command:
`wc -l sorted_snp_position.txt sorted_maize.txt joined_snp_maize.txt`


_**Generating the required files**_


10 files with SNPs ordered in increasing position values: Typed the following `awk` command 10 times, once for each chromosome, to select chromosome number from the second column and piped it to the sort by numeric value command for the third column for SNP position  

` awk '$2 = (chromosomenumber)' joined_snp_maize.txt | sort -k 3 -n > (chromosomenumber)maize.txt`


10 files with SNPs ordered in decreasing position values: Typed the following `awk` command ten times, once for each chromosome, to select chromosome number from the second column and piped it to the sort by numeric value command and reverse the results for the third column for SNP position. This was in turn piped to the stream editor `sed` command to convert '?' to '-' for missing data. 

`awk '$2 = (chromosomenumber)' joined_snp_maize.txt |sort -r -n -k3 | sed 's/?/-/g' > (chromosomenumber)r_maize.txt`


The following command was used to generate a file containing SNPs with unknown positions. "unknown" string was extracted from the second column. 

`awk '$2 = "unknown"' joined_snp_maize.txt > unknown_maize.txt`


The following command was used to generate a file containing SNPs with multiple positions. "multiple" string was extracted from the second column.

`awk '$2 = "multiple"' joined_snp_maize.txt > multiple_maize.txt`

_**DATA PROCESSING - TEOSINTE:**_

Similar script as the Maize files was used to process the Teosinte files, with substitution of the group names for Teosinte:


`grep -w -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt >genotypes_teosinte.txt`

`cat header_fang.txt genotypes_teosinte.txt > genotypes_teosinte_header.txt`

`awk -f transpose.awk genotypes_teosinte_header.txt > genotypes_teosinte_transposed.txt`

`sort -k1,1 genotypes_teosinte_transposed.txt > sorted_teosinte.txt`

`join -1 1 -2 1 -a 2 sorted_snp_position.txt sorted_teosinte.txt > joined_snp_teosinte.txt`


_**Generating the required files**_

10 files with SNPs ordered in increasing position values

`awk '$2 = (chromosomenumber)' joined_snp_teosinte.txt | sort -k 3 -n > (chromosomenumber)teosinte.txt`

10 files with SNPs ordered in decreasing position values and '-' for missing data

`awk '$2 = (chromosomenumber)' joined_snp_teosinte.txt |sort -r -n -k3 | sed 's/?/-/g' > (chromosomenumber)r_teosinte.txt`

1 file with unknown SNP positions 

`awk '$2 = "unknown"' joined_snp_teosinte.txt > unknown_teosinte.txt`

1 file with multiple SNP positions

`awk '$2 = "multiple"' joined_snp_teosinte.txt > multiple_teosinte.txt`


#To check the files that were generated I used `cat -n 1 (filename)` and `vim (filename)`

##I worked on this assignment and generated these files in the class hpc_class directory. I then moved these to a seperate directory in my home directory, initialized it with Git and committed and pushed these files to my repository after git remote adding it.  
