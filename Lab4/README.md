# Основы обработки данных с помощью R и Dplyr


## Цель работы

1.  Зекрепить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R
3.  Закрепить навыки исследования метаданных DNS трафика

## Исходные данные

1.  Rstudio Desktop;
2.  Интерпретатор языка R 4.1;
3.  Программный пакет `dplyr` и `knitr`.

Вы исследуете подозрительную сетевую активность во внутренней сети
Доброй Организации. Вам в руки попали метаданные о DNS трафике в
исследуемой сети. Исследуйте файлы, восстановите данные, подготовьте их
к анализу и дайте обоснованные ответы на поставленные вопросы
исследования.

## Решение:

1.  Импортируйте данные DNS

<!-- -->

    > install.packages("tidyverse")
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/ignat/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    устанавливаю также зависимость ‘bit’, ‘farver’, ‘labeling’, ‘RColorBrewer’, ‘viridisLite’, ‘rematch’, ‘bit64’, ‘prettyunits’, ‘backports’, ‘blob’, ‘DBI’, ‘data.table’, ‘gtable’, ‘isoband’, ‘S7’, ‘scales’, ‘gargle’, ‘uuid’, ‘cellranger’, ‘ids’, ‘rematch2’, ‘cpp11’, ‘timechange’, ‘systemfonts’, ‘textshaping’, ‘clipr’, ‘vroom’, ‘tzdb’, ‘progress’, ‘selectr’, ‘broom’, ‘conflicted’, ‘dbplyr’, ‘dtplyr’, ‘forcats’, ‘ggplot2’, ‘googledrive’, ‘googlesheets4’, ‘haven’, ‘hms’, ‘lubridate’, ‘modelr’, ‘purrr’, ‘ragg’, ‘readr’, ‘readxl’, ‘reprex’, ‘rstudioapi’, ‘rvest’, ‘tidyr’, ‘xml2’
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/bit_4.6.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/farver_2.1.2.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/labeling_0.4.3.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/RColorBrewer_1.1-3.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/viridisLite_0.4.2.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/rematch_2.0.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/bit64_4.6.0-1.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/prettyunits_1.2.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/backports_1.5.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/blob_1.2.4.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/DBI_1.2.3.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/data.table_1.17.8.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/gtable_0.3.6.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/isoband_0.2.7.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/S7_0.2.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/scales_1.4.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/gargle_1.6.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/uuid_1.2-1.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/cellranger_1.1.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/ids_1.0.1.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/rematch2_2.1.2.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/cpp11_0.5.2.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/timechange_0.3.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/systemfonts_1.3.1.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/textshaping_1.0.4.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/clipr_0.8.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/vroom_1.6.6.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/tzdb_0.5.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/progress_1.2.3.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/selectr_0.4-2.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/broom_1.0.10.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/conflicted_1.2.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/dbplyr_2.5.1.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/dtplyr_1.3.2.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/forcats_1.0.1.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/ggplot2_4.0.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/googledrive_2.1.2.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/googlesheets4_1.1.2.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/haven_2.5.5.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/hms_1.1.4.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/lubridate_1.9.4.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/modelr_0.1.11.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/purrr_1.1.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/ragg_1.5.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/readr_2.1.5.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/readxl_1.4.5.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/reprex_2.1.1.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/rstudioapi_0.17.1.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/rvest_1.0.5.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/tidyr_1.3.1.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/xml2_1.4.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/tidyverse_2.0.0.zip'
    пакет ‘bit’ успешно распакован, MD5-суммы проверены
    пакет ‘farver’ успешно распакован, MD5-суммы проверены
    пакет ‘labeling’ успешно распакован, MD5-суммы проверены
    пакет ‘RColorBrewer’ успешно распакован, MD5-суммы проверены
    пакет ‘viridisLite’ успешно распакован, MD5-суммы проверены
    пакет ‘rematch’ успешно распакован, MD5-суммы проверены
    пакет ‘bit64’ успешно распакован, MD5-суммы проверены
    пакет ‘prettyunits’ успешно распакован, MD5-суммы проверены
    пакет ‘backports’ успешно распакован, MD5-суммы проверены
    пакет ‘blob’ успешно распакован, MD5-суммы проверены
    пакет ‘DBI’ успешно распакован, MD5-суммы проверены
    пакет ‘data.table’ успешно распакован, MD5-суммы проверены
    пакет ‘gtable’ успешно распакован, MD5-суммы проверены
    пакет ‘isoband’ успешно распакован, MD5-суммы проверены
    пакет ‘S7’ успешно распакован, MD5-суммы проверены
    пакет ‘scales’ успешно распакован, MD5-суммы проверены
    пакет ‘gargle’ успешно распакован, MD5-суммы проверены
    пакет ‘uuid’ успешно распакован, MD5-суммы проверены
    пакет ‘cellranger’ успешно распакован, MD5-суммы проверены
    пакет ‘ids’ успешно распакован, MD5-суммы проверены
    пакет ‘rematch2’ успешно распакован, MD5-суммы проверены
    пакет ‘cpp11’ успешно распакован, MD5-суммы проверены
    пакет ‘timechange’ успешно распакован, MD5-суммы проверены
    пакет ‘systemfonts’ успешно распакован, MD5-суммы проверены
    пакет ‘textshaping’ успешно распакован, MD5-суммы проверены
    пакет ‘clipr’ успешно распакован, MD5-суммы проверены
    пакет ‘vroom’ успешно распакован, MD5-суммы проверены
    пакет ‘tzdb’ успешно распакован, MD5-суммы проверены
    пакет ‘progress’ успешно распакован, MD5-суммы проверены
    пакет ‘selectr’ успешно распакован, MD5-суммы проверены
    пакет ‘broom’ успешно распакован, MD5-суммы проверены
    пакет ‘conflicted’ успешно распакован, MD5-суммы проверены
    пакет ‘dbplyr’ успешно распакован, MD5-суммы проверены
    пакет ‘dtplyr’ успешно распакован, MD5-суммы проверены
    пакет ‘forcats’ успешно распакован, MD5-суммы проверены
    пакет ‘ggplot2’ успешно распакован, MD5-суммы проверены
    пакет ‘googledrive’ успешно распакован, MD5-суммы проверены
    пакет ‘googlesheets4’ успешно распакован, MD5-суммы проверены
    пакет ‘haven’ успешно распакован, MD5-суммы проверены
    пакет ‘hms’ успешно распакован, MD5-суммы проверены
    пакет ‘lubridate’ успешно распакован, MD5-суммы проверены
    пакет ‘modelr’ успешно распакован, MD5-суммы проверены
    пакет ‘purrr’ успешно распакован, MD5-суммы проверены
    пакет ‘ragg’ успешно распакован, MD5-суммы проверены
    пакет ‘readr’ успешно распакован, MD5-суммы проверены
    пакет ‘readxl’ успешно распакован, MD5-суммы проверены
    пакет ‘reprex’ успешно распакован, MD5-суммы проверены
    пакет ‘rstudioapi’ успешно распакован, MD5-суммы проверены
    пакет ‘rvest’ успешно распакован, MD5-суммы проверены
    пакет ‘tidyr’ успешно распакован, MD5-суммы проверены
    пакет ‘xml2’ успешно распакован, MD5-суммы проверены
    пакет ‘tidyverse’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\ignat\AppData\Local\Temp\RtmpkJ6HqM\downloaded_packages

``` r
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ✔ forcats   1.0.1     ✔ stringr   1.5.2
    ✔ ggplot2   4.0.0     ✔ tibble    3.3.0
    ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ✔ purrr     1.1.0     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
test <- tempfile()
download.file("https://storage.yandexcloud.net/dataset.ctfsec/dns.zip", test)
unzip(test, exdir = "dns_data")
dns_data <- read_tsv("dns_data/dns.log", col_names = FALSE)
```

    Rows: 427935 Columns: 23
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: "\t"
    chr (13): X2, X3, X5, X7, X9, X10, X11, X12, X13, X14, X15, X21, X22
    dbl  (5): X1, X4, X6, X8, X20
    lgl  (5): X16, X17, X18, X19, X23

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

1.  Добавьте пропущенные данные о структуре данных (назначении столбцов)

``` r
colnames(dns_data) <- c("ts", "uid", "id.orig_h", "id.orig_p", "id.resp_h", "id.resp_p", "proto", "trans_id", "rtt", "query", "qclass", "qclass_name","qtype", "qtype_name", "rcode", "rcode_name", "AA", "TC", "RD", "RA", "Z", "answers", "TTLs", "rejected")
```

1.  Преобразуйте данные в столбцах в нужный формат

``` r
dns_data <- dns_data %>% mutate(ts = as_datetime(ts), id.orig_h = as.character(id.orig_h), id.resp_h = as.character(id.resp_h), query = as.character(query))
```

1.  Просмотрите общую структуру данных с помощью функции glimpse()

``` r
glimpse(dns_data)
```

    Rows: 427,935
    Columns: 23
    $ ts          <dttm> 2012-03-16 12:30:05, 2012-03-16 12:30:15, 2012-03-16 12:3…
    $ uid         <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jljz7Bs…
    $ id.orig_h   <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", "19…
    $ id.orig_p   <dbl> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 1…
    $ id.resp_h   <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255", "1…
    $ id.resp_p   <dbl> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
    $ proto       <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "u…
    $ trans_id    <dbl> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 62187, 62…
    $ rtt         <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\…
    $ query       <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1"…
    $ qclass      <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C…
    $ qclass_name <chr> "33", "32", "32", "32", "32", "32", "32", "32", "32", "32"…
    $ qtype       <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB…
    $ qtype_name  <chr> "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ rcode       <chr> "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-…
    $ rcode_name  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ AA          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ TC          <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRU…
    $ RD          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ RA          <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0…
    $ Z           <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ answers     <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ TTLs        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…

1.  Сколько участников информационного обмена в сети Доброй Организации?

``` r
length(unique(c(dns_data$id.orig_h, dns_data$id.resp_h)))
```

    [1] 1359

1.  Какое соотношение участников обмена внутри сети и участников
    обращений к внешним ресурсам?

``` r
all_ips <- unique(c(dns_data$id.orig_h, dns_data$id.resp_h))
length(all_ips[grepl("^(10\\.|192\\.168\\.|172\\.(1[6-9]|2[0-9]|3[0-1])\\.)", all_ips)])/length(setdiff(all_ips, all_ips[grepl("^(10\\.|192\\.168\\.|172\\.(1[6-9]|2[0-9]|3[0-1])\\.)", all_ips)]))
```

    [1] 13.77174

1.  Найдите топ-10 участников сети, проявляющих наибольшую сетевую
    активность.

``` r
dns_data %>%  count(id.orig_h, sort = TRUE) %>%  head(10)
```

    # A tibble: 10 × 2
       id.orig_h           n
       <chr>           <int>
     1 10.10.117.210   75943
     2 192.168.202.93  26522
     3 192.168.202.103 18121
     4 192.168.202.76  16978
     5 192.168.202.97  16176
     6 192.168.202.141 14967
     7 10.10.117.209   14222
     8 192.168.202.110 13372
     9 192.168.203.63  12148
    10 192.168.202.106 10784

1.  Найдите топ-10 доменов, к которым обращаются пользователи сети и
    соответственное количество обращений

``` r
dns_data %>% count(id.orig_h, sort = TRUE) %>% head(10)
```

    # A tibble: 10 × 2
       id.orig_h           n
       <chr>           <int>
     1 10.10.117.210   75943
     2 192.168.202.93  26522
     3 192.168.202.103 18121
     4 192.168.202.76  16978
     5 192.168.202.97  16176
     6 192.168.202.141 14967
     7 10.10.117.209   14222
     8 192.168.202.110 13372
     9 192.168.203.63  12148
    10 192.168.202.106 10784

1.  Опеределите базовые статистические характеристики (функция summary()
    ) интервала времени между последовательным обращениями к топ-10
    доменам.

<!-- -->

    > top_domains_names <- top_domains$id.orig_h
    > time_intervals <- dns_data %>% filter(query %in% top_domains_names) %>% arrange(query, ts) %>% group_by(query) %>% mutate(time_diff = as.numeric(ts - lag(ts))) %>% filter(!is.na(time_diff))
    > time_intervals %>% group_by(query) %>% summarise(min = min(time_diff), q1 = quantile(time_diff, 0.25), median = median(time_diff), mean = mean(time_diff), q3 = quantile(time_diff, 0.75), max = max(time_diff))
    # A tibble: 0 × 7
    # ℹ 7 variables: query <chr>, min <dbl>, q1 <dbl>, median <dbl>, mean <dbl>, q3 <dbl>,
    #   max <dbl>

1.  Часто вредоносное программное обеспечение использует DNS канал в
    качестве канала управления, периодически отправляя запросы на
    подконтрольный злоумышленникам DNS сервер. По периодическим запросам
    на один и тот же домен можно выявить скрытый DNS канал. Есть ли
    такие IP адреса в исследуемом датасете?

<!-- -->

    dns_data %>% group_by(id.orig_h, query) %>% summarise( request_count = n(), time_range = as.numeric(max(ts) - min(ts)), avg_interval = time_range/(request_count - 1), periodicity_score = sd(diff(as.numeric(ts)), na.rm = TRUE) ) %>% filter(request_count > 10, time_range > 3600, periodicity_score < 300) %>% arrange(periodicity_score)
    `summarise()` has grouped output by 'id.orig_h'. You can override using the `.groups`
    argument.
    # A tibble: 0 × 6
    # Groups:   id.orig_h [0]
    # ℹ 6 variables: id.orig_h <chr>, query <chr>, request_count <int>, time_range <dbl>,
    #   avg_interval <dbl>, periodicity_score <dbl>

## Оценка результата

В результате лабораторной работы мы закрепили знания основных функций
обработки данных экосистемы tidyverse языка R и исследования метаданных
DNS трафика

## Вывод

## Таким образом, мы закрепили знания основных функций обработки данных экосистемы tidyverse языка R и исследования метаданных DNS трафика.

## Quarto

Quarto enables you to weave together content and executable code into a
finished document. To learn more about Quarto see <https://quarto.org>.

## Running Code

When you click the **Render** button a document will be generated that
includes both content and the output of embedded code. You can embed
code like this:

``` r
1 + 1
```

    [1] 2

You can add options to executable code like this

    [1] 4

The `echo: false` option disables the printing of code (only output is
displayed).
