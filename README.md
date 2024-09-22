# Análise do Consumo de Energia Elétrica no Brasil

![Energia eletrica](https://github.com/DevLuizPy/pib_energia_regressao/blob/main/energia-eletrica-covid-19.jpg)
## Sumário

## Introdução

Este trabalho busca estudar o consumo de energia elétrica no Brasil, com investigações acerca da correlação entre nosso objeto de estudo com diversas outras variáveis, que vão desde variáveis macroeconômicas – como PIB e emprego – até recursos naturais, como petróleo e gás natural. Também investigamos o consumo de energia elétrica em setores específicos, mais precisamente o comercial e o industrial. Nosso objetivo é desvendar os detalhes do consumo de eletricidade do país, e como ele é afetado por outras variáveis econômicas que consideramos relevantes para esse trabalho.

## Metodologia

Todos os dados estatísticos foram retirados do site do Banco
Central, enquanto os gráficos e regressões lineares foram feitos
com a linguagem de programação “Python”. Por mais que haja
dados acerca do consumo de energia elétrica a partir de 1979,
padronizamos a data de todos os dados a partir de 2002, quando
se iniciam os dados industriais no site do BACEN. A interpretação
dos resultados e a contextualização econômica dos mesmos
foram realizados por meio de conhecimentos prévios e atuais
obtidos das disciplinas do curso de Ciências Econômicas da UFRRJ.

## Código
``` python
# Importação das bibliotecas
import numpy as np
import pandas as pd
from scipy import stats
import statsmodels.api as sm
import matplotlib.pyplot as plt
import seaborn as sns; sns.set()
import mplcyberpunk
from bcb import sgs

import matplotlib
matplotlib.rcParams['figure.figsize'] = (14,7)
```

