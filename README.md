# Aula13-Econometria-Avancada
Testes de Raiz Unitária
                          #Aula 13 - Testes de Raiz Unitária
install.packages("forecast")                 #Instala Pacote Forecast
install.packages(urca)
install.packages("tseries")
install.packages(readxl)
install.packages("pwt8")
library(forecast)
library(tseries)
library("urca")                                #Carrega Pacote URCA
library(readxl)                                #Carrega Pacote readxl
library(pwt8)                                  #Carrega o pacote PWT8.0


data("pwt8.0")                                 #Carrega os dados elencados "pwt8.0" dispoiníveis no pacote
View(pwt8.0)                                   #Visualiza os dados na tabela pwt8.0


br <- subset(pwt8.0, country=="Brazil", 
             select = c("rgdpna","emp","xr"))  #Cria a tabela "br" com dados das linhas que assumem o valor "country" (país) igual a "Brazil", selecionando as colunas cujas variáveis são "rgdpna" (PIB), "avh" (TRABALHO)  e "xr" (CÂMBIO)

colnames(br) <-  c("PIB","Emprego","Câmbio")   #Renomeia as colunas para PIB, Trabalho e Câmbio

                                        #Separando as variáveis
PIB <- br$PIB[45:62]                    #Cria o vetor para variável PIB                  
EMPREGO <- br$Emprego[45:62]            #Cria o vetor para variável EMPREGO
CAMBIO <- br$Câmbio[45:62]              #Cria o vetor para variável CAMBIO
Anos <- seq(from=1994, to=2011, by=1)   #Cria um vetor para o tempo em anos de 1994 até 2011 


                                    #Analise para o PIB

plot(PIB, type = "l")                            #Cria gráfico para o PIB
pib <- ts(PIB, start = 1994, frequency = 1)  #Define como Série Temporal
plot(pib, main="Produto Interno Bruto", 
     ylab="Milhoes de dolares", 
     xlab="Ano")                                      #Cria gráfico da Série Temporal

acf(pib)                                          #Função de Autocorrelação
pacf(pib)                                         ##Função de Autocorrelação Parcial
reglinPIB <- lm(PIB ~ Anos)                       #Regressão linear simples do emprego em relação ao tempo
reglinPIB                                         #Exibe os resultados da regressão linear
summary(reglinPIB)
plot(pib)                                         #Gráfcio dos dados
abline(reglinPIB, col="Blue")                     #Insere a linha de regressão linear estimada

#Teste ADF

TesteADF_PIB_trend <- ur.df(pib, "trend", lags = 1)         #Teste DF-DickFuller com drift e com tendencia
summary(TesteADF_PIB_trend) 

TesteADF_PIB_drift <- ur.df(pib, "drif", lags = 1)         #Teste DF-DickFuller com drift e com tendencia
summary(TesteADF_PIB_drift) 

TesteADF_PIB_none <- ur.df(pib, "none", lags = 1)         #Teste DF-DickFuller com drift e com tendencia
summary(TesteADF_PIB_none)

#Teste Phillips-Perron

pp.test(pib)

#Teste KPSS

kpss.test(pib)

#Removendo Tendência por Meio de Regressão Linear

residuosPIB <- reglinPIB$residuals                    #Salva os resíduos no vetor residuosEMP
reglinPIBres <- lm(residuosPIB ~ Anos)                #Regressão linear dos resíduos em função do tempo
plot(residuosPIB,type="l")                            #Gráfico dos resíduos
abline(reglinPIBres, col="Blue")                      #Insere a linha de regressão linear dos resíduos
acf(residuosPIB)

#Teste ADF

TesteADF_residuosPIB_trend <- ur.df(residuosPIB, "trend", lags = 1)         #Teste DF-DickFuller com drift e com tendencia
summary(TesteADF_residuosPIB_trend)

TesteADF_residuosPIB_drift <- ur.df(residuosPIB, "drift", lags = 1)         #Teste DF-DickFuller com drift e com tendencia
summary(TesteADF_residuosPIB_drift)

TesteADF_residuosPIB_none <- ur.df(residuosPIB, "none", lags = 1)         #Teste DF-DickFuller com drift e com tendencia
summary(TesteADF_residuosPIB_none)

#Teste Phillips-Perron

pp.test(residuosPIB)

#Teste KPSS

kpss.test(residuosPIB)

#Removendo Tendência por meio da diferença

pdPIB <- diff(PIB)                                               #Calcula a primeira diferença da série de dados
diferenca1PIB <- (data.frame(PIB[2:18],pdPIB))                   #Exibe a tabela da série original coma diferença <- 
DIFERENCAPIB <- ts(diferenca1PIB, start = 1994, frequency = 1)   #Define serie temporal para a tabela diferenca1
plot(DIFERENCAPIB, plot.type="single", col=c("Black","Green"))   #Cria o grafico com as duas series
plot(pdePIB, type="l")                                           #Cria gráfico somente para a serie da diferença

#Teste ADF

TesteADF_pdPIB_trend <- ur.df(pdPIB, "trend", lags = 1)         #Teste DF-DickFuller com drift e com tendencia
summary(TesteADF_pdPIB_trend)

TesteADF_pdPIB_drift <- ur.df(pdPIB, "drift", lags = 1)         #Teste DF-DickFuller com drift e com tendencia
summary(TesteADF_pdPIB_drift)

TesteADF_pdPIB_none <- ur.df(pdPIB, "none", lags = 1)         #Teste DF-DickFuller com drift e com tendencia
summary(TesteADF_pdPIB_none)

#Teste Phillips-Perron

pp.test(pdPIB)

#Teste KPSS

kpss.test(pdPIB)

#Estimando a série temporal


#Notas na Aula 13

arima113 <- arima(pib, c(1,1,3))

#ARMA
arima110 <- arima(pib, c(1,1,0))
arima111 <- arima(pib, c(1,1,1))
arima112 <- arima(pib, c(1,1,2))

arima210 <-  arima(pib, c(2,1,0))
arima211 <-  arima(pib, c(2,1,1))
arima212 <-  arima(pib, c(2,1,2))
arima213 <-  arima(pib, c(2,1,3))
#MA
arima011 <-  arima(pib, c(0,1,1))
arima012 <-  arima(pib, c(0,1,2))
arima013 <-  arima(pib, c(0,1,3))
#AR
arima020 <-  arima(pib, c(0,1,0))

#Escolher o melhor modelo com base no menor AIC/BIC
estimacoes <- list(arima113,arima110,arima111,
                   arima112,arima210,arima211,
                   arima212,arima213,arima011,arima011, arima012,
                   arima013,arima010)
AIC <- sapply(estimacoes, AIC)
AIC <- as.data.frame(AIC)
BIC <- sapply(estimacoes, BIC)
BIC <- as.data.frame(BIC)
Modelo <-c("arima113","arima110","arima111",
                "arima112","arima210","arima211",
                "arima212","arima213","arima011","arima011", "arima012",
                "arima013","arima010") 
Resultados <- data.frame(Modelo,AIC,BIC)
View(Resultados)

#Previsão - Notas na Aula 11

melhor_modelo <-                          #Preencha com o melhor modelo de acordo com os AIC s estimados
previsto <- predict(melhor_modelo,6)      #Previsão para os próximos 6 anos seguindo o melhor modelo estimado
View(previsto$pred)                       #Valores previstos
previsto1 <- forecast(melhor_modelo,6)    #Mesma previsão mas pelo comando forecast que retorna um intervalo de confiança
previsto1                                 #Valores e intervalos previstos

previsao01 <- forecast(melhor_modelo,6)
previstoBEST <- previsao01$fitted
modelo01 <- data.frame(previstoBEST,PIB)
modelo01 <- ts(modelo01,start = 1994, frequency = 1) 
plot(modelo01, main="Previsto e Observado - Best Model", 
     plot.type="single",
     xlab="Data", 
     ylab="PIB", 
     col=c("Green","Black"))                  #Gráfico com valores previstos e observados


plot(forecast(melhor_modelo,6), 
     main ="Previsao do PIB - Melhor Modelo") #Gráfco com valores e intervalos previstos

