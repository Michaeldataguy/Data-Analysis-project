 library(tinytex)
> foragedata <- read.csv("QVI_transaction_data.csv", stringsAsFactors = FALSE)
> foragebehaviour <- read.csv("QVI_purchase_behaviour.csv")
> foragedata <- data.table(foragedata)
Error in data.table(foragedata) : could not find function "data.table"
> library(data.table)
package �data.table� was built under R version 4.0.5data.table 1.14.2 using 2 threads (see ?getDTthreads).  Latest news: r-datatable.com
> library(ggplot2)
package �ggplot2� was built under R version 4.0.5
> library(ggmosaic)
package �ggmosaic� was built under R version 4.0.5Registered S3 method overwritten by 'htmlwidgets':
  method           from         
  print.htmlwidget tools:rstudio
> library(readr)
package �readr� was built under R version 4.0.5
> library(tidyverse)
package �tidyverse� was built under R version 4.0.5Registered S3 methods overwritten by 'dbplyr':
  method         from
  print.tbl_lazy     
  print.tbl_sql      
-- Attaching packages ------------------------------------------------------------- tidyverse 1.3.1 --
v tibble  3.1.4     v dplyr   1.0.7
v tidyr   1.1.4     v stringr 1.4.0
v purrr   0.3.4     v forcats 0.5.1
package �tibble� was built under R version 4.0.5package �tidyr� was built under R version 4.0.5package �purrr� was built under R version 4.0.5package �dplyr� was built under R version 4.0.5package �stringr� was built under R version 4.0.5package �forcats� was built under R version 4.0.5-- Conflicts ---------------------------------------------------------------- tidyverse_conflicts() --
x dplyr::between()   masks data.table::between()
x tidyr::expand()    masks Matrix::expand()
x dplyr::filter()    masks stats::filter()
x dplyr::first()     masks data.table::first()
x dplyr::lag()       masks stats::lag()
x dplyr::last()      masks data.table::last()
x tidyr::pack()      masks Matrix::pack()
x dplyr::recode()    masks arules::recode()
x purrr::transpose() masks data.table::transpose()
x tidyr::unpack()    masks Matrix::unpack()
> foragedata <- read.csv("QVI_transaction_data.csv", stringsAsFactors = FALSE)
> foragebehaviour <- read.csv("QVI_purchase_behaviour.csv")
> foragedata <- data.table(foragedata)
> foragebehaviour <- data.table(foragebehaviour)
> str(foragedata)
Classes ‘data.table’ and 'data.frame':	264836 obs. of  8 variables:
 $ DATE          : int  43390 43599 43605 43329 43330 43604 43601 43601 43332 43330 ...
 $ STORE_NBR     : int  1 1 1 2 2 4 4 4 5 7 ...
 $ LYLTY_CARD_NBR: int  1000 1307 1343 2373 2426 4074 4149 4196 5026 7150 ...
 $ TXN_ID        : int  1 348 383 974 1038 2982 3333 3539 4525 6900 ...
 $ PROD_NBR      : int  5 66 61 69 108 57 16 24 42 52 ...
 $ PROD_NAME     : chr  "Natural Chip        Compny SeaSalt175g" "CCs Nacho Cheese    175g" "Smiths Crinkle Cut  Chips Chicken 170g" "Smiths Chip Thinly  S/Cream&Onion 175g" ...
 $ PROD_QTY      : int  2 3 2 5 3 1 1 1 1 2 ...
 $ TOT_SALES     : num  6 6.3 2.9 15 13.8 5.1 5.7 3.6 3.9 7.2 ...
 - attr(*, ".internal.selfref")=<externalptr> 
> head(foragedata)
> foragedata$DATE <- as.Date(foragedata$DATE, origin = "1899-12-30")
> head(foragedata)
> foragedata[ , .N, PROD_NAME]
> productWords <- data.table(unlist(strsplit(unique(foragedata[, PROD_NAME]), " ")))
> setnames(productWords, 'words')
> productwords <- productWords[grepl("\\d", words) == FALSE,] 
> productWords <- productWords[grepl("[:alpha:]", words), ]
> productWords[, .N, words][order(N, decreasing = TRUE)]
> 
> foragedata[, SALSA := grepl("salsa", tolower(PROD_NAME))]
> foragedata <- foragedata[SALSA == FALSE, ][, SALSA := NULL]
> 
> summary(foragedata)
      DATE              STORE_NBR     LYLTY_CARD_NBR        TXN_ID           PROD_NBR     
 Min.   :2018-07-01   Min.   :  1.0   Min.   :   1000   Min.   :      1   Min.   :  1.00  
 1st Qu.:2018-09-30   1st Qu.: 70.0   1st Qu.:  70015   1st Qu.:  67569   1st Qu.: 26.00  
 Median :2018-12-30   Median :130.0   Median : 130367   Median : 135183   Median : 53.00  
 Mean   :2018-12-30   Mean   :135.1   Mean   : 135531   Mean   : 135131   Mean   : 56.35  
 3rd Qu.:2019-03-31   3rd Qu.:203.0   3rd Qu.: 203084   3rd Qu.: 202654   3rd Qu.: 87.00  
 Max.   :2019-06-30   Max.   :272.0   Max.   :2373711   Max.   :2415841   Max.   :114.00  
  PROD_NAME            PROD_QTY         TOT_SALES      
 Length:246742      Min.   :  1.000   Min.   :  1.700  
 Class :character   1st Qu.:  2.000   1st Qu.:  5.800  
 Mode  :character   Median :  2.000   Median :  7.400  
                    Mean   :  1.908   Mean   :  7.321  
                    3rd Qu.:  2.000   3rd Qu.:  8.800  
                    Max.   :200.000   Max.   :650.000  
> foragedata[PROD_QTY == 200]
> foragedata[LYLTY_CARD_NBR == 226000,]
> summary(foragedata)
      DATE              STORE_NBR     LYLTY_CARD_NBR        TXN_ID           PROD_NBR     
 Min.   :2018-07-01   Min.   :  1.0   Min.   :   1000   Min.   :      1   Min.   :  1.00  
 1st Qu.:2018-09-30   1st Qu.: 70.0   1st Qu.:  70015   1st Qu.:  67569   1st Qu.: 26.00  
 Median :2018-12-30   Median :130.0   Median : 130367   Median : 135183   Median : 53.00  
 Mean   :2018-12-30   Mean   :135.1   Mean   : 135531   Mean   : 135131   Mean   : 56.35  
 3rd Qu.:2019-03-31   3rd Qu.:203.0   3rd Qu.: 203084   3rd Qu.: 202654   3rd Qu.: 87.00  
 Max.   :2019-06-30   Max.   :272.0   Max.   :2373711   Max.   :2415841   Max.   :114.00  
  PROD_NAME            PROD_QTY         TOT_SALES      
 Length:246742      Min.   :  1.000   Min.   :  1.700  
 Class :character   1st Qu.:  2.000   1st Qu.:  5.800  
 Mode  :character   Median :  2.000   Median :  7.400  
                    Mean   :  1.908   Mean   :  7.321  
                    3rd Qu.:  2.000   3rd Qu.:  8.800  
                    Max.   :200.000   Max.   :650.000  
> foragedata[, .N, by = DATE]
> allDates <- data.table(seq(as.Date("2018/07/01"),
+ as.Date("2019/06/30"), by = "day"))
> setnames(allDates, "DATE")
> foragedata_by_day <- merge(allDates, foragedata[, .N, by = DATE], all.x = TRUE)
> theme_set(theme_bw())
> theme_update(plot.title = element_text(hjust = 0.5))
> ggplot(foragedata_by_day, aes(x = DATE, y = N)) +
+ geom_line(col = "blue") +
+ labs(x = "Day", y = "Number of transactions",
+ title = "Transactions over time") +
+ scale_x_date(breaks = "1 month") +
+ theme(axis.text.x = element_text(angle = 90, vjust = 0.5))
> ggplot(foragedata_by_day[month(DATE) == 12, ],
+ aes(x = DATE, y = N)) +
+ geom_line(col = "blue") +
+ labs(x = "Day", y = "Number of transactions",
+ title = "Transactions over time") +
+ scale_x_date(breaks = "1 day") +
+ theme(axis.text.x = element_text(angle = 90, vjust = 0.5))
> foragedata[, PACK_SIZE := parse_number(PROD_NAME)]
> foragedata[, .N, PACK_SIZE][order(PACK_SIZE)]
> hist(foragedata[, PACK_SIZE],
+ col = "green",border = "black" ,
+ xlab = "PACK SIZE",
+ ylab = "Total no of chips purchased",
+ main = "NO. OF CHIPS PURCHASED ACCORDING TO THEIR PACK SIZES")
> foragedata[, BRAND := toupper(substr(PROD_NAME, 1, regexpr(pattern = ' ', PROD_NAME) - 1))]
> foragedata[, .N, by = BRAND][order(-N)]
> foragedata[BRAND == "RED", BRAND := "RRD"]
> foragedata[BRAND == "SNBTS", BRAND := "SUNBITES"]
> foragedata[BRAND == "INFZNS", BRAND := "INFUZIONS"]
> foragedata[BRAND == "WW", BRAND := "WOOLWORTHS"]
> foragedata[BRAND == "SMITH", BRAND := "SMITHS"]
> foragedata[BRAND == "NCC", BRAND := "NATURAL"]
> foragedata[BRAND == "DORITO", BRAND := "DORITOS"]
> foragedata[BRAND == "GRAIN", BRAND := "GRNWVES"]
> foragedata[, .N, by = BRAND][order(BRAND)]
> str(foragebehaviour)
Classes ‘data.table’ and 'data.frame':	72637 obs. of  3 variables:
 $ LYLTY_CARD_NBR  : int  1000 1002 1003 1004 1005 1007 1009 1010 1011 1012 ...
 $ LIFESTAGE       : chr  "YOUNG SINGLES/COUPLES" "YOUNG SINGLES/COUPLES" "YOUNG FAMILIES" "OLDER SINGLES/COUPLES" ...
 $ PREMIUM_CUSTOMER: chr  "Premium" "Mainstream" "Budget" "Mainstream" ...
 - attr(*, ".internal.selfref")=<externalptr> 
> summary(foragebehaviour)
 LYLTY_CARD_NBR     LIFESTAGE         PREMIUM_CUSTOMER  
 Min.   :   1000   Length:72637       Length:72637      
 1st Qu.:  66202   Class :character   Class :character  
 Median : 134040   Mode  :character   Mode  :character  
 Mean   : 136186                                        
 3rd Qu.: 203375                                        
 Max.   :2373711                                        
> foragebehaviour[, .N, by = PREMIUM_CUSTOMER][order(-N)]
> data <- merge(foragedata, foragebehaviour, all.x = TRUE)
> str(data)
Classes ‘data.table’ and 'data.frame':	246742 obs. of  12 variables:
 $ LYLTY_CARD_NBR  : int  1000 1002 1003 1003 1004 1005 1007 1007 1009 1010 ...
 $ DATE            : Date, format: "2018-10-17" "2018-09-16" "2019-03-07" ...
 $ STORE_NBR       : int  1 1 1 1 1 1 1 1 1 1 ...
 $ TXN_ID          : int  1 2 3 4 5 6 7 8 9 10 ...
 $ PROD_NBR        : int  5 58 52 106 96 86 49 10 20 51 ...
 $ PROD_NAME       : chr  "Natural Chip        Compny SeaSalt175g" "Red Rock Deli Chikn&Garlic Aioli 150g" "Grain Waves Sour    Cream&Chives 210G" "Natural ChipCo      Hony Soy Chckn175g" ...
 $ PROD_QTY        : int  2 1 1 1 1 1 1 1 1 2 ...
 $ TOT_SALES       : num  6 2.7 3.6 3 1.9 2.8 3.8 2.7 5.7 8.8 ...
 $ PACK_SIZE       : num  175 150 210 175 160 165 110 150 330 170 ...
 $ BRAND           : chr  "NATURAL" "RRD" "GRNWVES" "NATURAL" ...
 $ LIFESTAGE       : chr  "YOUNG SINGLES/COUPLES" "YOUNG SINGLES/COUPLES" "YOUNG FAMILIES" "YOUNG FAMILIES" ...
 $ PREMIUM_CUSTOMER: chr  "Premium" "Mainstream" "Budget" "Budget" ...
 - attr(*, ".internal.selfref")=<externalptr> 
 - attr(*, "sorted")= chr "LYLTY_CARD_NBR"
> write.csv(data, "QVI_data.csv")
> sales <- data[, .(SALES = sum(TOT_SALES)), . (LIFESTAGE,PREMIUM_CUSTOMER)]
> sales[order(-SALES)]
> p <- ggplot(data = sales) +
+ geom_mosaic(aes(weight = SALES, x = product(PREMIUM_CUSTOMER, LIFESTAGE) , fill = PREMIUM_CUSTOMER)) +
+ labs(x = "Lifestage", y = "Premium Customer", title = "Proportion of Sales") + theme(axis.text.x = element_text(angle = 90, vjust = 0.5, size = 10))
> p + geom_text(data = ggplot_build(p)$data[[1]],
+ aes(x = (xmin + xmax)/2 , y = (ymin + ymax)/2,
+ label = as.character(paste(round(.wt/sum(.wt),3)*100, '%'))))
> customers <- data[, .(CUSTOMERS = uniqueN(LYLTY_CARD_NBR)), .(LIFESTAGE, PREMIUM_CUSTOMER)][order(-CUSTOMERS)]
> customers
> labels <- c("A", "b", "c", "D", "e", "f", "G")
> p <- ggplot(data = customers) +
+ geom_mosaic(aes(weight = CUSTOMERS, x = product(PREMIUM_CUSTOMER, LIFESTAGE), fill = PREMIUM_CUSTOMER)) +
+ labs(x = "Lifestage", y = "Premium Customer", title = "Proportion of Customers") +
+ theme(axis.text.x = element_text(angle = 90, vjust = 0.5, size = 10)) + scale_x_productlist(labels = labels)
> p +
+ geom_text(data = ggplot_build(p)$data[[1]],
+ aes(x = (xmin + xmax)/2 , y = (ymin + ymax)/2,
+ label = as.character(paste(round(.wt/sum(.wt),3)*100, '%'))))
> avg_units <- data[, .(AVG = sum(PROD_QTY)/uniqueN(LYLTY_CARD_NBR)), .(LIFESTAGE, PREMIUM_CUSTOMER)][order(-AVG)]
> avg_units
> ggplot(data = avg_units,
+ aes(weight = AVG, x = LIFESTAGE, fill = PREMIUM_CUSTOMER)) +
+ geom_bar(position = position_dodge()) +
+ labs(x = "Lifestage", y = "Avg units per transaction",
+ title = "Units Per Customer") +
+ theme(axis.text.x = element_text(angle = 90, vjust = 0.75, size = 10))
> avg_price <- data[, .(AVG = sum(TOT_SALES)/sum(PROD_QTY)), .(LIFESTAGE, PREMIUM_CUSTOMER)][order(-AVG)]
> avg_price
> ggplot(data = avg_price,
+ aes(weight = AVG, x = LIFESTAGE, fill = PREMIUM_CUSTOMER)) +
+ geom_bar(position = position_dodge()) +
+ labs(x = "Lifestage", y = "Avg price per unit",
+ title = "Price Per Unit") +
+ theme(axis.text.x = element_text(angle = 90, vjust = 0.5))
> pricePerUnit <- data[, price := TOT_SALES/PROD_QTY]
> t.test(data[LIFESTAGE %in% c("YOUNG SINGLES/COUPLES", "MIDAGE SINGLES/COUPLES") & PREMIUM_CUSTOMER == "Mainstream", price],
+ data[LIFESTAGE %in% c("YOUNG SINGLES/COUPLES", "MIDAGE SINGLES/COUPLES") & PREMIUM_CUSTOMER != "Mainstream", price], alternative = "greater")

	Welch Two Sample t-test

data:  data[LIFESTAGE %in% c("YOUNG SINGLES/COUPLES", "MIDAGE SINGLES/COUPLES") & PREMIUM_CUSTOMER == "Mainstream", price] and data[LIFESTAGE %in% c("YOUNG SINGLES/COUPLES", "MIDAGE SINGLES/COUPLES") & PREMIUM_CUSTOMER != "Mainstream", price]
t = 37.624, df = 54791, p-value < 2.2e-16
alternative hypothesis: true difference in means is greater than 0
95 percent confidence interval:
 0.3187234       Inf
sample estimates:
mean of x mean of y 
 4.039786  3.706491 

> segment <- data[LIFESTAGE == "YOUNG SINGLES/COUPLES" &
+ PREMIUM_CUSTOMER == "Mainstream",]
> others <- data[!(LIFESTAGE == "YOUNG SINGLES/COUPLES" &
+ PREMIUM_CUSTOMER == "Mainstream"),]
> qtySegment <- segment[, sum(PROD_QTY)]
> qtyOthers <- others[, sum(PROD_QTY)]
> qtySegmentBrand <- segment[, .(TargetSegment =
+ sum(PROD_QTY)/qtySegment), by = BRAND]
> qtyOthersBrand <- others[, .(Others =
+ sum(PROD_QTY)/qtyOthers), by = BRAND]
> brandProportions <- merge(qtySegmentBrand, qtyOthersBrand)[, AffinityToBrand := TargetSegment/Others]
> brandProportions[order(-AffinityToBrand)]
> qtySegmentPack <- segment[, .(TargetSegment =
+ sum(PROD_QTY)/qtySegment), by = PACK_SIZE]
> qtyOtherPack <- others[, .(Others =
+ sum(PROD_QTY)/qtyOthers), by = PACK_SIZE]
> packProportions <- merge(qtySegmentPack, qtyOtherPack)[, AffinityToPack := TargetSegment/Others]
> packProportions[order(-AffinityToPack)]
> segment1 <- data[LIFESTAGE == "MIDAGE SINGLES/COUPLES" &
+ PREMIUM_CUSTOMER == "Mainstream",]
> others1 <- data[!(LIFESTAGE == "MIDAGE SINGLES/COUPLES" &
+ PREMIUM_CUSTOMER == "Mainstream"),]
> qtySegment1 <- segment1[, sum(PROD_QTY)]
> qtyOthers1 <- others1[, sum(PROD_QTY)]
> qtySegmentBrand1 <- segment1[, .(TargetSegment =
+ sum(PROD_QTY)/qtySegment1), by = BRAND]
> qtyOthersBrand1 <- others1[, .(Others =
+ sum(PROD_QTY)/qtyOthers1), by = BRAND]
> brandProportions1 <- merge(qtySegmentBrand1, qtyOthersBrand1)[, AffinityToBrand := TargetSegment/Others]
> brandProportions1[order(-AffinityToBrand)]
> qtySegmentPack1 <- segment1[, .(TargetSegment =
+ sum(PROD_QTY)/qtySegment1), by = PACK_SIZE]
> qtyOtherPack1 <- others1[, .(Others =
+ sum(PROD_QTY)/qtyOthers1), by = PACK_SIZE]
> packProportions1 <- merge(qtySegmentPack1, qtyOtherPack1)[, AffinityToPack := TargetSegment/Others]
> packProportions1[order(-AffinityToPack)]
