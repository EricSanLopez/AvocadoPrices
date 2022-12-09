# Avocado Prices

## Introducció

Els alvocats són un aliment molt popularitzat els últims anys a tot arreu del món. L'anomenat or verd conté molts nutrients i d'altres beneficis per la salut, el que l'ha fet una fruita indispensable a tot tipus de cuines, des de les veganes fins en forma de guacamole pels natxos. <br><br>
Aquesta base de dades registra les vendes d'alvocats durant 4 anys (2015-2018) de diversos tipus a múltiples regions dels EEUU, amb la quantitat, el format de venda i el preu, entre d'altres. L'objectiu d'aquest estudi serà entendre com ha evolucionat el preu al llarg del temps en funció de les features.<br><br>
Les features són les següents:
 - Index: nombre del 0 al 52, no s'explica el significat *(int)*
 - Date: data de l'observació *(date)*
 - Total Volume: quantitat total d'alvocats venuts *(float)*
 - 4046: nombre total d'alvocats amb codi 4046 venuts *(float)*
 - 4225: nombre total d'alvocats amb codi 4225 venuts *(float)*
 - 4770: nombre total d'alvocats amb codi 4770 venuts *(float)*
 - Total Bags: nombre total de bosses venudes *(float)*
 - Small Bags: nombre de bosses petites venudes *(float)*
 - Large Bags: nombre de bosses grans venudes *(float)*
 - XLarge Bags: nombre de bosses molt grans venudes *(float)*
 - Type: tipus (organic o conventional) (str)
 - Year: any de la venda *(int)*
 - Region: ciutat o regió de la venda *(str)*
 - <font color=blue>**AveragePrice: preu mitjà de venta dels alvocats**</font>

### Imports 
```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### Observacions prèvies

Es poden observar alguns valors estranys, com el fet que la quantitat d'alvocats venuts sigui decimal, que el volum total no correspongui a la suma dels alvocats venuts o que el total de bosses no sempre sigui la suma de les tres bosses.<br>

```py
data = pd.read_csv('avocado.csv')

data.head(10)
```

```py
# La suma d'alvocats no correspon amb el total

pd.concat([pd.DataFrame(data['Total Volume']), pd.DataFrame(data['4046'] + data['4225'] + data['4770'])], axis=1).head(10)
```

```py
# La suma de bosses no sempre correspon amb el total

bags_sum = data['Small Bags'] + data['Large Bags'] + data['XLarge Bags']
coincidencies = list(data['Total Bags'] == bags_sum)

sns.countplot(x=coincidencies)
```

En un cas real s'hauria de consultar al creador del dataset i preguntar sobre aquests casos per interpretar el que volen dir realment i saber d'on poden haver sorgit aquests errors. En aquest cas, però, no tenim accés a aquestes explicacions, en canvi a l'apartat de discussions d'aquest dataset a Kaggle hi ha un usuari anomenat Pedro Israel que ha netejat les dades per tal que aquestes tinguin sentit.<br>

El [nou dataset](https://www.kaggle.com/datasets/pedroisrael/avocado-sales) ha realitzat els següents canvis al dataset original:
 - Ha canviat a enters totes les quantitats enumeratives, com la quantitat de bosses o alvocats
 - Ha recalculat les columnes dels totals per a que coincidissin amb la suma de les corresponents columnes
 - Ha eliminat les regions "totals" (aquelles que feien recomptes agrupant diverses regions)
 - Ha eliminat els estats, només hi ha ciutats (per tal que les files siguin independents)
 - Ha renombrat les ciutats introduint espais
 
El dataset resultant té un 18% menys de files.

```py
data_clean = pd.read_csv('avocado_clean.csv')

data_clean.head(10)
```

```py
data_clean.shape
```

### Nou dataset

Amb els canvis esmentats els atributs del nou dataset són els següents:
 - Sale ID: variable originada per l'original Index, no té sentit, només pren el valor 1 (prescindirem d'ella)
 - Date: dia de la venda *(date)*
 - AveragePrice: preu de venta mitjà per alvocat en format '$xx.xx' *(str)*
 - Total Avocados: quantitat total venuda *(int)*
 - Small 4046: quantital d'alvocats petits 4046 venuts *(int)*
 - Large 4225: quantital d'alvocats grans 4225 venuts *(int)*
 - Extra Large 4770: quantital d'alvocats extra grans 4770 venuts *(int)*
 - Total Bags: quantitat total de bosses d'alvocats venudes *(int)*
 - Small Bags: quantitat de bosses d'alvocats petits venudes *(int)*
 - Large Bags: quantitat de bosses d'alvocats grans venudes *(int)* 
 - XLarge Bags: quantitat de bosses d'alvocats extra grans venudes *(int)*
 - type: tipus d'alvocat *(str)*
 - Cities: ciutats nord-americanes on es produeix la venta *(str)*

```py
set(data_clean['Sale ID'])
```
