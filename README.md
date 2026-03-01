# Produktion

Produktions-Monorepo für Automatisierungs- und Entwicklungs-Services.

## Systemarchitektur

```
┌─────────────────────────────────────────────────────────────────┐
│                        ORCHESTRATOR                             │
│              /opt/Projekte/Orchestrator  (kein GitHub)          │
│  Nimmt Aufgaben entgegen, delegiert an Experten-Systeme         │
│  Trigger: Webhook, Telegram, E-Mail, Shelly IoT                 │
└───────────────────────────┬─────────────────────────────────────┘
                            │ delegiert Jobs
            ┌───────────────┴───────────────┐
            ▼                               ▼
┌───────────────────────┐     ┌─────────────────────────┐
│    agent-pipeline     │     │   weitere Experten-      │
│  /opt/Projekte/       │     │   Services (geplant)     │
│  agent-pipeline       │     └─────────────────────────┘
│  (kein GitHub)        │
│  Entwicklungs-Jobs:   │
│  SPEC → PLAN → Code   │
│  LXC 133, :8430       │
└───────────────────────┘
```

## Projekte

| Projekt | Beschreibung | Pfad | GitHub |
|---------|-------------|------|--------|
| **Orchestrator** | Zentraler Aufgaben-Orchestrator – nimmt Aufgaben entgegen und delegiert an Experten-Services | `/opt/Projekte/Orchestrator` | [MarcoFPO/Orchestrator](https://github.com/MarcoFPO/Orchestrator) |
| **agent-pipeline** | Automatisierter Entwicklungs-Service – generiert SPEC/PLAN und implementiert Code via Claude API | `/opt/Projekte/agent-pipeline` | [MarcoFPO/agent-pipeline](https://github.com/MarcoFPO/agent-pipeline) |

## Infrastruktur

| Service | LXC | IP | Port | Status |
|---------|-----|----|------|--------|
| agent-pipeline | 133 | 10.1.1.208 | 8430 | produktiv |
| Orchestrator | – | – | – | Entwicklung |

## Deployment

### agent-pipeline (LXC 133 – IP `10.1.1.208`)

```bash
cd /opt/Projekte/agent-pipeline
./deploy.sh
```

### Orchestrator

```bash
cd /opt/Projekte/Orchestrator
# systemd/orchestrator.service auf Ziel-LXC installieren
```

## Rollen & Verantwortlichkeiten

| Komponente | Rolle |
|------------|-------|
| **Orchestrator** | Eingangs-Gateway: empfängt Aufgaben über Webhook, Telegram, E-Mail, IoT. Plant Workflows und delegiert Steps an Experten-Services. |
| **agent-pipeline** | Experten-Service für Softwareentwicklung: empfängt strukturierte Entwicklungsaufgaben vom Orchestrator, generiert Spezifikation + Plan und implementiert Code autonom via Claude API. |

## GitHub-Repositories

| Repo | URL |
|------|-----|
| Produktion (dieses Repo) | https://github.com/MarcoFPO/Produktion |
| Orchestrator | https://github.com/MarcoFPO/Orchestrator |
| agent-pipeline | https://github.com/MarcoFPO/agent-pipeline |
