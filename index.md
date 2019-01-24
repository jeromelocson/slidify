---
title       : Top 5 Tech Companies
subtitle    : Stocks Visualization
author      : Jerome Locson
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Overview

This app will allows you to explore the stock performance of the top 5 tech companies namely: Facebook, Google, Netflix, Amazon, and Microsoft based on a given data range specified by user. The financial data are retrieved from the Yahoo! Finance website using the **quantmod** R library. The app is primarily created using Shiny.

- Shiny is an R package that makes it easy to build interactive web applications (apps) straight from R. This lesson will get you started building Shiny apps right away.

- quantmod or Quantitative Financial Modelling Framework is used to specify, build, trade, and analyse quantitative financial trading strategies.

- The app comprises of two main files: ui.R and sever.R, you can view the code in https://github.com/jeromelocson/techstockviz.

--- .class #id 

## ui.R


```r
library(shiny)
library(quantmod)
shinyUI(fluidPage(
  titlePanel("Top 5 Tech Companies - Stocks Visualization"),
  sidebarLayout(
    sidebarPanel(
      selectInput("symb", "Select Tech Company:",
                  c("Facebook" = "FB",
                    "Google" = "GOOG",
                    "Amazon" = "AMZN",
                    "Netflix" = "NFLX",
                    "Microsoft" = "MSFT")),
      dateRangeInput("dates", "Date range",start = "2018-01-01", 
                     end = as.character(Sys.Date())),
    mainPanel(plotOutput("plot"))
)))
```

--- .class #id 
## server.R


```r
library(shiny)
shinyServer(function(input, output) {
   
  dataInput <- reactive({
    getSymbols(input$symb, src = "yahoo", 
               from = input$dates[1],
               to = input$dates[2],
               auto.assign = FALSE)
  })
  output$plot <- renderPlot({
    chartSeries(dataInput(), name =  paste("TICKER: ", input$symb), 
                theme = chartTheme("white"),
                type = "candlesticks",  TA = 'addVo()', 
                up.col ="green", dn.col ="red")
  }, height=600)
})
```

--- .class #id 
## Preview

 <iframe src = 'https://jlocson.shinyapps.io/TechStock/' height='600px'></iframe>


