# Autonomous Trading System

Systeme de trading autonome end-to-end — de l'ingestion de donnees brutes
a l'execution broker, avec infrastructure auto-reparatrice en production 24/7.

---

```
╔══════════════════════════════════════════════════════════════════════════════╗
║              AUTONOMOUS TRADING SYSTEM — ARCHITECTURE COMPLETE              ║
╚══════════════════════════════════════════════════════════════════════════════╝

━━━━━━━━━━━━━━━━━━━━━━━━━━━━  ETAPE 1 — DATA LAYER  ━━━━━━━━━━━━━━━━━━━━━━━━━━

   ┌─────────────────────┐  ┌─────────────────────┐  ┌─────────────────────┐
   │   BROKER API        │  │   FINANCIAL API     │  │  ECONOMIC CALENDAR  │
   │  ─────────────────  │  │  ─────────────────  │  │  ─────────────────  │
   │  Live OHLCV         │  │  Macro D1           │  │  Surprises          │
   │  Multi-asset        │  │  Equities / Bonds   │  │  USD / EUR / JPY    │
   │  H1 + M15           │  │  FX / Commodities   │  │  Calendar events    │
   └──────────┬──────────┘  └──────────┬──────────┘  └──────────┬──────────┘
              └─────────────────────────┴──────────────────────────┘
                                        │
                           ┌────────────▼────────────┐
                           │   INCREMENTAL PIPELINE  │
                           │  ─────────────────────  │
                           │  Fetch · Dedup          │
                           │  Normalize · Parquet    │
                           └────────────┬────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  ETAPE 2 — ML LAYER  ━━━━━━━━━━━━━━━━━━━━━━━━━━━

                           ┌────────────▼────────────┐
                           │  FEATURE ENGINEERING    │
                           │  ─────────────────────  │
                           │  Multi-source fusion    │
                           │  Temporal encoding      │
                           │  Anti-leakage strict    │
                           └────────────┬────────────┘
                                        │
                           ┌────────────▼────────────┐       ┌─────────────────┐
                           │   PREDICTIVE MODELS     │       │  WEEKLY RETRAIN │
                           │  ─────────────────────  │◄──────│  ─────────────  │
                           │  Per asset              │       │  Temporal split │
                           │  Per direction          │       │  Embargo 15j    │
                           │  Parallel training      │       │  OOS calibrate  │
                           └────────────┬────────────┘       └─────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━  ETAPE 3 — DECISION & EXECUTION  ━━━━━━━━━━━━━━━━━━━━━━

                           ┌────────────▼────────────┐
                           │    DECISION ENGINE      │
                           │  ─────────────────────  │
                           │  State machine          │
                           │  Multi-phase logic      │
                           │  Entry / exit rules     │
                           └────────────┬────────────┘
                                        │
                           ┌────────────▼────────────┐
                           │    EXECUTION LAYER      │
                           │  ─────────────────────  │
                           │  Adaptive sizing        │
                           │  4-factor risk-adjusted │
                           │  Broker bridge          │
                           └────────────┬────────────┘
                                        │
                           ┌────────────▼────────────┐
                           │         BROKER          │
                           └─────────────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━  ETAPE 4 — RISK MANAGEMENT  ━━━━━━━━━━━━━━━━━━━━━━━━━

   Evalue en continu a resolution M15 sur l'ensemble des positions :

   ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐
   │     SAFETY       │    │      BUST        │    │    COOLDOWN      │
   │  ──────────────  │    │  ──────────────  │    │  ──────────────  │
   │  Daily drawdown  │    │  Hard floor      │    │  Post-safety     │
   │  threshold hit   │    │  breached        │    │  lockout         │
   │  → close all     │    │  → close all     │    │  next session    │
   │  → cooldown      │    │  → reset phase   │    │  no new entries  │
   └──────────────────┘    └──────────────────┘    └──────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━  ETAPE 5 — OBSERVABILITY  ━━━━━━━━━━━━━━━━━━━━━━━━━━

   ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐
   │   EVENT LOG      │    │    REST API      │    │  TELEGRAM CLI    │
   │  ──────────────  │    │  ──────────────  │    │  ──────────────  │
   │  Append-only     │    │  FastAPI + SSE   │    │  Bidirectional   │
   │  JSONL           │    │  Real-time push  │    │  Remote control  │
   │  Immutable       │    │  7 endpoints     │    │  LLM-backed      │
   └──────────────────┘    └──────────────────┘    └──────────────────┘

━━━━━━━━━━━━━━━━━━━━━  ETAPE 6 — SELF-HEALING INFRASTRUCTURE  ━━━━━━━━━━━━━━━━━

   ┌──────────────────┐  ┌────────────────────────────┐  ┌──────────────────┐
   │    WATCHDOG      │  │        HEALTHCHECK         │  │    AUTOPILOT     │
   │  ──────────────  │  │  ────────────────────────  │  │  ──────────────  │
   │  every 5 min     │  │  every 2 hours             │  │  every 20 min    │
   │                  │  │                            │  │                  │
   │  crash detect    │  │  scan warning logs         │  │  exception scan  │
   │  → auto-restart  │  │  → prompt LLM              │  │  → LLM repair    │
   │                  │  │  → edit source code        │  │  → commit Git    │
   │                  │  │  → validate syntax         │  │  → restart       │
   │                  │  │  → commit Git + restart    │  │                  │
   └──────────────────┘  └────────────────────────────┘  └──────────────────┘

              Git = source de verite operationnelle
              Chaque correction automatique est committee et reversible
```

## Decisions de design

**Meme code path backtest et live**
Le moteur de backtest est identique au moteur live — seul un flag d'environnement
change. Toute divergence est une anomalie a corriger, pas une hypothese a accepter.

**Event log append-only**
Chaque signal est ecrit avec son horodatage, son score et les seuils actifs.
On peut rejouer n'importe quelle decision historique sans relancer l'inference ML —
separation stricte entre "quel signal a ete genere" et "quelle regle a fire".

**Tracks paralleles (LIVE / DRY / SHADOW)**
Trois representations simultanees de l'equity. L'ecart est mesure et decompose
en composantes a chaque barre — spread, commission, swap, slippage.

**LLM-in-the-loop**
Le healthcheck ne detecte pas seulement les anomalies — il mandate un LLM pour
lire les logs, identifier la cause, corriger le code et committer. La reparation
est autonome et tracee.

## Stack

| Composant      | Usage                              |
|----------------|------------------------------------|
| Python 3.11+   | Langage principal                  |
| PyTorch        | Modelisation ML                    |
| pandas / numpy | Pipeline de donnees et features    |
| FastAPI        | API REST et streaming SSE          |
| MetaTrader 5   | Connexion broker et execution      |
| Git            | Source de verite operationnelle    |

---

*La logique interne, les parametres, les seuils et les features ne sont pas publies.
Ce README documente l'architecture et les decisions de design.*
