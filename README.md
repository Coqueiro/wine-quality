# Teste de modelagem de qualidade de vinhos

Siga para este link para ver uma nelhor descrição do teste e modelagem.

## Resolução das questões

### 1. Faça uma análise exploratória para avaliar a consistência dos dados e identificar possíveis variáveis que impactam na qualidade do vinho.

Análise exploratória presente no [seguinte arquivo](https://github.com/Coqueiro/wine-quality/blob/master/Wine%20Quality%20Prediction%20Model.ipynb), junto com outras etapas da estratégia proposta de modelagem. Versão em HTML está disposta no [seguinte link](https://github.com/Coqueiro/wine-quality/blob/master/Wine_Quality_Prediction_Model_minified.html).

-------------------------------

### 2. Para a realização deste teste você pode utilizar o software de sua preferência (Python ou R), só pedimos que compartilhe conosco o código fonte (utilizando um repositório git). Além disso, inclua um arquivo README.md onde você deve cobrir as respostas para os 5 pontos abaixo:

#### a. Como foi a definição da sua estratégia de modelagem?

A estratégia de modelagem foi dividida em algumas etapas, algumas de preprocessamento e compreensão do dataset:

1. Importação dos dados
1. Separação de masssa de dados em treino e teste
1. Limpeza e manipulação dos dados para consumo
1. Análise exploratória
1. Modelagem preditiva
1. Validação dos modelos com massa de teste

Antes mesmo da etapa de **Importação dos dados** o problema foi lido com calma para a melhor compreensão do desafio. Já que os dados possuiam variáveis descritivas, as mesmas foram utilizadas para auxiliar pesquisas com o intuito de entender seus significados físico-químicos.

Na etapa de **Separação de masssa de dados em treino e teste** nós separamos o nosso dataset em um dataset de treino e outro de validação, com o intuito de evitar qualquer viés dos dados de validação em nossas análises e modelagem, desta forma obtendo uma validação mais justa.

Na etapa de **Limpeza e manipulação dos dados para consumo**, foi feita a extração de outliers utilizando-se do z-score absoluto das variáveis do dataset de treino, além da criação de funções de higienização dos dados para facilitar a sua ingestão.

A etapa de **Análise exploratória** foi responsável por uma melhor compreensão do dataset e relacionamentos gerais entre as variáveis, que levantaram pontos de atenção para a modelagem.  

Na etapa de **Modelagem preditiva** os dados de treino foram padronizados e depois houve um processo de seleção de modelo e seleção de features, seguido de uma tunagem de hiperparâmetros. Nesta etapa os modelos foram testados utilizando uma validação cruzada 5-fold e uma função de custo customizada. 
O problema foi tratado como um problema de regressão mas os modelos foram pontuados de maneira classificatória. Uma das evoluções desta modelagem seria modelar o problema de maneira classificatória, ampliando as observações exploratórias para a compreensão das variáveis dentro de cada uma das categorias de `quality`.

Por fim, houve uma etapa de **Validação dos modelos com massa de teste** para averiguar a capacidade preditiva dos modelos treinados, comparando suas performances com a performance da seleção do valor de `quality` baseado na sua moda.

#### b. Como foi definida a função de custo utilizada?

A função de custo escolhida foi a acurácia (`accuracy`) ou a soma de classificações feitas de maneira precisa dividida pelo total de classificações.

A variável `quality` é discreta. Visto que o objetivo da modelagem é prever o valor da variável `quality`, que não é contínua no espaço linear, é interessante evitar previsões irreais, de características diferentes do esperado. Não podemos usar, portanto, funções de custo adequadas para o espaço contínuo (MAE, MSE, RMSE, MAD...).

Existem outras métricas capazes de medir o desempenho de classificadores, o viés observado da variável `quality` para valores intermediários (5, 6) não demonstrou ser grande o bastante para ofuscar melhoras de previsão em relação ao baseline. Ainda mais, a métrica de acurácia é de fácil compreensão, algo que é desejável quando trabalhando com stakeholders de diversos backgrounds.

#### c. Qual foi o critério utilizado na seleção do modelo final?

O critério utilizado na seleção final foi o resultado das validações cruzadas e, por final, o resultado da validação com os dados reservados para teste. Essa abordagem é uma derivação do método de holdout, com a adição das validações cruzadas. O resultados de validações em todas as etapas se utilizou da função de custo definida, afim de inferir os melhores modelos.

#### d. Qual foi o critério utilizado para validação do modelo? Por que escolheu utilizar este método?

A validação do modelo foi realizada utilizando um subset dos dados reservado para teste, que podemos chamar de set holdout. 

A principal motivação desta abordagem é justamente diminuir o viés que pode ser introduzido pelo set holdout nas análises e modelagens. A ideia é simular uma situação real, em que este modelo seria utilizado para analisar novos dados.

Ainda mais, o fitting dos modelos na etapa de modelagem foi feito com o uso de técnicas de validação cruzada que tem a tendência de diminuir efeitos de overfitting, algo que pode ser observado ao compararmos o desempenho dos modelos na etapa de modelagem e validação, que não apresentam diferenças expressivas, algo que por sua vez não poderia ser observado corretamente sem um set holdout.

#### e. Quais evidências você possui de que seu modelo é suficientemente bom?

É dificil de determinar se um modelo é "suficientemente" sem entender o seu contexto, a sua aplicação final. Mesmo a necessidade de um modelo muda a sua natureza. Um modelo de auxílio à decisão, por exemplo, é normalmente mais descritivo do que preditivo, auxiliando a intuição humana.

O que podemos constatar foi que o modelo selecionado apresentou uma performance notavelmente melhor que a performance obtida pelo modelo de estimativa fixa na média, o que indica que houve acúmulo de aprendizado.
