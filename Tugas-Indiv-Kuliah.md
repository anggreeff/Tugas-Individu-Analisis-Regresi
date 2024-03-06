    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    library(plotly)

    ## Warning: package 'plotly' was built under R version 4.3.2

    ## Loading required package: ggplot2

    ## 
    ## Attaching package: 'plotly'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     last_plot

    ## The following object is masked from 'package:stats':
    ## 
    ##     filter

    ## The following object is masked from 'package:graphics':
    ## 
    ##     layout

    library(car)

    ## Warning: package 'car' was built under R version 4.3.2

    ## Loading required package: carData

    ## Warning: package 'carData' was built under R version 4.3.2

    ## 
    ## Attaching package: 'car'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     recode

    library(randtests)
    library(lmtest)

    ## Warning: package 'lmtest' was built under R version 4.3.2

    ## Loading required package: zoo

    ## Warning: package 'zoo' was built under R version 4.3.2

    ## 
    ## Attaching package: 'zoo'

    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric

    library(readxl)

    ## Warning: package 'readxl' was built under R version 4.3.2

    anreg <-read_excel("D:/Kuliah/SEMESTER 4/Analisis Regresi/Tugas/Kuliah/Tugas Indiv Kuliah.xlsx")

    head(anreg)

    ## # A tibble: 6 × 3
    ##      No     X     Y
    ##   <dbl> <dbl> <dbl>
    ## 1     1     2    54
    ## 2     2     5    50
    ## 3     3     7    45
    ## 4     4    10    37
    ## 5     5    14    35
    ## 6     6    19    25

## Model Awal

    model.reg=lm(formula= Y ~ X,data = anreg)
    summary(model.reg)

    ## 
    ## Call:
    ## lm(formula = Y ~ X, data = anreg)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -7.1628 -4.7313 -0.9253  3.7386  9.0446 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 46.46041    2.76218   16.82 3.33e-10 ***
    ## X           -0.75251    0.07502  -10.03 1.74e-07 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 5.891 on 13 degrees of freedom
    ## Multiple R-squared:  0.8856, Adjusted R-squared:  0.8768 
    ## F-statistic: 100.6 on 1 and 13 DF,  p-value: 1.736e-07

Diperoleh model persamaan regresi sebagai berikut

*Y duga = 46.46041 − 0.7525X + e*

Namun model tersebut belum bisa dipastikan sebagai model terbaik karena
belum melalui proses uji asumsi, sehingga dibutuhkan eksplorasi kondisi
dan pengujian asumsi Gaus Markov dan normalitas untuk mendapatkan model
terbaik.

### **Eksplorasi Kondisi**

\#Scatter Plot Variabel X dan Y

    plot(x = anreg$X,y = anreg$Y)

![](Tugas-Indiv-Kuliah_files/figure-markdown_strict/unnamed-chunk-4-1.png)

Hasil plot hubungan X dan Y menggambarkan hubungan yang tidak linier dan
cenderung melengkung (tidak garis lurus) atau membentuk parabola

\#Plot Sisaan dan Y duga

    plot(model.reg,1)

![](Tugas-Indiv-Kuliah_files/figure-markdown_strict/unnamed-chunk-5-1.png)

\#Plot Sisaan dan Urutan

    plot(x = 1:dim(anreg)[1],
         y = model.reg$residuals,
         type = 'b', 
         ylab = "Residuals",
         xlab = "Observation")

![](Tugas-Indiv-Kuliah_files/figure-markdown_strict/unnamed-chunk-6-1.png)

Tebaran membentuk pola kurva maka sisaan tidak saling bebas sehingga
model belum baik

### Normalitas Sisaan dengan QQ-Plot

    plot(model.reg,2)

![](Tugas-Indiv-Kuliah_files/figure-markdown_strict/unnamed-chunk-7-1.png)

**Uji Asumsi**

1.  Kondisi Gaus Markov

𝐻0: Nilai Harapan Sisaan = 0

𝐻1: Nilai Harapan Sisaan ≠ 0

    t.test(model.reg$residuals,mu = 0,conf.level = 0.95)

    ## 
    ##  One Sample t-test
    ## 
    ## data:  model.reg$residuals
    ## t = -4.9493e-16, df = 14, p-value = 1
    ## alternative hypothesis: true mean is not equal to 0
    ## 95 percent confidence interval:
    ##  -3.143811  3.143811
    ## sample estimates:
    ##     mean of x 
    ## -7.254614e-16

p-value = 1 &gt;alpha = 0.05, maka tidak tolak h0 yang berarti nilai
harapan sisaan sama dengan nol, asumsi terpenuhi

1.  Kehomogenan

𝐻0: Ragam sisaan homogen

𝐻1: Ragam sisaan tidak homogen

    homogen = lm(formula = abs(model.reg$residuals) ~ X, # y: abs residual
        data = anreg)
    summary(homogen)

    ## 
    ## Call:
    ## lm(formula = abs(model.reg$residuals) ~ X, data = anreg)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -4.2525 -1.7525  0.0235  2.0168  4.2681 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  5.45041    1.27241   4.284  0.00089 ***
    ## X           -0.01948    0.03456  -0.564  0.58266    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.714 on 13 degrees of freedom
    ## Multiple R-squared:  0.02385,    Adjusted R-squared:  -0.05124 
    ## F-statistic: 0.3176 on 1 and 13 DF,  p-value: 0.5827

    bptest(model.reg)

    ## 
    ##  studentized Breusch-Pagan test
    ## 
    ## data:  model.reg
    ## BP = 0.52819, df = 1, p-value = 0.4674

    ncvTest(model.reg)

    ## Non-constant Variance Score Test 
    ## Variance formula: ~ fitted.values 
    ## Chisquare = 0.1962841, Df = 1, p = 0.65774

karena p-value &gt; 0.05, maka tidak tolak h0 sehingga ragam sisaannya
homogen, asumsi terpenuhi

1.  Sisaan Saling Bebas atau tidak

𝐻0: Sisaan saling bebas

𝐻1: Sisaan tidak saling bebas

    library(randtests)
    runs.test(model.reg$residuals)

    ## 
    ##  Runs Test
    ## 
    ## data:  model.reg$residuals
    ## statistic = -2.7817, runs = 3, n1 = 7, n2 = 7, n = 14, p-value =
    ## 0.005407
    ## alternative hypothesis: nonrandomness

    library(lmtest)
    dwtest(model.reg)

    ## 
    ##  Durbin-Watson test
    ## 
    ## data:  model.reg
    ## DW = 0.48462, p-value = 1.333e-05
    ## alternative hypothesis: true autocorrelation is greater than 0

Karena p-value &lt; 0.05 pada Durbin-Watson Test, maka tolak H0 sehingga
sisaan tidak saling bebas, asumsi tidak terpenuhi

1.  Sisaan Menyebar Normal

𝐻0: Sisaan menyebar normal

𝐻1: Sisaan tidak menyebar normal

    shapiro.test(model.reg$residuals)

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  model.reg$residuals
    ## W = 0.92457, p-value = 0.226

Karena p-value = 0.226 &gt; 0.05, maka tidak tolak H0, sehingga sisaan
menyebar normal, asumsi terpenuhi

## Penanganan kondisi tak standar

1.  Transformasi Data

<!-- -->

    Y_ubah = sqrt(anreg$Y)
    X_ubah = sqrt(anreg$X)

    plot(x = anreg$X, y = Y_ubah)

![](Tugas-Indiv-Kuliah_files/figure-markdown_strict/unnamed-chunk-15-1.png)

    plot(x = X_ubah, y = anreg$Y)

![](Tugas-Indiv-Kuliah_files/figure-markdown_strict/unnamed-chunk-16-1.png)

    plot(x = X_ubah, y = Y_ubah)

![](Tugas-Indiv-Kuliah_files/figure-markdown_strict/unnamed-chunk-17-1.png)

Karena hubungan X dan Y cenderung membentuk sebuah parabola dan nilai B1
&lt; 0, maka data dapat di transformasi dengan mengecilkan X dan/atau Y
dengan membentuknya menjadi pangkat setengah atau akar dari data asli.
TErdapat perbedaan antara hasil plot hubungan X\_ubah dengan Y, X dengan
Y\_ubah, dan X\_ubah dengan Y\_ubah, sehingga perlu ditelusuri lebih
lanjut untuk memperoleh model terbaik dengan pemeriksaan asumsi pada
data dengan sisaan paling bebas

### Model dan Uji Asumsi

1.  **X\_ubah dengan Y**

<!-- -->

    model1 = lm(formula = anreg$Y ~ X_ubah)
    summary(model1)

    ## 
    ## Call:
    ## lm(formula = anreg$Y ~ X_ubah)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -4.4518 -2.8559  0.7657  2.0035  5.2422 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  63.2250     2.2712   27.84 5.67e-13 ***
    ## X_ubah       -7.7481     0.4097  -18.91 7.68e-11 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 3.262 on 13 degrees of freedom
    ## Multiple R-squared:  0.9649, Adjusted R-squared:  0.9622 
    ## F-statistic: 357.7 on 1 and 13 DF,  p-value: 7.684e-11

Diperoleh model sebagai berikut

*Y duga = 63.2250 - 7.7481x + e*

Apakah sisaan saling bebas?

    dwtest(model1)

    ## 
    ##  Durbin-Watson test
    ## 
    ## data:  model1
    ## DW = 1.1236, p-value = 0.01422
    ## alternative hypothesis: true autocorrelation is greater than 0

Karena p-value = 0.01422 &lt; alpha = 0.05, maka tolak H0, sehingga
sisaan tidak saling bebas, asumsi tidak terpenuhi, maka ini bukan model
terbaik

1.  **X dengan Y\_ubah**

<!-- -->

    model2 = lm(formula = Y_ubah ~ anreg$X)
    summary(model2)

    ## 
    ## Call:
    ## lm(formula = Y_ubah ~ anreg$X)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.53998 -0.38316 -0.01727  0.36045  0.70199 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  7.015455   0.201677   34.79 3.24e-14 ***
    ## anreg$X     -0.081045   0.005477  -14.80 1.63e-09 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.4301 on 13 degrees of freedom
    ## Multiple R-squared:  0.9439, Adjusted R-squared:  0.9396 
    ## F-statistic: 218.9 on 1 and 13 DF,  p-value: 1.634e-09

Diperoleh model sebagai berikut

*Y duga = 7.015455 - 0.081045x + e*

Apakah sisaan saling bebas?

    dwtest(model2)

    ## 
    ##  Durbin-Watson test
    ## 
    ## data:  model2
    ## DW = 1.2206, p-value = 0.02493
    ## alternative hypothesis: true autocorrelation is greater than 0

Karena p-value = 0.02493 &lt; alpha = 0.05, maka tolak H0, sehingga
sisaan tidak saling bebas, asumsi tidak terpenuhi, maka ini bukan model
terbaik

1.  **X\_ubah dengan Y\_ubah**

<!-- -->

    model3 = lm(formula = Y_ubah ~ X_ubah)
    summary(model3)

    ## 
    ## Call:
    ## lm(formula = Y_ubah ~ X_ubah)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.42765 -0.17534 -0.05753  0.21223  0.46960 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  8.71245    0.19101   45.61 9.83e-16 ***
    ## X_ubah      -0.81339    0.03445  -23.61 4.64e-12 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.2743 on 13 degrees of freedom
    ## Multiple R-squared:  0.9772, Adjusted R-squared:  0.9755 
    ## F-statistic: 557.3 on 1 and 13 DF,  p-value: 4.643e-12

Diperoleh model sebagai berikut

*Y duga = 8.71245 - 0.81339x + e*

Apakah sisaan saling bebas?

    dwtest(model3)

    ## 
    ##  Durbin-Watson test
    ## 
    ## data:  model3
    ## DW = 2.6803, p-value = 0.8629
    ## alternative hypothesis: true autocorrelation is greater than 0

Karena p-value = 0.8629 &gt; alpha = 0.05 maka tidak tolak H0 sehingga
sisaan saling bebas, asumsi terpenuhi.

Selanjutnya, kita uji asumsi yang lainnya

a\. Nilai Harapan Sisaan = 0

    t.test(model3$residuals,mu = 0,conf.level = 0.95)

    ## 
    ##  One Sample t-test
    ## 
    ## data:  model3$residuals
    ## t = 2.0334e-16, df = 14, p-value = 1
    ## alternative hypothesis: true mean is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.1463783  0.1463783
    ## sample estimates:
    ##    mean of x 
    ## 1.387779e-17

Karena p-value = 0.1 &gt; alpha = 0.05, maka tidak tolak H0 sehingga
Nilai Harapan Sisaan sama dengan nol, asumsi terpenuhi

b\. Ragam Sisaan Homogen

    Y_ubah = sqrt(anreg$Y)
    X_ubah = sqrt(anreg$X)

    model3 = lm(formula = Y_ubah ~ X_ubah)
    summary(model3)

    ## 
    ## Call:
    ## lm(formula = Y_ubah ~ X_ubah)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.42765 -0.17534 -0.05753  0.21223  0.46960 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  8.71245    0.19101   45.61 9.83e-16 ***
    ## X_ubah      -0.81339    0.03445  -23.61 4.64e-12 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.2743 on 13 degrees of freedom
    ## Multiple R-squared:  0.9772, Adjusted R-squared:  0.9755 
    ## F-statistic: 557.3 on 1 and 13 DF,  p-value: 4.643e-12

    homogen3 = lm(formula = abs(model3$residuals) ~ X_ubah, 
        data = anreg)
    summary(homogen)

    ## 
    ## Call:
    ## lm(formula = abs(model.reg$residuals) ~ X, data = anreg)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -4.2525 -1.7525  0.0235  2.0168  4.2681 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  5.45041    1.27241   4.284  0.00089 ***
    ## X           -0.01948    0.03456  -0.564  0.58266    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.714 on 13 degrees of freedom
    ## Multiple R-squared:  0.02385,    Adjusted R-squared:  -0.05124 
    ## F-statistic: 0.3176 on 1 and 13 DF,  p-value: 0.5827

    bptest(model3)

    ## 
    ##  studentized Breusch-Pagan test
    ## 
    ## data:  model3
    ## BP = 3.9621, df = 1, p-value = 0.04654

    ncvTest(model3)

    ## Non-constant Variance Score Test 
    ## Variance formula: ~ fitted.values 
    ## Chisquare = 2.160411, Df = 1, p = 0.14161

Karena p-value &gt; 0.05, maka tidak tolak H0 sehingga ragam sisaan
homogen, asumsi terpenuhi

c\. Sisaan Menyebar Normal

    shapiro.test(model3$residuals)

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  model3$residuals
    ## W = 0.96504, p-value = 0.7791

Karena p-value = 0.7791 &gt; alpha = 0.05 maka tidak tolak H0 sehingga
sisaan menyebar normal, asumsi terpenuhi

## Kesimpulan

    plot(x = X_ubah, y = Y_ubah)

![](Tugas-Indiv-Kuliah_files/figure-markdown_strict/unnamed-chunk-31-1.png)

    model3 = lm(formula = Y_ubah ~ X_ubah)
    summary(model3)

    ## 
    ## Call:
    ## lm(formula = Y_ubah ~ X_ubah)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.42765 -0.17534 -0.05753  0.21223  0.46960 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  8.71245    0.19101   45.61 9.83e-16 ***
    ## X_ubah      -0.81339    0.03445  -23.61 4.64e-12 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.2743 on 13 degrees of freedom
    ## Multiple R-squared:  0.9772, Adjusted R-squared:  0.9755 
    ## F-statistic: 557.3 on 1 and 13 DF,  p-value: 4.643e-12

Hasil visualisasi analisi data dengan scatter plot dapat diketahui bahwa
ada hubungan linear yang negatif antara variabel penjelas X\_ubah dengan
variabel Y\_ubah. Hal ini mengindikasikan bahwa ketika unsur variabel
X\_ubah mengalami kenaikan akan menghasilkan penuruan nilai pada
variabel Y\_ubah.

Berdasarkan hasil analisis regresi linear sederhana setelah
transformasi, diketahui bahwa nilai Adjusted R-Squared sebesar 0.9755
atau sebanyak 97,55% keragaman yang terdapat pada variabel Y\_ubah dapat
dijelaskan oleh variabel penjelas X\_ubah dan memiliki sisa yaitu 2,45%
dijelaskan oleh faktor lain yang tidak terdapat pada model

Adapun nilai korelasinya didapat dengan menghitung akar dari nilai
determinasi (0.9772) yaitu 0.9885, nilai ini sangat mendekati nilai 1,
sehingga dapat dikatakan terdapat korelasi yang sangat kuat antara
peubah X dan Y

Model regresi didapat sebagai berikut

*Y duga = 8.71245 - 0.81339x + e*

Nilai koefisien B1 sebesar -0.81339 menunjukkan bahwa kenaikan setiap
satu satuan pada variabel x diduga berpengaruh terhadap penurunan
rata-rata pada variabel y sebesar 0.81339. Koefisien B0 (intersep)
sebesar 8.71245 menunjukkan perkiraan rata-rata variabel y ketika nilai
variabel x tidak ada, disebabkan oleh faktor-faktor lain yang tidak
termasuk kedalam model. Dengan kata lain, terdapat nilai rata-rata y
tertentu yang tidak dapat dijelaskan oleh variabel x.

Nilai p-value koefisien intersep sebesar 9.83e-16 &lt; alpha = 0.05,
maka tolak H0, yaitu ada signifikansi. Terdapat cukup bukti untuk
menyatakan bahwa terdapat nilai variabel y yang tidak tidak dapat
dijelaskan oleh variabel penjelas x pada tingkat kepercayaan 95%
