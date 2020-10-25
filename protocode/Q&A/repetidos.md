Rutina para eliminar duplicados
================
[buscOSINT](https://github.com/Eclectikus/buscosint)

## Control de duplicados

Uno de los fallos a evitar en las tablas son los registros duplicados.
Esta es una norma general de puro *sentido común*, y es fácil diseñar
una rutina que elimine las **`filas`** duplicadas sin mayores
contemplaciones.

En tablas como esta es un poco más delicado porque se pueden presentar
casos en los que registros repetidos podrían ser legítimos. Por ejemplo,
si introducimos el término **`canva`** en el buscador obtenemos estos
registros (a finales de octubre 2020)

![Términos
repetidos](https://raw.githubusercontent.com/Eclectikus/buscosint/main/img/repes1.png)

En este caso se podrían considerar legítimos las tres instancias del
sitio, quizá el tercero sea redundante, la decisión quedaría para *el
administrador*.

Para ayudar en la detección de registros duplicados se han escrito las
siguientes líneas de código. Son rudimentarias, y de momento requieren
la inspección manual de los listados que proporcionan, una acción
indispensable especialmente tras subir en bloque centenares de enlaces,
como ha sido el caso de la primera fase del proyecto.

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

**Recursos repetidos:** 30

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

49

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

65

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

77

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

121

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

155

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

212

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

270

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

316

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

410

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

435

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

572

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

579

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

643

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

659

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

679

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

709

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

771

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

883

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

940

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

950

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

1014

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

1023

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

1024

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

1031

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

1112

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

1140

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

1231

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

1241

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

1267

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

59

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

127

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

273

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

317

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

318

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

322

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

341

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

351

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

547

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

555

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

636

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

843

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

983

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

1041

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

1044

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

1109

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

1123

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

1188

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

1189

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

1251

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
