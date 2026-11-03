Analyse d’article
================
Madeleine GUILLAUME

### **Probiotics reshape the coral microbiome in situ without detectable off-target effects in the surrounding environment**

# Importation des données de l’étude

On importe la table de métadonnées de l’étude (projet PRJEB65896), qui
décrit chaque échantillon : organisme (corail, sédiment, eau ou blanc),
temps de prélèvement (T1 à T4) et traitement. Elle servira plus tard à
relier les séquences aux conditions expérimentales.

``` r
meta_data_table <- read.csv("SraRunTable.csv", sep = ",", header = TRUE)
  # importation de la table de métadonnée 
```

On charge dada2 et on indique le dossier contenant les séquences
téléchargées depuis l’ENA. Contrairement au jeu de test, ces données
viennent d’une vraie étude sur le microbiote de coraux traités avec un
probiotique.

``` r
library(dada2); packageVersion("dada2")
```

    ## Loading required package: Rcpp

    ## [1] '1.40.0'

``` r
path <- "/home/rstudio/ADM_Données_articles/data_article" 
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

On charge les packages pour manipuler les données de microbiome
(phyloseq), les séquences (Biostrings) et faire les graphiques
(ggplot2). On charge également tous les packages utiles, dont decontam
(détection des contaminants) et vegan (statistiques de communautés).

``` r
load("~/ADM_Données_articles/environnement_AA.RData")
library(phyloseq); library(Biostrings); library(ggplot2)
```

    ## Loading required package: BiocGenerics

    ## Loading required package: generics

    ## 
    ## Attaching package: 'generics'

    ## The following objects are masked from 'package:base':
    ## 
    ##     as.difftime, as.factor, as.ordered, intersect, is.element, setdiff,
    ##     setequal, union

    ## 
    ## Attaching package: 'BiocGenerics'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     IQR, mad, sd, var, xtabs

    ## The following objects are masked from 'package:base':
    ## 
    ##     anyDuplicated, aperm, append, as.data.frame, basename, cbind,
    ##     colnames, dirname, do.call, duplicated, eval, evalq, Filter, Find,
    ##     get, grep, grepl, is.unsorted, lapply, Map, mapply, match, mget,
    ##     order, paste, pmax, pmax.int, pmin, pmin.int, Position, rank,
    ##     rbind, Reduce, rownames, sapply, saveRDS, table, tapply, unique,
    ##     unsplit, which.max, which.min

    ## Loading required package: S4Vectors

    ## Loading required package: stats4

    ## 
    ## Attaching package: 'S4Vectors'

    ## The following object is masked from 'package:utils':
    ## 
    ##     findMatches

    ## The following objects are masked from 'package:base':
    ## 
    ##     expand.grid, I, unname

    ## Loading required package: IRanges

    ## 
    ## Attaching package: 'IRanges'

    ## The following object is masked from 'package:phyloseq':
    ## 
    ##     distance

    ## Loading required package: XVector

    ## Loading required package: Seqinfo

    ## 
    ## Attaching package: 'Biostrings'

    ## The following object is masked from 'package:base':
    ## 
    ##     strsplit

``` r
library(decontam); library(vegan)
```

    ## Loading required package: permute

``` r
theme_set(theme_bw())
```

# Pipeline DADA2

On sépare les lectures forward (\_1) et reverse (\_2), triées dans le
même ordre pour que chaque paire corresponde. Les noms d’échantillons
sont extraits des noms de fichiers. Les motifs de nom sont différents de
ceux du jeu de test et ont donc dû être adaptés à nos fichiers.

``` r
fnFs <- sort(list.files(path, pattern="_1.fastq", full.names = TRUE))
fnRs <- sort(list.files(path, pattern="_2.fastq", full.names = TRUE))

sample.names <- sapply(strsplit(basename(fnFs), "_"), `[`, 1)
```

On regarde la qualité des lectures forward puis reverse sur deux
échantillons. Cela permet de voir à partir de quelle position la qualité
chute et de choisir où couper.

``` r
plotQualityProfile(fnFs[1:2])
```

![](Analyse_article_MadeleineGUILLAUME_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
#visualiser les profils de qualité des lectures
```

``` r
plotQualityProfile(fnRs[1:2])
```

![](Analyse_article_MadeleineGUILLAUME_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
#visualiser les profils de qualité des lectures inversées
```

Les profils de qualité des deux premiers échantillons montrent une
qualité médiane élevée (entre 33 et 37) sur toute la longueur des
lectures, en forward comme en reverse. Il n’y a pas de chute progressive
en fin de lecture. Seuls quelques cycles isolés présentent une baisse
brutale, que le filtrage par erreurs attendues élimine. Les lectures
sont donc de bonne qualité. Le choix de l’emplacement du tronquage
dépend surtout du chevauchement nécessaire entre forward et reverse pour
la région V3-V4.

On prépare ensuite, hles noms des futurs fichiers filtrés, un par
échantillon et par sens de lecture.

``` r
filtFs <- file.path(path, "filtered", paste0(sample.names, "_F_filt.fastq.gz"))
filtRs <- file.path(path, "filtered", paste0(sample.names, "_R_filt.fastq.gz"))
names(filtFs) <- sample.names
names(filtRs) <- sample.names
```

On nettoie les lectures : on coupe les extrémités de mauvaise qualité et
on élimine les séquences ambiguës, trop sujettes aux erreurs ou issues
du PhiX. La longueur de troncature est adaptée à nos données : la région
séquencée (V3-V4 du gène 16S) est plus longue que celle du jeu de test,
donc les lectures forward et reverse doivent encore se chevaucher après
la coupe pour pouvoir être fusionnées.

PS : Les étapes de calcul lourd (filtrage, débruitage, chimères,
taxonomie) ne sont pas réexécutées lors du knit : leurs résultats ont
été sauvegardés au préalable.

``` r
out <- filterAndTrim(fnFs, filtFs, fnRs, filtRs, truncLen=c(220,220),
              maxN=0,maxEE=c(2,2),truncQ=2,rm.phix=TRUE,
              compress=TRUE,multithread=FALSE) 
```

``` r
head(out)
```

    ##                        reads.in reads.out
    ## ERR12023394_1.fastq.gz   176017    156292
    ## ERR12023395_1.fastq.gz   155328    131205
    ## ERR12023396_1.fastq.gz   165204    144751
    ## ERR12023397_1.fastq.gz   156394    139950
    ## ERR12023398_1.fastq.gz   180940    159136
    ## ERR12023400_1.fastq.gz   211623    170554

On apprend le taux d’erreur de séquençage propre à nos données,
séparément pour les lectures forward et reverse, puis on vérifie
graphiquement que le modèle suit bien les erreurs observées.

``` r
errF <- learnErrors(filtFs, multithread=TRUE)
```

``` r
errR <- learnErrors(filtRs, multithread=TRUE)
```

``` r
plotErrors(errF, nominalQ=TRUE)
```

    ## Warning in scale_y_log10(): log-10 transformation introduced infinite values.

    ## Warning: Removed 164 rows containing missing values or values outside the scale range
    ## (`geom_line()`).
    ## Removed 164 rows containing missing values or values outside the scale range
    ## (`geom_line()`).

![](Analyse_article_MadeleineGUILLAUME_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

On débruite les lectures : DADA2 corrige les erreurs de séquençage et ne
garde que les vraies séquences biologiques (ASV). On affiche le résultat
du premier échantillon pour voir combien de séquences uniques de départ
se réduisent à de vraies ASV.

``` r
dadaFs <- dada(filtFs, err=errF, multithread=TRUE)
```

``` r
dadaRs <- dada(filtRs, err=errR, multithread=TRUE)
```

``` r
dadaFs[[1]]
```

    ## dada-class: object describing DADA2 denoising results
    ## 713 sequence variants were inferred from 38431 input unique sequences.
    ## Key parameters: OMEGA_A = 1e-40, OMEGA_C = 1e-40, BAND_SIZE = 16

On fusionne chaque lecture forward avec sa reverse grâce à leur zone de
chevauchement, pour reconstituer la séquence complète de la région
amplifiée.

``` r
mergers <- mergePairs(dadaFs, filtFs, dadaRs, filtRs, verbose=TRUE)
```

``` r
head(mergers[[1]])
```

    ##                                                                                                                                                                                                                                                                                                                                                                                                                                      sequence
    ## 1 TGTTTGCTCCCCACGCTTTCGCGCCTCAGTGTCAGTTGCAGACCAGGAAGCCGCCTTCGCCACTGGTGTTCCTCCATATCTCTACGCATTTCACCGCTACACATGGAATTCCACTTCCCTCTTCTGCACTCAAGTCAACCAGTTTCCAATGACCCTCCACGGTTAAGCCGTGGGCTTTCACATCAGACTTAATTAACCACCTGCGCGCTCTTTACGCCCAATAATTCCGGATAACGCTTGCCACCTACGTATTACCGCGGCTGCTGGCACGTAGTTAGCCGTGGCTTTCTCATAAGGTACCGTCAATTGATAGTCATTTCCTCCTATCACCTTTCTTCCCTTATAACAGAATTTTACAACCCGAAGGCCTTCTTCATTCACGCGGCGTTGCTCGGTCAGGCTTTCGCCCATTGCCGAAGATTCCCTA
    ## 2                           TGTTTGCTCCCCACGCTTTCGTGCCTCAGTGTCAGTTACAGTCCAGAGAGCCGCCTTCGCAACTGGTGTTCCTCCTAATATCTACGCATTTCACCGCTACACTAGGAATTCCACTCTCCTCTCCTGCACTCAAGTTCCCCAGTTTCAAAGGCTTACTACGGTTGAGCCGTAGCCTTTCACCTCTGACTTAAGAAACCACCTACGCACCCTTTACGCCCAGTAATTCCGGATAACGCTAGCCCCCTACGTATTACCGCGGCTGCTGGCACGTAGTTAGCCGGGGCTTCCTCCTCAAGTACCGTCATTATCTTCCTTGAGGACAGAGCTTTACGACCCGAAGGCCTTCATCGCTCACGCGGCGTTGCTGCATCAGGCTTTCGCCCATTGTGCAATATTCCCCA
    ## 3                           TGTTTGCTCCCCACGCTTTCGTGCCTCAGTGTCAGTTACAGTCCAGAGAGCCGCCTTCGCAACTGGTGTTCCTCCTAATATCTACGCATTTCACCGCTACACTAGGAATTCCACTCTCCTCTCCTGCACTCAAGTTTCCCAGTTTCAAAGGCTTACTACGGTTGAGCCGTAGCCTTTCACCTCTGACTTAAGAAACCACCTACGCACCCTTTACGCCCAGTAATTCCGGATAACGCTAGCCCCCTACGTATTACCGCGGCTGCTGGCACGTAGTTAGCCGGGGCTTCCTCCTCAAGTACCGTCATTATCTTCCTTGAGGACAGAGCTTTACGACCCGAAGGCCTTCATCGCTCACGCGGCGTTGCTGCATCAGGCTTTCGCCCATTGTGCAATATTCCCCA
    ## 4 TGTTCGCTACCCATGCTTTCGAGCCTCAGCGTCAGTTACAGACCAGAGAGCCGCCTTCGCCACTGGTGTTCTTCCATATATCTACGCATTCCACCGCTACACATGGAGTTCCACTCTCCTCTTCTGCACTCAAGAAACACAGTTTCCGATGCAATTCCTCGGTTAAGCCGGGGGCTTTCACATCAGACTTATCTTTCCGCCTGCGCTCGCTTTACGCCCAATAAATCCGGACAACGCTTGCCACCTACGTATTACCGCGGCTGCTGGCACGTAGTTAGCCGTGACTTTCTGGTTGATTACCGTCAAATAGGGGCCAGTTACTACCTCTATCCTTCTTCACCAACAACAGAGCTTTACGATCCGAAAACCTTCTTCACTCACGCGGCGTTGCTCCATCAGGCTTTCGCCCATTGTGGAAGATTCCCTA
    ## 5  TATTTGCTCCCCACGCTTTCGTGCCTCAGCGTCAGTAACAGGCCAGGCGGCCGCCTTCGCCACTGGTGTTCTTCCATATATCTACGCATTTTACCGCTACACATGGAGTTCCACCGCCCTCTCCTGTCCTCCAGCCTGCCAGTTTCCAAAGCCTGTACCGGTTGAGCCGGTACCTTTCACTTCAGACTTAACAGGCCGCCTACGCACCCTTTACGCCCAATCATTCCGGATAACGCTCGCCACCTACGTATTACCGCGGCTGCTGGCACGTAGTTAGCCGTGACTTCCTCGAAAGGTAATATCACTTCACCAGCATTTCCTCTGGTGACCCTTTTTCCCTCTCAACAGAACTTTACGATCCGAAGACCTTCCTCGTTCACGCGGCATTGCTCGTTCAGGCTTGCGCCCATTGACGAAAATTCCCTA
    ## 6                          TGTTTGCTCCCCACGCTTTCGAGCCTCAGCGTCAGTTACAGTCCAGAGAATCGCCTTCGCCACTGGTGTTCTTCCTAATCTCTACGCATTTCACCGCTACACTAGGAATTCCATTCTCCTCTCCTGCACTCTAGACTTCCAGTTTGAAATGCAGCACTCAAGTTGAGCCCGAGTATTTCACATCTCACTTAAAAGTCCGCCTACGCTCCCTTTACGCCCAGTAAATCCGGACAACGCTCGCCACCTACGTATTACCGCGGCTGCTGGCACGTAGTTAGCCGTGGCTTCCTCCTCAGGTACCGTCATTATCGTCCCTGAAGACAGAGCTTTACAACCCGAAGGCCGTCATCACTCACGCGGCGTTGCTGCATCAGGGTTTCCCCCATTGTGCAATATTCCCCA
    ##   abundance forward reverse nmatch nmismatch nindel prefer accept
    ## 1     27874       1       2     13         0      0      1   TRUE
    ## 2     23221       2       1     39         0      0      2   TRUE
    ## 3     11317       3       1     39         0      0      2   TRUE
    ## 4      9007       4       3     13         0      0      1   TRUE
    ## 5      4191       6       4     14         0      0      1   TRUE
    ## 6      3043       7       5     38         0      0      1   TRUE

Le modèle d’erreur est appris sur environ 126 millions de bases (572 198
lectures issues de 4 échantillons), ce qui est largement suffisant. Sur
le graphique, le taux d’erreur diminue quand la qualité augmente, comme
attendu. La courbe noire (modèle) suit globalement les points observés.
Les avertissements affichés viennent de l’échelle logarithmique et sont
sans gravité. Les cadres vides correspondent à des substitutions
impossibles (par exemple A vers A). Le modèle est donc valide pour le
débruitage.

On construit la *table d’ASV* (échantillons en lignes, ASV en colonnes,
nombre de lectures en valeurs). On vérifie ensuite que les longueurs des
séquences correspondent à celles attendues pour la région V3-V4.

``` r
seqtab <- makeSequenceTable(mergers)
dim(seqtab)
```

    ## [1]    156 329450

``` r
table(nchar(getSequences(seqtab)))
```

    ## 
    ##    220    221    222    223    224    225    226    227    228    229    230 
    ##     76    159    158    159    146    163    154    181    197    182    213 
    ##    231    232    233    234    235    236    237    238    239    240    241 
    ##    144    153    143    159    181    132    150    146    123    105     82 
    ##    242    243    244    245    246    247    248    249    250    251    252 
    ##     51     47     41     45     37     38     40     60     69     68     73 
    ##    253    254    255    256    257    258    259    260    261    262    263 
    ##     76     75     92     80     94     83    115    299    129    105    101 
    ##    264    265    266    267    268    269    270    271    272    273    274 
    ##    109     93    103    105     97     94     99     78     68     77     66 
    ##    275    276    277    278    279    280    281    282    283    284    285 
    ##     54     74     90    102     87    125    120    125    108     91    100 
    ##    286    287    288    289    290    291    292    293    294    295    296 
    ##    124    142    139    134    130     88     44     15     25     24     25 
    ##    297    298    299    300    301    302    303    304    305    306    307 
    ##     14     17     19     24     28     25     45     52     32     25     27 
    ##    308    309    310    311    312    313    314    315    316    317    318 
    ##     30     35     26     17     25     23     26     46     25     35     31 
    ##    319    320    321    322    323    324    325    326    327    328    329 
    ##     22     28     36     27     27     24     21     23     25     15     19 
    ##    330    331    332    333    334    335    336    337    338    339    340 
    ##     21     23     22     20     19     25     21     24     18     27     14 
    ##    341    342    343    344    345    346    347    348    349    350    351 
    ##     22     26     23     12     19      8     14     18      8     15      9 
    ##    352    353    354    355    356    357    358    359    360    361    362 
    ##      9     13      8     23     21     26     17     23     16     16     19 
    ##    363    364    365    366    367    368    369    370    371    372    373 
    ##     19     22     18     12     23     23     15     16     14     34     26 
    ##    374    375    376    377    378    379    380    381    382    383    384 
    ##     16     10     45     10     19     22     27     12     29     89    209 
    ##    385    386    387    388    389    390    391    392    393    394    395 
    ##    368    100     43     23     24     58     26     23     33     56     15 
    ##    396    397    398    399    400    401    402    403    404    405    406 
    ##     13     21     38     61    171   2856  22941   5752   5727   4189   1208 
    ##    407    408    409    410    411    412    413    414    415    416    417 
    ##   1686    544    266    147    116    129    119    292     94    483    344 
    ##    418    419    420    421    422    423    424    425    426    427    428 
    ##    439    734   1968   4731  19377   2426   7466   1040  11022 169047  52952

On retire les séquences chimériques, c’est-à-dire des séquences
artificielles formées pendant la PCR par le recollage de deux séquences
différentes. On vérifie aussi quelle part des lectures est conservée :
une valeur proche de 1 indique que ces chimères représentent peu de
lectures.

``` r
seqtab.nochim <- removeBimeraDenovo(seqtab, method="consensus", multithread=TRUE, verbose=TRUE)
```

``` r
dim(seqtab.nochim)
```

    ## [1]    156 108818

``` r
sum(seqtab.nochim)/sum(seqtab)
```

    ## [1] 0.8690942

Le débruitage porte sur 156 échantillons. Pour le premier, DADA2 passe
de 38 431 séquences uniques à 713 variants réels. La grande majorité des
séquences uniques étaient donc des erreurs de séquençage. Le nombre de
séquences uniques varie beaucoup selon les échantillons, de 10 000 à
plus de 100 000. Certains échantillons sont donc nettement plus bruités
ou plus diversifiés. La fusion des lectures forward et reverse
reconstitue la séquence complète de la région amplifiée.

Après fusion, la table contient 156 échantillons et 329 450 ASV. La
majorité des séquences font 427 pb, ce qui correspond à la région V3-V4
attendue. On observe aussi des pics secondaires vers 402-405 pb et 422
pb, probablement dus à d’autres groupes bactériens ou à des séquences
d’organites. Les séquences très courtes (moins de 300 pb) sont rares.

Après retrait des chimères, il reste 108 818 ASV, soit environ deux
tiers de moins. Ces chimères ne représentent pourtant que 13 % des
lectures, car 86,9 % sont conservées. Ce sont donc surtout des variants
rares.

On suit le nombre de lectures conservées à chaque étape du pipeline,
pour repérer une étape où l’on perd trop de données.

``` r
getN <- function(x) sum(getUniques(x))
track <- cbind(out, sapply(dadaFs, getN), sapply(dadaRs, getN), sapply(mergers, getN), rowSums(seqtab.nochim))
colnames(track) <- c("input", "filtered", "denoisedF", "denoisedR", "merged", "nonchim")
rownames(track) <- sample.names
head(track)
```

    ##              input filtered denoisedF denoisedR merged nonchim
    ## ERR12023394 176017   156292    153199    153435 146847  116614
    ## ERR12023395 155328   131205    116271    115210  99153   87926
    ## ERR12023396 165204   144751    135427    134652 126585  111926
    ## ERR12023397 156394   139950    134139    134537 127002  103677
    ## ERR12023398 180940   159136    152799    152490 143272  109454
    ## ERR12023400 211623   170554    167193    167300 161219  155843

Le tableau de suivi montre le nombre de lectures conservées à chaque
étape. Le filtrage retire 10 à 20 % des lectures. Le débruitage, la
fusion et le retrait des chimères en retirent encore un peu. La part de
lectures conservées jusqu’à la table finale est de 72,8 % en médiane (de
44,3 % à 84,1 %). C’est une perte acceptable, avec quelques échantillons
plus touchés, comme le deuxième (environ 57 %).

# Assignation taxonomique

On attribue à chaque ASV un nom de bactérie (du règne au genre) en le
comparant à la base de référence SILVA. Les premières ASV, les plus
abondantes, sont assignées à la famille des Endozoicomonadaceae (genre
Endozoicomonas), au sein des Gammaproteobacteria. Ce sont des bactéries
typiques du microbiote des coraux, ce qui est cohérent avec les
échantillons. Une ASV reste sans classification dès le phylum. Les noms
d’échantillons (ERR12023394 à ERR12023555) correspondent bien à la table
de métadonnées.

``` r
taxa <- assignTaxonomy(seqtab.nochim,
                       "/home/rstudio/ADM_Données_articles/silva_nr99_v138.1_train_set.fa.gz",
                       multithread = TRUE)
```

On vérifie les noms d’échantillons présents dans la table, puis on
résume le pourcentage de lectures qui traversent tout le pipeline
jusqu’à la table finale. Cela permet de juger si la perte globale est
acceptable.

``` r
taxa.print <- taxa
rownames(taxa.print) <- NULL
head(taxa.print)
```

    ##      Kingdom    Phylum           Class                 Order            
    ## [1,] "Bacteria" "Proteobacteria" "Gammaproteobacteria" "Pseudomonadales"
    ## [2,] "Bacteria" NA               NA                    NA               
    ## [3,] "Bacteria" "Proteobacteria" "Gammaproteobacteria" "Pseudomonadales"
    ## [4,] "Bacteria" "Proteobacteria" "Gammaproteobacteria" "Pseudomonadales"
    ## [5,] "Bacteria" "Proteobacteria" "Gammaproteobacteria" "Pseudomonadales"
    ## [6,] "Bacteria" "Proteobacteria" "Gammaproteobacteria" "Pseudomonadales"
    ##      Family                Genus           
    ## [1,] "Endozoicomonadaceae" "Endozoicomonas"
    ## [2,] NA                    NA              
    ## [3,] "Endozoicomonadaceae" "Endozoicomonas"
    ## [4,] "Endozoicomonadaceae" "Endozoicomonas"
    ## [5,] "Endozoicomonadaceae" "Endozoicomonas"
    ## [6,] "Endozoicomonadaceae" "Endozoicomonas"

``` r
rownames(seqtab.nochim)
```

    ##   [1] "ERR12023394" "ERR12023395" "ERR12023396" "ERR12023397" "ERR12023398"
    ##   [6] "ERR12023400" "ERR12023401" "ERR12023402" "ERR12023403" "ERR12023404"
    ##  [11] "ERR12023405" "ERR12023406" "ERR12023407" "ERR12023408" "ERR12023409"
    ##  [16] "ERR12023410" "ERR12023411" "ERR12023412" "ERR12023413" "ERR12023414"
    ##  [21] "ERR12023415" "ERR12023416" "ERR12023417" "ERR12023418" "ERR12023419"
    ##  [26] "ERR12023420" "ERR12023421" "ERR12023422" "ERR12023423" "ERR12023424"
    ##  [31] "ERR12023425" "ERR12023426" "ERR12023427" "ERR12023428" "ERR12023429"
    ##  [36] "ERR12023430" "ERR12023431" "ERR12023432" "ERR12023433" "ERR12023434"
    ##  [41] "ERR12023435" "ERR12023436" "ERR12023437" "ERR12023438" "ERR12023439"
    ##  [46] "ERR12023440" "ERR12023441" "ERR12023442" "ERR12023443" "ERR12023444"
    ##  [51] "ERR12023445" "ERR12023446" "ERR12023447" "ERR12023448" "ERR12023449"
    ##  [56] "ERR12023450" "ERR12023451" "ERR12023452" "ERR12023453" "ERR12023454"
    ##  [61] "ERR12023455" "ERR12023456" "ERR12023457" "ERR12023458" "ERR12023459"
    ##  [66] "ERR12023460" "ERR12023461" "ERR12023462" "ERR12023463" "ERR12023464"
    ##  [71] "ERR12023465" "ERR12023466" "ERR12023467" "ERR12023468" "ERR12023469"
    ##  [76] "ERR12023470" "ERR12023471" "ERR12023472" "ERR12023473" "ERR12023474"
    ##  [81] "ERR12023475" "ERR12023476" "ERR12023477" "ERR12023478" "ERR12023479"
    ##  [86] "ERR12023480" "ERR12023481" "ERR12023482" "ERR12023483" "ERR12023484"
    ##  [91] "ERR12023485" "ERR12023486" "ERR12023487" "ERR12023488" "ERR12023489"
    ##  [96] "ERR12023490" "ERR12023491" "ERR12023492" "ERR12023493" "ERR12023494"
    ## [101] "ERR12023495" "ERR12023496" "ERR12023497" "ERR12023498" "ERR12023499"
    ## [106] "ERR12023500" "ERR12023501" "ERR12023502" "ERR12023503" "ERR12023504"
    ## [111] "ERR12023505" "ERR12023506" "ERR12023507" "ERR12023508" "ERR12023509"
    ## [116] "ERR12023510" "ERR12023511" "ERR12023512" "ERR12023513" "ERR12023514"
    ## [121] "ERR12023515" "ERR12023516" "ERR12023517" "ERR12023519" "ERR12023520"
    ## [126] "ERR12023521" "ERR12023522" "ERR12023523" "ERR12023524" "ERR12023525"
    ## [131] "ERR12023526" "ERR12023527" "ERR12023528" "ERR12023529" "ERR12023530"
    ## [136] "ERR12023533" "ERR12023534" "ERR12023536" "ERR12023537" "ERR12023538"
    ## [141] "ERR12023539" "ERR12023540" "ERR12023541" "ERR12023543" "ERR12023544"
    ## [146] "ERR12023545" "ERR12023546" "ERR12023547" "ERR12023548" "ERR12023549"
    ## [151] "ERR12023550" "ERR12023551" "ERR12023552" "ERR12023553" "ERR12023554"
    ## [156] "ERR12023555"

``` r
summary(round(100 * track[, "nonchim"] / track[, "input"], 1))
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   44.30   65.35   72.80   70.55   79.12   84.10

# Préparation des données pour l’analyse

On aligne la table de métadonnées sur la table d’ASV, en retrouvant la
colonne qui contient les bons identifiants d’échantillons, et on vérifie
que l’ordre est identique. On crée aussi deux variables :

• le traitement (probiotique ou placebo, d’après le nom de
l’échantillon) • l’indicateur des blancs négatifs

Ces deux variables serviront notamment à repérer les contaminations.

``` r
silva <- "/home/rstudio/ADM_Données_articles/silva_nr99_v138.1_train_set.fa.gz"

id_col <- names(which.max(sapply(meta_data_table,
                function(x) sum(rownames(seqtab.nochim) %in% x))))
samdf <- as.data.frame(meta_data_table)
samdf <- samdf[samdf[[id_col]] %in% rownames(seqtab.nochim), ]
rownames(samdf) <- samdf[[id_col]]
samdf <- samdf[rownames(seqtab.nochim), , drop = FALSE]
stopifnot(all(rownames(samdf) == rownames(seqtab.nochim)))

samdf$Treatment <- ifelse(grepl("BMC", samdf$Sample_name), "Probiotic",
                   ifelse(grepl("CTRL", samdf$Sample_name), "Placebo", NA))
samdf$is.neg <- samdf$Organism == "blank sample"
```

On sauvegarde l’environnement par sécurité, puis on supprime les gros
objets devenus inutiles pour libérer de la mémoire et faciliter
l’enregistrement.

``` r
save.image("~/ADM_Données_articles/environnement_AA.RData")   

rm(dadaFs, dadaRs, mergers, errF, errR)
gc()
```

# Correction des séquences en sens inverse

Un grand nombre d’ASV n’étaient pas classées par la base SILVA. Notre
hypothèse est que certaines séquences sont orientées en sens inverse. On
teste donc l’assignation après les avoir retournées, d’abord sur les 200
ASV non classées les plus abondantes, puis sur un plus grand nombre.

Les 200 ASV non classées les plus abondantes représentent 29,1 % des
lectures. Après retournement, 199 sur 200 sont classées, surtout en
Proteobacteria (101), Firmicutes (30) et Cyanobacteria (24). L’hypothèse
d’une orientation inversée est donc confirmée. La correction est ensuite
appliquée aux 2 000 ASV non classées les plus abondantes (32,3 % des
lectures) : 1 992 sont reclassées. Les séquences retournées sont
réintégrées dans la table d’ASV et dans la taxonomie.

``` r
load("~/ADM_Données_articles/environnement_AA.RData")
rm(list = intersect(c("dadaFs", "dadaRs", "mergers", "errF", "errR", "seqtab"), ls()))
gc()
library(dada2)
silva <- "/home/rstudio/ADM_Données_articles/silva_nr99_v138.1_train_set.fa.gz"
taxa  <- taxa[colnames(seqtab.nochim), , drop = FALSE]

cand <- rownames(taxa)[is.na(taxa[, "Phylum"])]
ab   <- sort(colSums(seqtab.nochim[, cand]), decreasing = TRUE)
top  <- names(ab)[1:200]

cat("Part des lectures couverte par ces 200 ASV :",
    round(100 * sum(ab[1:200]) / sum(seqtab.nochim), 1), "%\n")

t200 <- assignTaxonomy(dada2::rc(top), silva, multithread = 2)
cat("ASV classées après retournement :", sum(!is.na(t200[, "Phylum"])), "sur 200\n")
sort(table(t200[, "Phylum"], useNA = "ifany"), decreasing = TRUE)[1:5]
```

``` r
n <- min(2000, length(ab))
top <- names(ab)[1:n]
cat("Part des lectures couverte :", round(100 * sum(ab[1:n]) / sum(seqtab.nochim), 1), "%\n")

top.rc  <- dada2::rc(top)
taxa.rc <- assignTaxonomy(top.rc, silva, multithread = 2)
rownames(taxa.rc) <- top.rc
ok  <- !is.na(taxa.rc[, "Phylum"])
sel <- top[ok]                    
cat("ASV reclassées après retournement :", length(sel), "\n")

keep <- seqtab.nochim[, !(colnames(seqtab.nochim) %in% sel), drop = FALSE]
flip <- seqtab.nochim[,  colnames(seqtab.nochim) %in% sel,  drop = FALSE]
colnames(flip) <- dada2::rc(colnames(flip))
flip <- t(rowsum(t(flip), group = colnames(flip)))        # fusionne les doublons

new_taxa <- taxa.rc[dada2::rc(sel), , drop = FALSE]
new_taxa <- new_taxa[!duplicated(rownames(new_taxa)), , drop = FALSE]

common <- intersect(colnames(flip), colnames(keep))
if (length(common) > 0) keep[, common] <- keep[, common] + flip[, common]
new <- setdiff(colnames(flip), colnames(keep))

seqtab.fix <- cbind(keep, flip[, new, drop = FALSE])
taxa.fix   <- rbind(taxa[colnames(keep), , drop = FALSE], new_taxa[new, , drop = FALSE])
taxa.fix   <- taxa.fix[colnames(seqtab.fix), , drop = FALSE]

stopifnot(identical(rownames(taxa.fix), colnames(seqtab.fix)))
dim(seqtab.nochim); dim(seqtab.fix)
round(100 * colMeans(is.na(taxa.fix)), 1)
```

``` r
dim(seqtab.nochim); dim(seqtab.fix)
```

    ## [1]    156 108818

    ## [1]    156 106900

Le nombre d’échantillons reste identique (156) et aucune lecture n’est
perdue, car les comptages des ASV retournées sont additionnés. Le nombre
d’ASV passe de 108 818 à 106 900. Sur les 1 992 ASV retournées, 1 918
correspondaient en réalité à une séquence déjà présente dans le bon sens
: elles ont été fusionnées avec leur équivalent, et seules 74 sont de
nouvelles ASV. Une partie de la diversité apparente venait donc d’une
même séquence comptée deux fois, dans les deux orientations.

``` r
round(100 * colMeans(is.na(taxa.fix)), 1)
```

    ## Kingdom  Phylum   Class   Order  Family   Genus 
    ##     8.5    44.0    48.4    55.2    64.0    79.0

La part d’ASV non classées augmente à chaque rang taxonomique : 8,5 % au
règne, 44 % au phylum, 48,4 % à la classe, 55,2 % à l’ordre, 64 % à la
famille et 79 % au genre. Le règne est presque toujours connu, mais près
de la moitié des ASV restent sans phylum, surtout des ASV rares qui
n’ont pas été traitées par la correction. Ce taux de séquences non
classées est fréquent pour des communautés marines, mal représentées
dans les bases de référence.

# Construction de l’objet phyloseq et nettoyage

On regroupe la table d’ASV, les métadonnées et la taxonomie dans un seul
objet. On retire ensuite ce qui ne correspond pas au microbiote du
corail : les séquences de chloroplastes et de mitochondries, qui
viennent de l’hôte ou des algues. On renomme les ASV pour plus de
lisibilité.

On identifie enfin les contaminants grâce aux blancs négatifs
(échantillons sans matériel biologique). L’analyse en trouve 903 sur 103
891 ASV, soit moins de 1 %. Ils sont supprimés, ainsi que les blancs
eux-mêmes et les ASV devenues absentes.

L’objet final contient 97 914 ASV et 149 échantillons :

- 114 coraux (58 placebo, 56 probiotique)

- 16 sédiments (7 placebo, 9 probiotique)

- 19 échantillons d’eau (10 placebo, 9 probiotique)

Les groupes sont équilibrés entre traitements.

``` r
ps <- phyloseq(otu_table(seqtab.fix, taxa_are_rows = FALSE),
               sample_data(samdf), tax_table(taxa.fix))

ps <- subset_taxa(ps, is.na(Order)  | Order  != "Chloroplast")
ps <- subset_taxa(ps, is.na(Family) | Family != "Mitochondria")

dna <- DNAStringSet(taxa_names(ps)); names(dna) <- taxa_names(ps)
ps  <- merge_phyloseq(ps, dna)
taxa_names(ps) <- paste0("ASV", seq(ntaxa(ps)))

# Contaminants (blancs négatifs), puis retrait des blancs
contamdf <- isContaminant(ps, method = "prevalence", neg = "is.neg")
table(contamdf$contaminant)
ps <- prune_taxa(!contamdf$contaminant, ps)
ps <- subset_samples(ps, Organism != "blank sample")
ps <- prune_taxa(taxa_sums(ps) > 0, ps)
ps
table(sample_data(ps)$Organism, sample_data(ps)$Treatment, useNA = "ifany")
```

``` r
table(contamdf$contaminant)
```

    ## 
    ##  FALSE   TRUE 
    ## 102938    904

``` r
ps
```

    ## phyloseq-class experiment-level object
    ## otu_table()   OTU Table:         [ 97878 taxa and 149 samples ]
    ## sample_data() Sample Data:       [ 149 samples by 44 sample variables ]
    ## tax_table()   Taxonomy Table:    [ 97878 taxa by 6 taxonomic ranks ]
    ## refseq()      DNAStringSet:      [ 97878 reference sequences ]

``` r
table(sample_data(ps)$Organism, sample_data(ps)$Treatment, useNA = "ifany")
```

    ##                             
    ##                              Placebo Probiotic
    ##   coral metagenome                58        56
    ##   marine sediment metagenome       7         9
    ##   seawater metagenome             10         9

On prépare les différentes versions de l’objet selon l’analyse :

- comptages bruts des coraux pour la diversité alpha

- version filtrée (ASV présentes dans au moins 3 échantillons),
  transformée en abondances relatives, pour les comparaisons de
  composition

- sous-ensemble des coraux à T3, le temps où l’effet du traitement est
  étudié

``` r
ps.coral.raw <- subset_samples(ps, Organism == "coral metagenome")      
ps.filt <- filter_taxa(ps, function(x) sum(x > 0) >= 3, TRUE)          
ps.prop <- transform_sample_counts(ps.filt, function(x) x / sum(x))
ps.coral <- subset_samples(ps.prop, Organism == "coral metagenome")
ps.T3    <- subset_samples(ps.coral, Sampling_time == "T3")
```

# Diversité alpha

On compare la diversité au sein de chaque échantillon (indices de
Shannon et Simpson). Pour les coraux, on compare ensuite probiotique et
placebo à chaque temps avec un test de Wilcoxon, adapté aux petits
effectifs et aux données non normales.

Sur le premier graphique, les trois types d’échantillons se distinguent
nettement. Les sédiments ont la diversité de Shannon la plus élevée
(environ 7,5 à 8). L’eau de mer est intermédiaire (environ 4 à 5,5). Les
coraux sont les moins divers (environ 2 à 4), ce qui est cohérent avec
un microbiote dominé par quelques taxons, comme Endozoicomonas. L’indice
de Simpson, proche de 1 pour tous les groupes, sature et distingue moins
bien les échantillons.

Pour les coraux seuls, la diversité de Shannon ne diffère pas entre
probiotique et placebo à T1 (p = 0,73), T2 (p = 0,62) et T4 (p = 0,65).
À T3, elle est significativement plus élevée chez les coraux traités (p
= 0,0009). Ce résultat reste significatif après correction pour les 4
comparaisons (seuil de 0,0125). Sur le graphique, il vient surtout de
quelques coraux traités très divers (valeurs de 4 à plus de 6), alors
que la majorité des points des deux groupes restent entre 2 et 3,5.
L’effet est donc ponctuel (T3 uniquement) et porté par une partie des
coraux traités.

``` r
plot_richness(ps, x = "Sampling_time", measures = c("Shannon", "Simpson"), color = "Organism")
```

    ## Warning: `aes_string()` was deprecated in ggplot2 3.0.0.
    ## ℹ Please use tidy evaluation idioms with `aes()`.
    ## ℹ See also `vignette("ggplot2-in-packages")` for more information.
    ## ℹ The deprecated feature was likely used in the phyloseq package.
    ##   Please report the issue at <https://github.com/joey711/phyloseq/issues>.
    ## This warning is displayed once per session.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

![](Analyse_article_MadeleineGUILLAUME_files/figure-gfm/unnamed-chunk-38-1.png)<!-- -->

``` r
plot_richness(ps.coral.raw, x = "Sampling_time", measures = "Shannon", color = "Treatment")
```

![](Analyse_article_MadeleineGUILLAUME_files/figure-gfm/unnamed-chunk-38-2.png)<!-- -->

``` r
alpha <- estimate_richness(ps.coral.raw, measures = "Shannon")
alpha$Treatment     <- sample_data(ps.coral.raw)$Treatment
alpha$Sampling_time <- sample_data(ps.coral.raw)$Sampling_time
for (t in c("T1", "T2", "T3", "T4")) {
  a <- subset(alpha, Sampling_time == t)
  cat(t, ": p =", signif(wilcox.test(Shannon ~ Treatment, data = a)$p.value, 3), "\n")
}
```

    ## T1 : p = 0.511 
    ## T2 : p = 0.618 
    ## T3 : p = 0.000925 
    ## T4 : p = 0.715

# Diversité bêta

On compare la composition des communautés entre échantillons avec la
distance de Bray-Curtis (qui tient compte des abondances) et celle de
Jaccard (présence/absence). Les résultats sont projetés en 2D par PCoA.
À T3, un test PERMANOVA vérifie si la composition diffère entre
probiotique et placebo.

La PCoA de tous les échantillons montre trois groupes bien séparés :
coraux, sédiments et eau de mer. Les deux premiers axes expliquent 48,2
% et 18,8 % de la variation. L’organisme est donc le facteur principal
de structuration des communautés, bien avant le traitement ou le temps.
Les coraux forment un groupe étiré, avec une majorité d’échantillons
très proches et quelques échantillons plus éloignés.

Sur les coraux seuls, probiotiques et placebo se mélangent largement,
sans séparation nette selon le traitement ni selon le temps.

À T3, les ellipses se chevauchent mais n’ont pas la même taille. Les
placebo forment un groupe compact. Les coraux traités sont beaucoup plus
dispersés et leur ellipse englobe celle des placebo. Le test confirme
une différence : le traitement explique environ 13 % de la variation de
composition (29 échantillons), avec Bray-Curtis (R² = 0,131 ; p = 0,005)
et avec Jaccard (R² = 0,130 ; p = 0,001). L’effet est modéré mais
significatif, et il concerne à la fois les abondances et la
présence/absence des ASV.

``` r
ord.all <- ordinate(ps.prop, method = "PCoA", distance = "bray")
plot_ordination(ps.prop, ord.all, color = "Organism", shape = "Sampling_time")
```

![](Analyse_article_MadeleineGUILLAUME_files/figure-gfm/unnamed-chunk-39-1.png)<!-- -->

``` r
ord.coral <- ordinate(ps.coral, method = "PCoA", distance = "bray")
plot_ordination(ps.coral, ord.coral, color = "Treatment", shape = "Sampling_time")
```

![](Analyse_article_MadeleineGUILLAUME_files/figure-gfm/unnamed-chunk-39-2.png)<!-- -->

``` r
# PERMANOVA à T3 : Bray-Curtis et Jaccard
adonis2(phyloseq::distance(ps.T3, "bray") ~ Treatment, data = data.frame(sample_data(ps.T3)))
```

    ## Permutation test for adonis under reduced model
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = phyloseq::distance(ps.T3, "bray") ~ Treatment, data = data.frame(sample_data(ps.T3)))
    ##          Df SumOfSqs      R2      F Pr(>F)   
    ## Model     1  0.29887 0.13093 4.0676  0.004 **
    ## Residual 27  1.98380 0.86907                 
    ## Total    28  2.28267 1.00000                 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
adonis2(phyloseq::distance(ps.T3, "jaccard", binary = TRUE) ~ Treatment,
        data = data.frame(sample_data(ps.T3)))
```

    ## Permutation test for adonis under reduced model
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = phyloseq::distance(ps.T3, "jaccard", binary = TRUE) ~ Treatment, data = data.frame(sample_data(ps.T3)))
    ##          Df SumOfSqs      R2      F Pr(>F)    
    ## Model     1   1.2585 0.13065 4.0576  0.001 ***
    ## Residual 27   8.3743 0.86935                  
    ## Total    28   9.6328 1.00000                  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
ord.T3 <- ordinate(ps.T3, method = "PCoA", distance = "bray")
plot_ordination(ps.T3, ord.T3, color = "Treatment", title = "Coraux à T3") +
  stat_ellipse()
```

![](Analyse_article_MadeleineGUILLAUME_files/figure-gfm/unnamed-chunk-39-3.png)<!-- -->

# Composition taxonomique

On s’intéresse à la famille Endozoicomonadaceae, bactéries typiquement
associées aux coraux sains, et on compare son abondance entre
traitements à chaque temps. On représente aussi les 20 ASV les plus
abondantes, regroupées par famille. Les ASV hors du top 20 sont
regroupées dans la catégorie « Autres », afin que chaque barre
représente 100 % de la communauté.

Les Endozoicomonadaceae dominent le microbiote des coraux : elles
représentent environ 70 à 85 % de l’abondance relative selon les groupes
et les temps. Les autres familles (Burkholderiaceae, Simkaniaceae,
Phycisphaeraceae, Oxalobacteraceae, Rhodobacteraceae) et la catégorie «
Autres » restent minoritaires.

Le boxplot montre que leur abondance est plus faible chez les coraux
traités à T3 : la médiane est d’environ 0,68, contre environ 0,89 chez
les placebo, avec une grande variabilité chez les traités. La différence
est significative à T3 (p = 0,0068) et pas aux autres temps (p de 0,29 à
0,72). Le probiotique est donc associé à une baisse transitoire de cette
famille dominante, à T3.

Cette baisse s’accompagne d’une augmentation de la diversité : quand une
famille dominante recule, les autres familles pèsent davantage, ce qui
augmente l’indice de Shannon. Les barplots le suggèrent aussi, avec une
part plus importante de Burkholderiaceae et de Simkaniaceae chez les
traités à certains temps.

``` r
tax  <- as(tax_table(ps.coral), "matrix")
endo <- rownames(tax)[tax[, "Family"] %in% "Endozoicomonadaceae"]
sample_data(ps.coral)$Endozoicomonadaceae <- sample_sums(prune_taxa(endo, ps.coral))
sdc <- data.frame(sample_data(ps.coral))

ggplot(sdc, aes(Sampling_time, Endozoicomonadaceae, fill = Treatment)) + geom_boxplot()
```

![](Analyse_article_MadeleineGUILLAUME_files/figure-gfm/unnamed-chunk-40-1.png)<!-- -->

``` r
for (t in c("T1", "T2", "T3", "T4")) {
  a <- subset(sdc, Sampling_time == t)
  cat(t, ": p =", signif(wilcox.test(Endozoicomonadaceae ~ Treatment, data = a)$p.value, 3), "\n")
}
```

    ## T1 : p = 0.401 
    ## T2 : p = 0.294 
    ## T3 : p = 0.00683 
    ## T4 : p = 0.78

``` r
top20 <- names(sort(taxa_sums(ps.coral), decreasing = TRUE))[1:20]
df <- psmelt(ps.coral)
df$Family <- as.character(df$Family)
df$Family[is.na(df$Family)] <- "non classé"
df$Family[!(df$OTU %in% top20)] <- "Autres"

s <- aggregate(Abundance ~ Sample + Treatment + Sampling_time + Family, data = df, sum)
m <- aggregate(Abundance ~ Treatment + Sampling_time + Family, data = s, mean)

ggplot(m, aes(Sampling_time, Abundance, fill = Family)) +
  geom_col() + facet_wrap(~Treatment) + ylab("Abondance relative moyenne")
```

![](Analyse_article_MadeleineGUILLAUME_files/figure-gfm/unnamed-chunk-40-2.png)<!-- -->

On refait la même représentation en regroupant d’abord les ASV par
famille et en gardant les 8 familles principales. Les autres familles
sont regroupées dans la catégorie « Autres », afin que chaque barre
atteigne 1. Le graphique est plus lisible, puisque le résultat ne dépend
plus du découpage en ASV.

``` r
ps.fam <- tax_glom(ps.coral, taxrank = "Family", NArm = FALSE)
topf   <- names(sort(taxa_sums(ps.fam), decreasing = TRUE))[1:8]

df <- psmelt(ps.fam)                                  
df$Family <- as.character(df$Family)
df$Family[is.na(df$Family)] <- "non classé"
df$Family[!(df$OTU %in% topf)] <- "Autres"             

s <- aggregate(Abundance ~ Sample + Treatment + Sampling_time + Family, data = df, sum)
m <- aggregate(Abundance ~ Treatment + Sampling_time + Family, data = s, mean)

ggplot(m, aes(Sampling_time, Abundance, fill = Family)) +
  geom_col() + facet_wrap(~Treatment) + ylab("Abondance relative moyenne")
```

![](Analyse_article_MadeleineGUILLAUME_files/figure-gfm/unnamed-chunk-41-1.png)<!-- -->

# Vérification de la dispersion

La PERMANOVA suppose une dispersion comparable entre groupes. On teste
donc l’homogénéité des dispersions, puis on regarde quel groupe est le
plus hétérogène. On refait le test avec Jaccard.

Les dispersions diffèrent entre groupes (Bray-Curtis : p = 0,019 ;
Jaccard : p = 0,034). Sur le graphique, les coraux placebo forment un
petit polygone compact, alors que les coraux traités couvrent un espace
beaucoup plus large. La distance moyenne au centre du groupe est de 0,29
contre 0,15. Le résultat de la PERMANOVA doit donc être nuancé, car il
peut refléter une différence de dispersion et pas seulement de
composition.

``` r
bd <- betadisper(phyloseq::distance(ps.T3, "bray"), sample_data(ps.T3)$Treatment)
permutest(bd)
```

    ## 
    ## Permutation test for homogeneity of multivariate dispersions
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## Response: Distances
    ##           Df  Sum Sq  Mean Sq     F N.Perm Pr(>F)  
    ## Groups     1 0.13371 0.133712 7.325    999  0.014 *
    ## Residuals 27 0.49287 0.018254                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
# Quel groupe est le plus dispersé ?
bd <- betadisper(phyloseq::distance(ps.T3, "bray"), sample_data(ps.T3)$Treatment)
tapply(bd$distances, sample_data(ps.T3)$Treatment, mean)
```

    ##   Placebo Probiotic 
    ## 0.1545353 0.2904214

``` r
plot(bd)
```

![](Analyse_article_MadeleineGUILLAUME_files/figure-gfm/unnamed-chunk-43-1.png)<!-- -->

``` r
# Même test avec Jaccard
bdj <- betadisper(phyloseq::distance(ps.T3, "jaccard", binary = TRUE),
                  sample_data(ps.T3)$Treatment)
permutest(bdj)
```

    ## 
    ## Permutation test for homogeneity of multivariate dispersions
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## Response: Distances
    ##           Df   Sum Sq   Mean Sq      F N.Perm Pr(>F)  
    ## Groups     1 0.011602 0.0116016 5.9785    999  0.018 *
    ## Residuals 27 0.052395 0.0019406                       
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

# Analyse de sensibilité

Comme la dispersion diffère entre groupes, on vérifie que le résultat ne
dépend pas de quelques échantillons extrêmes. On retire les deux coraux
probiotiques les plus éloignés de leur centre (27 échantillons
restants), puis on refait la PERMANOVA et le test de dispersion.

La différence de composition reste significative (R² = 0,121 ; p =
0,012). La différence de dispersion n’est plus significative (p =
0,055). L’effet sur la composition ne dépend donc pas de ces deux
échantillons extrêmes. Cette analyse reste exploratoire.

``` r
d  <- phyloseq::distance(ps.T3, "bray")
bd <- betadisper(d, sample_data(ps.T3)$Treatment)
dist.c <- bd$distances
sdT3 <- data.frame(sample_data(ps.T3))

# On retire les 2 probiotiques les plus éloignés de leur centre
extremes <- names(sort(dist.c[sdT3$Treatment == "Probiotic"], decreasing = TRUE))[1:2]
keep <- !(rownames(sdT3) %in% extremes)

ps.sens <- prune_samples(keep, ps.T3)
adonis2(phyloseq::distance(ps.sens, "bray") ~ Treatment,
        data = data.frame(sample_data(ps.sens)))
```

    ## Permutation test for adonis under reduced model
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = phyloseq::distance(ps.sens, "bray") ~ Treatment, data = data.frame(sample_data(ps.sens)))
    ##          Df SumOfSqs     R2      F Pr(>F)  
    ## Model     1  0.18158 0.1205 3.4252  0.014 *
    ## Residual 25  1.32530 0.8795                
    ## Total    26  1.50687 1.0000                
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
permutest(betadisper(phyloseq::distance(ps.sens, "bray"),
                     sample_data(ps.sens)$Treatment))
```

    ## 
    ## Permutation test for homogeneity of multivariate dispersions
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## Response: Distances
    ##           Df   Sum Sq  Mean Sq     F N.Perm Pr(>F)  
    ## Groups     1 0.050343 0.050343 4.189    999  0.057 .
    ## Residuals 25 0.300447 0.012018                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

# Conclusion

À T3, les coraux traités par probiotique présentent une diversité de
Shannon plus élevée que les placebo (Wilcoxon, p = 0,0009) et une
abondance plus faible d’Endozoicomonadaceae (p = 0,0068). Ces
différences n’apparaissent pas aux autres temps (p \> 0,2).

La composition du microbiome diffère entre traitements à T3 (PERMANOVA,
Bray-Curtis : R² = 0,131, p = 0,005 ; Jaccard : R² = 0,130, p = 0,001).
La dispersion multivariée diffère cependant entre groupes (Bray-Curtis :
p = 0,019 ; Jaccard : p = 0,034), les coraux traités étant plus
hétérogènes que les placebo (distance moyenne au centre : 0,29 contre
0,15).

Dans l’analyse de sensibilité exploratoire, qui exclut les deux coraux
probiotiques les plus éloignés de leur centre, la différence de
composition reste significative (R² = 0,121 ; p = 0,012) et la
différence de dispersion n’atteint plus le seuil de 0,05 (p = 0,055). La
différence observée reflète donc probablement à la fois un décalage de
composition et une hétérogénéité plus grande des coraux traités.

Ces résultats vont dans le sens de ceux de l’étude originale (R² = 0,137
; p = 0,002 ; Bray-Curtis).
