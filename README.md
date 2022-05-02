# Otimização do sistema de manutenção de caminhões de uma frota


## Descrição do problema

Nesta análise nós exploramos uma base de dados que contém informações sobre os veículos de uma frota de caminhões. As informações gerais sobre os veículos encontram-se em colunas com nomenclatura codificada e também temos uma coluna que indica se o veículo precisou realizar manutenção no sistema de ar.

O principal objetivo desta análise é aplicar técnicas de Machine Learning para prever os veículos que precisarão de manutenção. Essa predição poderia então ser utilizada para reduzir os custos da empresa. 

Uma manutenção preventiva custa $10 e previne que um caminhão apresente problemas nos sistema de ar. Se por outro lado um caminhão não realizar a manutenção preventiva e apresentar problema no sistema de ar, ele então precisará de uma manutenção corretiva que custa $500.   

Temos dados de 2016 à 2019, que chamaremos de dados pré 2020, e também dados de 2020. Além disso, sabemos quanto foi o custo da mutenção do sistema de ar da frota em 2020. A ideia central desse projeto é que se obtivermos uma boa predição dos veículos que precisarão manutenção e a mutenção preventiva (que é muito mais barata) for realizada nesses veículos preditos, então a empresa poderá reduzir os seus custos com manutenção. 

Nós exploramos e ajustamos modelos de classificação nos dados pré 2020. O modelo com a melhor performance foi então aplicado nos dados de 2020 e obtivemo o valor que poderia ter sido economizado em 2020 caso uma estratégia de manutenção baseada na predição de modelos estatísticos tivesse sido empregada.   

## Redução da dimensionalidade 

Os dados apresentam uma dimensionalidade bastante alta e também há colunas com muitos dados faltantes. 

Nós reduzimos a dimensionidade eliminando colunas que apresentam uma quantidade de dados faltantes acima de um limiar. Esse limiar foi determinado através de uma pré análise utilizando um modelo de Árvore de Decisão. Testando diversos limiares na classificação (veja a figura abaixo) verificamos que a sensibilidade , que é a nossa métrica mais importante nesse problema, já atinge valores razoáveis utilizando apenas colunas com menos de 700 dados faltantes e a performance não apresenta melhora significativa esse limiar é aumentado. Esse procedimento permite eliminarmos 72 colunas.  

![limiar](https://user-images.githubusercontent.com/88217999/166174802-d61342d0-91a1-46dd-8c7f-6800edf49c02.png)

## Teste de modelos

Nós testamos alguns modelos de classificação e o modelo XGBoost foi o que apresentou melhor performance. Esse foi então o modelo que utilizamos para realizar a predição da manutenção dos veículos. 

![modelos](https://user-images.githubusercontent.com/88217999/166236593-3860d051-16d5-4a63-bf75-6cf77ef4fb1d.png)

## "Correção" do desbalanceamento das classes

Uma outra característica importante nesses dados é que há um forte desbalanceamento nas classes da variável de resposta. Nos dados pré 2020 temos 59000 pertencentes à classe "neg" (veículos que não precisaram de manutenção) e apenas 1000 na classe "pos" (veículos que precisaram de manutenção). Esse desblancemanto foi remediado incluindo-se um peso maior na classe minoritária durante o ajuste dos modelos. 

No modelo XGBoost esse peso adicional é incluído através do hyperparêmetro "scale_pos_weight". Utilizamos o valor scale_pos_weight = 1000, que foi o valor que forneceu a maior sensibilidade na previsão durante a tunagem desse parâmetro. 

Com esse ajuste a sensibilidade passa de 0.76 para 0.83.

## Resultado da aplicação do modelo nos dados de 2020 e conclusões sobre a otimização no sistema

O modelo XGBoost treinado nos dados pré 2020 apresenta sensibilidade de 0.848 (s = 0.848) quando aplicado nos dados de 2020. Ou seja, o modelo prediz corretamente 84.8% dos veículos que precisaram de manutenção.

Para determinarmos quanto teria sido gasto em 2020 se uma estratégia que utilizasse dessas predições fosse utilizada, nós olhamos para alguns outros números.

O modelo prediz a manutenção de 396 veículos (P = 396) enquanto os dados apresentam 375 veículos que precisaram de manutenção (M = 375).

Número de predições corretas:

PC = s . 375 = 318.

Número de veículos que precisaram de manutenção mas não foram preditos:

NP = M - PC = 375 - 318 = 57.

Calculamos agora o valor que teria sido gasto em 2020. Lembrando que o valor da manutenção preventiva é vp = $10 e o valor da manutenção corretiva é de vc = $500, o valor total gasto se a manutenção preventiva tivesse sido feita nos veículos preditos teria sido de:

total gasto  = vp . P + vc . NP = $10 . 396 + $500 . 57 = $32460. 

Esse valor está bem abaixo do valor real que foi gasto de $187500.

Portanto, essa nova estratégia baseada em Machine Learning fornece uma grande redução nos custo com a manutenção dos veículos.
