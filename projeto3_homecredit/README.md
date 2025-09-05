
# Introdução

Este projeto foca na análise de risco de inadimplência em empréstimos, utilizando técnicas de Data Science para prever a probabilidade de inadimplência. Foi implementado usando modelos de Regressão Logística e modelos baseados em árvores para oferecer insights sobre os fatores que afetam o risco de crédito.

## Objetivos

Explorar e entender o conjunto de dados fornecido.
Realizar o pré-processamento necessário para adequar os dados às técnicas de modelagem.
Desenvolver modelos preditivos para estimar o risco de inadimplência.
Avaliar e comparar a eficácia dos modelos implementados.


### Nota :

O projeto foi dividido em 4 arquivos para melhor entendimento e organização. No caso o arquivo EDA_Projeto3 , possui a exploração dos dados, o arquivo modelos_projeto3, possui os diferentes modelos desenvolvidos (separei o modelo de regressão logística) e o arquivo Home_CreditProjeto3 possui a junção da EDA + Modelo de Regressão + Modelo de melhores resultados. Além disso, nos próprios notebooks você encontrará alguns comentários! Então temos para o projeto 3:
(obs: a pasta de evidência tem algumas imagens do código desenvolvido)

# Projeto 3


[Exploração de dados](EDA_Projeto3.ipynb)

[Modelos baseados em Árvore](modelos_projeto3.ipynb)

[Modelo de regressão logística](reg_log_com_balanceamento.ipynb)

[Junção EDA + Reg + Xgboost](Home_CreditProjeto3.ipynb)

# Desenvolvimento do projeto e justificativas :

Nesta seção irei apresentar alguns pontos do projeto e justificativas para a seleção de modelos (não irei entrar em detalhes muito específicos apenas aqueles de maior importância!). 

#### Exploração de Dados/ Pré-processamento

Nessa parte do projeto era interessante visualizarmos as variáveis em categóricas e numéricas, além disso, procedimentos padrões como visualização de valores únicos e tipos das colunas. A grande maioria dos gráficos utilizados foram histogramas , boxplots (para detecção de outliers), gráficos heatmaps e de distribuição,  os outliers não foram de fato tratados por causa dos modelos utilizados e pelo contexto do problema julguei ser melhor não fazer esse tratamento. Uma função de visualização foi criada : (como comentado no notebook ela tem o objetivo de  visualizar a distribuição de uma variável contínua, diferenciando entre as categorias do alvo, TARGET.)

![Evidencia 1](evidencias/evi1.png)

Podemos visualizar outros gráficos como:

![Evidencia 2](evidencias/evi2.png)

Para o pré-processamento, utilizei a visualização de outliers, e verifiquei valores nulos, além disso defini um threshold em 0.8 para possivelmente dropar colunas com correlações alta e evitar redundância. Utilizando o mesmo conceito das correlações, mantive aquelas de maiores valores com a variável target. Isso pode ser visualizado no notebook de EDA na seção de pré-processamentos!

O enconding utilizado foi o LabelEncoder (tive contato no projeto anterior), tentei utilizar também o One Hot encoding porém enfrentei problemas (algumas variáveis se mantinham como categóricas mesmo depois da aplicação do encoding)

![Evidencia 3](evidencias/evi3.png)

# Comparação de Modelos para Análise de Risco de Crédito

## Introdução
Neste relatório, comparamos os desempenhos dos modelos **XGBoost**, **LightGBM** e **Random Forest** aplicados ao problema de risco de crédito. Avaliamos os modelos com base nas métricas **Accuracy Score**, **Classification Report** e **ROC AUC Score**.

*obs* : não irei anexar imagens pois em vídeo a visualização será melhor para os modelos! Em resumo, a escolha dos mesmo é justificada pelos seguintes motivos 

Modelos baseados em árvores -
Capturam interações complexas entre variáveis sem necessidade de engenharia de features muito avançada.
Lidam bem com dados tabulares (dados estruturados), que é o formato mais comum para crédito.
Resistentes a outliers e não exigem normalização de dados, ao contrário de modelos lineares.
Podem lidar com dados desbalanceados, ajustando pesos de classes.
Interpretabilidade: Permitem a análise da importância das features.

## Resumo dos Resultados
### Modelos Avaliados

| Modelo | Accuracy | Precision (Classe 1) | Recall (Classe 1) | F1-Score (Classe 1) | ROC AUC |
|--------|----------|----------------------|--------------------|---------------------|---------|
| **XGBoost (sem ajustes)** | 0.9199 | 0.57 | 0.02 | 0.04 | 0.7509 |
| **XGBoost (Balanceado)** | 0.7085 | 0.17 | 0.65 | 0.27 | 0.7510 |
| **XGBoost (Ajustado Manualmente)** | 0.7587 | 0.18 | 0.58 | 0.28 | 0.7478 |
| **XGBoost (Randomized Search)** | 0.7080 | 0.17 | 0.66 | 0.27 | 0.6842 |
| **LightGBM (Sem ajustes)** | 0.6993 | 0.16 | 0.67 | 0.26 | 0.6867 |
| **LightGBM (Com Ajustes)** | 0.7136 | 0.17 | 0.66 | 0.27 | 0.6883 |
| **LightGBM (Final)** | 0.7046 | 0.17 | 0.67 | 0.27 | 0.6882 |
| **Random Forest** | 0.7202 | 0.17 | 0.62 | 0.26 | 0.6743 |

## Análise Comparativa

### 1. **Precisão Geral (Accuracy Score)**
- O **XGBoost sem ajustes** obteve a maior acurácia geral (**0.9199**), mas sua capacidade de identificar a classe **1 (inadimplentes)** foi extremamente baixa (recall de **0.02**), o que indica um viés para a classe majoritária (**clientes adimplentes**).
- Após ajustes no balanceamento (**scale_pos_weight**), a acurácia do XGBoost caiu para **0.7085**, mas o recall da classe **1** aumentou significativamente (**0.65**), tornando-o mais adequado para a detecção de inadimplentes.
- O **Random Forest** alcançou uma acurácia intermediária de **0.7202**, enquanto os modelos **LightGBM** ficaram na faixa de **0.70-0.71**.

### 2. **Recall da Classe 1 (Clientes Inadimplentes)**
- O **XGBoost balanceado** foi o modelo com o **maior recall da classe 1 (0.65)**, sendo a melhor opção caso o objetivo seja reduzir o risco de crédito detectando inadimplentes corretamente.
- O **LightGBM ajustado** teve um recall de **0.66**, próximo ao do XGBoost balanceado.
- O **Random Forest** apresentou um recall de **0.62**, inferior ao do LightGBM e XGBoost balanceado.

### 3. **ROC AUC Score**
- O **XGBoost sem ajustes** teve o melhor ROC AUC (**0.7509**), mas novamente mostrou baixa capacidade de prever inadimplentes.
- O **XGBoost balanceado** obteve um ROC AUC similar (**0.7510**), o que demonstra que a modificação no balanceamento ajudou a melhorar a detecção da classe minoritária.
- O **LightGBM ajustado** teve um **ROC AUC de 0.6883**, inferior ao XGBoost balanceado.
- O **Random Forest** obteve o menor ROC AUC (**0.6743**), indicando que pode não ser a melhor escolha para esse problema.

## Conclusões e Escolha do Melhor Modelo

- **Se o objetivo for acurácia geral (minimizar erros totais):** O **XGBoost sem ajustes** é a melhor escolha, mas falha na detecção de inadimplentes.
- **Se o objetivo for detectar inadimplentes (alto recall para classe 1):** O **XGBoost balanceado** e o **LightGBM ajustado** são as melhores opções.
- **Se houver necessidade de um modelo mais interpretável e robusto:** O **Random Forest** pode ser uma opção, mas com um recall menor para inadimplentes.

### **Melhor Escolha:**
O **XGBoost balanceado** obteve um **bom recall para classe 1 (0.65)**, um **ROC AUC alto (0.7510)** e um **f1-score razoável para a classe de inadimplentes (0.27)**. Isso o torna a melhor escolha para um cenário onde minimizar riscos de crédito é fundamental.

## Próximos Passos
- **Testar feature engineering adicional**, criando novas variáveis a partir das existentes para melhorar o modelo.
- **Experimentar outros métodos de balanceamento de classe**, como SMOTE ou undersampling dinâmico.
- **Ajustar hiperparâmetros do LightGBM** para tentar aproximar seu desempenho ao XGBoost balanceado.

### **Conclusão Final:** O **XGBoost balanceado** apresentou o melhor desempenho geral para a detecção de inadimplentes, tornando-se o modelo mais indicado para essa análise de risco de crédito.





