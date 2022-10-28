# Portfolio
Análise de retorno, volatilidade, custo de capital e alocação do portfolio


import numpy as np
import pandas as pd
from pandas_datareader import data as wb
import matplotlib.pyplot as plt
tickers=['COGN3.SA', 'BBDC4.SA', 'VVAR3.SA', 'ITUB4.SA']
mydata=pd.DataFrame()
for t in tickers:
mydata[t]=wb.DataReader(t,'yahoo','2019-1-1')['Adj Close']
mydata.iloc[0]

#Gráfico Retorno
(mydata/mydata.iloc[0]*100).plot(figsize=(8,5))
plt.show()

#Retorno Anual Médio
pesos=np.array([0.5, 0.15, 0.25, 0.1 ])
retorno=(mydata/mydata.shift(1))-1
retorno_anual=retorno.mean()*250
portfolio=str(round(np.dot(retorno_anual,pesos),4)*100) + ' %'
print(portfolio)

#Análise Vol
retorno_vol=np.log(mydata/mydata.shift(1))

#Vol Diária
vol_d=retorno_vol[["COGN3.SA", 'BBDC4.SA', 'VVAR3.SA', 'ITUB4.SA']].std()
vol_d

#Vol Anual
vol_a=vol_d*250**0.5
vol_a

#Vol Carteira
vol_cart=(np.dot(pesos.T,np.dot(retorno.cov()*250,pesos)))**0.5
print(str(round(vol_cart,4)*100)+ ' %')

#Beta
import numpy as np
import pandas as pd
from pandas_datareader import data as wb

acoes = ['ITUB4.SA', '^BVSP']
data = pd.DataFrame()
for t in acoes:
data[t] = wb.DataReader(t, 'yahoo', '2015-1-1')['Adj Close']

retorno = np.log( data / data.shift(1) )
cov = retorno.cov() * 250
cov_mercado = cov.iloc[0,1]
var_mercado = retorno['^BVSP'].var() * 250

beta = cov_mercado / var_mercado
beta

#CAPM
capm=0.025*beta**0.05
print(str(round(capm,3)*100)+ ' %')

#Sharpe
Sharpe = (capm - 0.025) / (retorno['ITUB4.SA'].std() * 250 ** 0.5)
Sharpe
