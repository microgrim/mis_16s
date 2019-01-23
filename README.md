# 2018 analysis of MIS environmental and experimental samples

## 20180305

uploaded all files to server: /geomicro/data2/sgrim/MIS/MIS_2017

make ini files

```
for f in `ls *R1*.gz | awk 'BEGIN{FS="_"}{print $1"_"$2}'`
do
	{
	echo "[general]
	
	project_name = ${f}
	researcher_email = sgrim@umich.edu
	input_directory = /geomicro/data2/sgrim/MIS/MIS_2017
	output_directory = /geomicro/data2/sgrim/MIS/MIS_2017/merged/
	
	[files]
	pair_1 = ${f}_L001_R1_001.fastq.gz
	pair_2 = ${f}_L001_R2_001.fastq.gz
	
	[prefixes]" > $f.ini
	};
done


more SG0525_S109.ini
```
> [general]
> 
> project_name = SG0525_S109
> researcher_email = sgrim@umich.edu
> input_directory = /geomicro/data2/sgrim/MIS/MIS_2017
> output_directory = /geomicro/data2/sgrim/MIS/MIS_2017/merged/
> 
> [files]
> pair_1 = SG0525_S109_L001_R1_001.fastq.gz
> pair_2 = SG0525_S109_L001_R2_001.fastq.gz
> 
> [prefixes]

```
mkdir /geomicro/data2/sgrim/MIS/MIS_2017/merged/

for f in `ls *.ini`
do
{
iu-merge-pairs --min-qual-score 25 --min-overlap-size 200 --compute-qual-dicts /geomicro/data2/sgrim/MIS/MIS_2017/${f}
};
done

cd /geomicro/data2/sgrim/MIS/MIS_2017/merged/

for i in `ls *_MERGED`
do
	{
	iu-filter-merged-reads -m 5 ${i}
	}
done
```
interpret the negative extraction controls: 20171005blank
20171010blank
20171017blank


```
for f in `ls 2017*R1*.gz | awk 'BEGIN{FS="_"}{print $1"_"$2}'`
do
	{
	echo "[general]
	
	project_name = ${f}
	researcher_email = sgrim@umich.edu
	input_directory = /geomicro/data2/sgrim/MIS/MIS_2017
	output_directory = /geomicro/data2/sgrim/MIS/MIS_2017/merged/
	
	[files]
	pair_1 = ${f}_L001_R1_001.fastq.gz
	pair_2 = ${f}_L001_R2_001.fastq.gz
	
	[prefixes]" > $f.ini
	};
done


more 20171005blank*.ini
```
> [general]
> 
> project_name = 20171005blank_S311
> researcher_email = sgrim@umich.edu
> input_directory = /geomicro/data2/sgrim/MIS/MIS_2017
> output_directory = /geomicro/data2/sgrim/MIS/MIS_2017/merged/
> 
> [files]
> pair_1 = 20171005blank_S311_L001_R1_001.fastq.gz
> pair_2 = 20171005blank_S311_L001_R2_001.fastq.gz
> 
> [prefixes]

```
for f in `ls 2017*.ini`
do
{
iu-merge-pairs --min-qual-score 25 --min-overlap-size 200 --compute-qual-dicts /geomicro/data2/sgrim/MIS/MIS_2017/${f}
};
done


cat /geomicro/data2/sgrim/MIS/MIS_2017/merged/2017*_STATS 
```
Abbreviated results:
> Number of pairs analyzed ...............................	41
> Merged total ...........................................	7
> Mismatches breakdown in final merged reads:
> 
> 0	3
> 4	1
> 9	1
> 11	1
> 17	1
> 
> 
> Command line ...........................................	/usr/local/bin/iu-merge-pairs --min-qual-score 25 --min-overlap-size 200 --compute-qual-dicts /geomicro/data2/sgrim/MIS/MIS_2017/20171005blank_S311.ini


> Number of pairs analyzed ...............................	34
> Merged total ...........................................	8
> 
> Mismatches breakdown in final merged reads:
> 
> 0	2
> 1	2
> 2	1
> 5	1
> 55	1
> 57	1
> 
> 
> Command line ...........................................	/usr/local/bin/iu-merge-pairs --min-qual-score 25 --min-overlap-size 200 --compute-qual-dicts /geomicro/data2/sgrim/MIS/MIS_2017/20171010blank_S323.ini

> Number of pairs analyzed ...............................	36
> Merged total ...........................................	9
> 
> Mismatches breakdown in final merged reads:
> 
> 0	2
> 1	2
> 3	3
> 6	1
> 7	1
> 
> 
> Command line ...........................................	/usr/local/bin/iu-merge-pairs --min-qual-score 25 --min-overlap-size 200 --compute-qual-dicts /geomicro/data2/sgrim/MIS/MIS_2017/20171017blank_S335.ini


So these negative extraction controls contributed fewer than 9 reads per sample, of which <70% had 5 or fewer mismatches.
Making the executive decision to not include these.

Retain only those reads with 5 or fewer mismatches in the overlap:

```
for f in `ls SG*_MERGED-MAX-MISMATCH-5`
do
	cat $f >> ../MIS2017.fasta
done

cd /geomicro/data2/sgrim/MIS/MIS_2017/
o-get-sample-info-from-fasta /geomicro/data2/sgrim/MIS/MIS_2017/MIS2017.fasta
```
> Samples and read counts found in the FASTA file:
> SG0716_S289                    63,778
> SG0592_S141                    40,127
> SG0591_S129                    37,064
> SG0608_S143                    37,007
> SG0720_S337                    36,804
> SG0680_S236                    35,226
> SG0718_S313                    35,175
> SG0590_S117                    34,421
> SG0515_S83                     34,167
> SG0531_S181                    33,940
> SG0526_S121                    33,406
> SG0719_S325                    33,107
> SG0681_S248                    31,274
> SG0509_S11                     31,235
> SG0442_S74                     30,804
> SG0611_S179                    30,484
> SG0574_S127                    30,264
> SG0607_S131                    30,246
> SG0537_S158                    29,938
> SG0679_S224                    29,843
> SG0686_S213                    29,841
> SG0717_S301                    29,700
> SG0682_S260                    29,688
> SG0527_S133                    29,630
> SG0645_S208                    29,412
> SG0710_S204                    29,338
> SG0535_S134                    29,117
> SG0433_S61                     28,945
> SG0677_S200                    28,906
> SG0688_S237                    28,738
> SG0553_S160                    28,639
> SG0605_S107                    28,630
> SG0707_S263                    28,218
> SG0435_S85                     28,185
> SG0587_S176                    27,963
> SG0583_S128                    27,962
> SG0575_S139                    27,926
> SG0695_S226                    27,677
> SG0538_S170                    27,601
> SG0495_S33                     27,357
> SG0452_S87                     27,279
> SG0656_S245                    27,218
> SG0698_S262                    27,117
> SG0572_S103                    27,018
> SG0715_S264                    26,957
> SG0584_S140                    26,956
> SG0683_S272                    26,866
> SG0530_S169                    26,863
> SG0646_S220                    26,857
> SG0609_S155                    26,740
> SG0558_S125                    26,701
> SG0487_S32                     26,671
> SG0712_S228                    26,670
> SG0693_S202                    26,667
> SG0706_S251                    26,578
> SG0616_S144                    26,554
> SG0551_S136                    26,551
> SG0708_S275                    26,523
> SG0600_S142                    26,520
> SG0649_S256                    26,462
> SG0529_S157                    26,447
> SG0606_S119                    26,423
> SG0547_S183                    26,359
> SG0631_S230                    26,343
> SG0672_S235                    26,253
> SG0525_S109                    26,226
> SG0671_S223                    26,182
> SG0692_S285                    26,113
> SG0430_S25                     25,934
> SG0589_S105                    25,803
> SG0684_S284                    25,750
> SG0678_S212                    25,743
> SG0620_S205                    25,701
> SG0434_S73                     25,652
> SG0617_S156                    25,562
> SG0685_S201                    25,501
> SG0576_S151                    25,455
> SG0550_S124                    25,286
> SG0493_S9                      25,256
> SG0555_S184                    25,182
> SG0626_S277                    25,158
> SG0585_S152                    25,048
> SG0545_S159                    25,019
> SG0676_S283                    25,017
> SG0577_S163                    25,005
> SG0696_S238                    24,901
> SG0554_S172                    24,898
> SG0560_S149                    24,894
> SG0711_S216                    24,784
> SG0658_S269                    24,741
> SG0662_S222                    24,729
> SG0637_S207                    24,646
> SG0705_S239                    24,608
> SG0618_S168                    24,568
> SG0673_S247                    24,560
> SG0573_S115                    24,490
> SG0714_S252                    24,481
> SG0659_S281                    24,472
> SG0557_S113                    24,470
> SG0612_S191                    24,384
> SG0556_S101                    24,359
> SG0694_S214                    24,228
> SG0548_S100                    24,143
> SG0689_S249                    24,127
> SG0485_S8                      24,029
> SG0615_S132                    24,022
> SG0441_S62                     23,965
> SG0687_S225                    23,895
> SG0603_S178                    23,853
> SG0629_S206                    23,817
> SG0429_S13                     23,728
> SG0651_S280                    23,724
> SG0691_S273                    23,704
> SG0709_S287                    23,703
> SG0524_S97                     23,671
> SG0647_S232                    23,658
> SG0510_S23                     23,619
> SG0661_S210                    23,555
> SG0449_S63                     23,543
> SG0513_S59                     23,539
> SG0539_S182                    23,539
> SG0552_S148                    23,494
> SG0501_S10                     23,394
> SG0512_S47                     23,374
> SG0488_S44                     23,371
> SG0546_S171                    23,202
> SG0522_S60                     23,172
> SG0699_S274                    23,134
> SG0713_S240                    23,132
> SG0610_S167                    23,120
> SG0477_S7                      23,113
> SG0497_S57                     23,068
> SG0588_S188                    22,930
> SG0639_S231                    22,864
> SG0597_S106                    22,855
> SG0491_S80                     22,792
> SG0559_S137                    22,615
> SG0644_S196                    22,614
> SG0519_S36                     22,586
> SG0638_S219                    22,553
> SG0635_S278                    22,547
> SG0601_S154                    22,543
> SG0494_S21                     22,521
> SG0594_S165                    22,491
> SG0566_S126                    22,483
> SG0627_S194                    22,458
> SG0657_S257                    22,418
> SG0503_S34                     22,388
> SG0697_S250                    22,379
> SG0634_S266                    22,377
> SG0614_S120                    22,375
> SG0598_S118                    22,362
> SG0492_S92                     22,307
> SG0650_S268                    22,215
> SG0648_S244                    22,214
> SG0540_S99                     22,203
> SG0622_S229                    22,188
> SG0641_S255                    22,185
> SG0516_S95                     22,143
> SG0602_S166                    22,136
> SG0660_S198                    22,078
> SG0669_S199                    22,055
> SG0511_S35                     21,992
> SG0504_S46                     21,984
> SG0486_S20                     21,976
> SG0704_S227                    21,909
> SG0489_S56                     21,841
> SG0655_S233                    21,838
> SG0581_S104                    21,755
> SG0428_S1                      21,747
> SG0640_S243                    21,708
> SG0450_S75                     21,705
> SG0624_S253                    21,699
> SG0454_S16                     21,699
> SG0582_S116                    21,656
> SG0532_S98                     21,634
> SG0633_S254                    21,623
> SG0663_S234                    21,591
> SG0664_S246                    21,558
> SG0654_S221                    21,558
> SG0674_S259                    21,536
> SG0460_S88                     21,510
> SG0675_S271                    21,508
> SG0479_S31                     21,420
> SG0453_S4                      21,369
> SG0636_S195                    21,298
> SG0604_S190                    21,267
> SG0621_S217                    21,208
> SG0579_S187                    21,180
> SG0578_S175                    21,179
> SG0562_S173                    21,169
> SG0632_S242                    21,159
> SG0586_S164                    21,155
> SG0667_S270                    21,112
> SG0521_S48                     21,100
> SG0700_S286                    20,972
> SG0478_S19                     20,960
> SG0653_S209                    20,923
> SG0596_S189                    20,830
> SG0570_S174                    20,714
> SG0703_S215                    20,703
> SG0517_S12                     20,689
> SG0619_S193                    20,674
> SG0549_S112                    20,640
> SG0543_S135                    20,556
> SG0567_S138                    20,507
> SG0448_S51                     20,416
> SG0432_S49                     20,358
> SG0565_S114                    20,306
> SG0571_S186                    20,291
> SG0544_S147                    20,232
> SG0701_S203                    20,167
> SG0668_S282                    20,049
> SG0437_S14                     19,836
> SG0496_S45                     19,834
> SG0502_S22                     19,817
> SG0439_S38                     19,792
> SG0431_S37                     19,640
> SG0665_S258                    19,635
> SG0564_S102                    19,601
> SG0508_S94                     19,543
> SG0505_S58                     19,483
> SG0447_S39                     19,322
> SG0443_S86                     19,258
> SG0670_S211                    19,249
> SG0642_S267                    19,144
> SG0568_S150                    19,114
> SG0490_S68                     19,114
> SG0483_S79                     19,061
> SG0457_S52                     19,046
> SG0440_S50                     18,792
> SG0593_S153                    18,733
> SG0463_S29                     18,722
> SG0480_S43                     18,638
> SG0438_S26                     18,563
> SG0541_S111                    18,549
> SG0561_S161                    18,513
> SG0613_S108                    18,385
> SG0500_S93                     18,385
> SG0623_S241                    18,373
> SG0518_S24                     18,274
> SG0465_S53                     18,261
> SG0467_S77                     18,238
> SG0507_S82                     18,181
> SG0542_S123                    18,151
> SG0462_S17                     17,992
> SG0569_S162                    17,880
> SG0484_S91                     17,848
> SG0499_S81                     17,781
> SG0461_S5                      17,625
> SG0536_S146                    17,567
> SG0563_S185                    17,511
> SG0514_S71                     17,342
> SG0534_S122                    17,311
> SG0455_S28                     17,305
> SG0482_S67                     17,305
> SG0630_S218                    17,274
> SG0528_S145                    17,239
> SG0458_S64                     17,143
> SG0595_S177                    17,101
> SG0643_S279                    17,034
> SG0436_S2                      16,821
> SG0652_S197                    16,792
> SG0498_S69                     16,714
> SG0459_S76                     16,640
> SG0523_S72                     16,619
> SG0466_S65                     16,591
> SG0599_S130                    16,526
> SG0506_S70                     16,524
> SG0533_S110                    16,444
> SG0469_S6                      16,094
> SG0446_S27                     16,028
> SG0472_S42                     15,711
> SG0468_S89                     15,455
> SG0481_S55                     15,280
> SG0456_S40                     15,245
> SG0690_S261                    14,884
> SG0444_S3                      14,811
> SG0475_S78                     14,752
> SG0470_S18                     14,656
> SG0445_S15                     14,621
> SG0473_S54                     14,605
> SG0476_S90                     13,994
> SG0471_S30                     13,356
> SG0474_S66                     13,208
> SG0464_S41                     12,225
> SG0625_S265                    10,291
> SG0702_S299                    10
> SG0580_S358                    7
> SG0628_S370                    4
> SG0666_S382                    3
> SG0520_S346                    1
> 
> 
> Total number of samples:  292
> Total number of reads:  6,610,512


Previously, samples from 2009-2016 combined dataset were:
> Total number of samples:  636
> Total number of reads:  16,688,372

So for minsubstatitive abundance, use 5 (typically, 0.01% but this would translate to 2329 sequences)

```
cat /geomicro/data2/sgrim/MIS/MIS_LD_2016/MIS_2009_2016.fasta /geomicro/data2/sgrim/MIS/MIS_2017/MIS2017.fasta >> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017.fasta

o-pad-with-gaps MIS_2009_2017.fasta

```
set minsubstantive abundance to 5 

```
decompose -d 4 -N 3 --skip-gexf-files --skip-gen-figures --relocate-outliers --project MIS_2009_2017 --min-substantive-abundance 5 --sample-name-separator "_" /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017.fasta-PADDED-WITH-GAPS -o /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4
```
> Project ..........................................................: MIS_2009_2017
> Run date .........................................................: 07 Mar 18 09:41:53
> Library version ..................................................: 2.1
> Command line .....................................................: /usr/local/bin/decompose -d 4 -N 3 --skip-gexf-files --skip-gen-figures --relocate-outliers 
> --project MIS_2009_2017 --min-substantive-abundance 5 --sample-name-separator _ /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017.fasta-PADDED-WITH-GAPS -o /geomicro/data2/sgri
> m/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4
> Multi-threaded ...................................................: True
> Extraction info output file ......................................: /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/RUNINFO
> Log file path ....................................................: /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/RUNINFO.log
> Input file .......................................................: /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017.fasta-PADDED-WITH-GAPS
> Mapping file .....................................................: None
> Quick (and dirty) analysis requested .............................: False
> Merge homopolymer splits .........................................: False
> Skip removing outliers ...........................................: False
> Try to relocate outliers .........................................: True
> Store topology dict ..............................................: False
> Skip generating figures post analysis ............................: True
> Min entropy for a component to be picked for decomposition .......: 0.0965
> Perform entropy normalization heuristics .........................: True
> Max number of discriminants to use for decomposition .............: 4
> Min total abundance of oligotype in all samples ..................: 0
> Min substantive abundance of an oligotype (-M) ...................: 5
> Maximum variation allowed in each node (-V) ......................: 3
> Number of sequences analyzed .....................................: 23,298,884
> Average read length (without gaps) ...............................: 253
> Number of characters in each alignment ...........................: 302
> Output directory .................................................: /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4
> Directory to store final nodes ...................................: /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODES
> Temporary files directory ........................................: /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/TMP
> Figures directory ................................................: /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/FIGURES
> Number of raw nodes (before the refinement) ......................: 77,760
> Outliers removed due to -M .......................................: 755,187
> Outliers removed due to -V .......................................: 241,857
> Total number of outliers removed during the refinement ...........: 997,044
> Relocated outliers originally removed due to -M ..................: 313,584
> Relocated outliers originally removed due to -V ..................: 52,943
> Total number of relocated outliers ...............................: 366,527
> Number of samples found ..........................................: 927
> Number of final nodes (after the refinement) .....................: 78,512
> Number of sequences represented after quality filtering ..........: 22,668,367
> Final number of outliers due to -M ...............................: 441,603
> Final number of outliers due to -V ...............................: 188,914
> Final total number of outliers ...................................: 630,517
> Environment file .................................................: /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/ENVIRONMENT.txt
> Sample/oligotype abundance data matrix (counts) ..................: /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/MATRIX-COUNT.txt
> Sample/oligotype abundance data matrix (percents) ................: /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/MATRIX-PERCENT.txt
> Basic topology of MED nodes (txt) ................................: /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/TOPOLOGY.txt
> Representative sequences per node ................................: /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE-REPRESENTATIVES.fasta
> 
> Read distribution among samples table ............................: /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/READ-DISTRIBUTION.txt
> End of run .......................................................: 09 Mar 18 08:52:36


Use GAST to call taxonomy

```
cp /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE-REPRESENTATIVES.fasta /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.ng.fasta
sed -i "s/-//g" /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.ng.fasta
ln -s /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.ng.fasta /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.ng.unique.fasta
grep ">" /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.ng.fasta | awk 'BEGIN{FS=">"}{print $2"\t"$2}' >> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.ng.names
```

Copy the files to local destination via Globus to run gast
```
ln -s /Users/sgrim/Documents/MIS/MIS2017/NODE_REPRESENTATIVES.ng.fasta /Users/sgrim/Documents/MIS/MIS2017/NODE_REPRESENTATIVES.ng.unique.fasta
/Users/sgrim/gast/gast -in /Users/sgrim/Documents/MIS/MIS2017/NODE_REPRESENTATIVES.ng.fasta -ref /Users/sgrim/gast/refssu.fa -rtax /Users/sgrim/gast/refssu.tax -minp 0.9 -maj 66 -out /Users/sgrim/Documents/MIS/MIS2017/NODE_REPRESENTATIVES.fasta.gast -full
```
> usearch v7.0.1090_i86osx32, 4.0Gb RAM (17.2Gb total), 8 cores
> (C) Copyright 2013 Robert C. Edgar, all rights reserved.
> http://drive5.com/usearch
> 
> Licensed to: sgrim@umich.edu
> 
> 00:00  18Mb Reading /Users/sgrim/gast/refssu.fa, 593Mb
> 00:02 611Mb 401607 (401.6k) seqs, min 900, avg 1434, max 3749nt
> 00:20 615Mb  100.0% Masking
> 00:33 616Mb  100.0% Word stats
> 00:33 623Mb  100.0% Building slots
> 01:06 2.8Gb  100.0% Build index   
> 04:21 2.9Gb  100.0% Searching, 87.4% matched
> Argument "cnt" isn't numeric in foreach loop entry at /Users/sgrim/gast/gast line 492, <TAX> line 1.
```
grep -c "Unknown" /Users/sgrim/Documents/MIS/MIS2016LD/MIS_2009_2016-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.fasta.gast
```
8400 unknowns.
Transfer gast file to server.
    
Tidy up fastqs:

```
mkdir /geomicro/data2/sgrim/MIS/MIS_2017/fastqs
mv /geomicro/data2/sgrim/MIS/MIS_2017/*.fastq.gz /geomicro/data2/sgrim/MIS/MIS_2017/fastqs/.
```
Check taxonomy with SILVA release

```
blastn -query /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.ng.fasta -db /omics/PublicDB/silva/release_123/SILVA_123_SSURef_tax_silva.fasta -outfmt "7 std qcovs stitle  staxids" -out /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/blast_NODE-REPRESENTATIVES-SILVA123_NODE-REPRESENTATIVES.blastn -max_target_seqs 10 -dbsize 1000000 -num_threads 4 &
perl /geomicro/data1/COMMON/scripts/sandbox/postBlast.pl -b /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/blast_NODE-REPRESENTATIVES-SILVA123_NODE-REPRESENTATIVES.blastn -e 1e-5 -p 99 -c 80 -covCol 13 -o /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/blast_NODE-REPRESENTATIVES-SILVA123_NODE-REPRESENTATIVES-p99-c80.blastn

perl /geomicro/data1/COMMON/scripts/sandbox/top5.pl -b /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/blast_NODE-REPRESENTATIVES-SILVA123_NODE-REPRESENTATIVES-p99-c80.blastn -t 5 -o /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/blast_NODE-REPRESENTATIVES-SILVA123_NODE-REPRESENTATIVES-p99-c80-top5.blastn
```
Look at this file and compare to gast results:
/geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/blast_NODE-REPRESENTATIVES-SILVA123_NODE-REPRESENTATIVES-p99-c80-top5.blastn

Self-check for chimeras using Mothur

```
ln -s /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE-REPRESENTATIVES.fasta /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.fasta

mothur
chimera.uchime(fasta=/geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.fasta, reference=/geomicro/data2/sgrim/Silva/Silva119/silva.gold.align, processors=3)
```
> 01:08:52  47Mb  100.0% 1638/26207 chimeras found (6.2%) d  4(76M.b3 % ) miemrearsa sf ofuonudn d( 2(.51.%8)%
> 01:08:56  47Mb  100.0% 1553/26171 chimeras found (5.9%)
> 01:08:59  47Mb  100.0% 581/26131 chimeras found (2.2%)
> 
> It took 4140 secs to check 78512 sequences. 3772 chimeras were found.
> 
> Output File Names:
> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.ref.uchime.chimeras
> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.ref.uchime.accnos
> 
> [WARNING]: your sequence names contained ':'.  I changed them to '_' to avoid problems in your downstream analysis.
> 
```
remove.seqs(fasta=/geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.fasta, accnos=/geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.ref.uchime.accnos)
```
> Removed 3772 sequences from your fasta file.
> 
> Output File Names: 
> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.pick.fasta
> 
> [WARNING]: your sequence names contained ':'.  I changed them to '_' to avoid problems in your downstream analysis.

Mothur changed my headers.
so change 'em back.
```
sed -i 's/_/:/g' /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.pick.fasta

head /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.pick.fasta

```

grab all the unknowns
```
grep "Unknown" /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.fasta.gast | awk 'BEGIN{FS="\t"}{print $1}' >> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.unknown

for i in `cat /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.unknown`
do 
grep -A1 ${i} /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE-REPRESENTATIVES.fasta >> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE-REPRESENTATIVES.unknown.fasta
done
```

Look at the chimeric sequences removed, and check their taxonomies
```
sed -i 's/_/:/g' /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.ref.uchime.accnos

for i in `cat /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.ref.uchime.accnos`
do
{
grep ${i} /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.fasta.gast >> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.ref.uchime.accnos.gast
};
done

cat /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.ref.uchime.accnos.gast
```

```
for i in `cat /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.ref.uchime.accnos`
do
{
grep ${i} /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/blast_NODE-REPRESENTATIVES-SILVA123_NODE-REPRESENTATIVES-p99-c80-top5.blastn >> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.ref.uchime.accnos.silva123
};
done

cat /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017_NODE_REPRESENTATIVES.ref.uchime.accnos.silva123

```
3772 entries in the chimera ACCNOS file.
1773 entries in SILVA123 chimera file.
3772 entries in GAST chimera file.
Compare them:
* 3279 entries in the GAST chimera file do not have a corresponding entry in the SILVA123 blast file.
    * 140 are Unknown taxonomy.
* 493 do.

So remove the 3279 entries that are not in both files.

```
vi /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/MIS_2009_2017.chimeras

000159186
000049018
000159185
000015765
000015793
000167572
000250162
000295629
000295630
000295613
000211168
000104963
000205988
000092487
000329797
000322524
000322668
000322529
000328721
000287155
000290755
000287205
000322753
000202035
000322533
000322679
000322562
000105806
000322888
000322685
000322680
000322873
000290714
000287204
000218995
000291013
000290726
000210641
000210659
000208994
000322915
000208494
000207376
000322669
000290729
000287144
000209059
000202039
000322542
000291018
000204287
000290966
000322773
000210670
000322647
000208993
000208858
000322912
000322780
000291201
000290566
000209758
000208501
000290774
000290725
000287142
000208997
000204346
000105800
000322781
000322664
000291040
000286241
000208408
000203029
000322724
000322690
000290742
000289762
000287207
000287166
000210649
000094155
000322913
000290853
000098783
000322726
000321429
000287150
000210637
000209004
000208876
000322772
000322756
000291228
000291101
000290968
000289701
000287177
000235967
000210652
000209008
000208875
000094150
000328638
000322752
000322689
000291050
000290917
000287212
000287182
000091188
000328637
000322552
000289582
000208880
000322629
000322536
000322534
000290632
000290568
000290494
000289697
000287183
000286222
000210674
000328647
000322875
000322630
000321426
000290970
000290731
000290654
000290483
000287123
000277397
000017858
000322920
000322678
000322532
000322378
000291253
000291184
000291157
000287175
000235966
000218992
000208545
000105797
000328752
000328726
000322760
000322759
000322751
000322667
000322632
000322387
000322377
000291096
000210662
000290254
000210119
000092552
000207770
000210095
000322395
000210106
000109867
000095618
000091988
000207776
000290229
000329722
000290246
000091534
000185519
000290260
000161185
000210028
000329724
000264354
000329756
000329519
000154722
000261917
000154694
000109783
000154620
000323029
000151735
000091948
000323027
000227149
000206775
000264353
000154719
000109797
000300905
000315699
000262514
000227136
000206866
000151724
000093746
000329865
000329642
000329426
000315680
000329509
000322130
000286889
000329836
000329454
000264305
000264249
000210060
000329692
000329436
000329388
000329312
000315692
000289388
000289240
000264301
000261890
000206769
000330017
000315704
000261902
000210021
000207014
000151715
000110424
000037642
000328599
000300301
000264198
000289040
000152161
000153707
000153706
000185402
000286941
000316012
000036057
000202263
000092175
000204050
000095806
000206617
000288554
000092621
000285243
000285968
000092201
000316016
000286236
000278966
000131275
000122048
000329634
000288558
000278968
000061349
000054867
000288562
000205657
000277187
000055519
000304206
000287954
000210722
000321874
000321770
000288555
000286089
000181067
000286946
000007975
000036437
000316414
000181857
000065469
000210290
000208630
000100618
000214042
000092652
000329844
000277515
000329793
000181235
000261344
000150973
000261347
000150946
000004035
000035773
000330005
000163874
000002094
000234764
000015724
000119796
000235373
000235371
000096864
000221782
000096851
000096849
000201131
000221781
000107142
000015748
000237034
000201132
000222728
000201124
000015767
000297263
000201125
000284917
000015814
000144561
000124190
000266479
000266482
000271505
000191783
000078837
000191784
000281261
000005479
000266866
000005598
000191794
000169766
000078823
000078765
000005561
000111849
000159346
000078804
000054394
000191779
000191772
000078737
000281262
000242505
000169775
000078710
000316996
000316986
000191809
000169805
000078716
000054491
000191792
000078833
000078820
000005555
000054467
000266454
000191801
000191781
000169810
000169806
000271508
000271507
000242508
000169768
000169811
000191800
000054490
000266478
000266483
000169781
000278415
000278410
000278414
000018484
000243111
000159566
000279819
000078650
000129307
000159888
000129248
000024096
000159892
000159278
000266433
000169815
000169814
000078656
000247235
000243112
000266596
000242993
000243115
000129253
000247392
000243081
000242994
000247251
000243137
000247244
000242992
000243096
000283138
000247468
000129352
000247242
000243122
000198417
000247466
000247300
000129179
000129390
000283115
000134322
000247457
000243032
000247269
000025901
000243139
000247252
000243013
000134300
000308733
000129345
000308720
000129167
000283142
000247376
000129174
000247456
000247268
000283121
000283113
000247261
000129118
000278749
000243017
000283143
000247375
000247371
000134376
000129243
000308722
000134337
000129295
000283123
000134470
000129299
000134361
000129222
000320311
000308730
000283147
000278752
000278748
000247230
000198412
000134370
000024076
000306828
000283124
000130631
000129360
000129178
000129122
000320300
000306864
000283129
000247265
000247232
000243140
000243034
000134447
000129180
000320299
000283145
000243125
000242979
000134446
000320310
000247469
000247370
000129234
000306863
000283128
000283111
000247318
000247317
000243130
000243085
000129242
000024149
000279240
000320297
000283106
000279246
000283104
000283105
000236739
000308774
000308753
000243002
000308756
000243103
000243087
000129267
000129418
000129127
000308775
000247387
000242984
000088454
000308725
000129256
000247312
000247285
000243105
000024131
000319855
000308729
000247465
000243116
000243088
000129389
000308776
000247250
000129245
000247338
000243109
000325429
000235314
000235315
000325435
000235310
000119665
000119677
000235321
000235318
000119699
000021158
000325437
000325434
000119653
000293120
000016875
000246043
000308109
000132883
000246042
000242297
000246090
000242302
000128396
000235177
000303960
000160469
000303962
000235182
000323262
000200978
000200976
000212607
000211519
000323933
000212057
000323959
000299784
000284910
000099122
000299785
000296106
000201230
000323932
000099142
000323956
000211505
000097628
000211447
000324019
000323999
000324021
000292773
000211449
000323962
000296164
000292517
000106311
000099137
000201235
000323958
000323263
000296161
000292755
000219668
000211451
000099135
000292543
000201233
000201228
000299786
000219775
000211452
000106581
000323928
000212608
000292516
000212612
000200972
000099134
000098303
000000938
000323955
000323935
000323265
000292558
000211503
000201227
000098790
000284911
000099162
000098046
000296103
000097737
000201229
000015092
000090606
000211461
000292518
000292520
000284905
000200971
000284906
000212616
000212620
000073368
000115393
000303126
000232470
000105574
000276795
000022096
000200014
000109312
000003412
000284442
000199329
000266299
000158799
000158815
000158798
000199276
000320498
000199373
000320497
000160546
000044214
000160548
000179412
000294780
000294789
000323640
000294793
000294806
000179410
000007146
000097659
000097661
000002782
000165204
000004467
000087705
000115281
000198150
000165199
000000694
000306347
000266315
000101880
000040889
000018430
000266314
000033922
000065580
000011169
000115684
000115688
000078554
000078887
000078591
000159676
000129041
000043098
000129043
000119949
000316611
000276978
000032405
000063255
000276975
000032412
000276973
000063267
000183072
000307050
000022292
000247530
000243587
000183066
000247532
000247534
000183068
000216392
000022301
000216393
000069513
000159135
000053074
000227890
000016483
000284253
000284250
000307466
000124209
000307460
000244941
000089275
000244816
000244819
000267310
000267314
000161403
000267313
000035649
000161405
000045296
000046219
000276461
000178216
000276466
000307554
000179102
000276808
000276803
000014120
000215290
000307595
000072345
000178422
000161829
000161822
000267968
000317142
000267906
000267901
000267900
000317147
000162403
000317102
000317148
000317099
000267914
000178149
000317116
000267965
000317149
000267858
000162409
000162272
000001892
000317108
000317107
000267982
000267898
000178136
000317120
000267960
000007807
000004023
000162296
000162305
000162299
000070176
000162306
000242934
000242826
000242736
000128866
000121893
000175170
000174553
000274619
000058955
000058978
000058992
000174527
000319032
000174609
000274622
000274655
000319031
000197747
000299036
000269339
000113966
000269340
000048817
000298467
000128943
000128956
000242924
000130604
000130605
000306817
000306816
000128959
000014370
000178951
000178940
000276696
000178960
000276699
000284235
000062159
000276719
000276701
000178894
000284238
000130422
000130408
000320652
000161040
000243957
000088368
000130699
000243963
000243954
000089266
000243956
000243964
000088366
000009570
000276676
000284227
000276677
000284223
000003378
000162853
000046957
000162854
000046968
000260961
000260255
000148758
000147420
000133550
000260254
000147425
000260217
000032128
000260979
000149408
000308213
000308212
000251374
000315371
000149406
000315381
000251373
000253199
000260219
000257217
000174346
000314833
000259930
000148388
000314827
000174380
000314828
000314830
000174433
000174345
000174222
000314821
000148387
000314837
000148608
000314706
000032109
000032288
000330007
000314728
000260480
000248694
000310852
000256977
000273567
000060078
000329499
000273356
000172503
000027704
000273600
000217985
000314510
000273570
000260387
000029249
000318684
000312079
000260393
000143272
000137102
000248724
000311110
000311102
000311084
000315085
000314511
000311087
000311023
000172508
000172497
000326724
000217935
000172844
000143260
000137111
000260389
000260190
000248720
000248714
000314980
000273357
000255659
000254440
000193574
000318669
000273595
000273360
000256567
000256565
000255553
000082018
000326766
000326725
000318689
000314764
000312103
000255666
000255557
000246503
000172980
000172973
000273508
000254036
000259352
000137089
000329861
000273568
000314576
000190450
000273510
000172984
000273660
000273662
000273661
000273663
000259705
000312088
000058129
000255807
000032144
000273171
000001606
000029261
000136964
000030366
000136961
000028682
000260957
000260959
000254069
000137083
000312064
000318570
000176461
000273217
000273220
000273515
000259599
000260806
000032212
000149377
000255242
000260448
000133117
000312146
000173007
000143317
000254189
000145465
000143410
000260446
000254184
000144676
000145032
000145018
000143652
000143611
000255808
000254185
000318693
000273564
000190455
000318608
000053722
000076926
000315022
000273129
000191100
000320060
000315013
000260808
000190820
000260544
000329574
000169553
000022375
000281202
000176496
000149374
000172558
000169554
000320063
000315017
000273131
000273120
000191095
000318637
000191400
000273097
000281162
000190788
000190822
000190789
000190823
000191096
000149327
000281178
000281167
000147880
000259856
000246311
000273656
000246314
000259854
000259847
000246307
000273503
000327525
000314500
000147686
000096961
000259462
000137088
000314503
000260402
000260401
000329342
000327528
000260392
000314953
000259467
000027695
000248701
000133551
000149320
000149349
000082100
000060076
000027684
000027681
000329583
000273582
000312115
000311398
000314791
000147941
000311411
000147939
000137128
000318679
000273165
000173011
000148215
000311428
000273536
000259798
000145003
000318680
000314792
000273590
000273541
000104432
000318575
000177598
000096968
000027756
000329708
000259729
000259728
000246305
000149380
000218337
000137062
000148657
000260185
000311124
000326781
000217953
000311353
000137127
000254071
000030757
000326782
000311385
000311175
000217961
000028692
000028697
000315327
000315328
000172308
000312437
000326884
000217934
000326914
000312370
000248674
000295428
000315265
000292480
000311759
000311234
000256371
000273181
000312438
000218852
000312401
000218887
000032179
000311312
000295532
000218853
000217984
000253973
000311667
000260478
000145405
000295433
000190490
000311840
000312461
000217971
000311629
000292494
000312139
000253651
000217941
000326880
000326816
000318539
000312482
000295510
000027761
000273240
000312412
000136998
000218452
000097075
000312375
000295684
000273553
000097047
000218344
000173024
000326921
000292475
000097054
000133531
000030749
000308176
000295538
000273571
000145411
000140676
000326955
000312340
000256268
000217965
000140962
000137027
000027767
000326937
000312371
000256404
000217948
000312485
000312446
000256350
000217937
000326873
000312369
000312347
000326920
000326895
000312318
000311642
000311430
000015466
000311764
000295685
000253872
000144955
000144818
000144765
000137006
000315262
000311779
000295667
000218867
000105178
000315263
000312484
000311844
000308172
000295671
000255437
000255386
000140960
000027610
000318540
000312522
000312472
000312325
000311818
000311804
000295543
000273137
000260739
000256306
000256280
000255457
000248727
000246285
000218361
000143592
000143513
000137035
000105344
000096953
000326867
000311630
000308174
000295666
000295513
000273666
000260947
000256391
000218891
000218865
000217996
000211238
000140677
000076649
000015717
000000535
000326913
000315260
000312425
000311822
000311668
000298934
000295672
000273580
000273243
000253926
000246270
000218351
000144789
000137028
000137022
000326918
000326912
000326886
000311805
000311781
000311649
000273604
000273601
000256377
000248684
000218841
000218339
000172303
000144956
000144853
000140668
000096310
000027618
000315394
000312430
000311837
000260955
000256433
000256355
000256227
000255382
000255322
000254163
000246283
000145209
000326818
000311865
000311835
000311829
000295537
000255500
000253923
000248681
000211251
000143606
000143140
000137141
000137036
000097081
000097051
000096241
000329925
000329626
000326941
000326820
000315258
000312329
000311762
000273583
000273186
000256434
000255453
000255321
000218372
000143584
000143206
000140661
000136992
000096958
000096239
000311431
000295429
000292492
000292484
000273594
000256239
000253717
000253715
000218892
000218448
000218334
000217964
000191104
000148813
000144945
000143385
000143238
000097150
000096248
000027617
000329528
000328132
000312445
000312426
000312356
000312354
000312327
000311235
000295534
000295434
000292496
000292485
000273670
000256396
000256159
000256112
000253937
000253660
000218866
000173076
000149287
000143528
000137136
000137133
000326911
000326878
000312113
000292493
000292479
000273669
000273599
000273598
000273593
000256312
000255816
000254442
000248733
000218900
000217952
000029301
000273671
000312406
000030272
000148280
000031161
000145000
000314762
000314845
000143080
000314822
000314823
000077944
000030331
000260067
000256455
000313497
000295602
000260815
000218686
000313486
000313489
000218688
000218589
000295582
000260814
000256499
000256500
000313192
000261042
000327367
000313619
000257959
000256513
000146343
000097058
000029846
000015727
000260816
000257684
000256511
000313760
000104951
000218597
000148953
000313752
000327370
000315333
000315423
000315400
000314004
000313744
000313623
000258173
000327398
000313844
000327371
000327442
000313832
000258137
000146157
000313810
000313624
000327408
000327195
000315418
000313994
000258139
000313814
000327376
000313615
000313995
000313914
000327406
000313993
000258080
000313906
000313770
000177354
000031972
000327404
000327372
000313887
000295599
000133802
000314003
000295601
000313741
000327396
000313996
000313856
000313812
000258134
000257445
000257441
000327460
000327447
000315403
000313786
000246299
000146412
000329185
000313929
000257455
000257442
000146283
000145287
000257780
000313367
000327273
000218506
000257507
000214940
000257332
000147096
000327496
000313487
000312539
000145232
000260834
000256462
000313515
000256980
000256983
000146306
000328175
000145296
000312919
000312640
000327089
000314407
000312544
000256484
000146557
000059648
000312543
000251200
000295545
000295548
000146395
000146556
000105328
000130053
000077773
000258702
000295559
000258988
000058695
000295564
000258957
000258949
000327511
000258950
000313242
000274067
000258708
000314446
000313258
000274073
000274013
000174068
000274064
000274076
000173941
000274077
000274244
000176472
000174070
000173940
000274075
000174074
000274241
000314108
000274441
000274078
000320128
000295607
000274065
000257543
000058660
000320127
000313490
000274247
000246274
000318945
000174017
000274090
000174119
000173937
000137024
000136991
000174018
000148490
000146561
000274153
000174015
000133447
000146389
000274089
000274088
000327118
000133448
000082040
000310853
000281631
000274058
000312797
000281629
000274324
000274323
000174025
000060658
000274190
000259908
000257744
000295558
000274052
000274050
000176452
000274012
000276159
000059639
000177399
000177456
000177409
000173925
000274041
000176455
000274037
000136988
000145462
000256441
000327158
000136994
000314270
000145488
000145473
000031999
000147087
000097063
000312794
000025728
000256450
000145472
000327137
000314287
000313003
000256449
000327159
000314996
000314240
000314160
000145490
000313029
000031696
000257032
000310850
000326966
000142338
000031611
000314265
000031393
000147245
000323787
000314260
000313046
000310856
000256999
000218531
000031717
000312629
000097059
000329168
000327136
000323785
000314271
000148454
000314248
000314161
000257531
000256442
000148440
000258524
000257534
000147283
000327162
000326967
000257533
000248673
000017675
000256516
000314010
000313874
000256530
000327445
000314012
000313644
000256532
000256517
000315335
000313868
000313881
000313878
000256518
000187648
000044693
000014832
000017387
000103230
000081768
000281619
000174668
000281623
000201021
000201023
000193248
000017414
000268928
000268932
000164457
000231969
000233094
000269393
000269394
000269396
000159192
000165322
000192286
000192278
000248319
000215988
000193130
000163032
000215998
000103193
000102737
000281358
000164375
000017411
000215903
000192245
000294504
000088251
000294486
000017394
000002988
000216020
000164638
000164376
000103199
000294500
000268191
000192256
000159194
000294494
000248284
000215993
000048492
000323580
000318146
000281361
000279982
000102734
000017395
000170373
000163318
000163258
000323584
000294502
000269392
000051780
000320089
000318147
000188092
000188091
000197312
000282818
000023941
000282841
000282819
000302204
000282706
000282825
000073747
000328269
000282768
000328271
000230185
000188098
000188093
000136034
000295493
000136038
000133205
000326401
000136173
000247943
000268828
000247944
000243503
000211249
000136036
000134965
000112557
000160576
000164311
000136050
000030404
000247908
000247827
000176270
000157438
000136030
000308973
000268444
000187706
000030394
000303237
000136040
000308925
000135862
000299228
000247949
000247915
000232991
000157099
000266055
000325197
000160842
000040960
000325221
000266122
000230068
000267083
000297315
000179585
000221324
000316864
000302071
000157437
000063079
000297318
000103296
000160845
000040965
000325225
000316868
000230078
000179587
000266084
000179588
000325237
000230176
000302082
000267089
000266088
000266060
000316863
000267082
000266091
000032397
000325196
000266073
000160836
000160832
000113276
000329428
000325195
000266065
000041575
000302118
000302079
000266070
000160876
000267094
000266087
000299450
000201064
000149623
000157082
000222104
000299926
000299927
000201134
000297745
000157093
000026149
000032680
000300102
000222108
000201146
000201208
000243264
000201206
000149624
000090297
000032683
000032603
000032583
000222042
000149619
000201153
000017389
000299456
000090508
000032547
000201154
000165063
000149713
000032587
000032574
000032575
000299455
000297749
000224345
000222219
000201190
000224382
000222145
000108124
000201199
000201135
000090326
000329691
000300104
000300101
000284981
000224362
000165049
000149692
000105570
000300003
000297773
000224380
000090401
000053014
000108076
000108069
000105568
000032610
000324706
000299452
000284980
000014888
000300016
000243265
000233307
000201205
000023171
000222209
000222092
000106813
000032628
000297755
000284985
000201069
000083758
000224386
000038475
000269223
000164802
000269318
000269292
000164846
000164670
000170708
000164843
000130237
000269316
000269235
000269193
000269234
000272595
000171776
000056721
000271680
000038804
000272596
000155171
000097084
000271668
000171769
000171725
000272627
000170507
000171749
000055737
000272536
000254793
000327960
000272649
000272532
000171427
000170518
000002110
000264666
000271681
000171792
000155175
000272512
000155265
000318260
000271684
000264664
000171830
000171730
000254819
000171767
000055590
000003480
000276868
000040601
000276884
000157248
000316756
000265559
000063138
000265507
000265587
000170506
000316755
000179361
000221400
000157254
000316750
000265591
000276936
000063090
000276958
000276889
000265538
000040700
000324495
000316760
000316716
000265549
000265471
000261191
000156151
000276937
000265508
000265421
000170514
000032390
000276956
000276897
000265435
000043606
000032383
000265586
000265518
000265462
000265444
000316749
000276941
000179360
000276882
000157250
000043635
000316718
000276938
000265597
000171387
000171391
000171304
000318196
000155222
000272417
000272257
000037928
000171388
000037956
000272406
000155235
000272413
000171884
000171301
000155284
000155213
000272488
000155162
000272476
000272357
000318244
000171410
000254860
000038638
000318139
000198228
000171299
000037955
000272151
000043744
000038497
000318197
000272185
000254859
000155282
000144291
000043772
000252634
000171909
000171889
000037930
000318151
000272445
000272440
000255019
000155156
000318243
000318211
000311542
000272415
000171384
000171285
000155216
000038496
000318199
000272308
000272154
000155236
000155227
000056751
000318205
000272506
000272310
000155244
000043757
000318230
000318228
000318202
000318169
000311550
000272654
000272403
000272264
000272147
000171864
000171416
000038226
000029826
000002105
000318159
000272431
000272170
000254943
000171892
000155330
000037940
000015854
000318093
000272464
000272411
000255015
000171859
000171424
000155223
000144292
000037941
000318203
000272671
000272495
000272462
000272436
000272429
000172310
000171837
000155245
000056552
000318206
000318163
000272311
000272276
000272272
000272271
000272190
000155328
000155238
000056749
000056504
000003510
000318193
000318189
000272548
000272505
000272258
000252635
000170673
000056771
000048853
000002152
000318162
000311514
000311499
000272405
000272166
000155231
000105721
000038221
000037946
000327978
000318229
000272467
000272461
000272423
000171898
000056746
000318215
000318152
000318143
000317320
000272470
000272410
000272246
000171908
000171865
000171498
000155234
000144204
000101141
000038529
000038217
000323549
000318210
000318099
000272653
000272363
000272270
000272175
000255091
000198231
000048865
000272437
000324494
000114732
000306574
000302954
000302886
000231832
000114621
000231861
000005278
000115057
000231734
000231718
000115061
000306578
000232150
000231812
000169682
000114545
000127992
000302939
000232182
000232146
000231710
000242149
000114539
000006221
000302862
000302888
000325356
000306577
000303059
000302968
000232171
000128006
000114896
000114501
000325355
000302874
000306579
000128002
000140324
000249378
000309469
000249374
000309466
000309475
000138361
000309496
000104553
000104552
000029050
000309495
000249471
000249408
000140325
000138268
000028359
000309465
000249397
000096286
000138374
000309472
000309473
000248394
000248388
000049087
000309058
000030400
000309061
000309062
000248174
000015718
000309059
000105359
000019044
000015443
000218139
000295464
000026992
000248235
000309077
000309075
000309074
000309099
000248227
000248239
000309069
000295465
000309085
000248267
000309078
000248244
000248234
000248249
000248228
000171500
000309105
000309103
000309088
000248220
000248350
000130257
000269960
000012445
000216845
000050669
000167059
000103595
000269824
000027011
000323693
000004545
000269923
000328804
000317589
000323682
000269942
000269854
000323690
000323689
000269877
000327939
000317559
000294945
000323715
000328808
000317588
000086523
000281442
000161319
000267229
000151055
000267230
000267244
000159205
000161308
000150918
000150919
000267426
000267414
000035850
000267267
000159207
000161323
000267248
000261335
000261334
000151053
000307911
000239031
000307899
000239032
000326161
000326160
000307914
000162167
000326156
000124367
000035720
000307917
000162038
000035723
000225672
000245443
000330004
000124378
000046323
000326159
000225674
000124241
000244994
000135376
000135355
000181527
000032357
000135429
000135243
000135186
000026625
000135368
000135230
000043677
000026466
000135406
000026350
000135149
000135259
000043684
000023571
000135411
000200946
000277601
000247644
000135284
000181512
000135348
000026624
000135172
000135154
000026302
000135503
000026719
000026664
000181501
000135447
000026419
000247645
000135523
000135399
000135398
000135223
000039021
000026759
000003277
000090097
000043665
000135535
000065108
000135237
000135520
000135388
000135228
000277605
000247646
000135530
000135450
000135401
000135197
000043676
000024635
000135526
000043682
000005267
000032366
000155178
000179540
000271962
000158751
000264879
000264880
000219180
000158774
000158728
000158752
000149536
000219179
000157826
000041953
000149537
000158721
000039700
000149530
000265774
000264873
000155787
000149529
000265773
000032400
000158775
000155796
000266271
000264870
000158731
000149544
000306391
000219181
000158735
000157996
000155784
000016766
000149543
000039709
000082576
000058625
000310651
000097035
000105409
000030144
000096917
000096888
000218776
000015762
000101442
000066712
000246179
000183095
000250812
000132986
000140053
000198326
000177480
000250827
000250832
000250823
000183098
000308506
000246847
000087971
000177496
000308482
000326359
000295522
000308619
000133769
000246928
000310188
000028690
000326369
000295569
000250906
000246334
000310208
000310190
000246935
000133911
000251197
000310195
000251198
000246950
000310192
000177517
000140239
000246609
000276203
000246328
000326619
000310194
000250708
000139935
000027356
000251222
000251215
000250910
000250900
000310337
000250841
000250724
000326599
000251201
000310209
000308613
000251224
000250791
000133273
000310116
000329420
000295517
000246926
000015792
000329848
000310340
000250726
000246923
000326602
000326366
000211179
000139944
000326362
000310115
000251216
000251001
000250907
000246936
000218443
000211174
000250904
000211155
000211153
000211158
000140824
000140733
000295637
000211194
000295633
000096993
000295635
000295636
000218797
000104229
000218458
000253277
000218796
000030254
000218455
000211221
000104222
000142708
000211219
000310993
000211220
000029810
000029806
000015446
000310919
000253157
000253118
000253270
000211218
000142711
000103998
000096996
000310954
000295642
000253123
000211197
000029803
000295646
000030233
000030229
000310997
000310902
000030198
000000578
000211217
000211216
000105420
000030429
000076573
000076577
000250497
000218757
000250506
000250505
000218755
000251280
000329814
000139879
000096932
000326580
000251292
000250501
000096940
000096942
000025751
000326579
000030490
000326494
000015720
000249731
000149930
000149934
000149928
000221816
000018110
000201110
000324677
000284971
000194083
000328985
000033921
000033944
000324636
000033927
000284925
000297658
000201040
000324684
000324682
000299108
000284967
000297626
000284931
000324676
000284934
000221835
000324649
000328987
000328983
000284930
000252982
000252973
000252977
000096884
000096919
000295631
000295632
000017211
000102020
000168237
000271177
000052529
000096912
000001583
000097622
000211601
000211610
000097562
000211594
000292875
000098573
000211466
000035351
000212322
000073451
000212262
000292836
000097643
000292829
000035383
000097645
000292892
000142116
000142128
000251441
000141103
000310627
000104472
000000392
000104449
000096003
000025712
000104501
000029435
000310629
000029503
000015804
000310637
000310636
000141307
000326679
000252543
000141833
000141132
000140857
000104455
000096847
000096013
000252529
000252478
000251608
000251835
000218321
000218320
000141130
000104478
000096867
000058242
000251837
000029405
000251599
000218000
000014825
000224068
000110663
000069452
000069454
000110936
000245904
000227593
000025248
000245913
000308079
000308080
000227594
000110941
000308078
000245938
000227595
000110664
000245887
000245899
000110957
000069445
000025288
000270471
000159143
000150645
000131871
000317780
000165846
000317972
000295279
000042457
000266404
000270932
000035283
000270412
000167734
000035294
000298933
000317686
000014843
000049958
000296407
000317689
000157104
000050529
000004690
000050724
000035292
000317779
000329787
000270472
000150719
000150687
000014921
000329412
000168307
000050532
000002844
000270475
000261297
000159112
000296358
000270573
000270411
000261302
000168305
000111820
000051622
000049960
000049833
000271123
000188850
000159086
000050946
000035264
000270469
000220077
000157103
000050725
000001543
000102669
000215750
000215749
000102663
000299153
000299830
000298971
000200989
000299024
000033654
000324561
000201034
000299030
000200995
000299025
000324625
000299834
000266024
000296185
000328982
000324563
000015027
000124180
000066658
000211145
000124183
000030432
000137338
000282001
000217884
000142024
000249024
000237017
000281996
000142025
000066657
000247526
000066659
000137258
000247525
000281012
000211146
000027851
000137247
000237020
000211147
000235377
000312565
000174868
000145605
000275340
000058741
000256682
000058745
000031467
000012618
000319089
000248468
000248494
000176974
000256676
000136604
000174956
000176886
000059158
000256688
000176890
000176884
000058742
000235406
000136613
000136591
000145861
000136608
000319054
000256675
000248434
000235380
000145610
000319095
000256685
000119833
000248451
000145591
000319051
000256670
000176888
000136600
000136529
000319173
000174873
000275344
000256683
000177589
000256677
000256667
000248496
000248445
000058740
000319172
000248497
000248436
000235393
000176975
000319057
000193473
000059167
000312570
000248470
000136549
000134619
000008438
000024330
000090271
000150003
000299243
000299246
000149994
000107164
000298730
000224234
000324688
000324693
000324536
000299315
000150004
000107733
000000846
000221973
000090263
000224246
000299262
000150002
000299186
000284919
000221978
000149996
000329665
000324698
000324694
000324548
000298774
000150001
000097083
000133743
000129862
000211165
000284924
000284923
000224000
000310783
000252231
000252223
000252338
000252219
000141309
000096875
000252347
000252235
000326692
000141566
000252341
000076517
000252356
000252342
000141631
000310785
000148812
000141627
000211166
000141539
000218001
000218326
000000878
000033459
000033470
000124483
000280916
000237026
000022874
000195644
000280532
000038073
000280918
000067348
000198089
000067347
000155183
000306127
000155182
000232715
000303949
000282516
000196619
000242966
000073555
000197608
000090141
000090165
000000813
000042598
000000872
000090152
000324131
000090228
000320344
000003357
000043695
000116288
000199433
000320350
000199430
000160215
000199457
000284775
000284877
000284881
000283715
000200622
000084728
000013805
000155203
000325993
000325995
000128359
000242263
000128358
000242264
000306619
000306618
000128453
000242261
000242262
000246080
000128365
000306628
000128456
000128378
000306627
000132866
000306614
000132868
000023792
000246024
000128364
000242294
000242288
000128373
000329299
000246081
000242271
000128450
000329298
000306631
000306620
000306616
000242319
000128462
000325996
000325994
000306633
000243815
000198517
000283156
000239173
000243131
000306835
000212020
000038072
000239557
000103239
000001462
000278843
000242277
000198457
000160251
000211180
000122515
000119702
000243128
000120614
000242977
000129254
000022879
000013792
000242981
000239555
000154684
000129371
000097019
000000255
000198473
000087706
000017410
000216388
000211189
000103290
000096928
000243143
000124946
000304107
000277709
000160219
000124451
000097448
000090312
000054094
000020552
000015801
000015528
000002132
000306838
000187862
000159166
000155166
000055403
000053370
000008931
000283166
000238206
000212019
000183078
000155168
000128403
000124500
000121151
000103244
000057424
000043693
000017522
000306829
000243118
000239561
000211183
000198476
000178018
000160221
000160218
000159150
000159149
000120757
000111876
000073573
000021495
000015716
000002099
000306629
000277408
000198454
000184203
000169686
000130245
000108975
000103256
000002890
000309135
000309134
000266402
000243007
000159147
000039462
000024455
000243135
000242968
000239560
000217854
000198422
000129379
000124447
000017427
000009699
000306830
000304110
000284915
000283165
000243816
000243006
000216524
000187867
000184202
000160254
000124954
000116624
000079068
000070075
000043874
000032398
000022438
000021148
000283161
000279218
000279217
000243133
000235964
000207307
000159142
000159138
000073341
000042808
000016025
000002133
```
Tranpose the MATRIX file first to remove these nodes (since it's a sample x node file)
```
awk '{for (i=1; i<=NF; i++) a[i,NR]=$i; max=(max<NF?NF:max)} END {for (i=1; i<=max; i++) {for (j=1; j<=NR; j++) printf "%s%s", a[i,j], (j==NR?RS:FS)}}' /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/MATRIX-COUNT.txt >> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/MATRIX-COUNT_transposed.txt

grep -v -F -f /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/MIS_2009_2017.chimeras /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/MATRIX-COUNT_transposed.txt > /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/MATRIX-COUNT_transposed.chimerachecked.txt

awk '{for (i=1; i<=NF; i++) a[i,NR]=$i; max=(max<NF?NF:max)} END {for (i=1; i<=max; i++) {for (j=1; j<=NR; j++) printf "%s%s", a[i,j], (j==NR?RS:FS)}}' /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/MATRIX-COUNT_transposed.chimerachecked.txt >> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/MATRIX-COUNT.chimerachecked.txt
```

Make a taxonomy table (dropping the chimeras in the process):
* Print out the header that we've been using in our Rstudio analyses
* Remove the chimeras from gast results
* Print out the taxonomy table, with the first column  (unlabeled) being used as rownames when imported into Rstudio, and chop off the original header from GAST file
* In R, add in the conditional value in the last column if Taxonomy is unknown

```
echo -e "\tnode\tcount\ttaxonomy\trefhvr_ids\tclassified_unknown\tputative_contaminant" > /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/taxonomy_table.txt

grep -v -F -f /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/MIS_2009_2017.chimeras /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4//NODE_REPRESENTATIVES.fasta.gast | awk 'BEGIN{FS="\t"}{print $1"\t"$2"\t"$11}' | awk 'BEGIN{FS="\|"}{print $1"\t"$2}' | awk 'BEGIN{FS="\tsize:"}{print $1"\t"$2}' | awk 'BEGIN{FS="\t"}{print $1"\t"$1"\t"$2"\t"$3"\t"$4}' | tail -n +2 >> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/taxonomy_table.txt

```
Download the files /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/MATRIX-COUNT.chimerachecked.txt and /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/taxonomy_table.txt and play around with them in Rstudio

Check for contaminants

```
grep "genus" /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.fasta.gast > /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.fasta.gast.genus

grep -F -f /geomicro/data7/CLIO/Clio_2017-m0.10-A0-M5-d4/CoDL_contaminants.txt /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.fasta.gast.genus | wc -l

grep -F -f /geomicro/data7/CLIO/Clio_2017-m0.10-A0-M5-d4/CoDL_contaminants.txt /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.fasta.gast.genus | awk 'BEGIN{FS="\t"}{print $1}' | awk 'BEGIN{FS="\|"}{print $1}' > /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.putativecontaminants
```
5951 putative contaminant genus-level nodes

## 20180517

Now that [PhytoRef](http://phytoref.sb-roscoff.fr) is back online, use that database to check out what these chloroplasts are.

```
grep "Chloroplast" /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/taxonomy_table.txt | awk 'BEGIN{FS="\t"}{print $1}' >> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/chloroplast_nodes.txt

for i in `cat /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/chloroplast_nodes.txt`
do
{
grep ${i} /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.ng.fasta -A 1 >> /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/chloroplast_NODE_REPRESENTATIVES.ng.fasta
};
done

blastn -query /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/chloroplast_NODE_REPRESENTATIVES.ng.fasta -db /geomicro/data7/CLIO/PhytoRef_with_taxonomy.fasta -outfmt "7 std qcovs" -out /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/chloroplast_NODE_REPRESENTATIVES.ng.fasta.PhytoRef.blastn -num_threads 3 -max_target_seqs 100 -dbsize 1000000 &

perl /geomicro/data1/COMMON/scripts/sandbox/postBlast.pl -b /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/chloroplast_NODE_REPRESENTATIVES.ng.fasta.PhytoRef.blastn -e 1e-5 -p 99 -c 80 -covCol 13 -o /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/chloroplast_NODE_REPRESENTATIVES.ng.fasta.PhytoRef-p99-c80.blastn

perl /geomicro/data1/COMMON/scripts/sandbox/top5.pl -b /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/chloroplast_NODE_REPRESENTATIVES.ng.fasta.PhytoRef-p99-c80.blastn -t 5 -o /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/chloroplast_NODE_REPRESENTATIVES.ng.fasta.PhytoRef-p99-c80-top5.blastn
```

Edited taxonomy table manually in Excel to have the same taxonomic organization as GAST calls (using ; to delineate taxonomic org), and replaced Chloroplast-attributed taxonomy with the PhytoRef refined.

## 20190122
```
grep "000186693" /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/NODE_REPRESENTATIVES.ng.fasta -A 1

blastn -query /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/chloroplast_NODE_REPRESENTATIVES.ng.fasta -db /geomicro/data7/CLIO/PhytoRef_with_taxonomy.fasta -outfmt "7 std qcovs" -out /geomicro/data2/sgrim/MIS/MIS_2017/MIS_2009_2017-m0.10-A0-M5-d4/chloroplast_NODE_REPRESENTATIVES.ng.fasta.PhytoRef.blastn -num_threads 3 -max_target_seqs 100 -dbsize 1000000 &
```