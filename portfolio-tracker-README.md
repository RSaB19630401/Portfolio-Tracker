# 📈 PortfolioTracker

Echtzeit-Portfolio-Tracker mit Yahoo Finance Anbindung, Dividenden-Dashboard und technischer Analyse.

## Architektur

```
GitHub Pages (Frontend)              Cloudflare Worker (API)
┌──────────────────────┐            ┌──────────────────────────┐
│  index.html          │  ──────►  │  portfolio-worker.js      │
│  React 18 + Babel    │  JSON     │  Yahoo Finance Proxy      │
│  CDN (kein Build)    │  ◄──────  │  Cache API (5 Min)        │
└──────────────────────┘            └──────────────────────────┘
rsab19630401.github.io/              portfolio-api.rsab1963.
  portfolio-tracker                    workers.dev
```

## Features

- **6 Tabs**: Dashboard, Depot, Dividenden, Analyse, Watchlist
- **115 Wertpapiere** vorgeladen (DAX, US Tech, Robotik, Rüstung, ETFs, Anleihen)
- **Echtzeitkurse** via Yahoo Finance (über Cloudflare Worker)
- **Dividenden-Dashboard**: Jahres-Dividende, Rendite, HV-Kalender, monatliche Ausschüttungsgrafik
- **Technische Analyse**: RSI(14), MACD, SMA(20/50) mit Kauf-/Verkaufssignalen
- **Sektor-/Regionen-Allokation**: Pie-Charts + Klumpenrisiko-Warnung
- **Globale Suche**: WKN, ISIN, Name über alle Werte
- **Detail-View**: Großer Chart, Fundamentaldaten, Indikatoren pro Wertpapier
- **Währungsumrechnung**: USD, JPY, CHF, HKD → EUR
- **CSV-Export**: Depot als Excel-kompatible CSV
- **Auto-Refresh**: alle 5 Min während Börsenzeiten
- **Offline-Fallback**: Cache → Mock-Daten wenn API nicht erreichbar
- **Mobile-Responsive**: optimiert für alle Bildschirmgrößen
- **Dark Mode**: professionelles Design

## Deployment

### 1. GitHub Pages (Frontend)

```bash
# Repository erstellen
git init
git add index.html README.md
git commit -m "Initial commit"
git remote add origin https://github.com/rsab19630401/portfolio-tracker.git
git push -u origin main

# GitHub Pages aktivieren:
# Settings → Pages → Source: main branch → / (root)
# → https://rsab19630401.github.io/portfolio-tracker
```

### 2. Cloudflare Worker (API)

```bash
cd worker
npm install -g wrangler    # Falls noch nicht installiert
wrangler login
wrangler deploy
# → https://portfolio-api.rsab1963.workers.dev
```

### 3. URL anpassen (falls Worker-Name anders)

In `index.html` die Worker-URL ändern:
```javascript
const WORKER_URL = "https://portfolio-api.rsab1963.workers.dev";
```

## Datenquellen

| Datenquelle | Tickers | Verfügbarkeit |
|-------------|---------|:---:|
| Yahoo Finance (Live) | Deutsche Aktien (.DE), US (.US), Japan (.T), Schweiz (.SW) | 🟢 |
| Mock-Daten (Fallback) | DWS Fonds, Anleihen (WKN), Delisted | 🟡 |

## Tech-Stack

- **Frontend**: React 18 (CDN) + Babel Standalone, kein Build-Prozess nötig
- **Backend**: Cloudflare Worker (Yahoo Finance Proxy mit Cache)
- **Persistenz**: localStorage (Portfolio, Watchlist, API-Cache)
- **Hosting**: GitHub Pages (kostenlos)
- **API-Proxy**: Cloudflare Workers Free Tier (100.000 Requests/Tag)

## Dateien

```
portfolio-tracker/
├── index.html              # Komplette App (React + Babel, ~2.400 Zeilen)
├── README.md
└── worker/
    ├── portfolio-worker.js  # Cloudflare Worker (Yahoo Finance Proxy)
    └── wrangler.toml        # Worker-Konfiguration
```
