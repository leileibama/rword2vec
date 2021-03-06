# rword2vec

[![Travis-CI Build Status](https://travis-ci.org/mukul13/rword2vec.svg?branch=master)](https://travis-ci.org/mukul13/rword2vec)

R wrapper to google's word2vec.
The word2vec tool takes a text corpus as input and produces the word vectors as output. It first constructs a vocabulary from the training text data and then learns vector representation of words. 

### Examples
To download sample text data, click [here](http://mattmahoney.net/dc/text8.zip).

To install rowrd2vec package
```R
library(devtools)
install_github("mukul13/rword2vec")
```

To list all functions supported by rword2vec

```R
library(rword2vec)
ls("package:rword2vec")
```
```{r eval=F}
## [1] "bin_to_txt"   "distance"     "vocab_count"  "word2phrase" 
## [5] "word2vec"     "word_analogy"
```

<b>Training word2vec model:</b>

To train text data to give word vectors
```R
model=word2vec(train_file = "text8",output_file = "vec.bin",binary=1)
```

<b>Distance:</b>

To get closest words
```R
### file_name must be binary
dist=distance(file_name = "vec.bin",search_word = "king",num = 20)
```
```R
##         word              dist
## 1     prince 0.714353382587433
## 2      kings 0.663175880908966
## 3    pileser 0.642198622226715
## 4    emperor 0.632584810256958
## 5      queen 0.631504416465759
## 6  antiochus 0.626743733882904
## 7    tiglath 0.622674822807312
## 8        vii  0.62063330411911
## 9     regent 0.619060039520264
## 10   alexius 0.616275072097778
```

```R
### file_name must be binary
dist=distance(file_name = "vec.bin",search_word = "princess",num = 10)
dist
```
```R
##        word              dist
## 1   duchess 0.769854545593262
## 2   consort 0.752401173114777
## 3    prince 0.727755606174469
## 4  daughter 0.701653897762299
## 5   empress  0.70031350851059
## 6  countess 0.693541049957275
## 7    hedwig 0.686940908432007
## 8     niece 0.686935067176819
## 9     bride  0.67846018075943
## 10  infanta 0.677732884883881
```
```R
### file_name must be binary
dist=distance(file_name = "vec.bin",search_word = "terrible",num = 10)
dist
```
```R
##          word              dist
## 1      sorrow 0.629752099514008
## 2    horrible  0.62950724363327
## 3  terrifying 0.627294421195984
## 4       dying 0.626088738441467
## 5       cruel 0.625054001808167
## 6      hunger 0.590250313282013
## 7      doomed 0.577929139137268
## 8    horrific 0.576288521289825
## 9       grief 0.572968125343323
## 10        cry 0.567858517169952
```

<b>Word analogy:</b>

To get analogy or to observe strong regularities in the word vector space
```R
### file name must be binary
ana=word_analogy(file_name = "vec.bin",search_words = "king queen man",num = 20)
```
```R
##         word              dist
## 1      woman 0.716363251209259
## 2       girl 0.613960087299347
## 3     blonde 0.580629587173462
## 4      bride 0.548110067844391
## 5       baby 0.541548788547516
## 6       lady 0.540741205215454
## 7    goddess 0.501877009868622
## 8       cute 0.499198257923126
## 9   stranger  0.49639430642128
## 10 gentleman 0.488330274820328
```

```R
### file name must be binary
ana=word_analogy(file_name = "vec.bin",search_words = "paris france berlin",num = 10)
ana
```
```R
##              word              dist
## 1         germany 0.792844653129578
## 2         austria 0.709049165248871
## 3         hungary 0.687035202980042
## 4          russia 0.644324779510498
## 5          poland 0.642726004123688
## 6         finland 0.639021933078766
## 7  czechoslovakia 0.617623269557953
## 8       lithuania 0.603914618492126
## 9             gdr 0.599377155303955
## 10     luxembourg 0.578022837638855
```

<b>Training word2phrase model:</b>

To convert words to phrases
```R
word2phrase(train_file = "text8",output_file = "vec.txt")

### use this new text file to give word vectors
model=word2vec(train_file = "vec.txt",output_file = "vec2.bin",binary=1)
```

<b>Word count:</b>

To do word count
```R
### to count word occurences in input file
vocab_count(file_name="text8",vocab_file="vocab.txt",min_count = 20)
d=read.table("vocab.txt")
head(d)
```
```R
##    V1      V2
## 1 the 1061396
## 2  of  593677
## 3 and  416629
## 4 one  411764
## 5  in  372201
## 6   a  325873
```

<b>Bin to txt:</b>

To convert binary output file to text format:<br />
```R
###convert .bin to .txt
bin_to_txt("vec.bin","vector.txt")
```
Use this text file to get word vectors:
```R
data=as.data.frame(read.table("vector.txt",skip=1))
data[1,]
```
```R
## data.frame':	71291 obs. of  101 variables:
## $ V1  : Factor w/ 71291 levels "a","aa","aaa",..: 55827 63881 45640 2646 45926 31473 1 64596 71091 44557 ...
## $ V2  : num  0.004 1.281 -0.577 -0.352 -0.361 ...
## $ V3  : num  0.00442 0.51466 -0.91757 -0.01408 0.04345 ...
## $ V4  : num  -0.00383 0.36052 0.15737 0.18496 -0.04641 ...
## $ V5  : num  -0.00328 0.0063 1.03664 0.94061 0.95325 ...
## $ V6  : num  0.00137 -0.29928 -0.78016 0.11719 0.46731 ...
## $ V7  : num  0.00302 0.36505 -0.60761 0.13251 1.0106 ...
## $ V8  : num  0.000941 -0.272078 1.016449 0.385708 -0.309844 ...
## $ V9  : num  0.000211 -0.27177 0.371277 -0.084057 -0.759528 ...
## $ V10 : num  -0.0036 -0.8509 -0.5182 0.5113 -0.0053 ...
## $ V11 : num  0.00222 -0.38638 -0.60463 -0.18529 0.23022 ...
## $ V12 : num  -0.00436 -0.13679 0.20418 0.3277 1.7405 ...
## $ V13 : num  0.00125 1.36504 -0.30284 -0.09633 -1.52368 ...
## $ V14 : num  -0.000751 -0.954647 1.317677 0.357123 0.525351 ...

## and so on. 
```
### Resources
* [word2vec](https://code.google.com/archive/p/word2vec/) 

### TODO
* to fix Windows warnings
