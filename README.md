# Big_Data_EX_06

# EX_06 - Implement an Application That Stores and Visualizes Big Data in R Using Shiny

## Aim:

To build a simple interactive Shiny web app in R that takes user input, processes (stores) data, and visualizes it dynamically.

## Prerequisites:

1. Install R and RStudio IDE (optional but recommended)

2. Install Shiny package:

```
install.packages("shiny")

```

## Step-by-Step Implementation:

### 1. Create a Single File Shiny App (app.R)

```
# Load the Shiny library
library(shiny)

# Define UI (User Interface)
ui <- fluidPage(
  titlePanel("Simple Big Data Storage & Visualization App"),
  
  sidebarLayout(
    sidebarPanel(
      sliderInput("num", "Choose number of data points:", 
                  min = 10, max = 10000, value = 1000, step = 10),
      selectInput("dist", "Select distribution type:", 
                  choices = c("Normal" = "norm", "Uniform" = "unif", "Exponential" = "exp")),
      actionButton("generate", "Generate Data")
    ),
    
    mainPanel(
      verbatimTextOutput("summary"),
      plotOutput("histPlot")
    )
  )
)

# Define Server Logic
server <- function(input, output) {
  
  # Reactive expression to generate data based on user input and button click
  dataset <- eventReactive(input$generate, {
    n <- input$num
    
    switch(input$dist,
           norm = rnorm(n),
           unif = runif(n),
           exp = rexp(n))
  })
  
  # Show summary statistics of generated data
  output$summary <- renderPrint({
    data <- dataset()
    if (is.null(data)) {
      "No data generated yet."
    } else {
      summary(data)
    }
  })
  
  # Show histogram of generated data
  output$histPlot <- renderPlot({
    data <- dataset()
    if (!is.null(data)) {
      hist(data, main = paste("Histogram of", input$dist, "distribution"), 
           col = "steelblue", border = "white")
    }
  })
}

# Run the application
shinyApp(ui = ui, server = server)

```
### Explanation:
1. UI:

- sliderInput to choose how many data points to generate (up to 10,000).

- selectInput to choose distribution type (Normal, Uniform, Exponential).

- actionButton triggers data generation.

- verbatimTextOutput displays summary stats.

- plotOutput displays a histogram of generated data.

2. Server:

- eventReactive waits for "Generate Data" button click.

- Based on selected distribution and number, generates random data.

- Outputs summary and histogram reactively.

### 2. Run the App

1. Save the code above as app.R in a folder.

2. Open R or RStudio, set working directory to folder containing app.R.

3. Run:
   
```
library(shiny)
runApp(".")

```
4. The app will open in your default web browser.

### 3. How This App Handles Big Data

1. Allows up to 10,000 data points interactively.

2. Data generated and stored reactively in memory.

3. Visualization and summary update dynamically based on input.

### 4. Extending This App to Real Big Data Storage

1. For truly large datasets, integrate with databases (e.g., SQLite, PostgreSQL) or cloud storage.

2. Use packages like DBI and RPostgres for database connection.

3. Implement paging or sampling for visualizations.

4. Utilize shinyWidgets, DT package for advanced UI and data table outputs.

### 5. Deployment Options

1. Deploy locally by sharing app.R.

2. Deploy on Shiny Server for Linux servers.

3. Deploy on shinyapps.io for cloud hosting (free tier available).

4. Use rsconnect package for deployment:

```
library(rsconnect)
rsconnect::deployApp('path_to_app_folder')

```

## Result:

1. A summary text output showing statistics (min, 1st quartile, median, mean, 3rd quartile, max) of the generated big data.

2. A histogram plot visualizing the distribution of the generated dataset based on your input choices.
