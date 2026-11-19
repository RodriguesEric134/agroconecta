> **Repositórios por disciplina** — o projeto foi separado em três repositórios para a entrega:
> - Big Data Architecture & Data Integration → [agroconecta-bigdata](https://github.com/RodriguesEric134/agroconecta-bigdata)
> - Robotic Process Automation → [agroconecta-rpa](https://github.com/RodriguesEric134/agroconecta-rpa)
> - Project-Based Maker Lab → [agroconecta-makerlab](https://github.com/RodriguesEric134/agroconecta-makerlab) (robô de campo com TinyML + LLM)
>
> Este repositório reúne os três e segue como referência do projeto completo.

# AgroConecta — Big Data & RPA

Plataforma de apoio à decisão para pequenos produtores rurais (Global Solution 2026.2 — CitizenTech).
ODS 2 (Fome Zero e Agricultura Sustentável) e ODS 13 (Ação Contra a Mudança do Clima).

## Equipe

| Integrante | RM |
|---|---|
| Eric de Carvalho Rodrigues | RM550249 |
| Renato Izumi Ichikawa | RM99242 |
| Bernardo Pinto Rocha | RM99209 |

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
| [makerlab/](makerlab/README.md) | Robô de campo no **Wokwi** (ESP32): TinyML embarcado, LLM via n8n, peça 3D e [vídeo](https://youtu.be/akfVlb_59Rs) |

## Fontes de dados

| Fonte | Tipo | Uso |
|---|---|---|
| [Open-Meteo](https://open-meteo.com/) | API aberta | clima observado + previsão 7 dias por município |
| [IBGE Localidades/Malhas](https://servicodados.ibge.gov.br/api/docs) | API aberta | município, UF, região e centróide (lat/lon) |
| Cadastro de produtores (MVP) | CSV exportado | cruzamento produtor × clima (dados pessoais → LGPD) |
| [CEAGESP Cotações](https://ceagesp.gov.br/cotacoes/) | Portal público (scraping permitido pelo robots.txt) | preços de atacado de hortifrúti |
