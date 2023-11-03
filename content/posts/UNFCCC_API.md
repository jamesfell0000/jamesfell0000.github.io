+++
title = 'How to access the UNFCCC emissions data via API in R'
date = 2023-06-25T20:50:58+11:00
+++
Evidence bases require quality tools. In the spirit of disseminating generic tools to help improve everyone's quality of research, here's a generic tool that I made to access UNFCCC emissions data in R. The API is undocumented as far as I can see. There is a Python script available, but even that no longer works (according to the creator's website). This particular example gets Kazakhstan's gasoline emissions from cars for 2016 to 2020, measured in CO2-equivalent. Note: rather ironically, the UNFCCC API (application programming interface) blocks the use of programs to access the already publicly available data, so you need to make yourself look like a desktop web browser. The script deals with this for you. This is a good workaround and seems to work, but I can't guarantee that it will always work, and you might have trouble using it on corporate/government networks due to IP address restrictions imposed by the blocking software that runs on their website.

First, installation (you need to have already installed the packages httr, jsonlite, rrapply and dplyr):
```R
#Installation
download.file("https://raw.githubusercontent.com/jamesfell0000/UNFCCC_emissions_API/main/UNFCCC_API_for_R.r", "UNFCCC_API_for_R.r",cacheOK = FALSE)
install.packages(c("httr", "jsonlite","rrapply","dplyr"))
```

Next, load the functions:
```R
#Load the functions
source("UNFCCC_API_for_R.r")
```

How to run a query:
```R
parties_list = list(36)
years_list = list(58,59,60,61,62)
variables_list = list(200187)
query_results <- flex_query(parties_list,years_list,variables_list)
```

The API doesn't seem like to queries with more than around 2000 variables, so you can split it up. This would be useful is you want to grab all the data for a given year and party:
```R
df_variableIDs <- get_variableIDs()
variables_list = as.list(df_variableIDs$variableId)
variables_list<-split(variables_list, ceiling(seq_along(variables_list)/2000))
years_list = list(62)
parties_list = list(36)
query_results <- flex_query(parties_list,years_list,variables_list[[1]])
query_results <- rbind(query_results,flex_query(parties_list,years_list,variables_list[[2]]))
query_results <- rbind(query_results,flex_query(parties_list,years_list,variables_list[[3]]))
query_results <- rbind(query_results,flex_query(parties_list,years_list,variables_list[[4]]))
query_results <- rbind(query_results,flex_query(parties_list,years_list,variables_list[[5]]))
query_results <- rbind(query_results,flex_query(parties_list,years_list,variables_list[[6]]))
query_results <- rbind(query_results,flex_query(parties_list,years_list,variables_list[[7]]))
query_results <- rbind(query_results,flex_query(parties_list,years_list,variables_list[[8]]))
query_results <- rbind(query_results,flex_query(parties_list,years_list,variables_list[[9]]))
query_results <- rbind(query_results,flex_query(parties_list,years_list,variables_list[[10]]))
query_results <- rbind(query_results,flex_query(parties_list,years_list,variables_list[[11]]))

#If, for some reason, you want to grab all the years, then try this:
#All years:
df_yearIDs <- get_years()
years_list = as.list(df_yearIDs$yearId)
```