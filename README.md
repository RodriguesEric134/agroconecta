# AgroConecta — Big Data & RPA

Plataforma de apoio à decisão para pequenos produtores rurais (Global Solution 2026.2 — CitizenTech).
ODS 2 (Fome Zero e Agricultura Sustentável) e ODS 13 (Ação Contra a Mudança do Clima).

Este repositório contém os componentes de **Big Data Architecture & Data Integration** e
**Robotic Process Automation**.

```
                    ┌──────────────────────── BIG DATA ────────────────────────┐
 Open-Meteo (API) ─┐│                                                          │
 IBGE (API)       ─┼┼► Airflow (diário 06:00) ─► chegada/ ─► Volume Databricks │
 Cadastro MVP     ─┘│                              Bronze ─► Prata ─► Ouro ────┼─► AgroConecta
                    └──────────────────────────────────────────────────────────┘    (clima + alertas)

                    ┌────────────────────────── RPA ───────────────────────────┐
 Portal CEAGESP ───►│ Robô n8n (diário 07:30): coleta ─► valida ─► grava ──────┼─► Supabase (banco do MVP)
                    │        alertas no Telegram + logs (execucoes_rpa)        │    (Central de Cotações e Alertas)
                    └──────────────────────────────────────────────────────────┘
```

| Pasta | Conteúdo |
|---|---|
| [bigdata/](bigdata/README.md) | DAG Airflow, coletores, notebooks Databricks (Bronze/Prata/Ouro), 5 consultas SQL |
| [rpa/](rpa/README.md) | Robô de cotações em **n8n** (Supabase + Telegram), schema SQL, BPMN e versão Python de referência |

## Fontes de dados

| Fonte | Tipo | Uso |
|---|---|---|
| [Open-Meteo](https://open-meteo.com/) | API aberta | clima observado + previsão 7 dias por município |
| [IBGE Localidades/Malhas](https://servicodados.ibge.gov.br/api/docs) | API aberta | município, UF, região e centróide (lat/lon) |
| Cadastro de produtores (MVP) | CSV exportado | cruzamento produtor × clima (dados pessoais → LGPD) |
| [CEAGESP Cotações](https://ceagesp.gov.br/cotacoes/) | Portal público (scraping permitido pelo robots.txt) | preços de atacado de hortifrúti |
