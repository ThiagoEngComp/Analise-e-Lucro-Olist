# 🧹 Projeto de Limpeza de Dados - Olist Products Dataset

Este projeto tem como objetivo realizar a limpeza e o pré-processamento dos dados dos produtos da Olist, com foco em corrigir inconsistências, lidar com valores ausentes e preparar o dataset para análises futuras ou modelagem preditiva.

## 📁 Arquivos Utilizados

- `olist_products_dataset.csv`: Dados brutos dos produtos vendidos.
- `product_category_name_translation.csv`: Tradução das categorias de produtos do português para o inglês.

## 🧼 Etapas da Limpeza

### 1. Carregamento dos Dados
Os dois arquivos CSV são carregados usando a biblioteca `pandas`.

### 2. Remoção de Dados Ausentes
- Registros com `product_id`, `product_category_name` ou `product_weight_g` ausentes são removidos.
- Valores nulos em outras colunas são tratados de acordo com a necessidade da análise.

### 3. Mesclagem de Dados
- O dataset de produtos é mesclado ao dataset de traduções usando a coluna `product_category_name` como chave.
- É adicionada uma nova coluna com a categoria em inglês: `product_category_name_english`.

### 4. Tratamento de Valores Inválidos
- Remoção de registros com peso, altura, largura ou comprimento igual a 0 ou nulo.
- Conversão de tipos de dados, se necessário.

### 5. Exportação dos Dados Limpos
- Os dados limpos são exportados como `olist_products_cleaned.csv`.

## 🧪 Código de Exemplo

```python
import pandas as pd

# Leitura dos arquivos
produtos = pd.read_csv('olist_products_dataset.csv')
categorias = pd.read_csv('product_category_name_translation.csv')

# Remoção de registros com valores nulos críticos
produtos.dropna(subset=['product_id', 'product_category_name', 'product_weight_g'], inplace=True)

# Remoção de registros com dimensões físicas inválidas
produtos = produtos[(produtos['product_weight_g'] > 0) &
                    (produtos['product_length_cm'] > 0) &
                    (produtos['product_height_cm'] > 0) &
                    (produtos['product_width_cm'] > 0)]

# Mesclagem com tradução de categorias
produtos = produtos.merge(categorias, on='product_category_name', how='left')

# Exportação do resultado final
produtos.to_csv('olist_products_cleaned.csv', index=False)
