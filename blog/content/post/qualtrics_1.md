---
title: "qualtRics 1.0 now available from CRAN"
layout: post
categories: ["R Package"]
date: "2017-04-26T23:02:00" 
tag: ["R", "qualtRics", "Qualtrics"]
author: Jasper Ginn
blog: true
description: Qualtrics allows users to collect online data through surveys. My R package qualtRics contains convenience functions to pull survey results straight into R using the Qualtrics API instead of having to download survey results manually.
---

[Qualtrics](https://www.qualtrics.com/about/)  allows users to collect online data through surveys. My R package [qualtRics](https://cran.r-project.org/web/packages/qualtRics/index.html)  contains convenience functions to pull survey results straight into R using the Qualtrics API instead of having to download survey results manually.

Currently, the package contains three functions:

1. **getSurveys()** fetches a list of all surveys that you own or have access to from Qualtrics.
2. **getSurvey()** downloads a survey from Qualtrics and loads it into R.
3. **readSurvey()** allows you to read CSV files you download manually from Qualtrics.

Getting started with the package is straightforward.  Note that your institution must support API access and that it must be enabled for your account. Whoever manages your Qualtrics account can help you with this. Refer to the [Qualtrics documentation](https://api.qualtrics.com/docs/authentication) to find your API token.

## Examples
Register your Qualtrics API key. You need to do this once every R session:

```R
library(qualtRics)
registerApiKey(API.TOKEN = "<yourapitoken>")
```

Get a data frame of all surveys to which you have access:

```R
surveys <- getSurveys(root_url="https://leidenuniv.eu.qualtrics.com") # URL is for my own institution
```

Export a survey and load it into R:

```R
mysurvey <- getSurvey(surveyID = surveys$id[6],
                      root_url = "https://leidenuniv.eu.qualtrics.com",
                      verbose = TRUE)
```

You can add a from/to date to only retrieve responses between those dates:

```R
surv <- getSurvey(survs$id[4],
                  root_url = "https://leidenuniv.eu.qualtrics.com",
                  startDate = "2016-09-18",
                  endDate = "2016-10-01",
                  useLabels = FALSE,
                  verbose = TRUE)
```

You may also reference a response ID. `getSurvey` will then download all responses that were submitted after that response:

```R
surv <- getSurvey(survs$id[4],
                  root_url = "https://leidenuniv.eu.qualtrics.com",
                  lastResponseId = "R_3mmovCIeMllvsER",
                  useLabels = FALSE,
                  verbose = TRUE)
```

You can store the results in a specific location if you like:

```R
mysurvey <- getSurvey(surveyID = surveys$id[6],
                      save_dir = "/users/jasper/desktop/",
                      root_url = "https://leidenuniv.eu.qualtrics.com",
                      verbose = TRUE)
```

Note that surveys that are stored in this way will be saved as an [RDS](https://stat.ethz.ch/R-manual/R-devel/library/base/html/readRDS.html) file rather than e.g. a CSV. Reading an RDS file is as straightforward as this:

```R
mysurvey <- readRDS(file = "/users/jasper/desktop/mysurvey.rds")
```

You can read a survey that you downloaded manually using `readSurvey`:

```R
mysurvey <- readSurvey("/users/jasper/desktop/mysurvey.csv")
```

To avoid special characters (mainly periods) in header names, `readSurvey` uses question labels as the header names. The question belonging to that label is then added using the [sjmisc](https://CRAN.R-project.org/package=sjmisc) package. Qualtrics gives names to these labels automatically, but you can easily change them.

![](img/blog/qualtrics-1-available/qualtricsdf.png)

If you have any questions or feature requests, please leave them [here](https://github.com/JasperHG90/qualtRics/issues).
