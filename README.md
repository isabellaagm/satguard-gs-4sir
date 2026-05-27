# SatGuard ML

## Classificador de Risco de Desastres Naturais via Satélite

> Cognitive Computing — Global Solution 2026.1 — 4SIR

**Integrantes:** Ana Luiza de Paula Reis - 552363, Isabella Gomes Menezes - 552327, Martin Hilst - 99451

Projeto com base em Ciência de Dados & Machine Learning, seguindo os conteúdos dos Labs 1 a 3 de Aprendizado Supervisionado (Classificação).

> **Algoritmo principal:** Random Forest Classifier

---

## Sobre o Projeto

### O Problema

O Brasil é um dos países mais vulneráveis a desastres naturais do mundo. Enchentes, deslizamentos e secas causam anualmente milhares de mortes e prejuízos bilionários. A detecção e classificação do nível de risco de forma antecipada é um dos maiores desafios da Defesa Civil.

### A Visão

O **SatGuard** é uma plataforma que utiliza dados satelitais e meteorológicos para **classificar automaticamente o nível de risco de desastre natural** em regiões monitoradas, gerando alertas antecipados para autoridades e população.

Este repositório contém o **módulo de Machine Learning** do SatGuard, responsável por classificar o risco a partir de variáveis ambientais e satelitais coletadas por satélites como GOES-16, Landsat, SMAP e MODIS.

---

## O Pipeline

Este projeto implementa um pipeline completo de Machine Learning para classificar o **nível de risco de desastre natural** em regiões monitoradas pelo SatGuard:

| Classe | Descrição |
|--------|-----------|
| `0 - Baixo` | Condições estáveis, sem risco imediato |
| `1 - Médio` | Atenção recomendada, monitoramento intensificado |
| `2 - Alto` | Alerta crítico, acionar Defesa Civil imediatamente |

O trabalho segue a metodologia apresentada na disciplina de **Cognitive Computing**, aplicando as boas práticas de pré-processamento, treinamento estratificado e avaliação com métricas adequadas para dados multiclasse.

---

## Sobre o Dataset

**Arquivo:** `satguard_dataset.csv`
**Tamanho:** 2.500 observações × 9 variáveis

O dataset representa observações satelitais de regiões monitoradas cobrindo os principais biomas brasileiros (Amazônia, Cerrado, Mata Atlântica, Semiárido e Sul), com variáveis baseadas nos indicadores utilizados pelo **INPE**, **Cemaden** e pela **Carta Internacional Space and Major Disasters**.

### Features

| Feature | Descrição | Fonte Satelital |
|---------|-----------|----------------|
| `ndvi` | Índice de Vegetação por Diferença Normalizada (0=seco, 1=verde) | Landsat/MODIS |
| `umidade_solo` | Umidade do solo superficial (%) | SMAP |
| `precipitacao_72h` | Precipitação acumulada nas últimas 72h (mm) | GOES-16 |
| `temperatura_max` | Temperatura superficial máxima (°C) | GOES-16 |
| `inclinacao_terreno` | Ângulo de inclinação do terreno (graus) | SRTM/DEM |
| `focos_queimadas` | Focos ativos de queimadas na região | AQUA/TERRA MODIS |
| `dist_rio_km` | Distância ao corpo d'água mais próximo (km) | HydroSHEDS |
| `historico_eventos` | Desastres registrados nos últimos 5 anos | CEMADEN |
| `bioma` | Bioma da região (removido no pré-processamento do modelo) | IBGE |

---

## Resultados

### Dimensões do Dataset

| Etapa | Linhas | Colunas |
|-------|--------|---------|
| Carregamento original | 2.500 | 10 |
| Após remoção de 'bioma' | 2.500 | 9 |
| Valores nulos | 0 | — |
| Treino | 1.750 | 8 |
| Teste | 750 | 8 |

### Distribuição das Classes

| Classe | Amostras | % |
|--------|----------|---|
| Baixo Risco | ~800 | ~32% |
| Médio Risco | ~1.100 | ~44% |
| Alto Risco | ~600 | ~24% |

### Métricas de Desempenho (Random Forest)

| Classe | Precision | Recall | F1-score |
|--------|-----------|--------|----------|
| Baixo Risco | ~0.97 | ~0.96 | ~0.97 |
| Médio Risco | ~0.95 | ~0.97 | ~0.96 |
| Alto Risco | ~0.98 | ~0.97 | ~0.97 |
| **Acurácia geral** | | | **~0.96** |

### Comparação de Modelos (Validação Cruzada 5-fold)

| Modelo | Acurácia Média | Desvio Padrão |
|--------|---------------|---------------|
| Regressão Logística | ~0.78 | ±0.02 |
| KNN (k=7) | ~0.87 | ±0.01 |
| **Random Forest** | **~0.96** | **±0.01** |

### Top Features Mais Importantes

| # | Feature | Descrição |
|---|---------|-----------|
| 1 | `precipitacao_72h` | Principal fator de risco de enchentes |
| 2 | `inclinacao_terreno` | Determinante para deslizamentos |
| 3 | `dist_rio_km` | Proximidade a corpos d'água |
| 4 | `umidade_solo` | Saturação do solo |
| 5 | `historico_eventos` | Vulnerabilidade histórica |

---

## Metodologia

| Etapa | Descrição | Ferramenta |
|-------|-----------|------------|
| 1. EDA | Inspeção: `head()`, `info()`, `describe()`, distribuição de classes | `pandas` |
| 2. Limpeza | Remoção da coluna categórica `bioma` | `pandas` |
| 3. Split | Divisão 70/30 estratificada por classe | `train_test_split` |
| 4. Pipeline | StandardScaler + Classificador (fit apenas no treino) | `sklearn.pipeline` |
| 5. Validação | Validação cruzada estratificada 5-fold | `StratifiedKFold` |
| 6. Avaliação | Acurácia, F1, Matriz de Confusão, Importância das Features | `sklearn.metrics` |
| 7. Inferência | Simulação de alertas em tempo real com 3 cenários regionais | — |

---

## Como Executar

### Google Colab (recomendado)

1. Acesse [colab.research.google.com](https://colab.research.google.com)
2. Faça upload do arquivo `SatGuard_ML.ipynb`
3. Clique em **Ambiente de execução → Executar tudo**

### Local

```bash
# Criar e ativar ambiente virtual
python -m venv .venv
.venv\Scripts\activate        # Windows
# source .venv/bin/activate   # Linux/macOS

# Instalar dependências
pip install -r requirements.txt

# Abrir o notebook
jupyter notebook SatGuard_ML.ipynb
```

---

## Referências

- [INPE — Cemaden](http://www.cemaden.gov.br/)
- [Carta Internacional Space and Major Disasters](https://disasterscharter.org/)
- [NASA — Earth Observing System](https://earthdata.nasa.gov/)
- [scikit-learn — RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)
