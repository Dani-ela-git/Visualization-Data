# Análise de Impacto Científico e Produção Acadêmica

Este projeto consiste em uma Análise Exploratória de Dados (EDA) sobre a produção científica e o impacto de pesquisadores de diferentes departamentos do Instituto de Ciências Matemáticas e de Computação (SCC, SSC, SME e SMA), da USP Campus São Carlos, utilizando dados extraídos do Google Scholar entre os anos de 2015 e 2025.

## Objetivos do Projeto

O principal objetivo é investigar a distribuição de produtividade e impacto científico, respondendo às seguintes perguntas de pesquisa:
1. Qual departamento possui o maior impacto científico (medido por citações, documentos, índice h e índice i10)?
2. Produzir mais documentos significa receber mais citações?
3. Quais pesquisadores apresentam maior impacto relativo por documento?
4. A produção científica está concentrada em poucos pesquisadores (Curva de Lorenz e Gini)?
5. Como foi a rotatividade (entradas e saídas) de pesquisadores nos últimos 10 anos?

## Tecnologias e Bibliotecas Utilizadas

O projeto foi desenvolvido em **Python** (ambiente Google Colab / Jupyter Notebook).
* `pandas` e `numpy`: Manipulação, limpeza e engenharia de dados.
* `plotly` (express e graph_objects): Criação de visualizações **interativas** (gráficos com tooltips, zoom e hover customizado).
* `matplotlib` e `seaborn`: Suporte para visualizações estáticas.
* `scipy.stats`: Cálculo da correlação de Spearman.

## Tratamento e Engenharia de Dados

Para garantir a integridade das análises, o código passou por um rigoroso pipeline de tratamento:
1. **Identificação Única:** Como o mesmo autor aparece em vários anos, criou-se um ID único por pesquisador, priorizando o ID do Google Scholar, depois o Número USP e, por fim, o nome. Isso evita contar a mesma pessoa duas vezes por variação de grafia.
2. **Snapshot Anual:** Para as análises de impacto (1 a 4), foi isolado o registro mais recente (ano de referência) de cada pesquisador, criando o DataFrame `df_ultimo` (ou `df_validos`).
3. **Preservação para Análise Temporal:** Para a Análise 5, o dataset foi recarregado do zero para não perder o histórico de pesquisadores que ingressaram recentemente e ainda não possuem métricas preenchidas.
4. **Tratamento de Zeros:** Uso da transformação `log1p(x)` nos gráficos para lidar com valores iguais a zero (já que logaritmos tradicionais não aceitam zero), garantindo a exibição de toda a distribuição sem distorções.

## Análises Realizadas e Principais Insights

### 1. Impacto Científico por Departamento
*   **Metodologia:** Gráficos de barras interativos para totais e Boxplots para distribuição de citações, documentos, índice h e i10.
*   **Insight:** A utilização de Boxplots permitiu visualizar a **mediana**, que é imune a distorções causadas por *outliers* (pesquisadores com citações extremamente altas). A escala logarítmica (`log1p`) foi aplicada para melhorar a visualização de distribuições assimétricas.

### 2. Produção vs. Impacto (Documentos x Citações)
*   **Metodologia:** Gráfico de dispersão interativo e Correlação de Spearman (ρ).
*   **Insight:** A correlação de Spearman indica uma forte relação positiva entre a quantidade de documentos e as citações totais. O gráfico interativo revelou "outliers de eficiência" (pesquisadores com pouca produção, mas altíssimo impacto).

### 3. Eficiência (Impacto Relativo por Documento)
*   **Metodologia:** Cálculo da razão `citações totais / documentos totais`, com aplicação de filtro mínimo (ex: 30 documentos) para evitar ruídos estatísticos.
*   **Insight:** Identificou-se os pesquisadores com maior "peso" por artigo. *Nota metodológica: essa métrica não considera a idade dos trabalhos nem as diferenças de convenções de citação entre as áreas.*

### 4. Concentração de Citações (Curva de Lorenz e Gini)
*   **Metodologia:** Cálculo da porcentagem de citações detidas pelo Top 10% dos pesquisadores, plotagem interativa da Curva de Lorenz e cálculo matemático do Índice de Gini.
*   **Insight:** A produção científica segue a Lei de Pareto. A Curva de Lorenz afasta-se significativamente da linha de igualdade perfeita, e o Gini elevado comprova que a grande maioria das citações concentra-se em uma minoria de pesquisadores.

### 5. Evolução e Rotatividade (Entradas e Saídas)
*   **Metodologia:** Análise temporal mapeando o primeiro e o último ano de registro de cada ID de pesquisador. Criação de gráficos de barras agrupadas (entradas vs. saídas) e gráficos de linha (evolução anual).
*   **Insight:** A análise revelou o saldo líquido de capital humano de cada departamento nos últimos 10 anos, demonstrando quais áreas estão em expansão e quais passaram por redução de quadro.

## Como Reproduzir

1. Clone este repositório.
2. Certifique-se de ter as bibliotecas instaladas: 
   ```bash
   pip install pandas numpy matplotlib seaborn scipy plotly
