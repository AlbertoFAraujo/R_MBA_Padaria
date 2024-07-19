![mba](https://github.com/AlbertoFAraujo/R_MBA_Padaria/assets/105552990/a48d54e7-f2ad-4a4e-8f91-67a37f3da876)

### Tecnologias utilizadas: 
| [<img align="center" alt="R_studio" height="60" width="60" src="https://github.com/AlbertoFAraujo/R_Petrobras/assets/105552990/02dff6df-07be-43dc-8b35-21d06eabf9e1">](https://posit.co/download/rstudio-desktop/) | [<img align="center" alt="dplyr" height="60" width="60" src="https://github.com/AlbertoFAraujo/R_MBA_Padaria/assets/105552990/95f7b928-17ae-4e8c-bda9-cbd96ac0504f">](https://www.rdocumentation.org/packages/dplyr/versions/1.0.10) | [<img align="center" alt="arules" height="60" width="60" src="https://github.com/AlbertoFAraujo/R_MBA_Padaria/assets/105552990/6cc91d4f-9d3a-4318-946d-c1c59dcd7ab3">](https://www.rdocumentation.org/packages/arules/versions/1.7-7) | [<img align="center" alt="quantmod" height="60" width="60" src="https://github.com/AlbertoFAraujo/R_MBA_Padaria/assets/105552990/87d28e2b-c64f-4e43-8125-4be12fca0293">](https://www.rdocumentation.org/packages/htmlwidgets/versions/1.6.4) | 
|:---:|:---:|:---:|:---:|
| R Studio | Dplyr | Arules | Htmlwidgets |
- **RStudio:** Ambiente integrado para desenvolvimento em R, oferecendo ferramentas para escrita, execução e depuração de código.
- **Dplyr:** Pacote para manipulação, com funções para filtragem, seleção, agrupamento e outras operações.
- **Arules:** Pacote para mineração de regras de associação em conjuntos de dados, útil para descobrir padrões e relações entre itens.
- **Htmlwidgets:** Framework para criação de widgets interativos em HTML, permitindo a integração de gráficos e visualizações interativas em aplicações web em R.
<hr>

### Objetivo: 

Identificar padrões de compras dos clientes em uma padaria, utilizando "Análise de Cestas de Mercado" (MBA). Através dessa análise, será possível descobrir quais produtos são frequentemente comprados juntos, a fim de melhorar as estratégias de marketing e promoções.

Base de dados: Arquivo "padaria.csv"
<hr>

### Código:

```r
# Ajustar as casas decimais
options(scipen = 999, digits = 4)
```

```r
knitr::opts_chunk$set(message = FALSE)
knitr::opts_chunk$set(warning = FALSE)
```
```r
# Carregando os pacotes
library(dplyr) # Manipulação de dados
library(arules) # Regras e MBA
library(arulesViz) # Visualização gráfica
library(htmlwidgets) # Interatividade html
library(RColorBrewer) # Paleta de cores
options(warn = -1)
```
```r
# Importar o dataset de estudo
df <- read.transactions('Padaria.csv',
                        format = 'basket',
                        sep = ',')
```
```r
inspect(head(df))
```
| Número | Itens                           |
|--------|---------------------------------|
| 1      | Leite, Pão                      |
| 2      | Café, Manteiga, Pão             |
| 3      | Croissant, Pão, Suco de laranja |
| 4      | Pão, Presunto, Queijo           |
| 5      | Baguete, Pão                    |
| 6      | Café, Leite, Pão, Rosquinhas    |

```r
# Verificando a frequência dos itens
summary(df)
```
transactions as itemMatrix in sparse format with
 83 rows (elements/itemsets/transactions) and
 10 columns (items) and a density of 0.341 

most frequent items:
| Item           | Quantidade |
|----------------|------------|
| Pão            | 72         |
| Café           | 44         |
| Suco de laranja| 35         |
| Leite          | 26         |
| Rosquinhas     | 26         |
| (Outros)       | 80         |

element (itemset/transaction) length distribution:
sizes
| Itemset | Transaction |
|------|-----|
| 2    | 10  |
| 3    | 43  |
| 4    | 16  |
| 5    | 14  |

Estatística Descritiva:

| Estatística | Valor |
|-------------|-------|
| Mínimo      | 2.00  |
| 1º Quartil  | 3.00  |
| Mediana     | 3.00  |
| Média       | 3.41  |
| 3º Quartil  | 4.00  |
| Máximo      | 5.00  |

```r
# Verificando a classe
class(df)
```
[1] "transactions"
attr(,"package")
[1] "arules"

```r
# Visualizando as frequências dos produtos
itemFrequencyPlot(df, 
                  topN = 5,
                  type = 'absolute',
                  col = brewer.pal(8,'Pastel2'),
                  main = 'Frequência dos Itens'
                  )
```
![image](https://github.com/AlbertoFAraujo/R_MBA_Padaria/assets/105552990/ad97ee8b-8532-496e-b35c-9dbc9eccc2c2)


```r
# Criação das regras
regra1 <- apriori(df,
                 parameter = list(
                    supp = 0.001,
                    conf = 0.5,
                    minlen = 2,
                    maxlen = 10)
                 )
```
Apriori

Parameter specification:

Algorithmic control:

Absolute minimum support count: 0 

set item appearances ...[0 item(s)] done [0.00s].
set transactions ...[10 item(s), 83 transaction(s)] done [0.00s].
sorting and recoding items ... [10 item(s)] done [0.00s].
creating transaction tree ... done [0.00s].
checking subsets of size 1 2 3 4 5 done [0.00s].
writing ... [252 rule(s)] done [0.00s].
creating S4 object  ... done [0.00s].

| Variável          | Valor  |
|-------------------|--------|
| confidence        | 0.5    |
| minval            | 0.1    |
| smax              | 1      |
| arem              | none   |
| aval              | FALSE  |
| originalSupport   | TRUE   |
| maxtime           | 5      |
| support           | 0.001  |
| arem              | none   |
| aval              | FALSE  |
| originalSupport   | TRUE   |
| maxtime           | 5      |
| support           | 0.001  |
| minlen            | 2      |
| maxlen            | 10     |
| target            | rules  |
| ext               | TRUE   |

Foram criadas no total 252 regras.
```r
# Analisando as regras dos itens
regra1 <- sort(regra1, by = 'confidence', decreasing = TRUE)
summary(regra1)
```
set of 252 rules

### Distribuição do comprimento das regras:
| Tamanho | Quantidade |
|---------|------------|
| 2       | 20         |
| 3       | 82         |
| 4       | 105        |
| 5       | 45         |

### Resumo das medidas de qualidade:
| Medida    | Mínimo | 1º Quartil | Mediana | Média | 3º Quartil | Máximo |
|-----------|--------|------------|---------|-------|------------|--------|
| Support   | 0.0120 | 0.0120     | 0.0241  | 0.0505| 0.0482     | 0.3976 |
| Confidence| 0.500  | 0.667      | 1.000   | 0.834 | 1.000      | 1.000  |
| Coverage  | 0.0120 | 0.0241     | 0.0241  | 0.0677| 0.0602     | 0.5301 |
| Lift      | 0.576  | 1.153      | 1.886   | 2.203 | 2.479      | 8.300  |
| Count     | 1.00   | 1.00       | 2.00    | 4.19  | 4.00       | 33.00  |

Das 252 regras:

-   4 Itens possuem o maior número de regras: 105
-   2 Itens possuem o menor número de regras: 20

  ```r
# Analisando as 10 primeiras regras

# lhs: Left Hand Side (Parte esquerda da regra da associação), conjunto de itens que estão sendo considerado  como a causa ou antecedência da regra.Ou seja, os itens que estão sendo comprados juntos e estão sendo usados para prever a compra de outros itens.

# rhs: Right Hand Side (Parte direita da regra da associação), conjunto de itens que está sendo previsto como consequência ou consequente na regra. Ou seja, são os itens que geralmente são comprados junto com o conjunto dos itens do LHS. 
inspect(head(regra1,10))
```
| LHS                    | RHS                 | Suporte | Confiança | Cobertura | Lift  | Contagem |
|------------------------|---------------------|---------|-----------|-----------|-------|----------|
| {Croissant}            | {Suco de laranja}  | 0.1928  | 1         | 0.1928    | 2.371 | 16       |
| {Rosquinhas}           | {Pão}              | 0.3133  | 1         | 0.3133    | 1.153 | 26       |
| {Croissant, Queijo}    | {Presunto}         | 0.0241  | 1         | 0.0241    | 6.917 | 2        |
| {Croissant, Presunto}  | {Queijo}           | 0.0241  | 1         | 0.0241    | 8.300 | 2        |
| {Queijo, Suco de laranja} | {Presunto}      | 0.0241  | 1         | 0.0241    | 6.917 | 2        |
| {Croissant, Queijo}    | {Suco de laranja} | 0.0241  | 1         | 0.0241    | 2.371 | 2        |
| {Queijo, Suco de laranja} | {Croissant}     | 0.0241  | 1         | 0.0241    | 5.188 | 2        |
| {Croissant, Queijo}    | {Café}             | 0.0241  | 1         | 0.0241    | 1.886 | 2        |
| {Manteiga, Queijo}     | {Leite}            | 0.0241  | 1         | 0.0241    | 3.192 | 2        |
| {Manteiga, Queijo}     | {Café}             | 0.0241  | 1         | 0.0241    | 1.886 | 2        |

```r
# Itens com relevância de compra (Confidence)
regras_100 <- regra1[quality(regra1)$confidence == 1]
inspect(head(regras_100, 10))
```
```r
# Scatterplot
plot(regra1, 
     method = 'scatter', 
     engine = 'htmlwidget',
     max = 250
     )
```
![image](https://github.com/AlbertoFAraujo/R_MBA_Padaria/assets/105552990/0605a183-329c-4e31-a18d-f6f7f1f52821)

```r
# Grafo
plot(regra1, 
     method = 'graph', 
     engine = 'htmlwidget', 
     max = 250
     )
```
![image](https://github.com/AlbertoFAraujo/R_MBA_Padaria/assets/105552990/6b44be6c-ec40-4683-a98d-5f602ae43456)

**Conclusão:** Compreender os padrões de compras de um estabelecimento varejista, permite criar ofertas personalizadas, ajustar layouts da loja e otimizar mix de produtos para atender às necessidades e preferências dos clientes, aumentando a satisfação e impulsionando as vendas.




