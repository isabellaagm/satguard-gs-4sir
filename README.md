# SatGuard

### Plataforma de Monitoramento e Alerta de Desastres Naturais via Satélite

> Cognitive Computing - Global Solution 2026.1 - 4SIR

**Integrantes:** Ana Luiza de Paula Reis - 552363, Isabella Gomes Menezes - 552327, Martin Hilst - 99451

---

## O Problema

Todos os anos, enchentes varrem cidades inteiras no litoral de São Paulo. Deslizamentos destroem comunidades na Serra Gaúcha. Secas devastam o Nordeste por meses. O Brasil convive com desastres naturais que matam, deslocam famílias e custam bilhões - e a Defesa Civil muitas vezes só consegue agir **depois** que o desastre já aconteceu.

O problema não é falta de dados. Satélites como o GOES-16, MODIS e SMAP monitoram o território brasileiro em tempo real, captando precipitação, umidade do solo, temperatura, vegetação e dezenas de outras variáveis. O problema é **transformar esse volume de dados em decisão rápida**.

---

## A Solução — SatGuard

O **SatGuard** é uma plataforma de inteligência que processa dados satelitais em tempo real e **classifica automaticamente o nível de risco de desastre natural** em regiões monitoradas — gerando alertas acionáveis para a Defesa Civil, prefeituras e população antes que o desastre ocorra.

Este repositório contém o **módulo de Machine Learning** do SatGuard: o classificador de risco treinado com variáveis satelitais e meteorológicas de regiões brasileiras.

---

## Como funciona

O modelo recebe uma "foto" ambiental de uma região - precipitação acumulada, umidade do solo, inclinação do terreno, distância a rios - e responde com um nível de risco:

| Nível | Significado | Ação |
|-------|-------------|------|
| **Baixo** | Condições estáveis | Monitoramento padrão |
| **Médio** | Atenção necessária | Equipes em alerta, verificação intensificada |
| **Alto** | Risco crítico | Acionar Defesa Civil, emitir alerta público |

---

## Dados Utilizados

**Arquivo:** `satguard_dataset.csv` - 2.500 observações de regiões brasileiras

As variáveis foram selecionadas com base nos indicadores utilizados pelo **INPE**, **Cemaden** e pela **Carta Internacional Space and Major Disasters**:

| Variável | Descrição |
|----------|-----------|
| `ndvi` | Índice de vegetação — detecta áreas desmatadas e vulneráveis à erosão |
| `umidade_solo` | Saturação do solo — solo encharcado aumenta risco de deslizamento |
| `precipitacao_72h` | Chuva acumulada nas últimas 72h — principal gatilho de enchentes |
| `temperatura_max` | Temperatura superficial — indicador de seca e estresse hídrico |
| `inclinacao_terreno` | Declividade — terrenos inclinados são mais vulneráveis a deslizamentos |
| `focos_queimadas` | Focos ativos — vegetação queimada perde capacidade de absorção |
| `dist_rio_km` | Proximidade a rios — regiões ribeirinhas têm maior risco de inundação |
| `historico_eventos` | Desastres registrados nos últimos 5 anos — vulnerabilidade histórica |

O dataset cobre os principais biomas brasileiros: **Amazônia, Cerrado, Mata Atlântica, Semiárido e Sul**, com distribuição de variáveis ajustada às características climáticas de cada região.

---

## Resultados

### Comparação de modelos (Validação Cruzada 5-fold)

| Modelo | Acurácia |
|--------|----------|
| Regressão Logística | ~0.78 |
| KNN (k=7) | ~0.87 |
| **Random Forest** | **~0.88** |

### Desempenho no conjunto de teste

| Classe | Precision | Recall | F1-score |
|--------|-----------|--------|----------|
| Baixo Risco | 0.874 | 0.759 | 0.812 |
| Médio Risco | 0.885 | 0.951 | 0.917 |
| Alto Risco | 0.880 | 0.709 | 0.785 |
| **Acurácia geral** | | | **0.883** |

### Features mais importantes

| # | Variável | Importância |
|---|----------|-------------|
| 1 | `inclinacao_terreno` | 22.2% |
| 2 | `umidade_solo` | 18.6% |
| 3 | `precipitacao_72h` | 18.5% |
| 4 | `dist_rio_km` | 18.4% |
| 5 | `ndvi` | 7.1% |

> As quatro features dominantes são fisicamente coerentes com os principais fatores de risco de enchentes e deslizamentos no Brasil - o que valida a qualidade do modelo.

---

## Estrutura do Repositório

```
satguard-gs/
├── SatGuard_ML.ipynb       # Notebook principal com todo o pipeline
├── satguard_dataset.csv    # Dataset com 2.500 observações satelitais
├── requirements.txt        # Dependências do projeto
└── README.md               # Este arquivo
```

---

## Como Executar

**Google Colab (recomendado — sem instalação):**
1. Acesse [colab.research.google.com](https://colab.research.google.com)
2. Faça upload do `SatGuard_ML.ipynb`
3. Clique em **Ambiente de execução → Executar tudo**

**Local:**
```bash
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
.venv\Scripts\activate     # Windows

pip install -r requirements.txt
jupyter notebook SatGuard_ML.ipynb
```
---

## Referências

- [INPE — Cemaden](http://www.cemaden.gov.br/)
- [Carta Internacional Space and Major Disasters](https://disasterscharter.org/)
- [NASA — Earth Observing System](https://earthdata.nasa.gov/)
