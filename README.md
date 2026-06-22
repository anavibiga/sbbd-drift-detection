# Detecção de Drift em Partidas de Futebol — SBBD 2026

Código e análises do artigo 'Aplicação de Métodos Baseados em Concept Drift para Previsão de Gols no Futebol Profissional' submetido ao **SBBD 2026**, que investiga o uso de algoritmos de detecção de *concept drift* para identificar mudanças no padrão de eventos de partidas de futebol próximas a gols.

**Competição:** La Liga — temporada 2015/16 (StatsBomb Open Data)  
**Janela de avaliação:** K = 10

## Estrutura do repositório

├── src/
│   ├── 01_feature_engineering.py     # Gera events_wide_minute.parquet
│   ├── 02_model_adwin.py             # Detector ADWIN
│   ├── 02_model_kswin.py             # Detector KSWIN
│   ├── 02_model_page_hinkley.py      # Detector Page-Hinkley
│   ├── 02_model_baseline_fixed.py    # Baseline: limiar fixo
│   └── 02_model_baseline_random.py   # Baseline: aleatório
├── notebooks/
│   ├── 01_data_sanity_check.ipynb    # Validação dos dados brutos
│   ├── 02_eda_laliga.ipynb           # Análise exploratória
│   └── 05_artigo_sbbd.ipynb         # Análises e figuras do artigo
├── results/model_v5_k10/             # Resultados dos modelos (.parquet)
└── figures/sbbd_k10_attack/          # Figuras geradas para o artigo


## Pipeline

1. **Feature engineering** (`src/01_feature_engineering.py`)  
   Converte os eventos brutos do StatsBomb para uma tabela wide com 1 linha por minuto por partida. Saída: `data/processed/events_wide_minute.parquet`.

2. **Execução dos modelos** (`src/02_model_*.py`)  
   Cada script roda um detector sobre a série temporal de eventos com janela deslizante e avaliação assimétrica (SoftEd, K=10). Resultados salvos em `results/model_v5_k10/`.

3. **Análises do artigo** (`notebooks/05_artigo_sbbd.ipynb`)  
   Comparação dos detectores via F1, MCC, curva precision-recall e análise de alarmes falsos.

## Detectores avaliados

| Detector | Biblioteca |
|---|---|
| ADWIN | `river` |
| KSWIN | `river` |
| Page-Hinkley | `river` |
| Baseline fixo | — |
| Baseline aleatório | — |

## Dados

Os dados brutos são do [StatsBomb Open Data](https://github.com/statsbomb/open-data) — La Liga 2015/16. Não estão incluídos neste repositório. Para reproduzir os experimentos, clone o repositório do StatsBomb e coloque os arquivos em `data/open-data/`.

## Dependências principais

- Python 3.10+
- `pandas`, `numpy`, `scipy`
- `river` (detectores de drift)
- `matplotlib`
- `pyarrow` / `fastparquet`
