# Análise do Consumo de Energia Elétrica no Brasil

![Energia eletrica](https://github.com/DevLuizPy/pib_energia_regressao/blob/main/energia-eletrica-covid-19.jpg)
![Made with Jupyter](https://img.shields.io/badge/Made%20with-Jupyter-F37626?style=for-the-badge&logo=Jupyter&logoColor=white)
![Made with Python](https://img.shields.io/badge/Made%20with-Python-3776AB?style=for-the-badge&logo=Python&logoColor=white)

Autores: Luiz Fernando Moreira Meirinho | Victor Assumpção Santos de Souza

Email: luizfcefetrj@gmail.com | victor.assump9@gmail.com

## Sumário

## 1. Introdução

Este trabalho busca estudar o consumo de energia elétrica no Brasil, com investigações acerca da correlação entre nosso objeto de estudo com diversas outras variáveis, que vão desde variáveis macroeconômicas – como PIB e emprego – até recursos naturais, como petróleo e gás natural. Também investigamos o consumo de energia elétrica em setores específicos, mais precisamente o comercial e o industrial. Nosso objetivo é desvendar os detalhes do consumo de eletricidade do país, e como ele é afetado por outras variáveis econômicas que consideramos relevantes para esse trabalho.

## 2. Metodologia

Todos os dados estatísticos foram retirados do site do Banco
Central, enquanto os gráficos e regressões lineares foram feitos
com a linguagem de programação “Python”. Por mais que haja
dados acerca do consumo de energia elétrica a partir de 1979,
padronizamos a data de todos os dados a partir de 2002, quando
se iniciam os dados industriais no site do BACEN. A interpretação
dos resultados e a contextualização econômica dos mesmos
foram realizados por meio de conhecimentos prévios e atuais
obtidos das disciplinas do curso de Ciências Econômicas da UFRRJ.

## 3. Código

#### Importação das bibliotecas
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
#### Obtendo os dados 
``` python
# Importação da base de dados do banco central
Energia=sgs.get({'Energia':1406},start='2002-01-01',end='2023-07-01')
pib_mes = sgs.get({'pib_mes':4380},start='2002-01-01',end='2023-07-01')
Industria_geral = sgs.get({'Industria_geral':28503},start='2002-01-01',end='2023-07-01')
Petroleo = sgs.get({'Petroleo':1389},start='2002-01-01',end='2023-07-01')
Comercio_total = sgs.get({'Comercio_total':28473},start='2002-01-01',end='2023-07-01')
Emprego = sgs.get({'Emprego':28763},start='2002-01-01',end='2023-07-01')
```

#### Normalização dos dados
``` python
# Normalização dos dados
demand = pd.DataFrame()
start_date = Industria_geral.index[0]
demand['Industria_geral'] = Industria_geral/Industria_geral.loc[start_date]
demand['pib_mes'] = pib_mes/pib_mes.loc[start_date]
demand['Energia'] = Energia/Energia.loc[start_date]
demand['Petroleo'] = Petroleo/Petroleo.loc[start_date]
demand['Comercio_total'] = Comercio_total/Comercio_total.loc[start_date]
demand['Emprego'] = Emprego/Emprego.loc[start_date]
```

#### Gerando gráfico de regressão simples
``` python
# Criação dos gráficos
plt.style.use('cyberpunk')
plt.grid(True)
sns.regplot(x='pib_mes',y='Energia', data= demand, color= 'white')
plt.title('Regressão linear Pib Mensal e Energia',fontsize=18)
plt.axis([0.8,9,0.7,2.5])
plt.xlabel('Pib Mensal',fontsize=16)
plt.ylabel('Consumo de Energia',fontsize=16)
plt.show
```

#### Criando os resultados da Regressão pelo metodo dos Minimos quadrados perfeitos
``` python
# Resultados da regressão linear
y = demand['Energia']
x1 = demand['pib_mes']
x = sm.add_constant(x1)
reg = sm.OLS(y,x).fit()
reg.summary()
```

#### Criando os resultados da Regressão pelo metodo dos Minimos quadrados perfeitos
``` python
# Resultados da regressão multipla
y = demand['Energia']
x1 = demand['Comercio_total']
x2 = demand['Industria_geral']
x3 = demand['Petroleo']
x = np.column_stack((x1,x2,x3))
x = sm.add_constant(x)
reg2 = sm.OLS(y,x).fit()
reg2.summary()
```

#### Criando gráfico de correlação (heatmap)
``` python
# Criando gráfico tipo heatmap para correlação
heatmap = sns.heatmap(demand.corr(), vmin=-1, vmax=1, cmap='Blues', annot = True)
heatmap.set_title('Correlation Heatmap', fontdict = {'fontsize':20}, pad = 20)
plt.figure(figsize=(12,12))
plt.show()
```

## 4. Resultados - Regressão Linear

| **Variável** | **Coeficiente** | **Erro Padrão** | **Estatística t** | **P-valor** | **[0.025** | **0.975]** |
|--------------|-----------------|-----------------|-------------------|-------------|------------|------------|
| Constante    | 1.1287           | 0.013           | 84.862            | 0.000       | 1.103      | 1.155      |
| PIB_Mes      | 0.1211           | 0.003           | 38.395            | 0.000       | 0.115      | 0.127      |

**R-squared**: 0.852

Em primeira análise, observamos a correlação entre a variação do PIB mensal com o consumo de energia elétrica. A teoria para tal é que, no caso do PIB aumentar, o consumo de energia elétrica também aumentará, visto que um aumento do Produto Interno Bruto representaria uma maior demanda de energia elétrica por parte dos consumidores. As estatísticas sugerem que tal teoria se mantém: o gráfico demonstra que conforme o PIB mensal aumenta, o consumo de energia o acompanha. A regressão linear também dá suporte a veracidade da teoria, com a hipótese nula (na qual a variável PIB não teria correlação com a variável energia) sendo rejeitada. O R-quadrado tem valor de 85,7%, sugerindo que essa é a porcentagem do consumo de energia que pode ser explicado pela variável PIB. O coeficiente de correlação é de 0,93, o que indica correlação fortíssima entre as variáveis. 

## 5. Resultados - Regressão multipla

| **Variável** | **Coeficiente** | **Erro Padrão** | **Estatística t** | **P-valor** | **[0.025** | **0.975]** |
|--------------|-----------------|-----------------|-------------------|-------------|------------|------------|
| Constante    | 0.2341           | 0.061           | 3.858             | 0.000       | 0.115      | 0.354      |
| x1           | 0.4083           | 0.062           | 6.563             | 0.000       | 0.359      | 0.458      |
| x2           | 0.2337           | 0.052           | 4.493             | 0.000       | 0.131      | 0.336      |
| x3           | 0.2713           | 0.025           | 10.710            | 0.000       | 0.221      | 0.321      |

**R-squared**: 0.953

A regressão múltipla mostra um coeficiente dedeterminação de 0,953. A hipótese de que nãohá correlação entre as variáveis é recusada. Omapa de calor, no entanto, indica que a indústriadestoa das outras variáveis e tem um baixo nívelde correlação. Isso pode ser explicado peladiversificada matriz energética industrialpreviamente mencionada. Por outro lado, energia,comércio e petróleo, mostram grande correlaçãoentre si, todas consideradas fortes oufortíssimas.Por fim, conclui-se que o consumo de energiaelétrica no Brasil é afetado por todas as variáveisestudadas. Todos os dados abordados sugeremuma correlação positiva entre as variáveisdependente e independente, exceto entreindústria e petróleo. Como observado na primeira correlação linear, acada unidade de aumento do PIB, há umaumento de 0,12 no consumo de energia. Portanto, é necessário que conforme o Brasilcresça, haja uma diversificação da matriz elétricapara absorver o aumento de demanda. Dados de2022 indicam que a produção é mais de 60%provida por hidrelétricas, que são frágeis a secas. Caso a oferta de eletricidade não acompanhe oaumento de demanda, um momento de escassezde chuva pode causar uma grave crise energéticanacional.

## 6. Resultados - Regressão multipla

EMPRESA DE PESQUISA ENERGÉTICA (EPE). Matriz energética e elétrica. Disponível em: https://www.epe.gov.br/pt/abcdenergia/matriz-energetica-e-eletrica. Acesso em: 22 set. 2024.

BANCO CENTRAL DO BRASIL (BCB). Séries temporais. Disponível em: https://www3.bcb.gov.br/sgspub/. Acesso em: 22 set. 2024.
