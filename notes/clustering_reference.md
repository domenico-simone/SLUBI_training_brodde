
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
