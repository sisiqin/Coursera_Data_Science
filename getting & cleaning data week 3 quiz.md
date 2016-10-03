#Week 3 Quiz

##Question 1 
The American Community Survey distributes downloadable data about United States communities. Download the 2006 microdata survey about housing for the state of Idaho using download.file() from here:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv

and load the data into R. The code book, describing the variable names is here:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FPUMSDataDict06.pdf

Create a logical vector that identifies the households on greater than 10 acres who sold more than $10,000 worth of agriculture products. Assign that logical vector to the variable agricultureLogical. Apply the which() function like this to identify the rows of the data frame where the logical vector is TRUE.

which(agricultureLogical)

What are the first 3 values that result?




    download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv", destfile = ".//Users/sissiqin/Desktop/Rfiles/q1.csv", method = "curl")
    setwd("/Users/sissiqin/Desktop/Rfiles")
    q1 <- read.csv("q1.csv")
    head(q1)
    agricultureLogical <- q1$ACR == 3 & q1$AGS == 6
    head(which(agricultureLogical))
    
    

##Question 2 
Using the jpeg package read in the following picture of your instructor into R

https://d396qusza40orc.cloudfront.net/getdata%2Fjeff.jpg

Use the parameter native=TRUE. What are the 30th and 80th quantiles of the resulting data? (some Linux systems may produce an answer 638 different for the 30th quantile)


    url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fjeff.jpg"
    place <- ".//Users/sissiqin/Desktop/Rfiles/t2.jpg"
    download.file(url, destfile = place, mode = "wb", method = "curl")
    q2 <- readJPEG("jeff.jpg", native = TRUE)
    q2Quantile <- quantile(q2, prob = c(0.3, 0.8))
    
    
##Question 3 
Load the Gross Domestic Product data for the 190 ranked countries in this data set:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv

Load the educational data from this data set:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FEDSTATS_Country.csv

Match the data based on the country shortcode. How many of the IDs match? Sort the data frame in descending order by GDP rank (so United States is last). What is the 13th country in the resulting data frame?

Original data sources:

http://data.worldbank.org/data-catalog/GDP-ranking-table

http://data.worldbank.org/data-catalog/ed-stats
    
    library(plyr)
    download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv", destfile = "./Users/sissiqin/Desktop/Rfiles/q31.csv", method = "curl")
    download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FEDSTATS_Country.csv", destfile = "./Users/sissiqin/Desktop/Rfiles/q32.csv", method = "curl")
    q31 <- read.csv("Country.csv")
    q32 <- read.csv("GDP.csv", skip=4, nrows = 190)
    names(q31)
    names(q32)
    head(q32, 6)
    q32 <- rename(q32, CountryCode = X, Rank = X.1)
    merged <- merge(q31, q32, by= "CountryCode")
    merged <- arrange(merged, desc(Rank))
    nrow(merged)
    head(merged, 13)

##Question 4
What is the average GDP ranking for the "High income: OECD" and "High income: nonOECD" group?

    str(merged)
    table(merged$Income.Group)
    OECD <- merged[(merged$Income.Group %in% c("High income: OECD")), ]
    nonOECD <- merged[(merged$Income.Group %in% c("High income: nonOECD")), ]
    meanOECD <- mean(OECD$Rank)
    meanNonOECD <- mean(nonOECD$Rank)


##Question 5 
Cut the GDP ranking into 5 separate quantile groups. Make a table versus Income.Group. How many countries

are Lower middle income but among the 38 nations with highest GDP?

    RankQuantile <- cut2(merged$Rank,g = 5)
    table(RankQuantile)
    RankQuestion <- cbind(merged, RankQuantile)
    cast <- dcast(RankQuestion, RankQuantile ~ Income.Group)
