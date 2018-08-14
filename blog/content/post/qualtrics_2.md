+++
categories = ["post"]
comments = false
date = "2017-06-16T15:59:13-04:00"
draft = false
showpagemeta = true
showcomments = true
slug = ""
tags = ["R", "Qualtrics", "qualtRics", "CRAN"]
title = "qualtRics 2.0 now available from CRAN"
description = "qualtRics version 2.0 is now released"
+++

qualtRics version 2.0 is now available [from CRAN](https://cran.r-project.org/web/packages/qualtRics/index.html). This version contains several additional features and simplifies the process of importing survey exports from Qualtrics. Below, I outline the most important changes. You can check the full changelog [here](https://github.com/JasperHG90/qualtRics#changelog).

### qualtRics configuration file

In previous versions, users needed to register their API key by calling the `registerApiKey()` function to avoid having to pass it to each function. However, they did need to pass the root url to each function, which is annoying and inconsistent.

In this version, the `registerApiKey()` function has been replaced by `registerOptions()`, which stores both the API key and the root url in environment variables. It also stores several other options (e.g. verbose logs to the R console). You can find more information about this [here](https://github.com/JasperHG90/qualtRics#registering-your-qualtrics-credentials)

This version also supports the use of a configuration file called '.qualtRics.yml' that the user can store in the working directory of an R project. If a config file is present in the working directory, then it will be loaded when the user loads the qualtRics library, eliminating the need to register credentials at all. You can find more information about this [here](https://github.com/JasperHG90/qualtRics#using-a-configuration-file).

### getSurvey() supports all available parameters

The `getSurvey()` function now supports all parameters that the [Qualtrics API](https://api.qualtrics.com/docs/create-response-export) provides. Concretely, I've added the following parameters since version 1.0:

1. **seenUnansweredRecode:** Recode seen but unanswered questions with a string value.
2. **limit:** Maximum number of responses exported. Defaults to NULL (all responses).
3. **useLocalTime:** Use local timezone to determine response date values.
4. **includedQuestionIds:** Export only specified questions.

You can use a new function called `getSurveyQuestions()` to retrieve a data frame of question IDs and labels, which you can in turn pass to the `getSurvey()` function to export only a subset of your questions.

### getSurveys() retrieves > 100 surveys

In previous versions of qualtRics, `getSurveys()` only retrieved 100 results. It now fetches all surveys. If you manage many surveys, this does mean that it could take some time for the function to complete.

If you notice any issues with the package, or if you would like to suggest a feature, please leave a comment [here](https://github.com/JasperHG90/qualtRics/issues).
