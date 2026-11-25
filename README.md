# README — Retrato Bibliométrico de Quatro Institutos da USP (2025)

## Sobre o projeto

Este repositório contém a **todas as etapas** do projeto da disciplina **SCC0252 — Visualização Computacional (2026/2)**. O trabalho constrói um retrato bibliométrico transversal dos pesquisadores de quatro institutos da USP em São Carlos, com base em dados de perfis do Google Scholar. Na primeira parte, foram feitas análises apenas do ICMC nos anos de 2015 a 2025, enquanto que na segunda etapa, foram analisados os demais institutos (IFSC, IQSC, EESC) apenas no ano de 2025 devido a restrições do dataset encontrado.

**Integrantes:**
- Daniela Costa da Silva (14613625)
- Isaac Ferreira Souza Santos (15637912)
- Thiago Oliveira dos Santos (13696220)
- Anna Lucia Moro Lanzuolo (13864436)

**Institutos analisados:** ICMC, EESC, IFSC e IQSC.

> **Nota sobre o período:** o ano **2025** identifica as listas de pesquisadores utilizadas. As métricas (documentos, citações e índice h) são **totais acumulados** registrados nas consultas — não representam produção apenas do ano de 2025. O recorte é **transversal**, não longitudinal: não há comparação de crescimento, entrada ou saída de docentes entre anos.

---

## Objetivo e perguntas de investigação

O objetivo **não é classificar áreas como melhores ou piores**, mas identificar diferenças de distribuição e perfis que mereçam investigação. As cinco perguntas exploratórias que guiam a análise são:

1. Como as citações se distribuem entre os departamentos de cada instituto?
2. Pesquisadores com mais documentos também tendem a registrar mais citações?
3. Quais pesquisadores se destacam em citações e em índice h?
4. As citações se concentram em uma parcela pequena dos pesquisadores?
5. Os departamentos apresentam perfis distintos de documentos, citações e índice h?

Citações e índice h dependem de área, tempo de carreira, práticas de publicação e cobertura do Google Scholar — as conclusões respeitam essas diferenças.

---

## Dados e tratamento

- **Fontes:** quatro CSVs originais (um por instituto), com fallback para um arquivo unificado já auditado (`Conjunto_2025_Tratado.csv`) e, em último caso, URLs públicas do repositório.
- **Padronização:** cabeçalhos normalizados (ex.: `documentos_total_consulta` e `documentos total na consulta` viram `documentos_totais`), filtro para 2025, remoção de linhas vazias.
- **Qualidade:** valores ausentes **não são tratados como zero**. Índices h incoerentes (h > documentos) são anulados e sinalizados em `alerta_qualidade`. Registros só entram em comparações documentos × citações se tiverem ambas as métricas (`registro_valido_documentos_citacoes`).
- **Reprodutibilidade:** o pré-processamento gera `Conjunto_2025_Tratado.csv`, usado nas etapas seguintes.
- **Amostra:** 425 registros de 2025, dos quais **411** têm documentos e citações válidos, distribuídos em **17 departamentos**.

---

## Análises realizadas

| # | Análise | Técnica | Principais achados |
|---|---------|---------|--------------------|
| 1 | Distribuição de citações por departamento | Boxplots (por instituto) | Forte assimetria; medianas variam de ~237 (SMA/ICMC) a ~5.475 (SQF/IQSC); outliers bem acima da caixa. |
| 2 | Documentos × citações | Scatter em escala log + correlação de Spearman | Associação monotônica forte e positiva em todos os institutos (ρ entre 0,774 e 0,873; p < 1e-9). Sem inferência causal. |
| 3 | Destaques individuais | Rankings por citações e por índice h | Ex.: no ICMC, André Carlos P. de L. F. de Carvalho lidera (24.240 citações, h=78). Rankings descritivos, não avaliativos. |
| 4 | Concentração de citações | Participação acumulada | Verifica se poucos pesquisadores concentram grande parte das citações. |
| 5 | Perfis departamentais | Comparação de medianas de documentos, citações e h | Departamentos com perfis distintos; contagem de pesquisadores varia de 8 (SGS/EESC) a 38 (SMA/ICMC), exigindo cuidado com somas. |

---

## Estrutura do repositório
├── DataVisualizationPart2.ipynb # Notebook principal (esta etapa)
├── Conjunto_2025_Tratado.csv # Base unificada e auditada (gerada no notebook)
├── DatasetICMC.csv # Fonte original (opcional)
├── DatasetEESC.csv # Fonte original (opcional)
├── DatasetIFSC.csv # Fonte original (opcional)
├── DatasetIQSC.csv # Fonte original (opcional)
└── README.md


O notebook procura os arquivos no diretório atual, em `/content`, `/mnt/data` e subpastas `dados/`. Se os quatro CSVs originais não estiverem presentes, usa o arquivo tratado; se nenhum existir, tenta os links públicos.

---

## Como executar

1. Abra `DataVisualizationPart2.ipynb` no Google Colab ou Jupyter.
2. Execute as células em ordem. A primeira célula instala as dependências:
   ```bash
   pip install "dash>=2.17,<4" "plotly>=5.20" pandas numpy scipy
