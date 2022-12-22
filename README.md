# Fsxgboost
# SNPkey
### R package for feature selection and genomic prediction

------------------------------------------------------------------------
### <u>Overview</u>

`SNPkey` uses the built-in function of the software to nest svm on the basis of xgboost algorithm for feature selection of gene data, and uses svm to evaluate the excellence of selected SNP sites. By using fewer loci, we can obtain similar or even better prediction effect than using all loci, improve prediction efficiency, and provide possibility for designing low-density chips.

### <u>Installation</u>

#### Install SNPkey

```R
install.packages("https://github.com/TXiang-lab/SNPkey/SNPkey_0.1.0.tar.gz",repos=NULL)
```
### <u>Data preparation</u>

#### 🌈Genotype Data

Snpkey requires users to provide corrected gene data. Each row corresponds to an individual, and each column represents the genotype of the corresponding site of the current individual.

``` R
2 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 1 0 1 1 1 1 1 2 1 1 0 2 2 2 0 0 0 0 2 2 0 2 0 2 2 2 0 2 0 2 0 0 0 2 0 0 0
2 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 1 0 1 1 1 1 1 2 1 1 0 2 2 2 0 0 0 0 2 2 0 2 0 2 2 2 0 2 0 2 0 0 0 2 0 0 0 
2 0 1 0 0 2 2 2 2 2 0 2 2 2 2 0 0 1 2 0 1 1 2 2 1 1 1 1 1 1 1 1 1 0 1 1 0 1 1 2 2 2 1 1 0 2 0 0 0 2 0 0 1 
2 0 0 0 0 2 2 2 2 2 0 2 2 2 2 0 0 0 2 0 2 2 0 2 2 2 0 2 2 2 0 0 0 0 2 2 0 2 0 2 2 2 0 2 0 2 0 0 0 2 0 0 0 
1 1 2 2 2 0 0 1 0 0 2 1 0 0 0 2 2 0 0 2 0 0 2 1 1 1 0 1 1 1 1 2 1 2 0 2 2 1 1 1 1 1 1 2 0 1 1 1 1 1 1 0 1 
2 0 0 0 0 2 2 2 2 2 0 2 2 2 2 0 0 0 2 0 2 2 0 2 2 2 0 2 1 1 0 1 0 1 1 2 0 2 0 2 2 2 0 2 0 2 0 0 0 2 0 0 0 
2 0 1 0 0 2 2 2 2 2 0 2 2 2 2 0 0 1 2 0 1 2 1 2 2 2 1 1 1 1 1 1 1 0 1 1 0 1 1 2 2 2 1 1 0 2 0 0 0 2 0 0 1 
2 2 2 2 1 0 0 0 0 0 2 0 0 0 0 2 1 0 0 2 0 0 2 2 0 0 0 2 2 2 0 0 0 1 1 2 0 2 0 2 2 2 0 2 0 2 0 0 0 2 0 0 1 
1 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 0 1 1 1 1 1 1 2 1 0 2 2 2 0 0 0 0 1 2 0 2 0 2 2 2 0 2 0 2 0 0 0 2 0 0 0 
2 1 1 0 0 2 2 2 2 2 0 2 2 2 2 0 0 1 2 1 1 2 1 2 2 2 1 1 1 1 1 1 1 0 1 1 0 1 1 2 2 2 1 1 0 2 0 0 0 2 0 0 1 
```

#### 🌈 Phenotype data:

Phenotype contains a column of data representing the phenotypic value of each individual, and represents the label of training in the model.

``` R
52.7770745835928
33.7246729802582
40.4822869798238
74.2821430158934
59.8024639952507
60.1368010639134
50.2545710251446
41.0888949489511
57.8273001812688
63.0823959083782
```

## <u>Usage</u>

#### 🌈Train model

``` R
library(SNPkey)
library(xgboost)
library(e1071)
load("exampledata.rda")
x = exampledata$markers
y = exampledata$phenoype
result = snpkey(
          train_x = x,
          train_y = y,
          seednum = 0,
          xgbround = 200,
          loopnum = 100,
          sc = 1,
          sg = 0.01)
```


#### 🌈Show result

``` R
set(result)
List of 4
  $ best_param:List of 7
    ..$ max_depth         : int 2
    ..$ eta               : num 7.32e-06
    ..$ gamma             : num 0.981
    ..$ min_child_weight  : int 44
    ..$ subsample         : num 0.748
    ..$ colsample_bylevel : num 0.764
    ..$ lamda             : int 8
  $ best_cor   : num 0.668
  $ best_rmse  : num 51.5
  $ bestsnp    : chr [1:116] "v2462" "v1818" "v497" ... 
```
In the output results, bestsnp is the site that the model thinks has a promoting effect on prediction.
```R
result$bestsnp
[1] "v2462" "v1818" "v497" "v2095" "v1316" "v265" ...
```
