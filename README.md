# Otimização do sistema de manutenção de caminhões de uma frota


## Descrição do problema

Nesta análise nós exploramos uma base de dados que contém informações sobre os veículos de uma frota de caminhões. As informações gerais sobre os veículos encontram-se em colunas com nomenclatura codificada e também temos uma coluna que indica se o veículo precisou realizar manutenção no sistema de ar.

O principal objetivo desta análise é aplicar técnicas de Machine Learning para prever os veículos que precisarão de manutenção. Essa predição poderia então ser utilizada para reduzir os custos da empresa. 

Uma manutenção preventiva custa $10 e previne que um caminhão apresente problemas nos sistema de ar. Se por outro lado um caminhão não realizar a manutenção preventiva e apresentar problema no sistema de ar, ele então precisará de uma manutenção corretiva que custa $500.   

Temos dados de 2016 à 2019, que chamaremos de dados pré 2020, e também dados de 2020. Além disso, sabemos quanto foi o custo da mutenção do sistema de ar da frota em 2020. A ideia central desse projeto é que se obtivermos uma boa predição dos veículos que precisarão manutenção e a mutenção preventiva (que é muito mais barata) for realizada nesses veículos preditos, então a empresa poderá reduzir os seus custos com manutenção. 

Nós exploramos e ajustamos modelos de classificação nos dados pré 2020. O modelo com a melhor performance foi então aplicado nos dados de 2020 e obtivemo o valor que poderia ter sido economizado em 2020 caso uma estratégia de manutenção baseada na predição de modelos estatísticos tivesse sido empregada.   

## Pré-processamento e ajuste de modelos 

Os dados apresentam uma dimensionalidade bastante alta e também há colunas com muitos dados faltantes. 

Nós reduzimos a dimensionidade eliminando colunas que apresentam uma quantidade de dados faltantes acima de um limiar. Esse limiar foi determinado através de uma pré análise utilizando um modelo de Árvore de Decisão. Testando diversos limiares na classificação (veja a figura abaixo) verificamos que a sensibilidade , que é a nossa métrica mais importante nesse problema, já atinge valores razoáveis utilizando apenas colunas com menos de 700 dados faltantes e a performance não apresenta melhora significativa esse limiar é aumentado. Esse procedimento permite eliminarmos 72 colunas.  


