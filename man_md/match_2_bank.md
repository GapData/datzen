# `match_2_bank`: The match_2_bank() function matches elements to a bank of entities

## Description


 Elements in df_raw[,col_name] will be fuzzyjoined to elements in bank_entity


## Usage

```r
match_2_bank(bank_entity, col_name, df_raw, method = "jw", max_dist = 0.2)
```


## Arguments

Argument      |Description
------------- |----------------
```bank_entity```     |     a single column data.frame that contains the unique population of entities inside bank_entity$bank_entity
```col_name```     |     a single string for the name of the column in 'df_raw' to be processed and matched to bank_entity
```df_raw```     |     a data.frame that contains the column specified by the 'col_name' argument
```method```     |     the method argument passed to [`stringdist_left_join`](stringdist_left_join.html)
```max_dist```     |     the max_dist argument passed to [`stringdist_left_join`](stringdist_left_join.html)

## Value


 A 2 column data.frame with elements in df_raw[,col_name] fuzzyjoined to 'bank_entity' via [`stringdist_left_join`](stringdist_left_join.html) 


## Examples

```r 
 
 #######################################
 # Create name bank of unique names
 # all population of names
 # need to disambiguate, get rid of redundant
 # most likely manually
 #######################################
 library(dplyr)
 
 
 df_raw = data.frame(name_a=c('joseph','joe','jo ',' joesephine','joseph','jo'),
 name_b=c('joseph','jo3','jo ',' joesephine','joseph','jo'),
 name_c=c('joseph','j0 '))
 
 names_col_to_pool = c('name_a','name_b','name_c')
 name_all_unique = as.character(unlist(df_raw[,names_col_to_pool])) %>% unique %>% sort
 
 # creating name bank: drop mispelled, appeal to appx string match latter
 bank_entity = name_all_unique %>% clean_name() %>% unique %>% sort %>%
 gsub(.,pattern='^(Jo3)$|(J0)|(j0)|(jo3)',replace='') %>% # Joseph
 # spotcheck additional manual gsubs()
 unique %>% sort
 
 bank_entity = data.frame(bank_entity=bank_entity)
 str(bank_entity)
 
 
 library(stringdist)
 
 # run one example to figure out distance boundary
 stringdist::stringdist("Jo","Joseph",method='jw')
 # c("osa", "lv","dl", "hamming", "lcs", "qgram", "cosine", "jaccard", "jw", "soundex")
 
 match_2_bank(bank_entity=bank_entity,col_name="name_a",df_raw=df_raw)
 
 match_2_bank(bank_entity=bank_entity,
 col_name="name_b",df_raw=df_raw,
 max_dist=0.6,method='jw')
 
 
 # make sure output of match_2_bank() == nrow(df_raw)
 nrow(df_raw)
 
 ##############################
 # Replace
 ##############################
 # head(df_raw)
 #
 # dat_edit = df_raw
 # dat_edit[,'name_a'] = match_2_bank(bank_entity=bank_entity,col_name="name_a",df_raw=df_raw,max_dist=0.2,method='jw')[,2]
 # dat_edit[,'name_b'] = match_2_bank(bank_entity=bank_entity,col_name="name_b",df_raw=df_raw,max_dist=0.2,method='jw')[,2]
 # dat_edit[,'name_c'] = match_2_bank(bank_entity=bank_entity,col_name="name_c",df_raw=df_raw,max_dist=0.2,method='jw')[,2]
 #
 # head(dat_edit)
 #
 #' # dat_edit[,c('name_a','name_b','name_c')]
 # df_raw[,c('name_a','name_b','name_c')]
 ``` 

