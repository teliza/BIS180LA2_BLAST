*git clone https://github.com/UCDBIS180L-17/Assignment_2_Tsang.Eliza*
*cd /home/ubuntu/Assignment_2_Tsang.Eliza/*
*touch lab2_notebook.md* create file
*git add lab2_notebook.md* add file (so it's tracked)
*git pull* in order to localize the file
*git push* back into github

## FISH AND BASH FOR LOOPS

__Exercise 1:__

*set fruits peach tomato potato*

*for fruit in $fruits*
    *echo {$fruit}es*
*end*

__Exercise 2:__

*set myfiles (ls)*
It takes everything from the ls turns them into variables of myfiles.

*echo $myfiles*
Displays the variables in myfiles.

*for file in $myfiles*
    *cat $file*
*end*
For all the files in myfiles, read and display the contents of the files.

this
is
silly


Note: "file" in the command is just whatever you want to name all the variables. It's not a keyword or important to the command.
e.g:

*for fil in $myfiles*
    *cat $fil*
*end*

Will display the same thing.

__Exercise 3:__ (my process)

~/for_example (master) $ for file in (ls)
                             echo cat $file
                         end
cat file1.txt
cat file2.txt
cat file3.txt

~/for_example (master) $ for file in (ls)
                             echo $file
                             cat $file
                         end
file1.txt
this
file2.txt
is
file3.txt
silly

~/for_example (master) $ for file in $myfiles
                             echo file {$file} contains
                             cat $file
                         end
file file1.txt contains
this
file file2.txt contains
is
file file3.txt contains
silly

~/for_example (master) $ for file in $myfiles
                             echo file {$file} contains:
                             cat $file
                         end
file file1.txt contains:
this
file file2.txt contains:
is
file file3.txt contains:
silly


## Need Help:
### [OPTIONAL part 2 of exercise 3:OPTIONAL exercise 4: OPTIONAL 5] Do dis later

__Exercise 5:__

**Nested for loop** = lil loop in a greater loop
For ex: Gotta clean table, desk, and chair in each room but ya got like 4 rooms. So in each room you clean a table, desk, and chair, then move onto the next room and clean a table, desk, and chair, and so on.


## BLAST for paralogs and orthologs

### Orthologs
*ln -s ~/Data/Species/D.melanogaster/dmel-all-translation-r5.56.fasta ./flypep*
*formatdb -i flypep -p T*

Made the flypep files and formatdb. Do the same first code for wormpep, but there are a lot of error messages when you format because blastp expects the FASTA headers to be formatted in a very specific manner. Everyone formats the file differently anyway.

*formatdb -i wormpep -p T 2> /dev/null*
The 2> token redirects the stderr stream to a file instead of the screen. /dev/null is the name of a dummy file that is never made. It’s like a black hole for data: stuff goes in but never comes out.

__Finding the time it takes for blastp:__

*time blastp -db flypep -query P450 > default.blast*

**0.69user 0.02system 0:00.74elapsed** 96%CPU (0avgtext+0avgdata 154296maxresident)k
2608inputs+472outputs (13major+2266minor)pagefaults 0swaps

It's much faster compared to *water*.
**Default blastp search uses a word_size of 3 and a threshold of 11.**

__With loop:__

*for T in 10 11 12 13 14 15*
                                          *echo "Starting blasp with threshold $T"*
                                          *time blastp -db flypep -query P450 -threshold $T > $T.blast*
                                      *end*
Starting blasp with threshold 10
1.09user 0.02system 0:01.12elapsed 99%CPU (0avgtext+0avgdata 149216maxresident)k
0inputs+472outputs (0major+2603minor)pagefaults 0swaps
Starting blasp with threshold 11
0.70user 0.02system 0:00.72elapsed 100%CPU (0avgtext+0avgdata 148952maxresident)k
0inputs+472outputs (0major+2462minor)pagefaults 0swaps
Starting blasp with threshold 12
0.49user 0.02system 0:00.51elapsed 99%CPU (0avgtext+0avgdata 150460maxresident)k
0inputs+464outputs (0major+2327minor)pagefaults 0swaps
Starting blasp with threshold 13
0.36user 0.03system 0:00.40elapsed 99%CPU (0avgtext+0avgdata 149044maxresident)k
0inputs+456outputs (0major+2514minor)pagefaults 0swaps
Starting blasp with threshold 14
0.30user 0.02system 0:00.33elapsed 99%CPU (0avgtext+0avgdata 149128maxresident)k
0inputs+416outputs (0major+2482minor)pagefaults 0swaps
Starting blasp with threshold 15
0.27user 0.02system 0:00.30elapsed 99%CPU (0avgtext+0avgdata 150388maxresident)k
0inputs+400outputs (0major+2293minor)pagefaults 0swaps

*for T* sets T as the variables.

__Comparing proteome to proteome__

|Threshold |Time|
|:-: |:-: |
|10 |0:01.11 |
|11| 0:00.72|
|12|0:00.51|
|13|0:00.39|
|14|0:00.32|
|15|0:00.30 |

The time taken is not linear but exponential with increasing threshold value. The more consecutive amino acids required for a match (higher threshold), the faster blastp takes to complete.

*blastp -query wormpep -db flypep -outfmt 6 -culling_limit 1*

Just do one gene from C.elegans (P450) against the proteome of D.melanogaster.

*-culling_limit 1* will make it so that BLAST only reports the single best alignment.

*-outfmt 7*  changes the output format to be tab-delimited fields with a little header information that tells you what each column is. Switch to output format 6 because we don’t really care about the header lines.

__Finding fly ortholog from fly proteome using worm gene:__

*time blastp -db flypep -query P450 -outfmt 6 -culling_limit 1*

B0213.10	**FBpp0074380**	27.11	498	314	12	19	494	47	517	**3e-44**	165
0.51user 0.03system 0:00.55elapsed 99%CPU (0avgtext+0avgdata 148960maxresident)k
0inputs+0outputs (0major+2297minor)pagefaults 0swaps

Now I'll search the fly proteome for the protein FBpp0074380 and then do the same command as above but vice versa.

*grep -A 11 FBpp0074380 /home/ubuntu/Data/Species/D.melanogaster/dmel-all-translation-r5.56.fasta | cat > flyortholog_worm*

*time blastp -db wormpep -query flyortholog_worm -outfmt 6 -culling_limit 1*

*time blastp -db wormpep -query flyortholog_worm -outfmt 6 -culling_limit 1*

FBpp0074380	F41B5.4#CE10222#WBGene00018262#locus:cyp-33C3#cytochrome	33.79	512	310	12	26	525	3	497	6e-84	272
0.39user 0.03system 0:00.43elapsed 99%CPU (0avgtext+0avgdata 141572maxresident)k
0inputs+0outputs (0major+2293minor)pagefaults 0swaps


### Paralogs

*grep -A 8 T21B10.2a c_elegans.PRJNA13758.WS240.protein.fa | cat > T21B10.2a*

I first started with -A 10 then adjusted as I saw where the protein sequence ended. I used cat and copied the file into a new file (because the T21B10.2a file didn't exist yet).

*blastp -db wormpep -query T21B10.2a > T21B10.2a.blastp*
*blastp -db wormpep -query P450 > P450.blastp* (B0213.10)

*less P450.blastp*

Score     E
Sequences producing significant alignments:                          (Bits)  Value

B0213.10#CE16781#WBGene00015040#locus:cyp-34A5#cytochrome P450#...  1026    0.0
B0213.15a#CE30582#WBGene00015044#locus:cyp-34A9#cytochrome P450...  786     0.0
B0213.15b#CE39545#WBGene00015044#locus:cyp-34A9#status:Confirme...  775     0.0
B0213.14#CE41971#WBGene00015043#locus:cyp-34A8#cytochrome P450#...  755     0.0
B0213.12#CE16783#WBGene00015042#locus:cyp-34A7#cytochrome P450#...  731     0.0
B0213.11a#CE35205#WBGene00015041#locus:cyp-34A6#cytochrome P-45...  714     0.0
T09H2.1#CE18231#WBGene00020386#locus:cyp-34A4#cytochrome P450#s...  674     0.0
B0213.16#CE16786#WBGene00015045#locus:cyp-34A10#cytochrome P-45...  671     0.0
T10H4.11#CE16396#WBGene00011699#locus:cyp-34A2#cytochrome P450#...  658     0.0
T10H4.10#CE16395#WBGene00011698#locus:cyp-34A1#cytochrome P450#...  603     0.0
B0213.11b#CE46874#WBGene00015041#locus:cyp-34A6#status:Partiall...  468     1e-162
C49G7.8#CE08866#WBGene00016786#locus:cyp-35A4#cytochrome P450#s...  400     8e-134
C03G6.15#CE35217#WBGene00015400#locus:cyp-35A2#cytochrome P450#...  395     1e-131
C06B3.3#CE27662#WBGene00007362#locus:cyp-35C1#cytochrome P450#s...  389     2e-129
K09D9.2#CE21056#WBGene00019565#locus:cyp-35A3#cytochrome P450#s...  383     4e-127
K07C6.4#CE17175#WBGene00019472#locus:cyp-35B1#cytochrome P450#s...  382     7e-126
K07C6.2#CE17173#WBGene00019470#locus:cyp-35B3#cytochrome P450#s...  376     2e-124
K07C6.5#CE17176#WBGene00019473#locus:cyp-35A5#cytochrome P450#s...  375     7e-124
K07C6.3#CE17174#WBGene00019471#locus:cyp-35B2#cytochrome P450#s...  368     4e-121
F14H3.10#CE15834#WBGene00008829#locus:cyp-35D1#cytochrome P450#...  334     4e-108
C03G6.14#CE41862#WBGene00015399#locus:cyp-35A1#status:Partially...  326     2e-105
F44C8.1#CE10378#WBGene00018413#locus:cyp-33C4#cytochrome P450#s...  278     1e-86
 F41B5.4#CE10222#WBGene00018262#locus:cyp-33C3#cytochrome P450#s...  273     1e-84
 F41B5.7#CE10228#WBGene00018264#locus:cyp-33C6#cytochrome P450#s...  271     2e-84
 F41B5.3#CE27379#WBGene00018261#locus:cyp-33C5#cytochrome P450#s...  265     9e-82
 C45H4.2#CE46272#WBGene00016686#locus:cyp-33C1#cytochrome P450#s...  265     2e-81
 B0304.3#CE27050#WBGene00015135#locus:cyp-23A1#Cytochrome P450#s...  264     7e-81
 Y17D7A.4#CE16589#WBGene00012448#locus:cyp-33D3#Cytochrome P450#...  262     2e-80
 K05D4.4#CE16230#WBGene00010589#locus:cyp-33D1#cytochrome P450#s...  261     3e-80
 C45H4.17#CE25818#WBGene00016697#locus:cyp-33C2#cytochrome P450#...  250     6e-76
 Y49C4A.9#CE22218#WBGene00021710#locus:cyp-33C11#status:Partiall...  246     1e-74
 F42A9.5#CE41900#WBGene00018334#locus:cyp-33E2#cytochrome P450#s...  243     2e-73
 C50H11.15#CE08931#WBGene00016860#locus:cyp-33C9#cytochrome P450...  242     5e-73
 F41B5.2#CE42114#WBGene00018260#locus:cyp-33C7#cytochrome P450#s...  238     2e-71
 C49C8.4#CE31452#WBGene00016768#locus:cyp-33E1#cytochrome P450#s...  236     1e-70
 K09A11.2#CE03473#WBGene00010705#locus:cyp-14A1#cytochrome p450#...  231     1e-68
 C25E10.2#CE35248#WBGene00016092#locus:cyp-33B1#cytochrome P450#...  230     2e-68
 K09A11.4#CE03475#WBGene00010707#locus:cyp-14A3#cytochrome p450#...  228     8e-68
 R08F11.3#CE12584#WBGene00019967#locus:cyp-33C8#cytochrome P450#...  225     1e-66
 R04D3.1#CE06222#WBGene00011009#locus:cyp-14A4#cytochrome P450#s...  221     3e-65
 K09A11.3#CE03474#WBGene00010706#locus:cyp-14A2#Cytochrome P450#...  220     8e-65
 C12D5.7#CE06809#WBGene00015709#locus:cyp-33A1#cytochrome P450#s...  220     1e-64
 Y5H2B.6#CE21315#WBGene00021168#locus:cyp-33C12#status:Confirmed...  211     6e-62
 F08F3.7#CE09262#WBGene00000373#locus:cyp-14A5#cytochrome P450#s...  207     5e-60
 B0213.15c#CE39546#WBGene00015044#locus:cyp-34A9#status:Confirme...  188     4e-57
 C34B7.3#CE08567#WBGene00007913#locus:cyp-36A1#cytochrome P450#s...  178     4e-49
 T13C5.1b#CE30451#WBGene00000905#locus:daf-9#cytochrome P450#sta...  132     2e-33
 T13C5.1a#CE27206#WBGene00000905#locus:daf-9#cytochrome P450#sta...  132     3e-33
 K06G5.2#CE37720#WBGene00010606#locus:cyp-13B2#Cytochrome P450#s...  109     1e-25
 F42A9.4#CE07226#WBGene00018333#locus:cyp-33E3#status:Partially_...  105     1e-25
 F02C12.5a#CE09177#WBGene00008519#locus:cyp-13B1#cytochrome P450...  108     3e-25
 F02C12.5c#CE23627#WBGene00008519#locus:cyp-13B1#status:Partiall...  105     3e-24
 Y17G9B.3#CE41715#WBGene00021200#locus:cyp-31A3#cytochrome P450#...  104     6e-24
 H02I12.8#CE37719#WBGene00010354#locus:cyp-31A2#Cytochrome P450#...  104     7e-24
 T10B9.7#CE01659#WBGene00011676#locus:cyp-13A2#cytochrome P450#s...  93.6    3e-20
 T10B9.10#CE01655#WBGene00000372#locus:cyp-13A7#cytochrome P450#...  93.2    4e-20
   C36A4.2#CE37617#WBGene00007964#locus:cyp-25A2#cytochrome P450#s...  92.4    6e-20
   F01D5.9#CE35449#WBGene00008499#locus:cyp-37A1#Cytochrome P450#s...  92.4    7e-20
   ZK1320.4#CE37736#WBGene00014254#locus:cyp-13A10#cytochrome P450...  91.7    1e-19
   T10B9.8#CE01660#WBGene00011677#locus:cyp-13A1#cytochrome P450#s...  89.7    5e-19
   T19B10.1#CE06455#WBGene00011830#locus:cyp-29A2#cytochrome P450#...  89.0    8e-19
   F14F7.3#CE15823#WBGene00008810#locus:cyp-13A12#cytochrome P450#...  87.8    2e-18
   T10B9.4#CE13541#WBGene00011674#locus:cyp-13A8#cytochrome P450#s...  86.3    7e-18
   T10B9.2#CE01656#WBGene00011672#locus:cyp-13A5#cytochrome P450#s...  85.9    1e-17
   Y80D3A.5#CE36516#WBGene00013585#locus:cyp-42A1#cytochrome P450#...  83.2    7e-17
   Y38C9B.1#CE35365#WBGene00021412#locus:cyp-29A3#cytochrome P450#...  83.2    7e-17
   B0331.1#CE43023#WBGene00007140#locus:cyp-29A4#cytochrome P450#s...  82.4    1e-16
   T10B9.5#CE01658#WBGene00011675#locus:cyp-13A3#cytochrome P450#s...  82.4    1e-16
   C36A4.1#CE43488#WBGene00007963#locus:cyp-25A1#cytochrome P450#s...  81.6    2e-16
   C36A4.3#CE40224#WBGene00014697#locus:cyp-25A3#status:Partially_...  81.3    3e-16
   F28G4.1#CE15919#WBGene00009226#locus:cyp-37B1#cytochrome P450#s...  80.5    5e-16
   T10B9.1#CE01654#WBGene00011671#locus:cyp-13A4#cytochrome P450#s...  78.6    2e-15
   T10B9.3#CE01657#WBGene00011673#locus:cyp-13A6#cytochrome P450#s...  77.4    5e-15
   C26F1.2#CE39228#WBGene00016147#locus:cyp-32A1#cytochrome P450#s...  70.9    6e-13
   C36A4.6#CE03074#WBGene00007967#locus:cyp-25A4#cytochrome P450#s...  70.1    1e-12
   F14F7.2#CE15822#WBGene00008809#locus:cyp-13A11#cytochrome P450#...  69.7    1e-12
   Y5H2B.5#CE26222#WBGene00021167#locus:cyp-32B1#status:Confirmed#...  65.9    2e-11
   ZK177.5#CE25682#WBGene00000375#locus:cyp-44A1#cytochrome p450#s...  57.0    1e-08
   E03E2.1#CE47225#WBGene00017108#locus:cyp-43A1#cytochrome P450#s...  43.1    3e-04



*less T21B10.2a.blastp*

Sequences producing significant alignments:                          (Bits)  Value

  T21B10.2a#CE03684#WBGene00011884#locus:enol-1#enolase#status:Co...  891     0.0
  T21B10.2c#CE36954#WBGene00011884#locus:enol-1#status:Confirmed#...  891     0.0
  T21B10.2b#CE32730#WBGene00011884#locus:enol-1#status:Confirmed#...  419     2e-144

**Should have taken 2 weeks with *water***

***blastp* will take around 9hours**

time blastp -query flypep -db wormpep -outfmt 6 -culling_limit 1 > fly_worm.blastp
Warning: [blastp] lcl|Query_6030 FBpp0073637 type=protein; loc=X:join(13506205..13506234,13506294..13506746,13506809..13507075); ID=FBpp0073637; name=BthD-PA; parent=FBgn0030501,FBtr0073806; dbxref=FlyBase:FBpp0073637,FlyBase_Annotation_IDs:CG11177-PA,GB_protein:AAF48293.3,REFSEQ:NP_572903,GB_protein:AAF48293,FlyMine:FBpp0073637,modMine:FBpp0073637; MD5=bb43103d666f8b12b6c8aeba77baa742; length=249; release=r5.56; species=Dmel: Warning: One or more U or O characters replaced by X for alignment score calculations at positions 36
Warning: [blastp] lcl|Query_21416 FBpp0079587 type=protein; loc=2L:join(10342759..10342915,10342976..10343354,10343411..10343604,10343659..10343762,10343816..10343899); ID=FBpp0079587; name=Sps2-PB; parent=FBgn0032224,FBtr0079997; dbxref=FlyBase:FBpp0079587,FlyBase_Annotation_IDs:CG5025-PB,GB_protein:AAN10746.2,REFSEQ:NP_477477,GB_protein:AAN10746,FlyMine:FBpp0079587,modMine:FBpp0079587; MD5=73eabdd4349b659cbf4b7e21de17d2d0; length=305; release=r5.56; species=Dmel: Warning: One or more U or O characters replaced by X for alignment score calculations at positions 23
Warning: [blastp] lcl|Query_21417 FBpp0079588 type=protein; loc=2L:join(10342759..10342915,10342976..10343354,10343411..10343604,10343659..10344041); ID=FBpp0079588; name=Sps2-PA; parent=FBgn0032224,FBtr0079998; dbxref=FlyBase:FBpp0079588,FlyBase_Annotation_IDs:CG5025-PA,GB_protein:AAF52918.2,REFSEQ:NP_477478,GB_protein:AAF52918,FlyMine:FBpp0079588,modMine:FBpp0079588; MD5=e62633b4d84240076254f371ae53b6d7; length=370; release=r5.56; species=Dmel: Warning: One or more U or O characters replaced by X for alignment score calculations at positions 23
Warning: [blastp] lcl|Query_21418 FBpp0301800 type=protein; loc=2L:join(10342759..10342915,10342976..10343354,10343411..10343604,10343659..10344041); ID=FBpp0301800; name=Sps2-PC; parent=FBgn0032224,FBtr0310115; dbxref=FlyBase:FBpp0301800,FlyBase_Annotation_IDs:CG5025-PC,REFSEQ:NP_001260329,GB_protein:AGB92864; MD5=e62633b4d84240076254f371ae53b6d7; length=370; release=r5.56; species=Dmel: Warning: One or more U or O characters replaced by X for alignment score calculations at positions 23
Warning: [blastp] lcl|Query_21419 FBpp0301801 type=protein; loc=2L:join(10342759..10342915,10342976..10343354,10343411..10343604,10343659..10343762,10343816..10343899); ID=FBpp0301801; name=Sps2-PD; parent=FBgn0032224,FBtr0310116; dbxref=FlyBase:FBpp0301801,FlyBase_Annotation_IDs:CG5025-PD,REFSEQ:NP_001260330,GB_protein:AGB92865; MD5=73eabdd4349b659cbf4b7e21de17d2d0; length=305; release=r5.56; species=Dmel: Warning: One or more U or O characters replaced by X for alignment score calculations at positions 23
Warning: [blastp] lcl|Query_29322 FBpp0073414 type=protein; loc=X:join(11781430..11781448,11781516..11781600,11781664..11781892); ID=FBpp0073414; name=SelG-PA; parent=FBgn0030350,FBtr0073570; dbxref=FlyBase:FBpp0073414,FlyBase_Annotation_IDs:CG1844-PA,GB_protein:AAF48111.2,REFSEQ:NP_572763,GB_protein:AAF48111,FlyMine:FBpp0073414,modMine:FBpp0073414; MD5=162e12a2221fb99a7b38c70a86f5f6e8; length=110; release=r5.56; species=Dmel: Warning: One or more U or O characters replaced by X for alignment score calculations at positions 108
26119.70user 10.35system **7:15:36elapsed** 99%CPU (0avgtext+0avgdata 151636maxresident)k
0inputs+14272outputs (0major+365043minor)pagefaults 0swaps

Identity: exact same
Similarity: some AA close enough

For orthologs: Check by not only looking at best hit but see next best hit. If the next best is almost the same, maybe not trust the blast result? Best criteria: find ONE best hit both ways.

(Also, increasing the word size means it requires more consecutive matches so I'd get more accurate results, but I may not get many hits in return. Decreasing the word size allows the search to find other similar matches but it may not be as exact.)

-------------------------------------------
fly_worm.blastp

fly gene on left; worm next; percent identity; matches? ; mismatches; gaps; query start (where did the alignment start in query seq); query stop; e-val; score

How to find orthologs? The way we're doing it now is not the most efficient way...kinda like doing it by hand

My best hit:
FBpp0081838     **ZK384.2** #CE16723#WBGene00013972#locus:scl-20#SCP-like    28.87   194     114     8       60      238     28      212     1e-07   51.2
