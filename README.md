# Trabalho Prático Individual – Tópicos de Big Data em Python

## Nome e matrícula

- **Nome:** Lucas Cordeiro Diniz  
- **Matrícula:** 202408700016

## Objetivo

Construir um pequeno pipeline de dados com Pandas e PySpark sobre transações de vendas de varejo (`vendas_bigdata_2026.csv`), documentando os problemas de qualidade encontrados e produzindo informações úteis para a tomada de decisão. O trabalho inclui diagnóstico e limpeza dos dados, análise exploratória com gráficos, processamento com PySpark (`filter`, `groupBy`, `agg`, `orderBy`) e Spark SQL, uma explicação sobre o HDFS e a correção de um problema de vazamento de dados (*data leakage*) em Machine Learning usando `Pipeline`.

## Tecnologias utilizadas

- Python 3.11 ou superior
- Pandas e NumPy (limpeza e análise)
- Matplotlib (gráficos)
- PySpark e Spark SQL (processamento distribuído)
- scikit-learn (`StandardScaler`, `Pipeline`, `LogisticRegression`)
- Jupyter Notebook / VS Code
- Java 17 (necessário para o PySpark)

## Como executar

1. Instale o Python e o Java 17.
2. Crie e ative um ambiente virtual e instale as dependências:

       python -m venv .venv
       .venv\Scripts\activate
       pip install pandas numpy matplotlib scikit-learn pyspark ipykernel

3. Mantenha o arquivo `vendas_bigdata_2026.csv` na mesma pasta do notebook.
4. Abra `trabalho_bigdata.ipynb`, selecione o kernel do `.venv` e execute todas as células (*Run All*).

Estrutura do repositório:

    trabalho_bigdata.ipynb     # código, saídas e explicações
    README.md
    vendas_bigdata_2026.csv    # dados de entrada
    resultado/
        resumo_regioes.csv     # faturamento, pedidos e itens por região (PySpark)
        vendas_tratadas.csv    # dataset limpo, gerado pelo notebook

## Principais conclusões

- O arquivo tinha 1.200 registros, com 8 duplicatas, 5 datas inválidas, grafias inconsistentes de região, valores ausentes, preços negativos, quantidades zeradas e 9 pedidos com `valor` incoerente. Correções só foram aplicadas quando verificáveis; os demais casos foram sinalizados e excluídos apenas do faturamento.
- O faturamento validado é de **R$ 543.133,51** (bruto, sem frete), idêntico no Pandas e no PySpark.
- O **Sudeste** lidera com 41% do faturamento, seguido de Sul e Nordeste. Norte e Centro-Oeste somam apenas 16%.
- O canal **Site** responde por 51% do faturamento, e o **Pix** é a forma de pagamento mais usada (41% dos pedidos).
- Mesa e Cadeira respondem por cerca de 22% da receita. O faturamento mensal é instável (entre R$ 41 mil e R$ 68 mil), e a queda aparente de novembro se deve à base terminar em 01/11.
- A taxa de devolução é de 6,3%, maior em Eletrônicos e Casa.
- Aplicar o `StandardScaler` antes do `train_test_split` causa vazamento de dados; a versão corrigida usa `Pipeline`, ajustando o scaler somente com o treino.
