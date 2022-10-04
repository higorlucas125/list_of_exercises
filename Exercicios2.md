# Lista de Exercícios sobre conceitos de Árvores
1. Em que tipo de problemas um algoritmo de classificação baseado em árvores é útil?
    
    - Os algoritmos de aprendizagem baseados em árvores de decisão são considerados um dos melhores e mais utilizados métodos de aprendizagem supervisionada. Os métodos baseados em árvores nos dão modelos preditivos de alta precisão, estabilidade e facilidade de interpretação. Ao contrário dos modelos lineares, eles mapeiam muito bem relações não-lineares. 

2. O que são medidas de impureza e como elas são utilizadas para as construções de árvores de decisões? Porque medidas de erros não são normalmente utilizadas nessa situação?

    - Usado pelo algoritmo CART (árvore de classificação e regressão) para árvores de classificação, a impureza de Gini (em homenagem ao matemático italiano Corrado Gini) é uma medida de quantas vezes um elemento escolhido aleatoriamente do conjunto seria rotulado incorretamente se fosse rotulado aleatoriamente de acordo com o distribuição de rótulos no subconjunto.

    - Pois a analise de erros não valem apena fazer como a gente sempre busca a entropia para encontrar os dados.

3. Descreva os três principais componentes de um algoritmo de aprendizado de máquina baseado em árvores de decisão.

    * Entropia 
    * Índice GINI
    * Regressão

4. Com o objetivo de identificar o sexo de penguins usando o dataset data(penguins), escreva um código em R
para medir a acurácia de um modelo baseado em árvore CART em um conjunto de treino.

    ```
    here::i_am("classificacao_classica.Rmd")
    library(here)
    library(tidyverse)
    library(tidymodels)
    library(FFTrees)
    library(palmerpenguins)

    dfs <- penguins
    str(dfs)
    dfs$species <- as.factor(dfs$species)


    summary(dfs$species )

    divisao_dados <- initial_split(dfs, prop=0.9)
    dfs_treino <- training(divisao_dados)

    preparacao <- recipe(species ~ ., dfs_treino) 

    modeloPadrao <- decision_tree( 
    # cost_complexity = 2, # hiperparametro para o algoritmo preferir modelos (árvores) mais simples
    #  tree_depth = 2, # profundidade maxima das arvores
    #  min_n = 20 # numero minimo de exemplos em um no para efetuar um divisao
    ) |> set_engine("rpart") |> 
        set_mode("classification")

    wf <- workflow(preparacao, modeloPadrao)
    modelo_fitted <- fit(wf, dfs_treino)

    dfs_teste <- testing(divisao_dados)

    resultados_teste <- predict(modelo_fitted, dfs_teste)
    resultados_teste <- mutate(resultados_teste, species = dfs_teste$species)

    resultados_teste |> metrics(species, .pred_class)
    ```
    ```
    Preprocessor: Recipe
    Model: decision_tree()

    ── Preprocessor ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    0 Recipe Steps

    ── Model ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    n= 309 

    node), split, n, loss, yval, (yprob)
        * denotes terminal node

    1) root 309 174 Adelie (0.436893204 0.200647249 0.362459547)  
    2) flipper_length_mm< 206.5 192  58 Adelie (0.697916667 0.296875000 0.005208333)  
        4) bill_length_mm< 43.35 134   4 Adelie (0.970149254 0.029850746 0.000000000) *
        5) bill_length_mm>=43.35 58   5 Chinstrap (0.068965517 0.913793103 0.017241379) *
    3) flipper_length_mm>=206.5 117   6 Gentoo (0.008547009 0.042735043 0.948717949)  
        6) bill_depth_mm>=17.2 8   3 Chinstrap (0.125000000 0.625000000 0.250000000) *
        7) bill_depth_mm< 17.2 109   0 Gentoo (0.000000000 0.000000000 1.000000000) *

    ```
5. Com o objetivo de identificar o sexo de penguins usando o dataset data(penguins), escreva um código em R
para medir a acur ́acia de um modelo baseado em árvore C5.0 em um conjunto de teste.
    


    ```
    data(penguins)

    #preparacao dos dados
    penguins <- mutate(penguins, species = as_factor(species))

    divisao_dados <- initial_split(penguins, prep = 3/4)

    treino <- training(divisao_dados)
    #teste <- testing(divisao_dados)

    receita <- recipe(sex ~ ., treino)

    #Definição de modelos

    arv_c50 <- decision_tree(mode = "classification", engine = "C5.0",
                            cost_complexity = 0.5, #hiperparametros
                            min_n = 10)
    wf <- workflow(receita, arv_c50)
    modelo_fitted <- fit(wf,treino)

    treino_teste <- testing(divisao_dados)

    resultados_teste <- predict(modelo_fitted, treino_teste)
    resultados_teste <- mutate(resultados_teste, sex = treino_teste$sex)

    resultados_teste |> metrics(sex, .pred_class)
    ```

6. Em qual situação existe risco de overfitting quando do aprendizado de árvores de decisão?

7.

8.

9.

10.

11.

12.

13.

14.

15.

16.

17.