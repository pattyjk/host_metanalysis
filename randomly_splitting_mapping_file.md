## Code to randomly split a mapping file
```
setwd("/Users/patty/Dropbox/R/host_meta/")
#install.packages('readr')
library(readr)
#read in data
map <- read_delim("map.txt", "\t", escape_double = FALSE, trim_ws = TRUE, na='na')

#install.packages('dplyr')
library(dplyr)
set.seed(1001)
out_internal<-map %>%
  group_by(internal_habitat_type, doi) %>%
  sample_n(10, replace=T)
#new frame 610 by 74

out_external<-map %>%
  group_by(external_habitat_type, doi) %>%
  sample_n(10, replace=T)
#new frame 570 by 74

#remove rows with NA's in the external habitat column
out_external<-out_external[!is.na(out_external$external_habitat_type),]

#remove rows with NA's in the internal habitat column
out_internal<-out_external[!is.na(out_internal$internal_habitat_type),]

#write files to .csv
write.csv(out_external, 'out_external.csv')
write.csv(out_internal, 'out_internal.csv')

###########################################################################
#####################Split by species/digestive/country####################
###########################################################################
setwd("/Users/patty/Dropbox/R/host_meta/")
#install.packages('readr')
library(readr)
#read in data
map <- read_delim("map.txt", "\t", escape_double = FALSE, trim_ws = TRUE, na='na')

#split
map_split<-split(map, map$internal_habitat_type)
#write digestive-associated toa  new object
map_digestive<-map_split$`digestive-associated`
#count rows aka samples
nrow(map_digestive)
#4920

#randomly split by species/sountry
#install.packages('dplyr')
library(dplyr)
set.seed(1001)
#always set seed to be the same to get repeatable output
digest_sp_country<-map_digestive %>%
  group_by(host_species, country) %>%
  sample_n(10, replace=T)
#this replaces the rows taken out in species n<10, so we'll have duplicate rows

#count rows in new table
nrow(digest_sp_country)
#2120

#remove duplicate rows
library(dplyr)
digest_sp_country<- digest_sp_country %>% distinct
nrow(digest_sp_country)
#759

#write mapping file to .csv file
write.csv(digest_sp_country, 'digest_sp_country.csv')

###########################################################################
#####################Split by species/external/country#####################
###########################################################################

#extract the external sites only
map_split_ext<-split(map, map$external_habitat_type)
map_external<-rbind(map_split_ext$animal_surface, map_split_ext$gill, map_split_ext$leaf_surface, map_split_ext$root_surface)
#count rows
nrow(map_external)
#6703

#randomly split by species/sountry
#install.packages('dplyr')
library(dplyr)
set.seed(1001)
#always set seed to be the same to get repeatable output
external_sp_country<-map_external %>%
  group_by(host_species, country) %>%
  sample_n(10, replace=T)

#count rows
nrow(external_sp_country)
#3630

#remove duplicate rows
library(dplyr)
external_sp_country<- external_sp_country %>% distinct
nrow(external_sp_country)
#1762

#write to .csv file
write.csv(external_sp_country, 'external_sp_country.csv')
```
