# MODplus
## Open search for post-translational modifications  
Post-translational modifications regulate various cellular processes and are of great biological interest. Unrestrictive searches of mass spectrometry data enable the detection of any type of modification. 
MODplus makes practical open searches possible by allowing (1) hundreds of modifications, (2) multiple modifications per peptide, (3) the whole proteome database, and (4) any tolerant values in search parameters.
MODplus supports identifying different modification types at multiple sites and reports real chemical and biological modifications, as it has been very labor intensive to link unrestrictive search results to real modifications.
<hr>


## 1. Usage
<pre>
1) Requirement : deMix requires Java 1.7 or later. 
	        Visit http://www.oracle.com/technetwork/java/index.html

2) Command 
java -Xmx3000M -jar modplus.jar [-options]
If the program requires more memory, you should adjust -Xmx option, 
	e.g., java -Xmx8000M -jar modplus.jar <options> <file>
 
3) Options
-i [input_filename]      : Config file path for search parameters [Required]
-o [output_filename]     : Output file path for search results [Optional]. 
		         : If not specified, it's automatically generated (specfileName.modplus.txt).
-odir [output_directory] : Output directory path for search results [Optional]. 
		         : In the directory, specfileName.modplus.txt files are generated.

4) Example
Example1 : java -Xmx3000M -jar modplus.jar -i foo.txt -o out.txt
Example2 : java -Xmx3000M -jar modplus.jar -i E:\data -odir E:\results
         : all spectra files in E:\data are search and the result files are written in E:\results.

	
</pre>
## 2. Parameters (See modp.params file)
<pre>
- Spectra=[FILENAME|DIRECTORY]
  Specifies path to the spectra file or directory to search
  Supported formats: *.mgf, *.pkl, *.dta, *.ms2, *mzXML, *mzML 
  Given a directory, all spectra files in it are searched. 

- Fragmentation=[1|2]
  Specifies the type of MS/MS used (or best matched) for peptide fragmentation.
  1: CID, 2: HCD (Default value is 2)
  According to the instrument type, different fragmentation models are be applied.

- Fasta=[FILENAME]
  Specifies path to the database file (*.fasta) to search

- MakeDecoy= [0|1] 
  Sets the value to 1 to append decoy sequences to the original input fasta file. (Default value is 0)
  Decoy protein prefix will be XXX_. 
  A new fasta file will generated and its name ends with '_wplusDecoy.fasta'. MODplus wiil automatically run with the new fasta file.
  Don't use if the input fasta from a user already includes decoy sequences.

- PeptTolerance=[VALUE][ppm|da]
  Sets a precursor mass tolerance in ppm or dalton. (Default value is 10ppm)
  e.g., PeptTolerance=15ppm or 0.5da

- C13Isotope=[MIN],[MAX]
  Sets the numbers of C13 (or isotope error) allowed in peptide masses. (Default values are -1,2)
  1.00235 Da is used as an isotope spacing

- FragTolerance=[VALUE][ppm|da]
  Sets a fragment ion mass tolerance in ppm or dalton. (Default value is 20ppm)
  e.g., FragTolerance=20ppm or 0.02da

- Enzyme=[NAME],[Cleavage/Teminus(N|C)]*
  Specifies the enzyme rule for protein digestion. (Default rule conforms to Trypsin)
  To ignore the enzyme specificity (for no-enzyme search), please use "*/*" rule (ex. 'Enzyme=NONE, */*').  
  e.g., Enzyme=Trypsin, KR/C

- NTT=[0|1|2|8|9]
  Sets the number of allowed enzymatic termini. (Default value is 1)
  0: any, 1: semi-digested, 2: fully digested, 8: N-term specific, 9: C-term specific.
  e.g., For trypsin, 0: non-tryptic, 1: semi-tryptic, 2: fully-tryptic.

# Specify modification-related parameters ################################################################################################################
- NumMods=[1|2]
  Sets the number of modifications per peptide. (Default value is 2)
  1 allows for one modification per peptide,
  2 allows for arbitrary number of modifications per peptide.

- ModMassSize=[MIN],[MAX]
  Sets the minimum/maximum modification size in dalton to consider. (Default values are -150,+350) 

- ModAnnotation=[1|2|3|4|5]
  Sets the modification list to annotate unknown modification masses. (Default value is 2)
  1 allows for only MS-common modifications (50 modifications including oxidation, deamidation, phospho, and so forth. For more details, see README file),
  2 allows for all modifications from Unimod except for amino-acid substitutions (www.unimod.org: 2,407 modifications in Jan.2023),
  3 allows for only amino-acid substitutions,
  4 allows for 1 plus 3 (i.e., MS-common modifications + AA substitutions),
  5 allows for 2 plus 3 (i.e., all Unimod modifications including AA substitutions),
  If unknown modification masses could not be annotated using the specified list, they will be presented in PSMs as UNKNOWN. 

- ModEnrichment=[0|1|2]
  Sets an enriched modification. Default is 0 (NO enrichment).
  1: Phospho (i.e., use for phospho-enrichment data analysis), 2: Acetyl.

- Mod=[RESIDUE(*|AMINO ACID)],[POSITION(ProtNterm|ProtCterm|Nterm|Cterm|any)],[MASS(in dalton)],[NAME].
  Use * to consider any residue in RESIDUE part. RESIDUE should be a single character.
  Specifies additional user-defined modifications if users know about unique or novel modifications.
  Or if users don't want to take into account all possible AA substitutions (by setting ModAnnotation=3), 
  set ModAnnotation=1 and add only a few AA substitutions by utilizing this parameter.
  e.g., Mod=M, any, 15.994915, Oxidation
  e.g., Mod=A, any, 28.006148, Lys->Arg
  e.g., Mod=Q, N-term, -17.026549, PyroGluQ
  e.g., Mod=*, Prot-N-term, 42.010565, Acetyl

- ADD=[RESIDUE],[MASS]
  Specifies the mass of a fixed (static) modification on a residue (e.g. cysteine alkylation and TMT-labeling)
  e.g., ADD=C, 57.021464
  e.g., ADD=N-Term, 229.162932
  e.g., ADD=K, 229.162932
</pre>

## 3. Citation
<pre>
- MODplus: robust and unrestrictive identification of post-translational modifications using mass spectrometry
  S Na, J Kim, E Paek
  Analytical chemistry <b>2019</b>, 91 (17), 11324-11333
</pre>
## 4. Rights and Permissions
<pre>
- MODplus © 2024 is licensed under Creative Commons Attribution-ShareAlike 4.0 International.
  This license requires that reusers give credit to the creator. It allows reusers to distribute, 
  remix, adapt, and build upon the material in any medium or format, even for commercial purposes. 
  If others remix, adapt, or build upon the material, they must license the modified material under identical terms.
  To view a copy of this license, visit https://creativecommons.org/licenses/by-sa/4.0/
</pre>

