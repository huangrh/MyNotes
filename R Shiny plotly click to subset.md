# How to grap clicked data from plotly

```
library(plotly)
library(shiny)
require(DT)

ui <- fluidPage(
  plotlyOutput("plot"),
  DT::dataTableOutput("click"),
  verbatimTextOutput("brush")
)

server <- function(input, output, session) {

  output$plot <- renderPlotly({
    # use the key aesthetic/argument to help uniquely identify selected observations
    key <- mtcars$cyl
    p <- ggplot(mtcars, aes(x = mpg, y = wt, colour = factor(vs), key = key)) +
      geom_point() + facet_wrap(~ cyl)
    ggplotly(p) %>% layout(dragmode = "select")
  })

  output$click <- renderDataTable({
    d <- plotly::event_data("plotly_click")
    shiny::req(d)
    # print(mtcars[d$key, ])
    # subset(mtcars, cyl %in% d$key)
    data.frame(d)%>%
      DT::datatable(rownames = F)
  })

  output$brush <- renderPrint({
    d <- event_data("plotly_selected")
    req(d)
    print(mtcars[d$key, ])
  })

}

shinyApp(ui, server)
```

https://stackoverflow.com/questions/48939382/how-can-i-grab-the-row-of-data-from-a-ggplotly-in-shiny
