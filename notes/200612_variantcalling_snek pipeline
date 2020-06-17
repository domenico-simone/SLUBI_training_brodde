# Setting up and troubleshooting of .snek pipeline for variant calling

*continuation, see previous notes in "NotepadNotes_Deplexing and vcallig_grid.md"*


### learning session with Domenico, 04.06.20

- moving .snek script and config folder into "SLUBI_training_brodde", which is a remote repository on github, for collaboration with Domenico

- creating conda environment that will be globally used by script, see README.md:

(*README.md, 12.06.2020*)
##### SLUBI training

``This repo hosts materials used for online training and it's awesome.

###### Install conda environment

```bash
conda env create -n pinus -f config/env.yaml
```

OBS: you need to activate the conda environment before running the snakemake workflow!

*# this is done in job submission file*
```bash
conda activate pinus
```


- running .snek script, Errow --> see issue *variant calling_conda create env #1* on github
-- solved: deleting conda enviroment defition in new rule "cleanBam2"



##### checking results from run after solved issue *variant calling_conda create env #1*

output location:
```bash
/nfs4/my-gridfront/mykopat-proj3/mykopat-pineg/Hiplex_data_200508/processed_clipping_200519
```

```bash
for i in $(ls *.bam | grep -v call); do echo $i $(samtools flagstat $i |grep mapped | grep "N/A"); done
#Output
1.bam 17842 + 0 mapped (93.52% : N/A)
10.bam 125169 + 0 mapped (98.64% : N/A)
100.bam 103147 + 0 mapped (97.87% : N/A)
101.bam 99022 + 0 mapped (93.08% : N/A)
104.bam 16665 + 0 mapped (99.09% : N/A)
105.bam 54 + 0 mapped (98.18% : N/A)
106.bam 34 + 0 mapped (94.44% : N/A)
107.bam 75198 + 0 mapped (99.68% : N/A)
108.bam 16138 + 0 mapped (97.55% : N/A)
109.bam 27661 + 0 mapped (98.45% : N/A)
11.bam 2 + 0 mapped (100.00% : N/A)
112.bam 0 + 0 mapped (N/A : N/A)
12.bam 61658 + 0 mapped (98.21% : N/A)
13.bam 16923 + 0 mapped (97.46% : N/A)
14.bam 38593 + 0 mapped (97.01% : N/A)
15.bam 19973 + 0 mapped (99.40% : N/A)
16.bam 16 + 0 mapped (100.00% : N/A)
17.bam 89 + 0 mapped (100.00% : N/A)
18.bam 16608 + 0 mapped (97.64% : N/A)
20.bam 0 + 0 mapped (N/A : N/A)
21.bam 0 + 0 mapped (N/A : N/A)
22.bam 0 + 0 mapped (N/A : N/A)
23.bam 9033 + 0 mapped (99.49% : N/A)
24.bam 18895 + 0 mapped (97.50% : N/A)
25.bam 12053 + 0 mapped (98.42% : N/A)
26.bam 2229 + 0 mapped (99.73% : N/A)
28.bam 1649 + 0 mapped (96.72% : N/A)
29.bam 21841 + 0 mapped (99.29% : N/A)
30.bam 30628 + 0 mapped (97.78% : N/A)
31.bam 32285 + 0 mapped (97.41% : N/A)
32.bam 15 + 0 mapped (100.00% : N/A)
33.bam 0 + 0 mapped (N/A : N/A)
34.bam 963 + 0 mapped (99.59% : N/A)
35.bam 0 + 0 mapped (N/A : N/A)
36.bam 70298 + 0 mapped (97.70% : N/A)
37.bam 76 + 0 mapped (98.70% : N/A)
38.bam 20265 + 0 mapped (97.05% : N/A)
39.bam 47462 + 0 mapped (98.85% : N/A)
4.bam 223950 + 0 mapped (95.65% : N/A)
40.bam 83629 + 0 mapped (98.03% : N/A)
41.bam 13965 + 0 mapped (96.83% : N/A)
42.bam 5571 + 0 mapped (99.54% : N/A)
44.bam 11715 + 0 mapped (99.53% : N/A)
45.bam 7867 + 0 mapped (99.16% : N/A)
46.bam 34715 + 0 mapped (97.42% : N/A)
47.bam 1 + 0 mapped (100.00% : N/A)
48.bam 972 + 0 mapped (99.39% : N/A)
49.bam 7560 + 0 mapped (99.06% : N/A)
5.bam 58847 + 0 mapped (99.30% : N/A)
50.bam 49535 + 0 mapped (97.44% : N/A)
51.bam 34028 + 0 mapped (99.51% : N/A)
52.bam 44099 + 0 mapped (97.61% : N/A)
53.bam 12596 + 0 mapped (99.49% : N/A)
54.bam 12074 + 0 mapped (99.70% : N/A)
55.bam 6 + 0 mapped (100.00% : N/A)
56.bam 181295 + 0 mapped (98.52% : N/A)
57.bam 85222 + 0 mapped (99.73% : N/A)
58.bam 126752 + 0 mapped (97.37% : N/A)
6.bam 33921 + 0 mapped (99.41% : N/A)
60.bam 119998 + 0 mapped (98.12% : N/A)
61.bam 61408 + 0 mapped (96.47% : N/A)
63.bam 3650 + 0 mapped (99.62% : N/A)
65.bam 54722 + 0 mapped (99.60% : N/A)
66.bam 30381 + 0 mapped (96.10% : N/A)
67.bam 59586 + 0 mapped (99.78% : N/A)
68.bam 0 + 0 mapped (N/A : N/A)
69.bam 32059 + 0 mapped (99.82% : N/A)
7.bam 111398 + 0 mapped (96.65% : N/A)
70.bam 76939 + 0 mapped (95.88% : N/A)
71.bam 40763 + 0 mapped (99.83% : N/A)
72.bam 0 + 0 mapped (N/A : N/A)
73.bam 213 + 0 mapped (100.00% : N/A)
74.bam 13 + 0 mapped (100.00% : N/A)
75.bam 2 + 0 mapped (100.00% : N/A)
76.bam 69416 + 0 mapped (98.84% : N/A)
77.bam 22908 + 0 mapped (99.58% : N/A)
78.bam 70068 + 0 mapped (98.33% : N/A)
79.bam 81505 + 0 mapped (97.47% : N/A)
8.bam 84311 + 0 mapped (99.65% : N/A)
80.bam 28925 + 0 mapped (99.83% : N/A)
81.bam 13562 + 0 mapped (99.65% : N/A)
82.bam 50063 + 0 mapped (97.68% : N/A)
83.bam 69096 + 0 mapped (97.10% : N/A)
84.bam 34 + 0 mapped (97.14% : N/A)
85.bam 43884 + 0 mapped (97.21% : N/A)
86.bam 79755 + 0 mapped (98.82% : N/A)
87.bam 100891 + 0 mapped (98.19% : N/A)
88.bam 8994 + 0 mapped (99.56% : N/A)
89.bam 10682 + 0 mapped (99.65% : N/A)
9.bam 218430 + 0 mapped (97.38% : N/A)
90.bam 0 + 0 mapped (N/A : N/A)
91.bam 113151 + 0 mapped (98.99% : N/A)
92.bam 1005 + 0 mapped (97.20% : N/A)
93.bam 0 + 0 mapped (N/A : N/A)
94.bam 47023 + 0 mapped (98.10% : N/A)
95.bam 133237 + 0 mapped (98.22% : N/A)
96.bam 85010 + 0 mapped (97.99% : N/A)
97.bam 45349 + 0 mapped (99.38% : N/A)
98.bam 26395 + 0 mapped (96.79% : N/A)
99.bam 116799 + 0 mapped (95.81% : N/A)
```



```bash
zgrep -v "##" all_merged.vcf | less -S

#output
#CHROM  POS     ID      REF     ALT     QUAL    FILTER  INFO    FORMAT  /proj/mykopat-pineg/Hiplex_data_200508/processed_clipping_200519/20_call3.bam   /proj/mykopat-pineg/Hiplex_data_200508/processed_clipping_200519/41_call3.bam   /proj/mykopat-pineg/Hiplex_data_200508/processed_clipping_200519/22_call3.bam   /proj
gi|28513700|emb|BX251122.1|     142     .       C       .       96      .       MQ0F=0;MQ=60;DP=15;DP4=15,0,0,0;AN=20   GT      ./.     0/0     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.
gi|28513700|emb|BX251122.1|     143     .       A       .       101     .       MQ0F=0;MQ=60;DP=15;DP4=15,0,0,0;AN=20   GT      ./.     0/0     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.
gi|28513700|emb|BX251122.1|     144     .       T       .       101     .       MQ0F=0;MQ=60;DP=15;DP4=15,0,0,0;AN=20   GT      ./.     0/0     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.
gi|28513700|emb|BX251122.1|     145     .       T       .       101     .       MQ0F=0;MQ=60;DP=15;DP4=15,0,0,0;AN=20   GT      ./.     0/0     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.
gi|28513700|emb|BX251122.1|     146     .       G       .       101     .       MQ0F=0;MQ=60;DP=15;DP4=15,0,0,0;AN=20   GT      ./.     0/0     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.
gi|28513700|emb|BX251122.1|     147     .       G       .       101     .       MQ0F=0;MQ=60;DP=15;DP4=15,0,0,0;AN=20   GT      ./.     0/0     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.
gi|28513700|emb|BX251122.1|     148     .       A       .       101     .       MQ0F=0;MQ=60;DP=15;DP4=15,0,0,0;AN=20   GT      ./.     0/0     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.
gi|28513700|emb|BX251122.1|     149     .       A       .       101     .       MQ0F=0;MQ=60;DP=15;DP4=15,0,0,0;AN=20   GT      ./.     0/0     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.
gi|28513700|emb|BX251122.1|     150     .       G       .       101     .       MQ0F=0;MQ=60;DP=15;DP4=15,0,0,0;AN=20   GT      ./.     0/0     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.
gi|28513700|emb|BX251122.1|     151     .       A       .       101     .       MQ0F=0;MQ=60;DP=15;DP4=15,0,0,0;AN=20   GT      ./.     0/0     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.
gi|28513700|emb|BX251122.1|     152     .       C       G       41.4148 .       SGB=-0.379885;MQ0F=0;MQ=60;VDB=0.02;DP=15;DP4=0,0,15,0;AN=20;AC=20      GT:PL   ./.:.   1/1:39,3,0      ./.:.   ./.:.   ./.:.   ./.:.   ./.:.   ./.:.   ./.:.   ./.:.   ./.:.   ./.:.   ./.:.   ./.:.   ./.:.   1/1:38,3,0      ./.:.   ./.:.
gi|28513700|emb|BX251122.1|     153     .       G       .       101     .       MQ0F=0;MQ=60;DP=15;DP4=15,0,0,0;AN=20   GT      ./.     0/0     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.     ./.     ./.     ./.     0/0     ./.     ./.
```
--> again a lot of "empty variation"?
