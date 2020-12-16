Rutina para chequear duplicados
================
[buscOSINT](https://github.com/Eclectikus/buscosint)

## Control de duplicados

Uno de los fallos a evitar en las tablas son los registros duplicados.
Esta es una norma general, de puro *sentido común*, y es muy fácil
diseñar una rutina que elimine las **`filas`** duplicadas sin mayores
contemplaciones.

Sin embargo en tablas como esta es un poco más delicado porque se pueden
presentar casos en los que los registros repetidos son perfectamente
legítimos. Por ejemplo, si introducimos el término **`canva`** en el
buscador obtenemos estos registros (a finales de *octubre 2020*).

![Términos
repetidos](https://raw.githubusercontent.com/Eclectikus/buscosint/main/img/repes1.png)

En este caso se podrían considerar legítimos las tres instancias del
sitio en la tabla, la primera enlaza a una herramienta específica, y la
segunda y tercera son el mismo enlace pero clasificados en dos
**`tipos`** diferentes, quedando la eliminación a criterio del
*administrador*.

-----

Para ayudar en la detección de registros duplicados se utilizan las
siguientes líneas de código. Son rudimentarias, y de momento requieren
la inspección manual de los listados que proporcionan, una acción
indispensable especialmente tras subir en bloque centenares de enlaces,
como ha sido el caso de la primera fase del proyecto. Idealmente con el
tiempo se dará con un algoritmo más elegante y eficaz, pero de momento,
aquí quedan estas líneas por si sirven a alguien.

### Chequeo de duplicados (Nombres)

``` r
gistact <- "https://raw.githubusercontent.com/Eclectikus/buscosint/main/data/buscosintLIST.csv"
tosint <- read.csv(gistact, fileEncoding = "UTF-8")

num_ocurr <- data.frame(table(tosint$Recurso))

RecDupr <- as.data.frame(num_ocurr[num_ocurr$Freq > 1,])

colnames(RecDupr) <- c("Recurso", "Repeticiones")

n <- nrow(na.omit(RecDupr))
cat(paste("**Recursos repetidos:**", n))
```

**Recursos repetidos:** 31

``` r
knitr::kable(RecDupr)
```

<table>

<thead>

<tr>

<th style="text-align:left;">

</th>

<th style="text-align:left;">

Recurso

</th>

<th style="text-align:right;">

Repeticiones

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

25

</td>

<td style="text-align:left;">

AIE

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

52

</td>

<td style="text-align:left;">

Ask

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

68

</td>

<td style="text-align:left;">

Banco Mundial

</td>

<td style="text-align:right;">

3

</td>

</tr>

<tr>

<td style="text-align:left;">

81

</td>

<td style="text-align:left;">

Bibsonomy

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

125

</td>

<td style="text-align:left;">

Canva

</td>

<td style="text-align:right;">

3

</td>

</tr>

<tr>

<td style="text-align:left;">

161

</td>

<td style="text-align:left;">

Clarify

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

218

</td>

<td style="text-align:left;">

Diigo

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

276

</td>

<td style="text-align:left;">

Evernote

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

322

</td>

<td style="text-align:left;">

Flipboard

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

417

</td>

<td style="text-align:left;">

GoogleDocs

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

442

</td>

<td style="text-align:left;">

Hashtagify

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

580

</td>

<td style="text-align:left;">

LibreOffice

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

587

</td>

<td style="text-align:left;">

Linkedin

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

653

</td>

<td style="text-align:left;">

Microsoft OneNote

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

669

</td>

<td style="text-align:left;">

MS Office

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

689

</td>

<td style="text-align:left;">

Netvibes

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

719

</td>

<td style="text-align:left;">

OCDE

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

783

</td>

<td style="text-align:left;">

PasteLert

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

814

</td>

<td style="text-align:left;">

Plotly

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

897

</td>

<td style="text-align:left;">

Riffle

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

955

</td>

<td style="text-align:left;">

SESRIC

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

965

</td>

<td style="text-align:left;">

Silobreaker

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1029

</td>

<td style="text-align:left;">

start.me

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1038

</td>

<td style="text-align:left;">

StoryMap

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1039

</td>

<td style="text-align:left;">

StoryMaps

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1046

</td>

<td style="text-align:left;">

Sway

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1128

</td>

<td style="text-align:left;">

TweetMap

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1156

</td>

<td style="text-align:left;">

UNCTAD

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1250

</td>

<td style="text-align:left;">

Worldcam

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1260

</td>

<td style="text-align:left;">

Xing

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1286

</td>

<td style="text-align:left;">

Zanran

</td>

<td style="text-align:right;">

2

</td>

</tr>

</tbody>

</table>

-----

### Chequeo de duplicados (Enlaces)

``` r
gistact <- "https://raw.githubusercontent.com/Eclectikus/buscosint/main/data/buscosintLIST.csv"
tosint <- read.csv(gistact, fileEncoding = "UTF-8")

num_ocure <- data.frame(table(tosint$Enlace))

RecDupe <- as.data.frame(num_ocure[num_ocure$Freq > 1,])

colnames(RecDupe) <- c("Enlace", "Repeticiones")

n <- nrow(na.omit(RecDupe))

cat(paste("**Enlaces repetidos:**", n))
```

**Enlaces repetidos:** 22

``` r
knitr::kable(RecDupe)
```

<table>

<thead>

<tr>

<th style="text-align:left;">

</th>

<th style="text-align:left;">

Enlace

</th>

<th style="text-align:right;">

Repeticiones

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

44

</td>

<td style="text-align:left;">

<http://clarify.io>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

114

</td>

<td style="text-align:left;">

<http://hashtagify.me>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

263

</td>

<td style="text-align:left;">

<http://twchat.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

307

</td>

<td style="text-align:left;">

<http://worldmap.harvard.edu/tweetmap>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

308

</td>

<td style="text-align:left;">

<http://worldwidescience.org>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

312

</td>

<td style="text-align:left;">

<http://www.2lingual.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

331

</td>

<td style="text-align:left;">

<http://www.ask.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

341

</td>

<td style="text-align:left;">

<http://www.bibsonomy.org>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

543

</td>

<td style="text-align:left;">

<http://www.netvibes.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

551

</td>

<td style="text-align:left;">

<http://www.newswhip.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

635

</td>

<td style="text-align:left;">

<http://www.silobreaker.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

749

</td>

<td style="text-align:left;">

<http://zanran.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

771

</td>

<td style="text-align:left;">

<https://bridge.suumitsu.eu>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

845

</td>

<td style="text-align:left;">

<https://duckduckgo.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

993

</td>

<td style="text-align:left;">

<https://products.office.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1053

</td>

<td style="text-align:left;">

<https://storymap.knightlab.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1057

</td>

<td style="text-align:left;">

<https://sway.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1122

</td>

<td style="text-align:left;">

<https://www.canva.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1138

</td>

<td style="text-align:left;">

<https://www.diigo.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1206

</td>

<td style="text-align:left;">

<https://www.libreoffice.org>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1207

</td>

<td style="text-align:left;">

<https://www.linkedin.com>

</td>

<td style="text-align:right;">

2

</td>

</tr>

<tr>

<td style="text-align:left;">

1270

</td>

<td style="text-align:left;">

<https://www.start.me>

</td>

<td style="text-align:right;">

2

</td>

</tr>

</tbody>

</table>
