Quantum Analysis
BY UDE MICHAEL

library(data.table)
## Warning: package 'data.table' was built under R version 4.0.5
library(ggplot2)
## Warning: package 'ggplot2' was built under R version 4.0.5
library(ggmosaic)
## Warning: package 'ggmosaic' was built under R version 4.0.5
library(readr)
## Warning: package 'readr' was built under R version 4.0.5
library(tidyverse)
## Warning: package 'tidyverse' was built under R version 4.0.5
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
## v tibble  3.1.4     v dplyr   1.0.7
## v tidyr   1.1.4     v stringr 1.4.0
## v purrr   0.3.4     v forcats 0.5.1
## Warning: package 'tibble' was built under R version 4.0.5
## Warning: package 'tidyr' was built under R version 4.0.5
## Warning: package 'purrr' was built under R version 4.0.5
## Warning: package 'dplyr' was built under R version 4.0.5
## Warning: package 'stringr' was built under R version 4.0.5
## Warning: package 'forcats' was built under R version 4.0.5
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::between()   masks data.table::between()
## x dplyr::filter()    masks stats::filter()
## x dplyr::first()     masks data.table::first()
## x dplyr::lag()       masks stats::lag()
## x dplyr::last()      masks data.table::last()
## x purrr::transpose() masks data.table::transpose()
Read files and assign the files to the data table
foragedata <- read.csv("QVI_transaction_data.csv", stringsAsFactors = FALSE)
foragebehaviour <- read.csv("QVI_purchase_behaviour.csv")
foragedata <- data.table(foragedata)
foragebehaviour <- data.table(foragebehaviour)
Explanatory Data Analysis
starting with the forage data, let’s understand the data by examining it using ‘str()’.
str(foragedata)
## Classes 'data.table' and 'data.frame':   264836 obs. of  8 variables:
##  $ DATE          : int  43390 43599 43605 43329 43330 43604 43601 43601 43332 43330 ...
##  $ STORE_NBR     : int  1 1 1 2 2 4 4 4 5 7 ...
##  $ LYLTY_CARD_NBR: int  1000 1307 1343 2373 2426 4074 4149 4196 5026 7150 ...
##  $ TXN_ID        : int  1 348 383 974 1038 2982 3333 3539 4525 6900 ...
##  $ PROD_NBR      : int  5 66 61 69 108 57 16 24 42 52 ...
##  $ PROD_NAME     : chr  "Natural Chip        Compny SeaSalt175g" "CCs Nacho Cheese    175g" "Smiths Crinkle Cut  Chips Chicken 170g" "Smiths Chip Thinly  S/Cream&Onion 175g" ...
##  $ PROD_QTY      : int  2 3 2 5 3 1 1 1 1 2 ...
##  $ TOT_SALES     : num  6 6.3 2.9 15 13.8 5.1 5.7 3.6 3.9 7.2 ...
##  - attr(*, ".internal.selfref")=<externalptr>
Examine the first ten column
head(foragedata)
##     DATE STORE_NBR LYLTY_CARD_NBR TXN_ID PROD_NBR
## 1: 43390         1           1000      1        5
## 2: 43599         1           1307    348       66
## 3: 43605         1           1343    383       61
## 4: 43329         2           2373    974       69
## 5: 43330         2           2426   1038      108
## 6: 43604         4           4074   2982       57
##                                   PROD_NAME PROD_QTY TOT_SALES
## 1:   Natural Chip        Compny SeaSalt175g        2       6.0
## 2:                 CCs Nacho Cheese    175g        3       6.3
## 3:   Smiths Crinkle Cut  Chips Chicken 170g        2       2.9
## 4:   Smiths Chip Thinly  S/Cream&Onion 175g        5      15.0
## 5: Kettle Tortilla ChpsHny&Jlpno Chili 150g        3      13.8
## 6: Old El Paso Salsa   Dip Tomato Mild 300g        1       5.1
Coverting DATE to Date format
foragedata$DATE <- as.Date(foragedata$DATE, origin = "1899-12-30")
confirming the Date format
head(foragedata)
##          DATE STORE_NBR LYLTY_CARD_NBR TXN_ID PROD_NBR
## 1: 2018-10-17         1           1000      1        5
## 2: 2019-05-14         1           1307    348       66
## 3: 2019-05-20         1           1343    383       61
## 4: 2018-08-17         2           2373    974       69
## 5: 2018-08-18         2           2426   1038      108
## 6: 2019-05-19         4           4074   2982       57
##                                   PROD_NAME PROD_QTY TOT_SALES
## 1:   Natural Chip        Compny SeaSalt175g        2       6.0
## 2:                 CCs Nacho Cheese    175g        3       6.3
## 3:   Smiths Crinkle Cut  Chips Chicken 170g        2       2.9
## 4:   Smiths Chip Thinly  S/Cream&Onion 175g        5      15.0
## 5: Kettle Tortilla ChpsHny&Jlpno Chili 150g        3      13.8
## 6: Old El Paso Salsa   Dip Tomato Mild 300g        1       5.1
Examine PROD_NAME column
foragedata[ , .N, PROD_NAME]
##                                     PROD_NAME    N
##   1:   Natural Chip        Compny SeaSalt175g 1468
##   2:                 CCs Nacho Cheese    175g 1498
##   3:   Smiths Crinkle Cut  Chips Chicken 170g 1484
##   4:   Smiths Chip Thinly  S/Cream&Onion 175g 1473
##   5: Kettle Tortilla ChpsHny&Jlpno Chili 150g 3296
##  ---                                              
## 110:    Red Rock Deli Chikn&Garlic Aioli 150g 1434
## 111:      RRD SR Slow Rst     Pork Belly 150g 1526
## 112:                 RRD Pc Sea Salt     165g 1431
## 113:       Smith Crinkle Cut   Bolognese 150g 1451
## 114:                 Doritos Salsa Mild  300g 1472
The PROD_NAME shows there are 114 product name with it sizes, while the N column shows the number of times the items was purchased. Checking the basic text analysis looking for potato chips in the PROD_NAME column.
productWords <- data.table(unlist(strsplit(unique(foragedata[, PROD_NAME]), " ")))
setnames(productWords, 'words')
As we are only interested in words that will tell us if the product is chips or not, let’s remove all words with digits and special characters such as ‘&’ from our set of product words. We can do this using `grepl(). #### Removing Digits
productwords <- productWords[grepl("\\d", words) == FALSE,] 
Removing special character
productWords <- productWords[grepl("[:alpha:]", words), ]
Sorting Distinct Words by frequecy of occurance
productWords[, .N, words][order(N, decreasing = TRUE)]
##             words  N
##   1:        Chips 21
##   2:       Smiths 16
##   3:      Crinkle 14
##   4:       Kettle 13
##   5:       Cheese 12
##  ---                
## 133: Chikn&Garlic  1
## 134:        Aioli  1
## 135:         Slow  1
## 136:        Belly  1
## 137:    Bolognese  1
There are salsa products in the dataset but we are only interested in the chips category, so let’s remove these.
foragedata[, SALSA := grepl("salsa", tolower(PROD_NAME))]
foragedata <- foragedata[SALSA == FALSE, ][, SALSA := NULL]
Next, we can use summary() to check summary statistics such as mean, min and max values for each feature to see if there are any obvious outliers in the data and if there are any nulls in any of the columns.
summary(foragedata)
##       DATE              STORE_NBR     LYLTY_CARD_NBR        TXN_ID       
##  Min.   :2018-07-01   Min.   :  1.0   Min.   :   1000   Min.   :      1  
##  1st Qu.:2018-09-30   1st Qu.: 70.0   1st Qu.:  70015   1st Qu.:  67569  
##  Median :2018-12-30   Median :130.0   Median : 130367   Median : 135183  
##  Mean   :2018-12-30   Mean   :135.1   Mean   : 135531   Mean   : 135131  
##  3rd Qu.:2019-03-31   3rd Qu.:203.0   3rd Qu.: 203084   3rd Qu.: 202654  
##  Max.   :2019-06-30   Max.   :272.0   Max.   :2373711   Max.   :2415841  
##     PROD_NBR       PROD_NAME            PROD_QTY         TOT_SALES      
##  Min.   :  1.00   Length:246742      Min.   :  1.000   Min.   :  1.700  
##  1st Qu.: 26.00   Class :character   1st Qu.:  2.000   1st Qu.:  5.800  
##  Median : 53.00   Mode  :character   Median :  2.000   Median :  7.400  
##  Mean   : 56.35                      Mean   :  1.908   Mean   :  7.321  
##  3rd Qu.: 87.00                      3rd Qu.:  2.000   3rd Qu.:  8.800  
##  Max.   :114.00                      Max.   :200.000   Max.   :650.000
There are two transactions where 200 packets of chips are bought in one transaction and both of these transactions were by the same customer.
foragedata[PROD_QTY == 200]
##          DATE STORE_NBR LYLTY_CARD_NBR TXN_ID PROD_NBR
## 1: 2018-08-19       226         226000 226201        4
## 2: 2019-05-20       226         226000 226210        4
##                           PROD_NAME PROD_QTY TOT_SALES
## 1: Dorito Corn Chp     Supreme 380g      200       650
## 2: Dorito Corn Chp     Supreme 380g      200       650
There are two transactions where 200 packets of chips are bought in one transaction and both of these transactions were by the same customer with LYLTY_CARD_NBR == 226000. let see if LYLTY_CARD_NBR has another transaction.
foragedata[LYLTY_CARD_NBR == 226000,]
##          DATE STORE_NBR LYLTY_CARD_NBR TXN_ID PROD_NBR
## 1: 2018-08-19       226         226000 226201        4
## 2: 2019-05-20       226         226000 226210        4
##                           PROD_NAME PROD_QTY TOT_SALES
## 1: Dorito Corn Chp     Supreme 380g      200       650
## 2: Dorito Corn Chp     Supreme 380g      200       650
It seems the customer only made these two(2) transactions in the year being used for the analysis. The customer might purchase such quantities for commercial purposes or for an event. Therefore, we will remove the LYLTY_CARD_NBR(226000) from further analysis.
foragedata <- foragedata[LYLTY_CARD_NBR != 226000,]
Examine the data
summary(foragedata)
##       DATE              STORE_NBR     LYLTY_CARD_NBR        TXN_ID       
##  Min.   :2018-07-01   Min.   :  1.0   Min.   :   1000   Min.   :      1  
##  1st Qu.:2018-09-30   1st Qu.: 70.0   1st Qu.:  70015   1st Qu.:  67569  
##  Median :2018-12-30   Median :130.0   Median : 130367   Median : 135182  
##  Mean   :2018-12-30   Mean   :135.1   Mean   : 135530   Mean   : 135130  
##  3rd Qu.:2019-03-31   3rd Qu.:203.0   3rd Qu.: 203083   3rd Qu.: 202652  
##  Max.   :2019-06-30   Max.   :272.0   Max.   :2373711   Max.   :2415841  
##     PROD_NBR       PROD_NAME            PROD_QTY       TOT_SALES     
##  Min.   :  1.00   Length:246740      Min.   :1.000   Min.   : 1.700  
##  1st Qu.: 26.00   Class :character   1st Qu.:2.000   1st Qu.: 5.800  
##  Median : 53.00   Mode  :character   Median :2.000   Median : 7.400  
##  Mean   : 56.35                      Mean   :1.906   Mean   : 7.316  
##  3rd Qu.: 87.00                      3rd Qu.:2.000   3rd Qu.: 8.800  
##  Max.   :114.00                      Max.   :5.000   Max.   :29.500
The data is filtered. Now let’s look at the number of transaction lines over time to see if there are any obvious data issues such as missing data.
foragedata[, .N, by = DATE]
##            DATE   N
##   1: 2018-10-17 682
##   2: 2019-05-14 705
##   3: 2019-05-20 707
##   4: 2018-08-17 663
##   5: 2018-08-18 683
##  ---               
## 360: 2018-12-08 622
## 361: 2019-01-30 689
## 362: 2019-02-09 671
## 363: 2018-08-31 658
## 364: 2019-02-12 684
There’s only 364 rows, meaning only 364 dates which indicates a missing date. Let’s create a sequence of dates from 1 Jul 2018 to 30 Jun 2019 and use this to create a chart of number of transactions over time to find the missing date. Creating a chart of number of transactions over time to find the missing date. ## Creating a sequence of dates
allDates <- data.table(seq(as.Date("2018/07/01"),
as.Date("2019/06/30"), by = "day"))
setnames(allDates, "DATE")
Joining the count of transactions by date
foragedata_by_day <- merge(allDates, foragedata[, .N, by = DATE], all.x = TRUE)
Setting plot themes to format graphs
theme_set(theme_bw())
theme_update(plot.title = element_text(hjust = 0.5))
Plot transactions over time
ggplot(foragedata_by_day, aes(x = DATE, y = N)) +
geom_line(col = "blue") +
labs(x = "Day", y = "Number of transactions",
title = "Transactions over time") +
scale_x_date(breaks = "1 month") +
theme(axis.text.x = element_text(angle = 90, vjust = 0.5))
  We can see that there is an increase in purchases in December and a break in late December. Let’s zoom in on this. #### Filter to December and look at individual days
ggplot(foragedata_by_day[month(DATE) == 12, ],
aes(x = DATE, y = N)) +
geom_line(col = "blue") +
labs(x = "Day", y = "Number of transactions",
title = "Transactions over time") +
scale_x_date(breaks = "1 day") +
theme(axis.text.x = element_text(angle = 90, vjust = 0.5))
  A closer look at the chart above shows that there was no transaction recorded on ‘2018-12-25’ which happens to be christmas day when most shops are locked. Now that we can confirm that there is no missing date of transaction and no outlier in our data, we move to analyzing other features like pack size and brand names starting with the pack. size.
###Pack Size We will work this out by taking the digits that are in the PROD_NAME column.
foragedata[, PACK_SIZE := parse_number(PROD_NAME)]
Checking if the pack size looks moderate
foragedata[, .N, PACK_SIZE][order(PACK_SIZE)]
##     PACK_SIZE     N
##  1:        70  1507
##  2:        90  3008
##  3:       110 22387
##  4:       125  1454
##  5:       134 25102
##  6:       135  3257
##  7:       150 40203
##  8:       160  2970
##  9:       165 15297
## 10:       170 19983
## 11:       175 66390
## 12:       180  1468
## 13:       190  2995
## 14:       200  4473
## 15:       210  6272
## 16:       220  1564
## 17:       250  3169
## 18:       270  6285
## 19:       330 12540
## 20:       380  6416
The pack size of the chips ranges from 70g to 380g which looks moderate.
Plotting a histogram showing the number of transactions by pack size
Let’s plot a histogram of PACK_SIZE showing the number of transactions by pack size since we know that it is a categorical variable and not a continuous variable even though it is numeric.
hist(foragedata[, PACK_SIZE],
col = "green",border = "black" ,
xlab = "PACK SIZE",
ylab = "Total no of chips purchased",
main = "NO. OF CHIPS PURCHASED ACCORDING TO THEIR PACK SIZES")
 
The histogram plot above depicts that the pack sizes between 160g to 180g were bought mostly. The plot also looks sensible and free from outlier.
Brands
We will use the first word in our PROD_NAME column as the brand name.
foragedata[, BRAND := toupper(substr(PROD_NAME, 1, regexpr(pattern = ' ', PROD_NAME) - 1))]
Checking brands
foragedata[, .N, by = BRAND][order(-N)]
##          BRAND     N
##  1:     KETTLE 41288
##  2:     SMITHS 27390
##  3:   PRINGLES 25102
##  4:    DORITOS 22041
##  5:      THINS 14075
##  6:        RRD 11894
##  7:  INFUZIONS 11057
##  8:         WW 10320
##  9:       COBS  9693
## 10:   TOSTITOS  9471
## 11:   TWISTIES  9454
## 12:   TYRRELLS  6442
## 13:      GRAIN  6272
## 14:    NATURAL  6050
## 15:   CHEEZELS  4603
## 16:        CCS  4551
## 17:        RED  4427
## 18:     DORITO  3183
## 19:     INFZNS  3144
## 20:      SMITH  2963
## 21:    CHEETOS  2927
## 22:      SNBTS  1576
## 23:     BURGER  1564
## 24: WOOLWORTHS  1516
## 25:    GRNWVES  1468
## 26:   SUNBITES  1432
## 27:        NCC  1419
## 28:     FRENCH  1418
##          BRAND     N
The table above shows that the KETTLE brand of chips is mostly purchased, closely looking at the brand names shows that some names are same just that some were abbreviated, such as ‘WW’ as ‘WOOL-WORTHS’, ‘INFZNS’ as ‘INFUZIONS’ and so much more, We have to merge such names together.
foragedata[BRAND == "RED", BRAND := "RRD"]
foragedata[BRAND == "SNBTS", BRAND := "SUNBITES"]
foragedata[BRAND == "INFZNS", BRAND := "INFUZIONS"]
foragedata[BRAND == "WW", BRAND := "WOOLWORTHS"]
foragedata[BRAND == "SMITH", BRAND := "SMITHS"]
foragedata[BRAND == "NCC", BRAND := "NATURAL"]
foragedata[BRAND == "DORITO", BRAND := "DORITOS"]
foragedata[BRAND == "GRAIN", BRAND := "GRNWVES"]
foragedata[, .N, by = BRAND][order(BRAND)]
##          BRAND     N
##  1:     BURGER  1564
##  2:        CCS  4551
##  3:    CHEETOS  2927
##  4:   CHEEZELS  4603
##  5:       COBS  9693
##  6:    DORITOS 25224
##  7:     FRENCH  1418
##  8:    GRNWVES  7740
##  9:  INFUZIONS 14201
## 10:     KETTLE 41288
## 11:    NATURAL  7469
## 12:   PRINGLES 25102
## 13:        RRD 16321
## 14:     SMITHS 30353
## 15:   SUNBITES  3008
## 16:      THINS 14075
## 17:   TOSTITOS  9471
## 18:   TWISTIES  9454
## 19:   TYRRELLS  6442
## 20: WOOLWORTHS 11836
We now have 20 brand names instead of the 28 brand names we had before merging the names.It seems like our ‘foragedata’ is somewhat ready. Let’s do some exploratory data analysis on the ‘foragebehaviour’.
str(foragebehaviour)
## Classes 'data.table' and 'data.frame':   72637 obs. of  3 variables:
##  $ LYLTY_CARD_NBR  : int  1000 1002 1003 1004 1005 1007 1009 1010 1011 1012 ...
##  $ LIFESTAGE       : chr  "YOUNG SINGLES/COUPLES" "YOUNG SINGLES/COUPLES" "YOUNG FAMILIES" "OLDER SINGLES/COUPLES" ...
##  $ PREMIUM_CUSTOMER: chr  "Premium" "Mainstream" "Budget" "Mainstream" ...
##  - attr(*, ".internal.selfref")=<externalptr>
Our ‘foragedata’ is in a class of “data.frame” and “data.table” with 72,637 rows of 3 columns. Let’s look at the #### Summary of the data.
summary(foragebehaviour)
##  LYLTY_CARD_NBR     LIFESTAGE         PREMIUM_CUSTOMER  
##  Min.   :   1000   Length:72637       Length:72637      
##  1st Qu.:  66202   Class :character   Class :character  
##  Median : 134040   Mode  :character   Mode  :character  
##  Mean   : 136186                                        
##  3rd Qu.: 203375                                        
##  Max.   :2373711
The ‘foragebehaviour’ looks more like a descriptive data as the LIFESTAGE and PREMIUM_CUSTOMER columns have both class and mode of character while the LYLTY_CARD_NBR column is in numeric format. Let’s examine the LIFESTAGE and PREMIUM_CUSTOMER columns starting with that of the LIFESTAGE column.
foragebehaviour[, .N, by = PREMIUM_CUSTOMER][order(-N)]
##    PREMIUM_CUSTOMER     N
## 1:       Mainstream 29245
## 2:           Budget 24470
## 3:          Premium 18922
The PREMIUM_CUSTOMER column has three(3) classes of Mainstream, Budget, and Premium. The ‘foragebehaviour’ looks goods and seem not to need further exploration. We will go ahead to merge the ‘foragedata’ and foragebehaviour’ for further analysis.
data <- merge(foragedata, foragebehaviour, all.x = TRUE)
Checking Data
str(data)
## Classes 'data.table' and 'data.frame':   246740 obs. of  12 variables:
##  $ LYLTY_CARD_NBR  : int  1000 1002 1003 1003 1004 1005 1007 1007 1009 1010 ...
##  $ DATE            : Date, format: "2018-10-17" "2018-09-16" ...
##  $ STORE_NBR       : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ TXN_ID          : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ PROD_NBR        : int  5 58 52 106 96 86 49 10 20 51 ...
##  $ PROD_NAME       : chr  "Natural Chip        Compny SeaSalt175g" "Red Rock Deli Chikn&Garlic Aioli 150g" "Grain Waves Sour    Cream&Chives 210G" "Natural ChipCo      Hony Soy Chckn175g" ...
##  $ PROD_QTY        : int  2 1 1 1 1 1 1 1 1 2 ...
##  $ TOT_SALES       : num  6 2.7 3.6 3 1.9 2.8 3.8 2.7 5.7 8.8 ...
##  $ PACK_SIZE       : num  175 150 210 175 160 165 110 150 330 170 ...
##  $ BRAND           : chr  "NATURAL" "RRD" "GRNWVES" "NATURAL" ...
##  $ LIFESTAGE       : chr  "YOUNG SINGLES/COUPLES" "YOUNG SINGLES/COUPLES" "YOUNG FAMILIES" "YOUNG FAMILIES" ...
##  $ PREMIUM_CUSTOMER: chr  "Premium" "Mainstream" "Budget" "Budget" ...
##  - attr(*, ".internal.selfref")=<externalptr> 
##  - attr(*, "sorted")= chr "LYLTY_CARD_NBR"
As the number of rows in ‘data’ is the same as that of ‘foragedata’, we can be sure that no duplicates were created. This is because we created ‘data’ by setting “all.x = TRUE” (in other words, a left join) which means take all the rows in ‘foragedata’ and find rows with matching values in shared columns and then joining the details in these rows to the “x” or the first mentioned table. Let’s check for nulls to see if some customers were not matched.
colSums(is.na(data))
##   LYLTY_CARD_NBR             DATE        STORE_NBR           TXN_ID 
##                0                0                0                0 
##         PROD_NBR        PROD_NAME         PROD_QTY        TOT_SALES 
##                0                0                0                0 
##        PACK_SIZE            BRAND        LIFESTAGE PREMIUM_CUSTOMER 
##                0                0                0                0
Good to see that there are no nulls in our dataset, meaning that all our customers in the transaction data has been accounted for in the customer dataset. We can finally say that our data exploration is complete! We will now save the data for further analysis in our next task.
write.csv(data, "QVI_data.csv")
Data analysis on customer segments
We will start by calculating total sales by LIFESTAGE and PREMIUM_CUSTOMER and plotting the split by these segments to describe which customer segment contribute most to chip sales. #### Total sales by LIFESTAGE and PREMIUM_CUSTOMER
sales <- data[, .(SALES = sum(TOT_SALES)), . (LIFESTAGE,PREMIUM_CUSTOMER)]
sales[order(-SALES)]
##                  LIFESTAGE PREMIUM_CUSTOMER     SALES
##  1:         OLDER FAMILIES           Budget 156863.75
##  2:  YOUNG SINGLES/COUPLES       Mainstream 147582.20
##  3:               RETIREES       Mainstream 145168.95
##  4:         YOUNG FAMILIES           Budget 129717.95
##  5:  OLDER SINGLES/COUPLES           Budget 127833.60
##  6:  OLDER SINGLES/COUPLES       Mainstream 124648.50
##  7:  OLDER SINGLES/COUPLES          Premium 123537.55
##  8:               RETIREES           Budget 105916.30
##  9:         OLDER FAMILIES       Mainstream  96413.55
## 10:               RETIREES          Premium  91296.65
## 11:         YOUNG FAMILIES       Mainstream  86338.25
## 12: MIDAGE SINGLES/COUPLES       Mainstream  84734.25
## 13:         YOUNG FAMILIES          Premium  78571.70
## 14:         OLDER FAMILIES          Premium  75242.60
## 15:  YOUNG SINGLES/COUPLES           Budget  57122.10
## 16: MIDAGE SINGLES/COUPLES          Premium  54443.85
## 17:  YOUNG SINGLES/COUPLES          Premium  39052.30
## 18: MIDAGE SINGLES/COUPLES           Budget  33345.70
## 19:           NEW FAMILIES           Budget  20607.45
## 20:           NEW FAMILIES       Mainstream  15979.70
## 21:           NEW FAMILIES          Premium  10760.80
##                  LIFESTAGE PREMIUM_CUSTOMER     SALES
The above analysis shows that OLDER FAMILIES (Budget) makes the highest purchase of chips. Let’s create a plot for our analysis.
p <- ggplot(data = sales) +
geom_mosaic(aes(weight = SALES, x = product(PREMIUM_CUSTOMER, LIFESTAGE) , fill = PREMIUM_CUSTOMER)) +
labs(x = "Lifestage", y = "Premium Customer", title = "Proportion of Sales") + theme(axis.text.x = element_text(angle = 90, vjust = 0.5, size = 10))
p + geom_text(data = ggplot_build(p)$data[[1]],
aes(x = (xmin + xmax)/2 , y = (ymin + ymax)/2,
label = as.character(paste(round(.wt/sum(.wt),3)*100, '%'))))
  As we can see from the above plot, the OLDER FAMILIES (Budget) made the highest purchase, followed by YOUNG SINGLES/COUPLES (Mainstream), then RETIREES (Mainstream). Going further into the analysis, let’s see if the higher sales are due to there being more customers who buy chips.
customers <- data[, .(CUSTOMERS = uniqueN(LYLTY_CARD_NBR)), .(LIFESTAGE, PREMIUM_CUSTOMER)][order(-CUSTOMERS)]
customers
##                  LIFESTAGE PREMIUM_CUSTOMER CUSTOMERS
##  1:  YOUNG SINGLES/COUPLES       Mainstream      7917
##  2:               RETIREES       Mainstream      6358
##  3:  OLDER SINGLES/COUPLES       Mainstream      4858
##  4:  OLDER SINGLES/COUPLES           Budget      4849
##  5:  OLDER SINGLES/COUPLES          Premium      4682
##  6:         OLDER FAMILIES           Budget      4611
##  7:               RETIREES           Budget      4385
##  8:         YOUNG FAMILIES           Budget      3953
##  9:               RETIREES          Premium      3812
## 10:  YOUNG SINGLES/COUPLES           Budget      3647
## 11: MIDAGE SINGLES/COUPLES       Mainstream      3298
## 12:         OLDER FAMILIES       Mainstream      2788
## 13:         YOUNG FAMILIES       Mainstream      2685
## 14:  YOUNG SINGLES/COUPLES          Premium      2480
## 15:         YOUNG FAMILIES          Premium      2398
## 16: MIDAGE SINGLES/COUPLES          Premium      2369
## 17:         OLDER FAMILIES          Premium      2231
## 18: MIDAGE SINGLES/COUPLES           Budget      1474
## 19:           NEW FAMILIES           Budget      1087
## 20:           NEW FAMILIES       Mainstream       830
## 21:           NEW FAMILIES          Premium       575
##                  LIFESTAGE PREMIUM_CUSTOMER CUSTOMERS
As we can see from the analysis above, the YOUNG SINGLES/COUPLES (Mainstream) has highest number of customers. Let’s create a plot.
labels <- c("A", "b", "c", "D", "e", "f", "G")
p <- ggplot(data = customers) +
geom_mosaic(aes(weight = CUSTOMERS, x = product(PREMIUM_CUSTOMER, LIFESTAGE), fill = PREMIUM_CUSTOMER)) +
labs(x = "Lifestage", y = "Premium Customer", title = "Proportion of Customers") +
theme(axis.text.x = element_text(angle = 90, vjust = 0.5, size = 10)) + scale_x_productlist(labels = labels)
p +
geom_text(data = ggplot_build(p)$data[[1]],
aes(x = (xmin + xmax)/2 , y = (ymin + ymax)/2,
label = as.character(paste(round(.wt/sum(.wt),3)*100, '%'))))
 
The above plot shows that there are more YOUNG SINGLES/COUPLES (Mainstream) and RETIREES (Mainstream) who buy chips. This contributes to there being more sales to these customer segments but this is not a major driver for the OLDER FAMILIES (Budget) segment. Higher sales may also be driven by more units of chips being bought per customer. Let’s have a look at this next. #### Average number of units per customer by LIFESTAGE and PREMIUM_CUSTOMER
avg_units <- data[, .(AVG = sum(PROD_QTY)/uniqueN(LYLTY_CARD_NBR)), .(LIFESTAGE, PREMIUM_CUSTOMER)][order(-AVG)]
avg_units
##                  LIFESTAGE PREMIUM_CUSTOMER      AVG
##  1:         OLDER FAMILIES       Mainstream 9.255380
##  2:         OLDER FAMILIES           Budget 9.076773
##  3:         OLDER FAMILIES          Premium 9.071717
##  4:         YOUNG FAMILIES           Budget 8.722995
##  5:         YOUNG FAMILIES          Premium 8.716013
##  6:         YOUNG FAMILIES       Mainstream 8.638361
##  7:  OLDER SINGLES/COUPLES           Budget 6.781398
##  8:  OLDER SINGLES/COUPLES          Premium 6.769543
##  9:  OLDER SINGLES/COUPLES       Mainstream 6.712021
## 10: MIDAGE SINGLES/COUPLES       Mainstream 6.432080
## 11:               RETIREES           Budget 6.141847
## 12:               RETIREES          Premium 6.103358
## 13: MIDAGE SINGLES/COUPLES          Premium 6.078514
## 14: MIDAGE SINGLES/COUPLES           Budget 6.026459
## 15:               RETIREES       Mainstream 5.925920
## 16:           NEW FAMILIES       Mainstream 4.891566
## 17:           NEW FAMILIES           Budget 4.821527
## 18:           NEW FAMILIES          Premium 4.815652
## 19:  YOUNG SINGLES/COUPLES       Mainstream 4.575597
## 20:  YOUNG SINGLES/COUPLES          Premium 4.264113
## 21:  YOUNG SINGLES/COUPLES           Budget 4.250069
##                  LIFESTAGE PREMIUM_CUSTOMER      AVG
LIFESTAGE PREMIUM_CUSTOMER AVG
The table above shows that the OLDER FAMILIES has the highest average unit of chips per customer. i.e. the bought more chips per customer. Let’s create a plot for the above analysis.
ggplot(data = avg_units,
aes(weight = AVG, x = LIFESTAGE, fill = PREMIUM_CUSTOMER)) +
geom_bar(position = position_dodge()) +
labs(x = "Lifestage", y = "Avg units per transaction",
title = "Units Per Customer") +
theme(axis.text.x = element_text(angle = 90, vjust = 0.75, size = 10))
 
The plot above shows that the OLDER FAMILIES and YOUNG FAMILIES purchased more of the chips per customer on the average. The average price per unit chips bought for each customer segment seems to be a driver of total sales. Let’s look into that. #### Average price per unit by LIFESTAGE and PREMIUM_CUSTOMER
avg_price <- data[, .(AVG = sum(TOT_SALES)/sum(PROD_QTY)), .(LIFESTAGE, PREMIUM_CUSTOMER)][order(-AVG)]
avg_price
##                  LIFESTAGE PREMIUM_CUSTOMER      AVG
##  1:  YOUNG SINGLES/COUPLES       Mainstream 4.074043
##  2: MIDAGE SINGLES/COUPLES       Mainstream 3.994449
##  3:           NEW FAMILIES       Mainstream 3.935887
##  4:               RETIREES           Budget 3.932731
##  5:           NEW FAMILIES           Budget 3.931969
##  6:               RETIREES          Premium 3.924037
##  7:  OLDER SINGLES/COUPLES          Premium 3.897698
##  8:  OLDER SINGLES/COUPLES           Budget 3.887529
##  9:           NEW FAMILIES          Premium 3.886168
## 10:               RETIREES       Mainstream 3.852986
## 11:  OLDER SINGLES/COUPLES       Mainstream 3.822753
## 12: MIDAGE SINGLES/COUPLES          Premium 3.780823
## 13:         YOUNG FAMILIES           Budget 3.761903
## 14:         YOUNG FAMILIES          Premium 3.759232
## 15: MIDAGE SINGLES/COUPLES           Budget 3.753878
## 16:         OLDER FAMILIES           Budget 3.747969
## 17:         OLDER FAMILIES       Mainstream 3.736380
## 18:         YOUNG FAMILIES       Mainstream 3.722439
## 19:         OLDER FAMILIES          Premium 3.717703
## 20:  YOUNG SINGLES/COUPLES          Premium 3.692889
## 21:  YOUNG SINGLES/COUPLES           Budget 3.685297
##                  LIFESTAGE PREMIUM_CUSTOMER      AVG
ggplot(data = avg_price,
aes(weight = AVG, x = LIFESTAGE, fill = PREMIUM_CUSTOMER)) +
geom_bar(position = position_dodge()) +
labs(x = "Lifestage", y = "Avg price per unit",
title = "Price Per Unit") +
theme(axis.text.x = element_text(angle = 90, vjust = 0.5))
  The above plot shows that the Mainstream YOUNG SINGLES/COUPLES and MIDAGE SINGLES/COUPLES are more willing to pay more per packet of chips compared to their Budget and Premium counterparts. This may be due to Premium shoppers being more likely to buy healthy snacks and when they buy chips, this is mainly for entertainment purposes rather than their own consumption. This is also supported by there being fewer Premium YOUNG SINGLES/COUPLES and MIDAGE SINGLES/COUPLES buying chips compared to their Mainstream counterparts. The difference in average price per unit isn’t large, let’s investigate if this difference is statistically different. #### Performing an independent t-test between Mainstream vs Premium and Budget MIDAGE SINGLES/COUPLES and YOUNG SINGLES/COUPLES.
pricePerUnit <- data[, price := TOT_SALES/PROD_QTY]
t.test(data[LIFESTAGE %in% c("YOUNG SINGLES/COUPLES", "MIDAGE SINGLES/COUPLES") & PREMIUM_CUSTOMER == "Mainstream", price],
data[LIFESTAGE %in% c("YOUNG SINGLES/COUPLES", "MIDAGE SINGLES/COUPLES") & PREMIUM_CUSTOMER != "Mainstream", price], alternative = "greater")
## 
##  Welch Two Sample t-test
## 
## data:  data[LIFESTAGE %in% c("YOUNG SINGLES/COUPLES", "MIDAGE SINGLES/COUPLES") & PREMIUM_CUSTOMER == "Mainstream", price] and data[LIFESTAGE %in% c("YOUNG SINGLES/COUPLES", "MIDAGE SINGLES/COUPLES") & PREMIUM_CUSTOMER != "Mainstream", price]
## t = 37.624, df = 54791, p-value < 2.2e-16
## alternative hypothesis: true difference in means is greater than 0
## 95 percent confidence interval:
##  0.3187234       Inf
## sample estimates:
## mean of x mean of y 
##  4.039786  3.706491
The t-test results in a p-value of 0.00000000000000022 signifies that the unit price for Mainstream YOUNG SINGLES/COUPLES and MIDAGE SINGLES/COUPLES are significantly higher than that of Budget or Premium, SINGLES/COUPLES and MIDAGE SINGLES/COUPLES. We have found quite a few interesting insights that we can dive deeper into. Let’s see if we can target customer segments that contribute the most to sales to retain them or further increase sales. Let’s look at YOUNG SINGLES/COUPLES (Mainstream). For instance, let’s find out if they tend to buy a particular brand of chips. #### YOUNG SINGLES/COUPLES (Mainstream) brands and others
segment <- data[LIFESTAGE == "YOUNG SINGLES/COUPLES" &
PREMIUM_CUSTOMER == "Mainstream",]
others <- data[!(LIFESTAGE == "YOUNG SINGLES/COUPLES" &
PREMIUM_CUSTOMER == "Mainstream"),]
Preferred brand compared to the rest of the population
qtySegment <- segment[, sum(PROD_QTY)]
qtyOthers <- others[, sum(PROD_QTY)]
qtySegmentBrand <- segment[, .(TargetSegment =
sum(PROD_QTY)/qtySegment), by = BRAND]
qtyOthersBrand <- others[, .(Others =
sum(PROD_QTY)/qtyOthers), by = BRAND]
brandProportions <- merge(qtySegmentBrand, qtyOthersBrand)[, AffinityToBrand := TargetSegment/Others]
brandProportions[order(-AffinityToBrand)]
##          BRAND TargetSegment      Others AffinityToBrand
##  1:   TYRRELLS   0.031552795 0.025692464       1.2280953
##  2:   TWISTIES   0.046183575 0.037876520       1.2193194
##  3:    DORITOS   0.122760524 0.101074684       1.2145526
##  4:     KETTLE   0.197984817 0.165553442       1.1958967
##  5:   TOSTITOS   0.045410628 0.037977861       1.1957131
##  6:   PRINGLES   0.119420290 0.100634769       1.1866703
##  7:       COBS   0.044637681 0.039048861       1.1431238
##  8:  INFUZIONS   0.064679089 0.057064679       1.1334347
##  9:      THINS   0.060372671 0.056986370       1.0594230
## 10:    GRNWVES   0.032712215 0.031187957       1.0488733
## 11:   CHEEZELS   0.017971014 0.018646902       0.9637534
## 12:     SMITHS   0.096369910 0.124583692       0.7735355
## 13:     FRENCH   0.003947550 0.005758060       0.6855694
## 14:    CHEETOS   0.008033126 0.012066591       0.6657329
## 15:        RRD   0.043809524 0.067493678       0.6490908
## 16:    NATURAL   0.019599724 0.030853989       0.6352412
## 17:        CCS   0.011180124 0.018895650       0.5916771
## 18:   SUNBITES   0.006349206 0.012580210       0.5046980
## 19: WOOLWORTHS   0.024099379 0.049427188       0.4875733
## 20:     BURGER   0.002926156 0.006596434       0.4435967
The above analysis shows that the YOUNG SINGLES/COUPLES (Mainstream) are 22.8% more likely to buy TYRRELLS brand of chips compared to the rest of the population.
Let’s also find out if our target segment tends to buy larger packs of chips. #### Preferred pack size compared to the rest of the population
qtySegmentPack <- segment[, .(TargetSegment =
sum(PROD_QTY)/qtySegment), by = PACK_SIZE]
qtyOtherPack <- others[, .(Others =
sum(PROD_QTY)/qtyOthers), by = PACK_SIZE]
packProportions <- merge(qtySegmentPack, qtyOtherPack)[, AffinityToPack := TargetSegment/Others]
packProportions[order(-AffinityToPack)]
##     PACK_SIZE TargetSegment      Others AffinityToPack
##  1:       270   0.031828847 0.025095929      1.2682873
##  2:       380   0.032160110 0.025584213      1.2570295
##  3:       330   0.061283644 0.050161917      1.2217166
##  4:       134   0.119420290 0.100634769      1.1866703
##  5:       110   0.106280193 0.089791190      1.1836372
##  6:       210   0.029123533 0.025121265      1.1593180
##  7:       135   0.014768806 0.013075403      1.1295106
##  8:       250   0.014354727 0.012780590      1.1231662
##  9:       170   0.080772947 0.080985964      0.9973697
## 10:       150   0.157598344 0.163420656      0.9643722
## 11:       175   0.254989648 0.270006956      0.9443818
## 12:       165   0.055652174 0.062267662      0.8937572
## 13:       190   0.007481021 0.012442016      0.6012708
## 14:       180   0.003588682 0.006066692      0.5915385
## 15:       160   0.006404417 0.012372920      0.5176157
## 16:        90   0.006349206 0.012580210      0.5046980
## 17:       125   0.003008972 0.006036750      0.4984423
## 18:       200   0.008971705 0.018656115      0.4808989
## 19:        70   0.003036577 0.006322350      0.4802924
## 20:       220   0.002926156 0.006596434      0.4435967
The analysis above shows that YOUNG SINGLES/COUPLES (Mainstream) are 26.83% more likely to buy 270g pack size of chips compared to the rest of the population. Let’s also look into MIDAGE SINGLES/COUPLES (Mainstream) preferred brand and pack size of chips 18 compared to the rest of the population starting with the brand.
MIDAGE SINGLES/COUPLES (Mainstream) brands and others
segment1 <- data[LIFESTAGE == "MIDAGE SINGLES/COUPLES" &
PREMIUM_CUSTOMER == "Mainstream",]
others1 <- data[!(LIFESTAGE == "MIDAGE SINGLES/COUPLES" &
PREMIUM_CUSTOMER == "Mainstream"),]
Preferred brand compared to the rest of the population
qtySegment1 <- segment1[, sum(PROD_QTY)]
qtyOthers1 <- others1[, sum(PROD_QTY)]
qtySegmentBrand1 <- segment1[, .(TargetSegment =
sum(PROD_QTY)/qtySegment1), by = BRAND]
qtyOthersBrand1 <- others1[, .(Others =
sum(PROD_QTY)/qtyOthers1), by = BRAND]
brandProportions1 <- merge(qtySegmentBrand1, qtyOthersBrand1)[, AffinityToBrand := TargetSegment/Others]
brandProportions1[order(-AffinityToBrand)]
##          BRAND TargetSegment      Others AffinityToBrand
##  1:     KETTLE   0.192570594 0.166893002       1.1538566
##  2:   TWISTIES   0.043935323 0.038260320       1.1483261
##  3:       COBS   0.044831000 0.039226512       1.1428750
##  4:   TOSTITOS   0.043558195 0.038313750       1.1368816
##  5:  INFUZIONS   0.061754584 0.057457267       1.0747915
##  6:   CHEEZELS   0.019846321 0.018535751       1.0707049
##  7:    DORITOS   0.108895489 0.102454217       1.0628698
##  8:   TYRRELLS   0.026917456 0.026107225       1.0310348
##  9:    GRNWVES   0.031961533 0.031274350       1.0219727
## 10:   PRINGLES   0.104181398 0.101982252       1.0215640
## 11:      THINS   0.057181917 0.057250226       0.9988068
## 12:     SMITHS   0.115165229 0.122753158       0.9381855
## 13:    CHEETOS   0.010135294 0.011832515       0.8565630
## 14:        RRD   0.054259181 0.066208653       0.8195180
## 15:    NATURAL   0.024654693 0.030239144       0.8153238
## 16:        CCS   0.014425117 0.018484548       0.7803879
## 17:     BURGER   0.004336963 0.006407145       0.6768948
## 18:     FRENCH   0.003818413 0.005703651       0.6694683
## 19: WOOLWORTHS   0.031348701 0.048238369       0.6498707
## 20:   SUNBITES   0.006222599 0.012377946       0.5027166
The above analysis shows that the MIDAGE SINGLES/COUPLES (Mainstream) are 15.39% more likely to buy KETTLE brand of chips compared to the rest of the population. #### Preferred pack size compared to the rest of the population
qtySegmentPack1 <- segment1[, .(TargetSegment =
sum(PROD_QTY)/qtySegment1), by = PACK_SIZE]
qtyOtherPack1 <- others1[, .(Others =
sum(PROD_QTY)/qtyOthers1), by = PACK_SIZE]
packProportions1 <- merge(qtySegmentPack1, qtyOtherPack1)[, AffinityToPack := TargetSegment/Others]
packProportions1[order(-AffinityToPack)]
##     PACK_SIZE TargetSegment      Others AffinityToPack
##  1:       270   0.030735870 0.025372563      1.2113821
##  2:       330   0.059727526 0.050607098      1.1802203
##  3:       110   0.102060058 0.090541557      1.1272178
##  4:       135   0.014519398 0.013143776      1.1046596
##  5:       210   0.027718852 0.025321359      1.0946826
##  6:       380   0.028425965 0.025980329      1.0941342
##  7:       250   0.013199453 0.012887757      1.0241854
##  8:       134   0.104181398 0.101982252      1.0215640
##  9:       175   0.268561731 0.268864123      0.9988753
## 10:       150   0.160420497 0.163092795      0.9836149
## 11:       170   0.079385283 0.081044378      0.9795286
## 12:       165   0.057087635 0.061978779      0.9210836
## 13:       190   0.010323858 0.012141963      0.8502626
## 14:       160   0.009051054 0.012048461      0.7512207
## 15:        70   0.004525527 0.006142222      0.7367899
## 16:       180   0.004242681 0.005952991      0.7126975
## 17:       220   0.004336963 0.006407145      0.6768948
## 18:       200   0.012068071 0.018186230      0.6635829
## 19:       125   0.003205581 0.005926276      0.5409100
## 20:        90   0.006222599 0.012377946      0.5027166
The analysis above shows that MIDAGE SINGLES/COUPLES (Mainstream) are 21.14% more likely to buy 270g pack size of chips compared to the rest of the population.
Findings The following are the findings derived from the analysis: 1. The OLDER FAMILIES (Budget) made the highest purchase of chips, followed by YOUNG SINGLES/COUPLES (Mainstream), then RETIREES (Mainstream). 2. There are more YOUNG SINGLES/COUPLES (Mainstream) and RETIREES (Mainstream) who buy chips. This contributes to there being more sales to these customer segments but this is not a major driver for the OLDER FAMILIES (Budget) segment. 3. The OLDER FAMILIES and YOUNG FAMILIES purchased more of the chips per customer on the average. 4. The Mainstream YOUNG SINGLES/COUPLES and MIDAGE SINGLES/COUPLES are more willing to pay more per packet of chips compared to their Budget and Premium counterparts. This may be due to Premium shoppers being more likely to buy healthy snacks and when they buy chips, this is mainly for entertainment purposes rather than their own consumption. This is also supported by there being fewer Premium YOUNG SINGLES/COUPLES and MIDAGE SINGLES/COUPLES buying chips compared to their Mainstream counterparts. 5. The t-test results in a p-value of 0.00000000000000022 signifies that the unit price for Mainstream YOUNG SINGLES/COUPLES and MIDAGE SINGLES/COUPLES are significantly higher than that of Budget or Premium, SINGLES/COUPLES and MIDAGE SINGLES/COUPLES. 6. The YOUNG SINGLES/COUPLES (Mainstream) are 22.8% more likely to buy TYRRELLS brand of chips and also 26.83% more likely to buy 270g pack size of chips compared to the rest of the population. 7. The MIDAGE SINGLES/COUPLES (Mainstream) are 15.39% more likely to buy KETTLE brand of chips and 21.14% more likely to buy 270g pack size of chips compared to the rest of the population.
Recommendations The followings are my top three(3) recommendations for Quantium Supermarket based on my findings. • Quantum supermarket should target more the Mainstream YOUNG SINGLES/COUPLES and MIDAGE SINGLES/COUPLES who are more willing to pay more per packet of chips. • They should buy more of the TYRRELLS and KETTLE brands of chips which the Mainstream YOUNG SINGLES/COUPLES and MIDAGE SINGLES/COUPLES customer segments, who are more willing to pay more per packet of chips purchased most. • They should also buy more of the 270g pack size of chips which the two(2) main target customer segments, Mainstream YOUNG SINGLES/COUPLES and MIDAGE SINGLES/COUPLES purchased most.
Conclusion The Mainstream YOUNG SINGLES/COUPLES and MIDAGE SINGLES/COUPLES customer segments are our major target and therefore, they should be focused more on as they are the top two(2) major customer segment who are more willing to pay more per packet of chips.
