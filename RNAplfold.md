## AIM: 
### Predict probability of local secondary structure formation of an input RNA sequence, in order to design guides for Cas targeting that avoids regions with high liklihood of secondary structures. 
  
## Workflow:
  - Upload/save your input sequence (ie *Malat1_isoform1.fa*) in fasta file format, which have *.fa* or *.fasta* extension.
  - Submit the by calling RNAplfold and specifying parameters for -u and -W ((Default window span size is       70)
  - Preview matrix file in the terminal
  - Save matrix file in .txt format
  - Open .txt file in excel:
    - Rename current tab as: *raw data*
    - Open and rename new tab: *heatmap*
    - In *heatmap* tab, copy all raw data in column format. **Confirm that the 3' and 5' direction aligns with the true direction of your input sequence in Snapgene.** Since the program scans the input sequence in default of **3' -> 5'** direction using a *Window* size *X* (that you specified), I had to invert the nucleotide (nt) order to correctly match up the nt positions of the input sequence in Snapgene as in matrix file output. 
    - Create a 2-color heatmap of your matrix file output of predicted probabilities of 2ndary structure using Conditional Formatting in excel. 
    - Design your guides to tile complimentary to heatmap regions with contiguous stretches of bases with low probability of secondary structure formation. 
      - Snapgene can make this step really convenient
      - Use *Add Feature* and color-coding to demarcate where you want to tile your guides. 
      - Record your chosen guides in Snapgene is (I find) easiest for designing Ultramer primers (for U6-PCR guide synthesis or cloning synthesis methods) later on. Save Snapgene file with chosen guides separately from your original input file/sequence.  

## Detailed Pipeline:
- An online tutorial (https://www.tbi.univie.ac.at/RNA/RNAplfold.1.html) states that RNAplfold 
   >*calculates the average pair probabilities for locally stable secondary structures.* and that the output format is
   >*The output is a plain text matrix containing on each line a position i followed by the probability that i is **unpaired**, [i-1..i] is **unpaired** [i-2..i] is **unpaired** and so on to the probability that [i-x+1..i] is unpaired.* 
- We want to design our targeting guides to contiguous stretches with HIGH probabilities 
   
### Submit with these flags: 

```
RNAplfold -u 22 < yoursequence.fa
```

##### Output is *_lunp* file, which is your matrix file. Preview matrix in terminal. You should see same # of columns as your specified *-u* flag, and same # of rows as your sequence length + 1 (1 extra for header). This step may seem trivial, but is important to weed out any gross errors. 

``` 
cat yourseq_lunp|less
```
##### You should see something like:
```
#i$ l=1 2   3   4   5   6   7   8   9   10  11
  1 0.9469881   NA  NA  NA  NA  NA  NA  NA  NA  NA  NA
  2 0.7428546   0.7108948   NA  NA  NA  NA  NA  NA  NA  NA  NA
  3 0.7833689   0.6985533   0.6691537   NA  NA  NA  NA  NA  NA  NA  NA
  4 0.8540909   0.7242232   0.655665    0.6532912   NA  NA  NA  NA  NA  NA  NA
  5 0.8320361   0.7807689   0.686046    0.6210906   0.6206707   NA  NA  NA  NA  NA  NA
  6 0.9145157   0.7697201   0.7289685   0.6372758   0.5771261   0.5777204   NA  NA  NA  NA  NA
  7 0.9481671   0.9134377   0.7694369   0.7286978   0.6370939   0.5769615   0.5775573   NA  NA  NA  NA
  8 0.9930493   0.9427189   0.9119256   0.7681018   0.7271193   0.6369164   0.5768006   0.577397    NA  NA  NA
  9 0.996136    0.9919387   0.9416257   0.9109458   0.7673214   0.7262351   0.6367918   0.5766883   0.5772853   NA  NA
 10 0.5856886   0.5843086   0.5835655   0.5382947   0.5172792   0.4713382   0.4463127   0.4205373   0.3834285   0.3872539   NA
 ```
 ##### The first 10 rows should not be *l* = length of *-u* that yo specified. For most low *l* (ie: under 3), you'll may see long stretches of low secondary structure probability, but as *l* increases, it's common to see 0.8 - 0.99 probabilities. 
 
 ##### Exclude first 10 rows from guide design. *NA's* in the first 10 rows are not useful because the program cannot calculate pairing of secondary structure over such a short span of nt's. They will be at the 3' tail end anyways.  
 
Counting rows
```
wc -l <filename>
```
Counting columns (this is untested!)
```
awk -F\t 'BEGIN{print NF}' yourseq_lunp
```
Save as *_lunp* file as *.txt* file

```
cp yourseq_lunp youseq_lunp.txt
```
Open youseq_lunp.txt in Excel. Continue with heatmap and guide design using combination of **Snapgene** and **Excel**. 

### Alternate Scenarios and Circular RNA Targeting 
If you want to test different lengths of guides, then submit a NEW RNAplfold -u *<spacer_length>* -W *<#>* for each length. I don't know if a batch processing method. If you figure it out, please add to this markdown!! 

For circular RNA targeting with Cas/CRISPR system, it was suggested to copy and paste identical segments of the full-length sequence 5' and 3' ends of your sequence. Save your new file with the *.fa* extension. Then perform same pipeline as above. The rationale is that circular RNA's don't have a beginning and end, so in order to "trick" the software to seeing the sequence as seamless, we had to "bury" the original sequence in the middle. Same pipeline can be applied, with the exception of only using final matrix rows that correlate to the middle "buried" sequence for heatmap synthesis. 







