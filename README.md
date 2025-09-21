# Projeto de Enriquecimento de Vendas – Databricks & ADLS Gen2

## 1. Objetivo do Projeto
Este projeto tem como objetivo demonstrar um **pipeline de engenharia de dados** que:
- Lê arquivos CSV de vendas, produtos e clientes de um Data Lake (ADLS Gen2).
- Enriquecer os dados através de **joins** e cálculos (como `total_price`).
- Salva o resultado final em formato **Parquet**, pronto para análise ou downstream.

O projeto foi desenvolvido utilizando **Databricks**, **PySpark** e **Azure Data Lake Storage (ADLS Gen2)**.

---

## 2. Tecnologias e Ferramentas Utilizadas
- **Databricks**: Plataforma para processamento distribuído de dados (Spark).  
- **PySpark**: API Python do Apache Spark para transformação de dados.  
- **Azure Data Lake Storage Gen2 (ADLS)**: Armazenamento escalável para arquivos raw e processados.  
- **Parquet**: Formato colunar eficiente para armazenamento e leitura de dados.  
- **Service Principal + OAuth**: Autenticação segura para acessar o ADLS.

---

## 3. Estrutura do Projeto
Project-Data-Eng/

├─ notebooks/
01_conexao_ADLS.ipynb # Conexão com ADLS via OAuth
02_enriquecimento_vendas.ipynb # Leitura, join, cálculo e escrita em Parquet

├─ data/
raw/ # Amostras de arquivos CSV
processed/ # Dados enriquecidos em Parquet

├─ utils/
funcoes.py # Funções auxiliares (listar arquivos, ler CSV, salvar Parquet)

├─ README.md
.gitignore


---

## 4. Passo a Passo de Execução
1. **Conexão ao ADLS Gen2**
    - Configurar as credenciais do Service Principal (client_id, client_secret, tenant_id).  
    - Testar a conexão usando funções para listar arquivos (`listar_container`, `listar_pasta`).

2. **Leitura dos arquivos CSV**
    - Ler os arquivos `sales.csv`, `products.csv` e `customers.csv` do container `raw`.  
    - Conferir os dados com `show()` ou `display()`.

3. **Enriquecimento dos dados**
    - Realizar joins entre os DataFrames:
      ```python
      df = sales.join(products, "product_id").join(customers, "customer_id")
      ```
    - Criar a coluna `total_price = quantity * unit_price`.

4. **Escrita em Parquet**
    - Salvar os dados enriquecidos no container `processed` em `enriched_sales/`.  
    - Spark gera arquivos `part-*.parquet` e `_SUCCESS`.

5. **Validação**
    - Ler o Parquet de volta para conferir dados:
      ```python
      df_enriched = spark.read.parquet("abfss://processed@<storage_account>.dfs.core.windows.net/enriched_sales/")
      df_enriched.show(5)
      ```
