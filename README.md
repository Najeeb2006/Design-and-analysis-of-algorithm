# R Charting Cheat-Sheet (DHV Lab Quick Reference)

A universal cheat-sheet — one minimal template per chart type. Replace `data$ColumnName` with whatever your question gives you. Almost every question in a DHV lab record maps to one of these.

## The only rule you need
```r
plot_function(X_variable, Y_variable, ...)
```
**X = what you're comparing FROM (cause/category/time). Y = what you're measuring.**

---

### 1. Line Chart (trend over time)
```r
plot(data$Date, data$Value, type="o", col="darkgreen",
     xlab="Date", ylab="Value", main="Trend")
```

### 2. Bar Chart (category vs value)
```r
barplot(data$Value, names.arg=data$Category, col="skyblue",
        xlab="Category", ylab="Value", main="Bar Chart")
```

### 3. Grouped Bar Chart (multiple series side-by-side)
```r
mat <- t(as.matrix(data[,c("Jan","Feb","Mar")]))
barplot(mat, beside=TRUE, names.arg=data$Product_Name,
        col=c("red","blue","green"), legend=c("Jan","Feb","Mar"))
```

### 4. Stacked Bar Chart (parts stacked on top)
```r
barplot(mat, beside=FALSE, names.arg=data$Product_Name,
        col=c("red","blue","green"), legend=c("Jan","Feb","Mar"))
```

### 5. Scatter Plot (relationship between 2 numeric vars)
```r
plot(data$X_var, data$Y_var, pch=19, col="blue",
     xlab="X label", ylab="Y label", main="Scatter Plot")
abline(lm(Y_var ~ X_var, data=data), col="red")   # trend line
cor(data$X_var, data$Y_var)                        # correlation
```

### 6. Bubble Chart (scatter + size variable)
```r
plot(data$X_var, data$Y_var, cex=data$Size_var/5, pch=19,
     col=rgb(0,0,1,0.5), xlab="X", ylab="Y", main="Bubble Chart")
```

### 7. Histogram (distribution of ONE variable)
```r
hist(data$Var, probability=TRUE, col="lightblue",
     xlab="Var", main="Histogram")
lines(density(data$Var), col="red", lwd=2)   # adds density curve
```

### 8. Pie Chart (proportions of categories)
```r
pie(table(data$Category), col=rainbow(length(unique(data$Category))),
    main="Pie Chart")
```

### 9. Boxplot (distribution by group)
```r
boxplot(Value ~ Group, data=data, col="orange",
        xlab="Group", ylab="Value", main="Boxplot")
```

### 10. Violin Plot (boxplot + density shape)
```r
library(vioplot)
vioplot(Value ~ Group, data=data, col="lightgreen")
```

### 11. Density Plot (smoothed distribution)
```r
plot(density(data$Var), main="Density Plot", col="purple", lwd=2)
```

### 12. Q-Q Plot (check normality)
```r
qqnorm(data$Var); qqline(data$Var, col="red")
```

### 13. ECDF Plot (cumulative distribution)
```r
plot(ecdf(data$Var), main="ECDF", col="blue")
```

### 14. Stacked Area Chart
```r
library(ggplot2)
ggplot(data, aes(x=Date, y=Value, fill=Category)) +
  geom_area()
```

### 15. Correlation Heatmap
```r
cor_matrix <- cor(data[,sapply(data, is.numeric)])
heatmap(cor_matrix, col=heat.colors(20), symm=TRUE)
```

### 16. Scatterplot Matrix (pairwise relationships)
```r
pairs(data[,c("Var1","Var2","Var3","Var4")])
```

### 17. Radar Chart
```r
library(fmsb)
radarchart(as.data.frame(rbind(rep(max(data$Score),ncol(data)),
                                rep(0,ncol(data)), data$Score)))
```

### 18. Word Cloud
```r
library(wordcloud)
wordcloud(words=text_data, freq=table(text_data), col=rainbow(8))
```

### 19. Map Chart
```r
library(maps)
map("world")
points(data$Longitude, data$Latitude, col="red", pch=19, cex=1.5)
```

### 20. Autocorrelation Plot (seasonality)
```r
acf(data$Value, main="Autocorrelation Plot")
```

### 21. Moving Average Smoothing (on a line chart)
```r
data$MA <- stats::filter(data$Value, rep(1/3,3), sides=2)
plot(data$Date, data$Value, type="l", col="grey")
lines(data$Date, data$MA, col="red", lwd=2)
```

### 22. Funnel Chart (no base R support — use plotly)
```r
library(plotly)
plot_ly(type="funnel", y=data$Category, x=data$Value)
```

---

## How to use this for ANY new question
1. Read the question, find the **chart name** (histogram, scatter, bar, etc.) → match it to the number above.
2. Identify which column is **X** and which is **Y** from the sentence (e.g. "scatter plot between A and B" → A=X, B=Y).
3. Copy the matching template, swap in your column names.
4. Add `main=`, `xlab=`, `ylab=` text from the question wording — that's usually all the "labeling" requirement needs.

That's the whole system — every question in a DHV lab set reduces to one of these 22 blocks.
