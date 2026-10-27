# 📊 Análise de Impacto Científico e Produção Acadêmica

Este projeto consiste em uma Análise Exploratória de Dados (EDA) sobre a produção científica e o impacto de pesquisadores de diferentes departamentos do Instituto de Ciências Matemáticas e de Computação (SCC, SSC, SME e SMA), da USP Campus São Carlos, utilizando dados extraídos do Google Scholar entre os anos de 2015 e 2025.

## 🎯 Objetivos do Projeto

O principal objetivo é entender como a produção científica e o impacto das citações se distribuem entre os departamentos e os pesquisadores, respondendo a perguntas como:
1. Qual departamento possui o maior impacto científico?
2. Produzir mais documentos significa receber mais citações?
3. Quais pesquisadores apresentam maior impacto relativo por documento?
4. A produção científica está concentrada em poucos pesquisadores?
5. Como foi a rotatividade (entradas e saídas) de pesquisadores nos últimos 10 anos?

## 🛠️ Tecnologias e Bibliotecas Utilizadas

O projeto foi desenvolvido em **Python**, majoritariamente em ambiente Google Colab / Jupyter Notebook. As seguintes bibliotecas foram utilizadas:
* `pandas` e `numpy`: Manipulação, limpeza e agregação dos dados.
* `matplotlib` e `seaborn`: Criação de visualizações gráficas (boxplots, dispersões, gráficos de linha e barras).
* `scipy.stats`: Cálculo da correlação de Spearman.

## 🧹 Tratamento dos Dados

Como o dataset original é um painel acumulado (o mesmo pesquisador aparece em vários anos com os mesmos valores de citações, gerando duplicatas), foram aplicadas as seguintes etapas de limpeza:
1. **Tipagem:** Conversão forçada das colunas numéricas para evitar erros (`errors='coerce'`).
2. **Remoção de Nulos:** Exclusão de linhas sem informações essenciais de citações ou documentos.
3. **Deduplicação para Análises Individuais:** Criação do DataFrame `df_ultimo`, mantendo apenas o registro mais recente de cada pesquisador. Isso evita que o mesmo autor seja contado 10 vezes nas métricas de distribuição (boxplots) e correlações.

## 📈 Análises Realizadas e Principais Insights

### 1. Impacto Científico por Departamento
*   **Metodologia:** Utilização de boxplots e medianas para comparar citações, documentos, índice h e índice i10.
*   **Insight:** A mediana é uma métrica mais robusta que a média aqui, pois a distribuição de citações é altamente assimétrica (poucos pesquisadores concentram números extremos). O uso do boxplot permitiu visualizar a dispersão e os outliers de cada departamento.

### 2. Produção vs. Impacto (Documentos x Citações)
*   **Metodologia:** Gráfico de dispersão e Correlação de Spearman (ρ).
*   **Insight:** Foi encontrada uma forte correlação positiva (ρ ≈ 0.89), indicando que, no geral, produzir mais documentos está associado a receber mais citações. No entanto, a análise visual revelou "outliers de eficiência": pesquisadores com poucos documentos, mas com um volume de citações excepcionalmente alto.

### 3. Eficiência (Impacto Relativo por Documento)
*   **Metodologia:** Cálculo da razão `citações totais / documentos totais` (com filtro mínimo de 30 documentos para evitar distorções) e comparação das medianas por departamento.
*   **Insight:** Identificou-se os pesquisadores com maior "peso" por artigo. *Nota metodológica: essa métrica não considera a idade dos trabalhos (artigos antigos acumulam mais citações) nem as diferenças de convenções de citação entre as áreas.*

### 4. Concentração de Citações (Curva de Lorenz e Gini)
*   **Metodologia:** Cálculo da porcentagem de citações detidas pelo Top 10% dos pesquisadores, plotagem da Curva de Lorenz e cálculo do Índice de Gini.
*   **Insight:** A ciência neste dataset segue a Lei de Pareto (regra 80/20). Uma parcela muito pequena de pesquisadores concentra a grande maioria das citações, caracterizando uma alta desigualdade de impacto (Gini elevado). A Curva de Lorenz afasta-se consideravelmente da linha de igualdade perfeita.

### 5. Evolução e Rotatividade (Entradas e Saídas)
*   **Metodologia:** Análise temporal (2015-2025) mapeando o primeiro e o último ano de registro de cada pesquisador em seu departamento.
*   **Insight:** Foram contabilizados os pesquisadores que surgiram (primeiro registro ≥ 2016) e os que saíram (último registro ≤ 2024). O gráfico de barras agrupadas revela o saldo líquido de capital humano de cada departamento nos últimos 10 anos, mostrando quais áreas estão em expansão ou retração.

## 🚀 Como Reproduzir

1. Clone este repositório.
2. Certifique-se de ter as bibliotecas instaladas: 
   ```bash
   pip install pandas numpy matplotlib seaborn scipy
