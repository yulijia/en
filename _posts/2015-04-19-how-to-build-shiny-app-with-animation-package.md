---
published: true
layout: post
title: "How to build shiny app with animation package"
author: Yu
categories:
- HowTo
- R-Language
tags:
- Shiny
- animation
- R
---

Definitely, animation package do not provide any interface for shiny app. Take `grad.desc()` for example, if we want to build an dynamic app that control all arguments of `grad.desc()` on app panel, we need split the function into sections and rewrite each section into server.R file.

I have another simple method to generate anmiation with shiny app. The method is generate pictures and use shiny app to show all pictures as slides.

Firstly, I need generate animation frames, use `saveHTML()` can get all pictures in `images` folder.

```r
saveHTML({
         ani.options(interval = 0.3)
         grad.desc()
     }, img.name = "grad.desc", htmlfile = "grad.desc.html", ani.height = 500, 
         ani.width = 500, title = "Demonstration of the Gradient Descent Algorithm", 
         description = "The arrows will take you to the optimum step by step.")
```


Then, write a slide-app.


**ui.R**

```r
library(shiny)
shinyUI(fluidPage(
  # Application title
  titlePanel("Grad.desc demo"),
  # Sidebar with a slider input for the number of frames
  sidebarLayout(
    sidebarPanel(
      sliderInput('myslider', 
                  'Steps', 
                  min=1, 
                  max=35, # count all frames(pictures) 
                  value=1, 
                  animate=animationOptions(interval = 0,loop=T)
      )
    ),  
    # Show ui
    mainPanel(
      uiOutput("ui")
    )
  )
))
```

**server.R**

```r
library(shiny)
shinyServer(function(input, output, session) {
  imgurl <- reactive({
    i=input$myslider
    return(paste0("./images/grad.desc",i,".png")) #the path of pictures
  })
  output$ui <- renderUI({
    tags$div(
      tags$img(src = imgurl())
    )
  })
})
```  

The directory structure.

~~~
.
|-- server.R
|-- ui.R
`-- www
    `-- images # images folder
~~~

Finally, `runApp("demo")` you would see the demo app running successfully!

![Imgur](https://i.imgur.com/eUkyY4l.png)


