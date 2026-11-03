R Notebook
================

## ls (list show) : savoir ce qu’on a dans le fichier

## wc = word count

## on est allé sur l’ena on a téléchargé les séquences, qu’on a mises dans notre dossier Data avec (ls ; source ena (tab) etn entrée)

## ensuite run selector avec PRJEB65896 de l’article et on télécharge la table de métadonnée et on l’ouvre dans une variable avec read.csv

# mettre commentaires dans commit, en dehors des chunks!!!!!!!!!!

``` r
meta_data_table <- read.csv("SraRunTable.csv", sep = ",", header = TRUE)
  # importation de la table de métadonnée 
```

``` r
library(dada2); packageVersion("dada2")
```

    ## Loading required package: Rcpp

    ## [1] '1.40.0'

``` r
path <- "/home/rstudio/ADM_Données_articles/data_article" # CHANGE ME to the directory containing the fastq files after unzipping.
list.files(path)
```

    ##   [1] "ena-file-download-read_run-PRJEB65896-fastq_ftp-20261007-0649-2.sh"
    ##   [2] "ERR12023394_1.fastq.gz"                                            
    ##   [3] "ERR12023394_2.fastq.gz"                                            
    ##   [4] "ERR12023395_1.fastq.gz"                                            
    ##   [5] "ERR12023395_2.fastq.gz"                                            
    ##   [6] "ERR12023396_1.fastq.gz"                                            
    ##   [7] "ERR12023396_2.fastq.gz"                                            
    ##   [8] "ERR12023397_1.fastq.gz"                                            
    ##   [9] "ERR12023397_2.fastq.gz"                                            
    ##  [10] "ERR12023398_1.fastq.gz"                                            
    ##  [11] "ERR12023398_2.fastq.gz"                                            
    ##  [12] "ERR12023400_1.fastq.gz"                                            
    ##  [13] "ERR12023400_2.fastq.gz"                                            
    ##  [14] "ERR12023401_1.fastq.gz"                                            
    ##  [15] "ERR12023401_2.fastq.gz"                                            
    ##  [16] "ERR12023402_1.fastq.gz"                                            
    ##  [17] "ERR12023402_2.fastq.gz"                                            
    ##  [18] "ERR12023403_1.fastq.gz"                                            
    ##  [19] "ERR12023403_2.fastq.gz"                                            
    ##  [20] "ERR12023404_1.fastq.gz"                                            
    ##  [21] "ERR12023404_2.fastq.gz"                                            
    ##  [22] "ERR12023405_1.fastq.gz"                                            
    ##  [23] "ERR12023405_2.fastq.gz"                                            
    ##  [24] "ERR12023406_1.fastq.gz"                                            
    ##  [25] "ERR12023406_2.fastq.gz"                                            
    ##  [26] "ERR12023407_1.fastq.gz"                                            
    ##  [27] "ERR12023407_2.fastq.gz"                                            
    ##  [28] "ERR12023408_1.fastq.gz"                                            
    ##  [29] "ERR12023408_2.fastq.gz"                                            
    ##  [30] "ERR12023409_1.fastq.gz"                                            
    ##  [31] "ERR12023409_2.fastq.gz"                                            
    ##  [32] "ERR12023410_1.fastq.gz"                                            
    ##  [33] "ERR12023410_2.fastq.gz"                                            
    ##  [34] "ERR12023411_1.fastq.gz"                                            
    ##  [35] "ERR12023411_2.fastq.gz"                                            
    ##  [36] "ERR12023412_1.fastq.gz"                                            
    ##  [37] "ERR12023412_2.fastq.gz"                                            
    ##  [38] "ERR12023413_1.fastq.gz"                                            
    ##  [39] "ERR12023413_2.fastq.gz"                                            
    ##  [40] "ERR12023414_1.fastq.gz"                                            
    ##  [41] "ERR12023414_2.fastq.gz"                                            
    ##  [42] "ERR12023415_1.fastq.gz"                                            
    ##  [43] "ERR12023415_2.fastq.gz"                                            
    ##  [44] "ERR12023416_1.fastq.gz"                                            
    ##  [45] "ERR12023416_2.fastq.gz"                                            
    ##  [46] "ERR12023417_1.fastq.gz"                                            
    ##  [47] "ERR12023417_2.fastq.gz"                                            
    ##  [48] "ERR12023418_1.fastq.gz"                                            
    ##  [49] "ERR12023418_2.fastq.gz"                                            
    ##  [50] "ERR12023419_1.fastq.gz"                                            
    ##  [51] "ERR12023419_2.fastq.gz"                                            
    ##  [52] "ERR12023420_1.fastq.gz"                                            
    ##  [53] "ERR12023420_2.fastq.gz"                                            
    ##  [54] "ERR12023421_1.fastq.gz"                                            
    ##  [55] "ERR12023421_2.fastq.gz"                                            
    ##  [56] "ERR12023422_1.fastq.gz"                                            
    ##  [57] "ERR12023422_2.fastq.gz"                                            
    ##  [58] "ERR12023423_1.fastq.gz"                                            
    ##  [59] "ERR12023423_2.fastq.gz"                                            
    ##  [60] "ERR12023424_1.fastq.gz"                                            
    ##  [61] "ERR12023424_2.fastq.gz"                                            
    ##  [62] "ERR12023425_1.fastq.gz"                                            
    ##  [63] "ERR12023425_2.fastq.gz"                                            
    ##  [64] "ERR12023426_1.fastq.gz"                                            
    ##  [65] "ERR12023426_2.fastq.gz"                                            
    ##  [66] "ERR12023427_1.fastq.gz"                                            
    ##  [67] "ERR12023427_2.fastq.gz"                                            
    ##  [68] "ERR12023428_1.fastq.gz"                                            
    ##  [69] "ERR12023428_2.fastq.gz"                                            
    ##  [70] "ERR12023429_1.fastq.gz"                                            
    ##  [71] "ERR12023429_2.fastq.gz"                                            
    ##  [72] "ERR12023430_1.fastq.gz"                                            
    ##  [73] "ERR12023430_2.fastq.gz"                                            
    ##  [74] "ERR12023431_1.fastq.gz"                                            
    ##  [75] "ERR12023431_2.fastq.gz"                                            
    ##  [76] "ERR12023432_1.fastq.gz"                                            
    ##  [77] "ERR12023432_2.fastq.gz"                                            
    ##  [78] "ERR12023433_1.fastq.gz"                                            
    ##  [79] "ERR12023433_2.fastq.gz"                                            
    ##  [80] "ERR12023434_1.fastq.gz"                                            
    ##  [81] "ERR12023434_2.fastq.gz"                                            
    ##  [82] "ERR12023435_1.fastq.gz"                                            
    ##  [83] "ERR12023435_2.fastq.gz"                                            
    ##  [84] "ERR12023436_1.fastq.gz"                                            
    ##  [85] "ERR12023436_2.fastq.gz"                                            
    ##  [86] "ERR12023437_1.fastq.gz"                                            
    ##  [87] "ERR12023437_2.fastq.gz"                                            
    ##  [88] "ERR12023438_1.fastq.gz"                                            
    ##  [89] "ERR12023438_2.fastq.gz"                                            
    ##  [90] "ERR12023439_1.fastq.gz"                                            
    ##  [91] "ERR12023439_2.fastq.gz"                                            
    ##  [92] "ERR12023440_1.fastq.gz"                                            
    ##  [93] "ERR12023440_2.fastq.gz"                                            
    ##  [94] "ERR12023441_1.fastq.gz"                                            
    ##  [95] "ERR12023441_2.fastq.gz"                                            
    ##  [96] "ERR12023442_1.fastq.gz"                                            
    ##  [97] "ERR12023442_2.fastq.gz"                                            
    ##  [98] "ERR12023443_1.fastq.gz"                                            
    ##  [99] "ERR12023443_2.fastq.gz"                                            
    ## [100] "ERR12023444_1.fastq.gz"                                            
    ## [101] "ERR12023444_2.fastq.gz"                                            
    ## [102] "ERR12023445_1.fastq.gz"                                            
    ## [103] "ERR12023445_2.fastq.gz"                                            
    ## [104] "ERR12023446_1.fastq.gz"                                            
    ## [105] "ERR12023446_2.fastq.gz"                                            
    ## [106] "ERR12023447_1.fastq.gz"                                            
    ## [107] "ERR12023447_2.fastq.gz"                                            
    ## [108] "ERR12023448_1.fastq.gz"                                            
    ## [109] "ERR12023448_2.fastq.gz"                                            
    ## [110] "ERR12023449_1.fastq.gz"                                            
    ## [111] "ERR12023449_2.fastq.gz"                                            
    ## [112] "ERR12023450_1.fastq.gz"                                            
    ## [113] "ERR12023450_2.fastq.gz"                                            
    ## [114] "ERR12023451_1.fastq.gz"                                            
    ## [115] "ERR12023451_2.fastq.gz"                                            
    ## [116] "ERR12023452_1.fastq.gz"                                            
    ## [117] "ERR12023452_2.fastq.gz"                                            
    ## [118] "ERR12023453_1.fastq.gz"                                            
    ## [119] "ERR12023453_2.fastq.gz"                                            
    ## [120] "ERR12023454_1.fastq.gz"                                            
    ## [121] "ERR12023454_2.fastq.gz"                                            
    ## [122] "ERR12023455_1.fastq.gz"                                            
    ## [123] "ERR12023455_2.fastq.gz"                                            
    ## [124] "ERR12023456_1.fastq.gz"                                            
    ## [125] "ERR12023456_2.fastq.gz"                                            
    ## [126] "ERR12023457_1.fastq.gz"                                            
    ## [127] "ERR12023457_2.fastq.gz"                                            
    ## [128] "ERR12023458_1.fastq.gz"                                            
    ## [129] "ERR12023458_2.fastq.gz"                                            
    ## [130] "ERR12023459_1.fastq.gz"                                            
    ## [131] "ERR12023459_2.fastq.gz"                                            
    ## [132] "ERR12023460_1.fastq.gz"                                            
    ## [133] "ERR12023460_2.fastq.gz"                                            
    ## [134] "ERR12023461_1.fastq.gz"                                            
    ## [135] "ERR12023461_2.fastq.gz"                                            
    ## [136] "ERR12023462_1.fastq.gz"                                            
    ## [137] "ERR12023462_2.fastq.gz"                                            
    ## [138] "ERR12023463_1.fastq.gz"                                            
    ## [139] "ERR12023463_2.fastq.gz"                                            
    ## [140] "ERR12023464_1.fastq.gz"                                            
    ## [141] "ERR12023464_2.fastq.gz"                                            
    ## [142] "ERR12023465_1.fastq.gz"                                            
    ## [143] "ERR12023465_2.fastq.gz"                                            
    ## [144] "ERR12023466_1.fastq.gz"                                            
    ## [145] "ERR12023466_2.fastq.gz"                                            
    ## [146] "ERR12023467_1.fastq.gz"                                            
    ## [147] "ERR12023467_2.fastq.gz"                                            
    ## [148] "ERR12023468_1.fastq.gz"                                            
    ## [149] "ERR12023468_2.fastq.gz"                                            
    ## [150] "ERR12023469_1.fastq.gz"                                            
    ## [151] "ERR12023469_2.fastq.gz"                                            
    ## [152] "ERR12023470_1.fastq.gz"                                            
    ## [153] "ERR12023470_2.fastq.gz"                                            
    ## [154] "ERR12023471_1.fastq.gz"                                            
    ## [155] "ERR12023471_2.fastq.gz"                                            
    ## [156] "ERR12023472_1.fastq.gz"                                            
    ## [157] "ERR12023472_2.fastq.gz"                                            
    ## [158] "ERR12023473_1.fastq.gz"                                            
    ## [159] "ERR12023473_2.fastq.gz"                                            
    ## [160] "ERR12023474_1.fastq.gz"                                            
    ## [161] "ERR12023474_2.fastq.gz"                                            
    ## [162] "ERR12023475_1.fastq.gz"                                            
    ## [163] "ERR12023475_2.fastq.gz"                                            
    ## [164] "ERR12023476_1.fastq.gz"                                            
    ## [165] "ERR12023476_2.fastq.gz"                                            
    ## [166] "ERR12023477_1.fastq.gz"                                            
    ## [167] "ERR12023477_2.fastq.gz"                                            
    ## [168] "ERR12023478_1.fastq.gz"                                            
    ## [169] "ERR12023478_2.fastq.gz"                                            
    ## [170] "ERR12023479_1.fastq.gz"                                            
    ## [171] "ERR12023479_2.fastq.gz"                                            
    ## [172] "ERR12023480_1.fastq.gz"                                            
    ## [173] "ERR12023480_2.fastq.gz"                                            
    ## [174] "ERR12023481_1.fastq.gz"                                            
    ## [175] "ERR12023481_2.fastq.gz"                                            
    ## [176] "ERR12023482_1.fastq.gz"                                            
    ## [177] "ERR12023482_2.fastq.gz"                                            
    ## [178] "ERR12023483_1.fastq.gz"                                            
    ## [179] "ERR12023483_2.fastq.gz"                                            
    ## [180] "ERR12023484_1.fastq.gz"                                            
    ## [181] "ERR12023484_2.fastq.gz"                                            
    ## [182] "ERR12023485_1.fastq.gz"                                            
    ## [183] "ERR12023485_2.fastq.gz"                                            
    ## [184] "ERR12023486_1.fastq.gz"                                            
    ## [185] "ERR12023486_2.fastq.gz"                                            
    ## [186] "ERR12023487_1.fastq.gz"                                            
    ## [187] "ERR12023487_2.fastq.gz"                                            
    ## [188] "ERR12023488_1.fastq.gz"                                            
    ## [189] "ERR12023488_2.fastq.gz"                                            
    ## [190] "ERR12023489_1.fastq.gz"                                            
    ## [191] "ERR12023489_2.fastq.gz"                                            
    ## [192] "ERR12023490_1.fastq.gz"                                            
    ## [193] "ERR12023490_2.fastq.gz"                                            
    ## [194] "ERR12023491_1.fastq.gz"                                            
    ## [195] "ERR12023491_2.fastq.gz"                                            
    ## [196] "ERR12023492_1.fastq.gz"                                            
    ## [197] "ERR12023492_2.fastq.gz"                                            
    ## [198] "ERR12023493_1.fastq.gz"                                            
    ## [199] "ERR12023493_2.fastq.gz"                                            
    ## [200] "ERR12023494_1.fastq.gz"                                            
    ## [201] "ERR12023494_2.fastq.gz"                                            
    ## [202] "ERR12023495_1.fastq.gz"                                            
    ## [203] "ERR12023495_2.fastq.gz"                                            
    ## [204] "ERR12023496_1.fastq.gz"                                            
    ## [205] "ERR12023496_2.fastq.gz"                                            
    ## [206] "ERR12023497_1.fastq.gz"                                            
    ## [207] "ERR12023497_2.fastq.gz"                                            
    ## [208] "ERR12023498_1.fastq.gz"                                            
    ## [209] "ERR12023498_2.fastq.gz"                                            
    ## [210] "ERR12023499_1.fastq.gz"                                            
    ## [211] "ERR12023499_2.fastq.gz"                                            
    ## [212] "ERR12023500_1.fastq.gz"                                            
    ## [213] "ERR12023500_2.fastq.gz"                                            
    ## [214] "ERR12023501_1.fastq.gz"                                            
    ## [215] "ERR12023501_2.fastq.gz"                                            
    ## [216] "ERR12023502_1.fastq.gz"                                            
    ## [217] "ERR12023502_2.fastq.gz"                                            
    ## [218] "ERR12023503_1.fastq.gz"                                            
    ## [219] "ERR12023503_2.fastq.gz"                                            
    ## [220] "ERR12023504_1.fastq.gz"                                            
    ## [221] "ERR12023504_2.fastq.gz"                                            
    ## [222] "ERR12023505_1.fastq.gz"                                            
    ## [223] "ERR12023505_2.fastq.gz"                                            
    ## [224] "ERR12023506_1.fastq.gz"                                            
    ## [225] "ERR12023506_2.fastq.gz"                                            
    ## [226] "ERR12023507_1.fastq.gz"                                            
    ## [227] "ERR12023507_2.fastq.gz"                                            
    ## [228] "ERR12023508_1.fastq.gz"                                            
    ## [229] "ERR12023508_2.fastq.gz"                                            
    ## [230] "ERR12023509_1.fastq.gz"                                            
    ## [231] "ERR12023509_2.fastq.gz"                                            
    ## [232] "ERR12023510_1.fastq.gz"                                            
    ## [233] "ERR12023510_2.fastq.gz"                                            
    ## [234] "ERR12023511_1.fastq.gz"                                            
    ## [235] "ERR12023511_2.fastq.gz"                                            
    ## [236] "ERR12023512_1.fastq.gz"                                            
    ## [237] "ERR12023512_2.fastq.gz"                                            
    ## [238] "ERR12023513_1.fastq.gz"                                            
    ## [239] "ERR12023513_2.fastq.gz"                                            
    ## [240] "ERR12023514_1.fastq.gz"                                            
    ## [241] "ERR12023514_2.fastq.gz"                                            
    ## [242] "ERR12023515_1.fastq.gz"                                            
    ## [243] "ERR12023515_2.fastq.gz"                                            
    ## [244] "ERR12023516_1.fastq.gz"                                            
    ## [245] "ERR12023516_2.fastq.gz"                                            
    ## [246] "ERR12023517_1.fastq.gz"                                            
    ## [247] "ERR12023517_2.fastq.gz"                                            
    ## [248] "ERR12023519_1.fastq.gz"                                            
    ## [249] "ERR12023519_2.fastq.gz"                                            
    ## [250] "ERR12023520_1.fastq.gz"                                            
    ## [251] "ERR12023520_2.fastq.gz"                                            
    ## [252] "ERR12023521_1.fastq.gz"                                            
    ## [253] "ERR12023521_2.fastq.gz"                                            
    ## [254] "ERR12023522_1.fastq.gz"                                            
    ## [255] "ERR12023522_2.fastq.gz"                                            
    ## [256] "ERR12023523_1.fastq.gz"                                            
    ## [257] "ERR12023523_2.fastq.gz"                                            
    ## [258] "ERR12023524_1.fastq.gz"                                            
    ## [259] "ERR12023524_2.fastq.gz"                                            
    ## [260] "ERR12023525_1.fastq.gz"                                            
    ## [261] "ERR12023525_2.fastq.gz"                                            
    ## [262] "ERR12023526_1.fastq.gz"                                            
    ## [263] "ERR12023526_2.fastq.gz"                                            
    ## [264] "ERR12023527_1.fastq.gz"                                            
    ## [265] "ERR12023527_2.fastq.gz"                                            
    ## [266] "ERR12023528_1.fastq.gz"                                            
    ## [267] "ERR12023528_2.fastq.gz"                                            
    ## [268] "ERR12023529_1.fastq.gz"                                            
    ## [269] "ERR12023529_2.fastq.gz"                                            
    ## [270] "ERR12023530_1.fastq.gz"                                            
    ## [271] "ERR12023530_2.fastq.gz"                                            
    ## [272] "ERR12023533_1.fastq.gz"                                            
    ## [273] "ERR12023533_2.fastq.gz"                                            
    ## [274] "ERR12023534_1.fastq.gz"                                            
    ## [275] "ERR12023534_2.fastq.gz"                                            
    ## [276] "ERR12023536_1.fastq.gz"                                            
    ## [277] "ERR12023536_2.fastq.gz"                                            
    ## [278] "ERR12023537_1.fastq.gz"                                            
    ## [279] "ERR12023537_2.fastq.gz"                                            
    ## [280] "ERR12023538_1.fastq.gz"                                            
    ## [281] "ERR12023538_2.fastq.gz"                                            
    ## [282] "ERR12023539_1.fastq.gz"                                            
    ## [283] "ERR12023539_2.fastq.gz"                                            
    ## [284] "ERR12023540_1.fastq.gz"                                            
    ## [285] "ERR12023540_2.fastq.gz"                                            
    ## [286] "ERR12023541_1.fastq.gz"                                            
    ## [287] "ERR12023541_2.fastq.gz"                                            
    ## [288] "ERR12023543_1.fastq.gz"                                            
    ## [289] "ERR12023543_2.fastq.gz"                                            
    ## [290] "ERR12023544_1.fastq.gz"                                            
    ## [291] "ERR12023544_2.fastq.gz"                                            
    ## [292] "ERR12023545_1.fastq.gz"                                            
    ## [293] "ERR12023545_2.fastq.gz"                                            
    ## [294] "ERR12023546_1.fastq.gz"                                            
    ## [295] "ERR12023546_2.fastq.gz"                                            
    ## [296] "ERR12023547_1.fastq.gz"                                            
    ## [297] "ERR12023547_2.fastq.gz"                                            
    ## [298] "ERR12023548_1.fastq.gz"                                            
    ## [299] "ERR12023548_2.fastq.gz"                                            
    ## [300] "ERR12023549_1.fastq.gz"                                            
    ## [301] "ERR12023549_2.fastq.gz"                                            
    ## [302] "ERR12023550_1.fastq.gz"                                            
    ## [303] "ERR12023550_2.fastq.gz"                                            
    ## [304] "ERR12023551_1.fastq.gz"                                            
    ## [305] "ERR12023551_2.fastq.gz"                                            
    ## [306] "ERR12023552_1.fastq.gz"                                            
    ## [307] "ERR12023552_2.fastq.gz"                                            
    ## [308] "ERR12023553_1.fastq.gz"                                            
    ## [309] "ERR12023553_2.fastq.gz"                                            
    ## [310] "ERR12023554_1.fastq.gz"                                            
    ## [311] "ERR12023554_2.fastq.gz"                                            
    ## [312] "ERR12023555_1.fastq.gz"                                            
    ## [313] "ERR12023555_2.fastq.gz"                                            
    ## [314] "filtered"

``` r
# Forward and reverse fastq filenames have format: SAMPLENAME_R1_001.fastq and SAMPLENAME_R2_001.fastq
fnFs <- sort(list.files(path, pattern="_1.fastq", full.names = TRUE))
fnRs <- sort(list.files(path, pattern="_2.fastq", full.names = TRUE))
# Extract sample names, assuming filenames have format: SAMPLENAME_XXX.fastq
sample.names <- sapply(strsplit(basename(fnFs), "_"), `[`, 1)
```

``` r
plotQualityProfile(fnFs[1:2])
```

![](AA_importation_données_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
#visualiser les profils de qualité des lectures
```

``` r
plotQualityProfile(fnRs[1:2])
```

![](AA_importation_données_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
#visualiser les profils de qualité des lectures inversées
```

``` r
# Place filtered files in filtered/ subdirectory
filtFs <- file.path(path, "filtered", paste0(sample.names, "_F_filt.fastq.gz"))
filtRs <- file.path(path, "filtered", paste0(sample.names, "_R_filt.fastq.gz"))
names(filtFs) <- sample.names
names(filtRs) <- sample.names
```

``` r
out <- filterAndTrim(fnFs, filtFs, fnRs, filtRs, truncLen=c(220,220),
              maxN=0, maxEE=c(2,2), truncQ=2, rm.phix=TRUE,
              compress=TRUE, multithread=FALSE) 
head(out)
```

    ##                        reads.in reads.out
    ## ERR12023394_1.fastq.gz   176017    156292
    ## ERR12023395_1.fastq.gz   155328    131205
    ## ERR12023396_1.fastq.gz   165204    144751
    ## ERR12023397_1.fastq.gz   156394    139950
    ## ERR12023398_1.fastq.gz   180940    159136
    ## ERR12023400_1.fastq.gz   211623    170554

``` r
##Le séquençage des régions V3-V4 du gène rRNA 16S a été effectué à l'aide des amorces universelles 341F 5' CCTACGGGNGGC WGCAG 3' et 785R 5' GAC TAC HVG GGT ATC TAA TCC 3' pour les échantillons de coraux, de sédiments et d'eau,
# somme read 1 read 2 pas moins de 444 + 20 ... 
```
