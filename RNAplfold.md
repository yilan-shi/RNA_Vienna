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

##### If RNAplfold "freezes" and takes too long, press 
```
@
```
##### and resubmit by appending job to background:
```
nohup RNAplfold -u 22 < yousequence.fa &

``` 
When you get a *_lunp* file, view it with 
```
cat yourseq_lunp|less
```
##### You should see something like:
```
#unpaired probabilities
 #i$    l=1     2       3       4       5       6       7       8       9       10      11      12      13      14      15      16      17      18      19      20      21      22      
1       0.9955786       NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA
2       0.400877        0.3596903       NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA
3       0.6198445       0.3583934       0.3223164       NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA
4       0.7293615       0.5967265       0.3456527       0.3104123       NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA
      NA
5       0.1887091       0.1495393       0.08214685      0.03598321      0.03874083      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA
      NA      NA
6       0.3607118       0.1851454       0.1465093       0.07957653      0.0342596       0.03725615      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA
      NA      NA      NA
7       0.1580956       0.1660783       0.1392442       0.1460875       0.07915852      0.03399026      0.0369834       NA      NA      NA      NA      NA      NA      NA      NA      NA      NA      NA
      NA      NA      NA      NA
8       0.09217745      0.04991289      0.05307836      0.05349025      0.06611021      0.05573458      0.01323808      0.01383373      NA      NA      NA      NA      NA      NA      NA      NA      NA
      NA      NA      NA      NA      NA
9       0.02865193      0.01914368      0.009915941     0.009410009     0.006936158     0.008364774     0.008871078     0.008587369     0.008575593     NA      NA      NA      NA      NA      NA      NA
      NA      NA      NA      NA      NA      NA
10      0.1214044       0.01989822      0.01214029      0.007366835     0.006882163     0.006349236     0.007785257     0.00832155      0.008190782     0.008350021     NA      NA      NA      NA      NA
      NA      NA      NA      NA      NA      NA      NA
11      0.9769534       0.1192293       0.01813042      0.01047252      0.006785199     0.006323771     0.006243568     0.007694724     0.008249398     0.008139791     0.008303794     NA      NA      NA
      NA      NA      NA      NA      NA      NA      NA      NA
12      0.009289779     0.00883976      0.003444976     0.002659111     0.001841304     0.000924893     0.0009473515    0.0006442341    0.0007682536    0.0008592422    0.0007296751    0.0007979251    NA
      NA      NA      NA      NA      NA      NA      NA      NA      NA
13      0.06182039      0.007199605     0.007015029     0.00284235      0.002529814     0.001750437     0.0008807138    0.0009032766    0.0006231421    0.0007436175    0.0008314334    0.0007033219    0.000
7748726    NA      NA      NA      NA      NA      NA      NA      NA      NA
14      0.278199        0.02819289      0.005178422     0.005144947     0.001777816     0.001696887     0.0009489348    0.0006074791    0.0006415639    0.0005670218    0.0006935496    0.0007897501    0.000
673749     0.0007475923    NA      NA      NA      NA      NA      NA      NA      NA
15      0.3221211       0.2727969       0.02711516      0.004984267     0.004944502     0.001694202     0.001647224     0.0009022887    0.000582334     0.0006157138    0.0005400667    0.0006603172    0.000
7511584    0.0006451304    0.0007398184    NA      NA      NA      NA      NA      NA      NA
16      0.7536555       0.08717862      0.04633733      0.01963026      0.002969474     0.002903269     0.001349904     0.001337751     0.0006361744    0.0004406432    0.0004671812    0.000369118     0.000
4484121    0.0005032952    0.0004058081    0.0004627196    NA      NA      NA      NA      NA      NA
17      0.2217248       0.1711174       0.06911637      0.03431756      0.01738626      0.002148824     0.002026901     0.0006543718    0.0005874491    0.0005617453    0.0004000407    0.000428307     0.000
3351278    0.0004074222    0.0004618205    0.0003731478    0.0004255935    NA      NA      NA      NA      NA
18      0.2597851       0.2056676       0.1602601       0.06403103      0.03132812      0.01657922      0.001798949     0.001653348     0.0006121485    0.0005480129    0.0005218443    0.0003674474    0.000
3946596    0.0002961178    0.0003588795    0.0004022143    0.000308473     0.0003510708    NA      NA      NA      NA
19      0.7332148       0.178783        0.127998        0.08618706      0.05485984      0.02770497      0.01526118      0.001407356     0.001240423     0.0005190422    0.0004603528    0.0004454047    0.000
3094494    0.0003400032    0.0002351437    0.0002843443    0.0003278581    0.0002586059    0.0002945363    NA      NA      NA
20      0.7463489       0.5679091       0.1666759       0.1187518       0.08037739      0.05198419      0.02634474      0.01468856      0.001224629     0.00104574      0.0004671223    0.0004220289    0.000
404837     0.0002745994    0.0003012581    0.0001918901    0.0002305987    0.0002616505    0.0001868941    0.0002114653    NA      NA
21      0.5654894       0.4476958       0.2667045       0.1308217       0.09310949      0.06165858      0.04041431      0.02149126      0.01290281      0.001079838     0.0009090618    0.0003872117    0.000
359718     0.000344102     0.0002542488    0.0002818453    0.0001871537    0.0002261515    0.0002576534    0.0001836572    0.0002082378    NA
22      0.5308364       0.4763659       0.3570608       0.1749542       0.1151834       0.08421125      0.05521087      0.03626616      0.01975369   
```
 ##### For most low *l* (ie *under 3*), you'll see high unpaired probability, but as *l* increases, it's common to see near-zero unpaired probabilities. Each *l* denotes length of guide you specified with the *-u* flag. Columns across are *l*. Rows down are base pair positions. *CRITICAL* base pair positions are 3' -> 5' direction. 
 
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
If you want to test different lengths of guides, then submit a NEW RNAplfold -u *<spacer_length>* -W *<#>* for each length. I don't know of a batch processing method. If you figure it out, please add to this markdown!! 

For circular RNA targeting with Cas/CRISPR system, it was suggested to copy and paste identical segments of the full-length sequence 5' and 3' ends of your sequence. Save your new file with the *.fa* extension. Then perform same pipeline as above. The rationale is that circular RNA's don't have a beginning and end, so in order to "trick" the software to seeing the sequence as seamless, we had to "bury" the original sequence in the middle. Same pipeline can be applied, with the exception of only using final matrix rows that correlate to the middle "buried" sequence for heatmap synthesis. 







