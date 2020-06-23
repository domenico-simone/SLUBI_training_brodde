
# Clustering the reference for variant calling (Hiplex pine proj)

**01.06.2020**
*continuations*


- last session wit Domenico: extracting scaffolds with mapped reads from
-   run that was used: *processed_200415* (pinus_genus reference) [bash], then creating new file (all.scaffolds) with
-     scaffold sequences that mapped, extracted from original reference file
-       (genus_pinus) [phyton]

```bash

for bamfile in $(ls *.bam)
do
    # do something
    echo $bamfile
    # samtools view $bamfile | awk '{print $3}' | sort | uniq > $bamfile.scaffolds
    samtools view -F 0x04 $bamfile | awk '{print $3}' | sort | uniq > ${bamfile/.bam/.scaffolds}
done
```

```python
# python
from Bio import SeqIO
sequence_names = open('processed_200415/bamfiles/all.scaffolds', 'r')
sequence_names_list = sequence_names.readlines()
sequence_names_list = [i.strip() for i in sequence_names_list]

ref = SeqIO.parse('genus_pinus.fasta', 'fasta')
ref2 = open('genus_pinus_2.fasta', 'w')
for s in ref:
    if s.id in sequence_names_list:
        ref2.write('>{}\n{}\n'.format(s.id, str(s.seq)))

ref2.close()

```



**01.06.2020**

Clustering reference "all.scaffolds" to create a non-redundand reference:

with:  **cd-hit-est**
see for options: *https://github.com/weizhongli/cdhit/wiki/3.-User%27s-Guide#CDHITEST*
see userguide: *http://weizhongli-lab.org/lab-wiki/doku.php?id=cd-hit-user-guide*

```bash
cd-hit-est -i est_human -o est_human95 -c 0.95 -n 10 -d 0 -M 16000 -T 8
```


Choose of word size:

  -n 10, 11 for thresholds 0.95 ~ 1.0
  -n 8,9    for thresholds 0.90 ~ 0.95
  -n 7      for thresholds 0.88 ~ 0.9
  -n 6      for thresholds 0.85 ~ 0.88
  -n 5      for thresholds 0.80 ~ 0.85
  -n 4      for thresholds 0.75 ~ 0.8

!! userguide: *Cd-hit-est cannot cluster very long sequences either (e.g. genome sized
sequences). In such cases, please use PSI-CD-HIT, which will be introduced in
following sections.*


First test:

```bash
cd-hit-est -i genus_pinus_2.fasta -o genus_pinus_mapped_clust -c 0.95 -n 10 -d 0 -M 16000 -T 0

#Program: CD-HIT, V4.8.1 (+OpenMP), Oct 26 2019, 14:51:47
#Command:
cd-hit-est -i genus_pinus_2.fasta -o genus_pinus_mapped_clust -c 0.95 -n 10 -d 0 -M 16000 -T 0

#Started: Thu Jun  4 12:04:37 2020
#Output:
#total number of CPUs in the system is 2
#Actual number of CPUs to be used: 2

#total seq: 269

#Warning:
#Some seqs are too long, please rebuild the program with make parameter MAX_SEQ=new-maximum-"length (e.g. make MAX_SEQ=10000000)
#Not fatal, but may affect results !!

#longest and shortest : 1191054 and 171
#Total letters: 1851814
#Sequences have been sorted

#Approximated minimal memory consumption:
#Sequence        : 1M
#Buffer          : 2 X 265M = 531M
#Table           : 2 X 16M = 33M
#Miscellaneous   : 4M
#Total           : 571M

#Table limit with the given memory limit:
#Max number of representatives: 40000
#Max number of word counting entries: 1928608752

# comparing sequences from          0  to        269

#Fatal Error:
#in diag_test_aapn_est, MAX_DIAG reached
#Program halted !!
```
**--> conclusion: psi-cd-hit-DNA is probably better to use**

##### 04.06.20 learning session with Domenico

psi-cd-hit: clustering of very long Sequences

userguide: http://gensoft.pasteur.fr/docs/cd-hit/4.6.6/cdhit-user-guide.pdf

- installing blastn
- addding to PATH
- installing psi-cd-hit.pl


##### 12.06.2020

- trying out example from userguide:
```bash
psi-cd-hit.pl -i genus_pinus_2.fasta -o pinus_clust -c 0.9 -G 1 -g 1 -prog megablast -s"-F F -e 0.000001 -b 100000 -v 100000" -exec local -core 32
```
- first, error message:

[...] Name "main::bl_dir" used only once: possible typo at /nfs4/my-gridfront/mykopat-proj3/mykopat-pineg/cd-hit-v4.8.1-2019-0228/psi-cd-hit/psi-cd-hit.pl line 103.
[...]

- running same command again, error messages disappears --> https://perlmaven.com/name-used-only-once-possible-typo
- not sure if it's running

##### 17.06.2020

- I need to add psi-cd-hit to PATH every time before I can access psi-cd-hit as a tool

```bash
PATH=$PATH:/nfs4/my-gridfront/mykopat-proj3/mykopat-pineg/cd-hit-v4.8.1-2019-0228/psi-cd-hit
```
- running same command as on 12.06. in submission script on cluster

```bash
#!/bin/bash

#$ -cwd
#$ -V
#$ -l h_rt=7:0:0
#$ -l h_vmem=50G

#source $HOME/.bashrc

psi-cd-hit.pl -i genus_pinus_2.fasta -o pinus_clust -c 0.9 -G 1 -g 1 -prog megablast -s"-F F -e 0.000001 -b 100000 -v 100000" -exec local -core 32
```
--> empty output and error file, job stops running after ~2sec
**--> It worked!!!**

##### 18.06.2020
--> use -prog blastn? (megablast not installed!)

```bash
psi-cd-hit.pl -i genus_pinus_2.fasta -o pinus_clust -c 0.9 -G 1 -g 1 -prog blastn -s"-F F -e 0.000001 -b 100000 -v 100000" -exec local -core 32
```

**checking output from 17.06.**
input: 1860207 May 25 15:34  genus_pinus_2.fasta
output: 15378 Jun 17 16:04  pinus_clust.clstr


outputfiles:
pinus_clust
pinus_clust.clstr
 pinus_clust.log
 pinus_clust.out
pinus_clust.restart

How can I do some quality control on the .fasta file?

```bash
cat pinus_clust.clstr | head
#output:
>Cluster 0
0       1191054aa, >gi|1511245057|ref|NC_039746.1|... *
>Cluster 1
0       129758aa, >gi|297747440|gb|AC241324.1|... *
>Cluster 2
0       113688aa, >gi|302024684|gb|AC241353.2|... *
>Cluster 3
0       57157aa, >gi|725798652|gb|KP172196.1|... *
>Cluster 4
0       8296aa, >gi|572218767|gb|GAQR01038049.1|... *
```

##### 18.06.2020

*session with Domenico*

- checking how many sequences are present in original (input) file:
```bash
grep -c ">" genus_pinus_2.fasta
269
```

- checking how many clusters
```bash
grep -c ">Cluster" pinus_clust.clstr
269
```


- problems with clusters of first run --> as many clusters as Sequences
- setting up new clustering, altered settings in command, we had much trouble to get it running
  - new installation of biopyhton *etc pp*
  - in the end it succeeded with job submission

**job submission**
```bash
#!/bin/bash

#$ -cwd
#$ -V
#$ -l h_rt=7:0:0
#$ -l h_vmem=50G

#source $HOME/.bashrc
export PATH=/nfs4/my-gridfront/mykopat-proj3/mykopat-pineg/cdhit-master/psi-cd-hit:$PATH
psi-cd-hit.pl -i genus_pinus_2.fasta -o pinus_clust_TEST_2 -c 0.9 -G 1 -g 1 -prog blastn -exec local -blp 10

## note: -blp 10 --> -cluster 32 option in guide was not existent in this script version

```

- checking output file *pinus_clust_TEST_2.clstr*
```bash
>Cluster 0
0       1191054aa, >gi|1511245057|ref|NC_039746.1|... *
>Cluster 1
0       129758aa, >gi|297747440|gb|AC241324.1|... *
>Cluster 2
0       113688aa, >gi|302024684|gb|AC241353.2|... *
>Cluster 3
0       57157aa, >gi|725798652|gb|KP172196.1|... *
1       795aa, >gi|292544593|gb|GW760802.1|... at 0.0/791aa/96.72%
2       761aa, >gi|292525974|gb|GW748131.1|... at 0.0/759aa/90.67%
3       244aa, >gi|339403872|emb|FN941260.1|... at 9e-100/244aa/93.44%
>Cluster 4
0       8296aa, >gi|572218767|gb|GAQR01038049.1|... *
1       7380aa, >gi|572218765|gb|GAQR01038051.1|... at 0.0/7319aa/99.17%
2       7220aa, >gi|572218766|gb|GAQR01038050.1|... at 0.0/7036aa/97.45%
>Cluster 5
0       8196aa, >gi|572218769|gb|GAQR01038047.1|... *
1       7280aa, >gi|572218771|gb|GAQR01038045.1|... at 0.0/7219aa/99.16%
2       7120aa, >gi|572218770|gb|GAQR01038046.1|... at 0.0/6936aa/97.41%
>Cluster 6
0       8136aa, >gi|572218764|gb|GAQR01038052.1|... *
>Cluster 7
0       8036aa, >gi|572218768|gb|GAQR01038048.1|... *
>Cluster 8
0       7212aa, >gi|572224302|gb|GAQR01033122.1|... *
1       7017aa, >gi|572224300|gb|GAQR01033124.1|... at 0.0/4546:2472aa/100.01%
2       800aa, >gi|1769155215|dbj|HX995365.1|... at 0.0/800aa/99.37%
3       535aa, >gi|38012458|emb|BX678520.1|... at 0.0/531aa/90.84%
>Cluster 9
0       6048aa, >gi|572234274|gb|GAQR01023150.1|... *
>Cluster 10
0       4926aa, >gi|572224883|gb|GAQR01032541.1|... *
1       811aa, >gi|49445315|gb|CO363998.1|... at 0.0/810aa/97.78%
2       607aa, >gi|38014923|emb|BX680467.1|... at 0.0/607aa/98.18%
>Cluster 11
0       4024aa, >gi|572240288|gb|GAQR01017136.1|... *
>Cluster 12
0       3933aa, >gi|572226739|gb|GAQR01030685.1|... *
```

- checking how many clusters by grep'ing ">" / ">Cluster" which is starter of each new cluster:

```bash
grep -c ">" pinus_clust_TEST_2 #file with sequences
112

grep -c ">Cluster" pinus_clust_TEST_2.clstr # file with cluster output
112
```
--> less clusters than original input file! :)

--> take this file [pinus_clust_TEST_2.clstr] as new reference for variant calling!!
--> try out different thresholds of identity ( -c option in psi-cd-hit.pl) for clustering

- renaming cluster file into .fasta file

```bash
cp pinus_clust_TEST_2 pinus_clust_ref.fasta
```
