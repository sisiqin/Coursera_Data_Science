#Week 4 Quiz

##Q1
The American Community Survey distributes downloadable data about United States communities. Download the 2006 microdata survey about housing for the state of Idaho using download.file() from here:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv

and load the data into R. The code book, describing the variable names is here:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FPUMSDataDict06.pdf

Apply strsplit() to split all the names of the data frame on the characters "wgtp". What is the value of the 123 element of the resulting list?

    download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv",destfile = "/Users/sissiqin/Desktop/week4Quiz.csv", method = "curl" )
    week4Quiz1 <- read.csv("/Users/sissiqin/Desktop/week4Quiz.csv")
    names(week4Quiz1)
    try1 <- strsplit(names(week4Quiz1), "wgtp")
    try1[123]


##Q2
Load the Gross Domestic Product data for the 190 ranked countries in this data set:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv

Remove the commas from the GDP numbers in millions of dollars and average them. What is the average?

Original data sources:

http://data.worldbank.org/data-catalog/GDP-ranking-table 

    download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv",destfile = "/Users/sissiqin/Desktop/week4Quiz2.csv", method = "curl" )
    week4Quiz2 <- read.csv("/Users/sissiqin/Desktop/week4Quiz2.csv", skip = 4, nrows = 215, stringsAsFactors = FALSE)
    head(week4Quiz2, 10)
    try2 <- week4Quiz2$X.4
    try2 <- gsub(",", "", try2, )
    try2 <- as.numeric(try2)
    mean(try2, na.rm = TRUE)


##Q3
In the data set from Question 2 what is a regular expression that would allow you to count the number of countries whose name begins with "United"? Assume that the variable with the country names in it is named countryNames. How many countries begin with United? 
    
    grep("^United",countryNames), 3


##Q4
Load the Gross Domestic Product data for the 190 ranked countries in this data set:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv

Load the educational data from this data set:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FEDSTATS_Country.csv

Match the data based on the country shortcode. Of the countries for which the end of the fiscal year is available, how many end in June?

Original data sources:

http://data.worldbank.org/data-catalog/GDP-ranking-table

http://data.worldbank.org/data-catalog/ed-stats

    download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FEDSTATS_Country.csv",destfile = "/Users/sissiqin/Desktop/week4Quiz4.csv", method = "curl" )
    week4Quiz4 <- read.csv("/Users/sissiqin/Desktop/week4Quiz4.csv")
    merged <- merge(week4Quiz2, week4Quiz4, by.x = "X", by.y = "CountryCode", all = TRUE)
    inJune <- grepl("Jun", merged$Special.Notes)
    isEnd <- grepl("Fiscal year end", merged$Special.Notes)
    table(inJune, isEnd)
 
##Q5 
 You can use the quantmod (http://www.quantmod.com/) package to get historical stock prices for publicly traded companies on the NASDAQ and NYSE. Use the following code to download data on Amazon's stock price and get the times the data was sampled.
   
    library(quantmod)
    amzn <- getSymbols("AMZN",auto.assign=FALSE)
    sampleTimes <- index(amzn)
    
How many values were collected in 2012? How many values were collected on Mondays in 2012?

    install.packages("quantmod")
    library(quantmod)
    amzn <- getSymbols("AMZN",auto.assign=FALSE)
    sampleTimes <- index(amzn)
    head(sampleTimes)
    class(sampleTimes)
    is2012 <- grepl("2012", sampleTimes)
    sampleTimes <- format(sampleTimes, "%a%b%d")
    isMonday <- grepl("一", sampleTimes)
    table(is2012, isMonday)
