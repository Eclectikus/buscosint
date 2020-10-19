# Sobre el código de los prototipos

**buscOSINT** se adhiere a la filosofía [**`JAMstack`**](https://jamstack.org/).

## Intro

El stack de desarrollo de los prototipos es el siguiente:

- Programación: [**`R`**](https://www.r-project.org/) / [**`RStudio`**](https://rstudio.com/)
  - Principales librerías: [knitr](https://yihui.org/knitr/), [tidiverse](https://www.tidyverse.org/) y [DT](https://rstudio.github.io/DT/)
- Desarrollo web:
  - Sitio generado con [**`Hugo`**]() y distribuido por [**`Netlify`**]()
  - Front end: [**`Bootstrap`**](https://getbootstrap.com/)

## Código

A continuación los fragmentos de código ([**`chunks`**](https://rmarkdown.rstudio.com/lesson-3.html)) principales.

:warning: Se trata del código utilizado para la versión [**`V.0.4`**](https://buscosint.netlify.app/es/buscosintv0.4/buscosint04), y puede variar sustancialmente entre versiones.

:information_source: Estos fragmentos de código no serán actualizados en el repositorio y se comparten por mero interés divulgativo. El código de la **`versión de producción`** será compartido integramente.

### Librerías

Algunas pueden no ser imprescindibles para correr el código pero pueden ser útiles para el pre-proceso, importación, etc...

```{r setup, include=FALSE}
library(tidyverse) # ggplot, dplyr...
library(magrittr) # %>%
library(knitr) # Dynamic report generation with R
knitr::opts_chunk$set(
	echo = FALSE,
	out.width = "100%",
	message = FALSE,
	warning = FALSE,
	cache = FALSE,
	comment = NA,
	dpi = 96,
	prompt = FALSE,
	results = "asis"
)
library(viridis) # Color palettes
library(plotly) # Interactive graphics
library(htmltools) # HTML generation and output
library(htmlwidgets) # framework for creating HTML widgets
library(widgetframe) # isolates HTML widgets in <iframe></iframe>
library(kableExtra) # Easily insert FontAwesome icons into R Markdown docs and Shiny apps
library(DT) # R interface to the JavaScript library DataTables 
library(formattable) # Provides functions to create formattable vectors and data frames
library(summarytools) # provides functions to summarize numerical and categorical data
st_options(plain.ascii = FALSE, style = "rmarkdown", lang = "es", dfSummary.na.col = FALSE, headings = FALSE)
library(wordcloud)
library(wordcloud2)
```

### buscOSINT

```{r buscosint}
## Actualización de la lista vía GIST
gistact <- "https://raw.githubusercontent.com/Eclectikus/buscosint/main/data/buscosintLIST.csv"

tosint <- read.csv(gistact, fileEncoding = "UTF-8")

datatable(tosint,
  extensions = c('FixedHeader','Select','SearchPanes','Buttons','AutoFill'),
  options = list(
  language = list(url = 'https://cdn.datatables.net/plug-ins/1.10.11/i18n/Spanish.json'),
  dom = 'Pfrtip',
  autoFill = TRUE,
  initComplete = JS("
                    function(settings, json) {
                    $(this.api().table().header()).css({
                    'background-color': '#0b0666',
                    'color': '#ffffff'
                    });
                    }
                    "),
  pageLength = 50,
  fixedHeader = TRUE,
  keys = TRUE,
  scrollX = FALSE,
  lengthMenu = c(10, 50, 100, 300, 500),
  # fixedColumns = list(leftColumns = 3, rightColumns = 3),
  columnDefs = list(list(
    className = 'dt-center',
    targets = 3
    )
  )
  )) %>%
  formatStyle(names(tosint),
    color = 'darkblue',
    fontsize = 'small',
    fontWeight = 'normal')
```

### Tabla de tópicos

```{r tipos}
# Enlace a tabla 
tablink <- "https://raw.githubusercontent.com/Eclectikus/buscosint/main/data/buscosintLIST.csv"

## Importar a data.frame
tosint <- read.csv(tablink, fileEncoding = "UTF-8")

tipos <- count(tosint,Tipo) # Cuenta de tipos de enlace

datatable(tipos, rownames = FALSE,
  extensions = c('FixedHeader','Select', 'SearchPanes', 'Buttons'),
  options = list(
  # dom = 't',
  language = list(url = 'https://cdn.datatables.net/plug-ins/1.10.11/i18n/Spanish.json'),
  initComplete = JS("
                    function(settings, json) {
                    $(this.api().table().header()).css({
                    'background-color': '#0b0666',
                    'color': '#ffffff'
                    });
                    }
                    "),
  # order = list(ncol(jhcasosTb), 'desc'),
  pageLength = 60,
  fixedHeader = TRUE,
  scrollX = FALSE,
  lengthMenu = c(10, 50, 100),
  # fixedColumns = list(leftColumns = 3, rightColumns = 3),
  columnDefs = list(list(
    className = 'dt-center',
    targets = 1
    )
  )
  )) %>%
  formatStyle(names(tipos),
    color = 'darkblue',
    fontWeight = 'bold') %>%
  formatStyle("n",
    background = styleColorBar(range(tipos$n), 'SkyBlue'),
    backgroundSize = '90% 75%',
    backgroundRepeat = 'no-repeat',
    backgroundPosition = 'center'
      )
```

### Nube de etiquetas

```{r cloud, echo=FALSE, dpi=300, fig.width=6, fig.height=6}
tablink <- "https://raw.githubusercontent.com/Eclectikus/buscosint/main/data/buscosintLIST.csv"

## Importar a data.frame
tosint <- read.csv(tablink, fileEncoding = "UTF-8")

tipos <- count(tosint,Tipo) # Cuenta de tipos de enlace

set.seed(333) # por reproducibilidad 

wordcloud(words = tipos$Tipo, freq = tipos$n, min.freq = 1, scale = c(2.4,0.45), max.words=60, random.order=FALSE, rot.per=0, colors = brewer.pal(10, "Spectral"))

```

### Gráfico de piruletas

```{r piruletas, echo=FALSE, dpi=300, fig.width=6, fig.height=6}
# Enlace a tabla
tablink <- "https://raw.githubusercontent.com/Eclectikus/buscosint/main/data/buscosintLIST.csv"

## Importar a data.frame
tosint <- read.csv(tablink, fileEncoding = "UTF-8")

tipos <- count(tosint,Tipo) # Cuenta de tipos de enlace
tipos %>%
  arrange(tipos,-n) %>% # Odenados por número de enlaces
  ggplot(aes(x = Tipo, y = n, label = n)) +
     geom_segment(aes(x=reorder(Tipo,n), xend=reorder(Tipo,n), y=0, yend=n), color="MediumBlue") +
     geom_point(size=3.3, color="MediumBlue") +
     geom_text(color = "white", size = 2.1) +
     coord_flip() +
     theme(
       panel.grid.minor.y = element_blank(),
       panel.grid.major.y = element_blank(),
       legend.position="none"
     ) +
     theme(axis.text = element_text(size = 6)) +
     theme(axis.title = element_text(size = 7.5)) +
     xlab("") +
     ylab("Número de enlaces ya indexados por cada tópico")
```

### Imágenes

Rutinas de transferencia de ficheros desde el **`repositorio privado`** (en este caso local) a este repositorio público ([**`/img`**](https://github.com/Eclectikus/buscosint/tree/main/img)).

```{r piccloud, include=FALSE}
## Files
direl <- "D:/Developer/OSINT/hackathon2020/static/es/buscosintV0.4/buscosint04_files/figure-html"
cloudpng <- "cloud-1.png"
direr <- "D:/Developer/misWEBS/buscosint/img"
file.copy(paste(direl,cloudpng, sep = "/"), paste(direr,cloudpng, sep = "/"), overwrite = TRUE, recursive = FALSE, copy.mode = TRUE, copy.date = TRUE)
```

```{r picpirul, include=FALSE}
## Files
direl <- "D:/Developer/OSINT/hackathon2020/static/es/buscosintV0.4/buscosint04_files/figure-html"
cloudpng <- "piruletas-1.png"
direr <- "D:/Developer/misWEBS/buscosint/img"
file.copy(paste(direl,cloudpng, sep = "/"), paste(direr,cloudpng, sep = "/"), overwrite = TRUE, recursive = FALSE, copy.mode = TRUE, copy.date = TRUE)
```
