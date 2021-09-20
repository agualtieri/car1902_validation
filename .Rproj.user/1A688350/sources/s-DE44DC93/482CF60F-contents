## R Validation
rm(list = ls())

library(tidyverse)
library(cleaninginspectoR)
library(openxlsx)
library(cluster)

source("./check_time.R")
source("./data_falsification.R")

# load analysis files
data <-"./data/CAR1902_HSM_dataset_april2021.xlsm"

sheets <- openxlsx::getSheetNames(data)
SheetList <- lapply(sheets,openxlsx::read.xlsx,xlsxFile=data)
names(SheetList) <- sheets

clean <- SheetList[[5]]
clean_log <- SheetList[[6]]
del <- SheetList[[7]]
timec <- SheetList[[4]]
raw <- SheetList[[2]]

## check issues
issues <- inspect_all(clean)
clean_red <- select(clean, c("_index", "uuid"))

write.csv(issues, paste0("./hsm_issues_",lubridate::today(),".csv"))

issues <- left_join(issues, clean_red, c("index" = "_index"))

## check time
time <- check_time(clean, "10", "60")

write.csv(time, paste0("./hsm_time issues_",lubridate::today(),".csv"))

## check del
delc <- semi_join(del, clean,  "_uuid")


## falsification
tool <- read.xlsx("./data/CAR1902_HSM_KoboQuest_new2021.xlsx")

false1 <- calculateDifferences(clean, tool)
write.csv(false1, paste0("./hsm_falsification issues_",lubridate::today(),".csv"))

false2.1 <- calculateEnumeratorSimilarity(clean, tool, "q0_1_enqueteur", "lieu_collecte_loc")
write.csv(false2.1, paste0("./hsm_enumerator falsification_",lubridate::today(),".csv"))
