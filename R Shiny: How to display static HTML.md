# How to display static webside
#
```
ui <- navbarPage(
    title = 'title',
    tabPanel(title = "Tab1",htmlOutput('html_src')),
    tabPanel(title = "Tab2",htmlOutput('pdf_src'))
)
server <- function(input, output){
    output$side = renderUI({selectInput('Market',"Market:",choices = mkt_levels,selected = '')})
    output$html_src = renderUI({tags$iframe(src = 'filename.html',height='1080',width='100%')})
    output$pdf_src  = renderUI({tags$iframe(src = 'filename.pdf', height='1080',width='100%')})
}
shinyApp(ui = ui, server = server)
```

# https://stackoverflow.com/questions/21465411/r-shiny-passing-reactive-to-selectinput-choices
# ui
uiOutput("carControls")

#server
output$carControls <- renderUI({
    selectInput("cars", "Choose cars", rownames(mtcars))
})


# Improve UI
https://www.christophenicault.com/post/improve_shiny_ui/
