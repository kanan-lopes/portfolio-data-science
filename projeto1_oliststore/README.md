
# Projeto 1 : Brazillian E-Commerce Public Dataset by Olist

[desafio1.ipynb](desafio1.ipynb)

## Introdução

O desafio consiste em importar e manipular datasets para um jupyter notebook e desenvolver uma análise usando os conceitos aprendidos nos cursos disponibilados nas trilhas de estudos. Durante esta trilha (segunda sprint) estudamos estatística com foco em python e sql para análise de dados. Ao longo deste arquivo será possível visulizar esses conceitos. 
O objetivo é responder os insights deferidos anteriormente à analise

## Insights

Em resumo os insight's são:

. Quais categorias com maior e menor receita

. Top 10 maiores/piores sellers a partir da receita

. Existem sellers que vendem o mesmo produto e se sim quais são e qual a variação de preço entre os sellers

. Se houve inflação no preço dos produtos, se sim, de quanto foi essa inflação em % e em R$

. Top 10 melhores sellers com melhores reviews/ Top 10 piores sellers com piores reviews

. Relação entre a quantidade de vendas e quantidade de reviews e se é possível indicar aumento ou queda nas vendas com base nessas reviews.

. Tópico livre

## Ferramentas 

Algumas das ferramentas utilizadas para o desenvolvimento do projeto:

1) Ambiente de execução: Google Colab ( https://colab.research.google.com/ )
2) Ambiente de desenvolvimento : visual studio code 
3) Ambiente de gerenciamento de alguns dados: pgAdmin4 (para algumas execuções de códigos sql)   
      observação: os códigos sql não são disponibilizados, uma vez que podemos anexar e visualizar as 'queries' diretamente no notebook desenvolvido.
4) Extensões no visual code para visualização dos datasets: (https://marketplace.visualstudio.com/items?itemName=janisdd.vscode-edit-csv) (https://marketplace.visualstudio.com/items?itemName=mechatroner.rainbow-csv)


## Desenvolvimento - Detalhamento de implementações

### Importação de bibliotecas :

1) import pandas as pd : Para utilização de gráficos, manipulação de dataframes , leitura de arquivos csv

2) import numpy as np : operações matemáticas e computações numéricas

3) import sqlite3 : Módulo para manipular bancos de dados e scripts sql diretamente no Python

4) import matplotlib.pyplot as plt : criação de gráficos e visualizações de dados

### Códigos e Queries

1) Inicialmente, precisamos ler os arquivos (no caso os dados disponibilizados) e carregá-los em DataFrames do pandas, a fim de manipularmos de maneira mais fácil. A partir dessa leitura, fiz uma conexão com o bando de dados SQLite com o objetivo de permitir de interagir e enviar comandos SQL.

    ![Imagem 1 Desafio](/evidencias/desafio1.png)


2) Deve-se tratar tabelas a serem deletas, ou seja , duplicatas e outliners, permitindo uma dinâmica melhor de manipulações sem precisar codificar individualmente cada uma. Para isso, criei um loop de verificação, o qual, itera sobre a lista de tabelas e verifica se ela existe no banco de dados, se ela existir utiliza-se DROP TABLE. Note que , ao criar as tabelas de manipulação, apelidei de forma mais simples para facilitar na execução e implementação das queries necessárias. A verificação final de duplicatas ocorre ao utilizarmos to_sql() e as devidas condições (if_exists = 'append'), não sobrescrevendo os dados e permitindo a atualização ou inserção de novos.

    ![Imagem 2 Desafio](/evidencias/desafio2.png)


    ![Imagem 3 Desafio](/evidencias/desafio3.png)

#### Insights

3) Para os insights, verifiquei (no ambiente pgAdmin4) a última compra feita para estabelecermos como data de período, no caso, como solicitado no enunciado, teremos o período de 12 meses anterior até a última compra para todos os insights.  
   Nestes insights algumas funções/ parâmetros serão similares para as queries. Por isso não entrarei em detalhes do funcionamento e/ou implementação de lógicas em alguns momentos, além de que, no markdown dessa mesma sprint há maiores fundamentos/explicações sobre o que fazem as funções. 

   I. Primeiro Insight (Maior e menor Categoria a partir da receita total)
    Assim como os próximos insights, irei fazer um resumo prático do que foi feito. Neste caso, devemos selecionar os produtos por categoria dado o período de 12 meses anteriores até a última compra. Juntamos as tabelas (items,orders e products) para obter os dados e gerar as receitas totais, a qual é calculada a partir da soma das vendas de produtos por cada categoria. No caso específico, temos o código com o respectivo gráfico/resultado :

    ![Imagem 4 Desafio](/evidencias/desafio4.png)


    ![Imagem 5 Desafio](/evidencias/desafio5.png)

    
    Conclusão: Maior receita para o produto de categoria saude_beleza e a menor para fashion_roupa_infanto_juvenil



   II. Segundo Insight (Top 10 maiores sellers) 
    Precisei filtrar os pedidos entregues , considerando como dito anteriormente, o período de 12 meses a partir da última compra. Agrupamos esses dados por categoria de produto. Finalizamos a conta de receita total gerada, que no caso soma-se os preços dos intens vendidos com o número total de vendas. Para visualizarmos mais facilmente, ordenamos o resultado em ordem decrescente. Com o resultado obtivo, fiz um gráfico em barras. 
    Obs: A adição de Seller_State foi feita já nesse insight para futuras manipulações ou análises, neste caso em específico, já podemos visualizar a predominância do estado de São Paulo por exemplo:

    ![Imagem 6 Desafio](/evidencias/desafio6.png)

    Como houve predominância do estado, optei por desenvolver o gráfico considerando o seller_id, caso contrário o gráfico não teria tanta importância de visualização

    ![Imagem 7 Desafio](/evidencias/desafio7.png)


    III. Terceiro Insight (Top 10 piores sellers)
     Este caso é mais fácil de ser desenvolvido, já que é basicamente o contrário para a ordenação do insight anterior, por isso tivemos como desenvolvimento da querie o uso de ASC. Percebe-se entretanto que todos os resultados foram limitados ao número total de vendas em 1. Caso manipulássemos alguma média definida (veremos isso posteriormente em outros insights) teríamos resultados diferentes. Nesse caso também podemos visualizar que não houve tamanha predominação do estado de São Paulo, o que é interessante pois podemos concluir que os sellers desse estado tendem a ter mais de uma venda.

    ![Imagem 8 Desafio](/evidencias/desafio8.png)

    ![Imagem 9 Desafio](/evidencias/desafio9.png)

    
    IV. Quarto Insight (Sellers que vendem o mesmo produto e a varição de preço entre os sellers)
      O código dessa querie realiza uma análise detalhada dos preços de produtos vendidos por diferentes sellers, no caso dividi em duas partes principais (utilizando uma consulta CTE com a função WITH). A primeira parte recuperei dados sobre os produtos vendidos (id, categoria e o código postal do seller em vez do id para melhoras na visualização gráfica). A segunda parte, calcula, para cada produto, o número de vendedores distintos , usando having para garantir que tenha mais de um vendedor. Optei por ordenar pela variação de preço. Finalizamos com o agrupamento e a sincronização em pandas. Devido o tamanho do código, anexarei apenas a tabela resultante e a parte gráfica do resultado:

    ![Imagem 10 Desafio](/evidencias/desafio10.png)

    Conclui-se a predominância de apenas 2 sellers para o mesmo produto, e podemos ver que a variação de preço, em termos econômicos, não é exagerada. Pelo gráfico (utilizei o método de bolhas para visualização), percebe-se uma concentração na variação de preço entre 0 R$ - 100 R$

    ![Imagem 11 Desafio](/evidencias/desafio11.png)


    V. Quinto Insigt (Inflação)
     Nessa querie, importei a função display da biblioteca IPython.display. Para mostrar os objetos ded forma mais interativa.
     O primeiro caso de inflação tratado foi para a fórmula de inflação simples, no caso ela considera , durante o período definido, o resultado do preço final (ultima compra do produto) substituida pela primeira compra do produto. Para transformar-mos em porcentual basta fazer esse mesmo cálculo divido pelo preço final e posteriormente multiplicamos por 100.
     Sobre a query, ela é dividida nas duas seleções (primeiro e ultimo preço) e posteriormente façamos a junção para os products_id e utilizamos o calculo. Novamente, irei indexar os resultados pois o código é ligeiramente grande e podemos visualizar ele no notebook em si.

     ![Imagem 12 Desafio](/evidencias/desafio12.png)

     ![Imagem 13 Desafio](/evidencias/desafio13.png)


     Quinto Insight parte 2: 
     Para mais análises, utilizei também o índice IPVA do ano de 2018 para comparmos a inflação, a ideia da query permanece bastante similar, basta mudarmos os parâmetros de cálculo. No caso esse índice foi retirado do seguinte documento: https://www.bcb.gov.br/conteudo/relatorioinflacao/EstudosEspeciais/Decomposicao_da_inflacao_de_2018.pdf

     ![Imagem 14 Desafio](/evidencias/desafio14.png)

     Com os diferentes parâmetros,  considerei criar um gráfico de comparação entre os percentuais de inflação, onde a inflação ajustada corresponde aquela com o índice aplicado:

    ![Imagem 15 Desafio](/evidencias/desafio15.png)

     Observa-se que em conclusão, com o índice aplicado temos extremos diferentes , onde a maior inflação percentual é de quase 52% e a menor é de "-3.6%", já que o produto barateou. Mas em geral temos predominância de encarecimento para ambas inflações.


    VI. Sexto Insight (Top 10 melhores sellers com melhores reviews)
     Neste insight, o objetivo era executar uma análise com base não somente em vendas mas como reviews para cada seller.
     Nesse sentido, como dito anteriormente, é interessante aplicarmos , não somente uma métrica para as reviews, mas também uma média para as vendas, isso porque se não adaptarmos o número de vendas, a métrica não corresponderia à realidade de média de reviews, por exemplo, um seller que vendeu um produto e recebeu nota 5 (ou 1) seria prejudicado, já que o peso seria unitário.
     Dito isso, fiz uma média de vendas totais (com o valor de 26) e filtrei para considerarmos sellers que tiveram número de vendas maior que essa média. 
     Para convertemos as médias em uma métrica aplicada em filtragem, optei por definir pesos para cada uma delas em escala, é possível observar isso no código, mas para parâmetros de explicação o peso é proporcional à nota, ou seja, para menores notas, menor é peso para o cálculo final.

     Em destaque, temos os pesos definidos aplicados para os scores de reviews:

    ![Imagem 16 Desafio](/evidencias/desafio16.png)

    Obtemos então a tabela e o gráfico respectivamente:

    ![Imagem 17 Desafio](/evidencias/desafio17.png)


    VII. Sétimo Insight (Top 10 piores sellers com piores reviews)
     Novamente a métrica aplicada é igual a anterior, a diferença neste caso é o tratamento de ordenação, onde selecionei os piores sellers com as menores médias de review_score:

     ![Imagem 18 Desafio](/evidencias/desafio18.png)
    

    Conclusão: No caso tratado, podemos adiantar o próximo insight, embora tratado individualmente, porém nota-se que não necessariamente a qualidade de reviews é diretamente proporcional ao total de vendas do seller. Observe que no gráfico, o seller com maior número de vendas (2eb70248d66e0e3ef83659f71b244378) possui um índice de review_score de 0.8 , entrando no top 10 de piores sellers de acordo com a review, porém possui grande quantidade de vendas. De certo modo, também é possível considerar que, quanto maior o número de vendas, maior a possibilidade de reviews serem aplicadas nos produtos, o que proporciona então a avaliação , que neste caso específico , tendeu-se a avaliações ruins.

    ![Imagem 19 Desafio](/evidencias/desafio19.png)

    VIII. Oitavo Insight: Relação entre quantidaded de vendas e qualidade das reviews, aumento ou queda de vendas de acordo com o score
     Como analisado anteriormente ja conseguimos definir essa relação, porém fiz outros tratamentos para esse insight.
     Primeiramente, devemos relacionar as vendas e reviews totais:

    ![Imagem 20 Desafio](/evidencias/desafio20.png)

     Note que, como esperado, os valores são diretamente proporcionais, temos agora que aplicá-los em um espaço tempo, para definirmos essa relação com mais detalhes, apliquei o perído mensal por venda, pois assim não generalizamos as vendas do seller no total de tempo, pois nesse caso é outro impacto aplicado para a compra dos clientes.

    ![Imagem 21 Desafio](/evidencias/desafio21.png)


    Finalizei então com um gráfico de relação entre o total de vendas e o total de reviews aplicadas aos sellers:
    Note então, que , diferentemente da conclusão anterior para o caso especíco tratado, temos uma relação proporcional, ou seja, quanto maior a quantidaded de vendas, maior o total de reviews - impactando então na comercialização dos produtos. Porém, para afirmação final , apliquei o concecito estatístico de Person, resultante em 0.96 ou seja índice forte, de fato existe a relação entre vendas e reviews.

    ![Imagem 22 Desafio](/evidencias/desafio22.png)


    ![Imagem 23 Desafio](/evidencias/desafio23.png)


    Insights de tópico livre 

     Nesta parte da documentação foi solicitado tópicos livres. Irei entrar em alguns detalhes os quais se alinham um pouco com os insights passados. Por exemplo, o primeiro tópico livre que quis avaliar foram de cidades com mais vendas 

    ![Imagem 24 Desafio](/evidencias/desafio24.png)

     Percebe-se que, como esperado, cidades do estado de São Paulo (e a própria capital) predominam , como visto no insight de top 10 sellers com maiores receitas. Embora tal predominância, também analisei as cidades que mais compram:

    ![Imagem 25 Desafio](/evidencias/desafio25.png)

    Nota-se que há uma maior discrepência em estados para o total de compras, apesar de novamente a liderança ser no estado de são paulo e novamente com a capital. Esse fator me fez relacionar o total de entregas, então decidi fazer uma query considerando isso, pois, caso o resultado fosse com diferentes cidades, seria um fator interessante a ser estudado, como que as cidades com mais compras não possuíriam a mesma quantidade de entregas? Em quais outros parâmetros são somados os totais de entregas? Porém isso não ocorre, já que temos os mesmos resultados:

    ![Imagem 26 Desafio](/evidencias/desafio26.png)


    Posteriormente, pela quantidade de entregas, tive a curiosidade de saber a média dos fretes e depois fiz um gráfico relacionando essa média com o total de entregas:

    ![Imagem 27 Desafio](/evidencias/desafio27.png)

    Conclui-se então que a uma variação não tão regular, embora a partir de em torno de 7000 vendas a média do frete tende a cair.


    
    Fiz um tópico sobre minha cidade natal sem fundamento prático, apenas para visualização de curiosidades:

    ![Imagem 28 Desafio](/evidencias/desafio28.png)