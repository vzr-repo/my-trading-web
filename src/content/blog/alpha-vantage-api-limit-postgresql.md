---
title: "Alpha Vantage, 25 requestů za den a jak mě API limit naučil více než dokumentace"
meta_title: ""
description: "Free tier API limity, návrat k yfinance a inkrementální refresh — základní princip každé datové pipeline."
date: 2026-05-23T00:00:00Z
image: "/images/image-placeholder.png"
categories: ["Data Engineering", "Trading"]
author: "Jakub Vozar"
tags: ["python", "yfinance", "alpha-vantage", "api", "incremental-refresh", "parquet"]
draft: false
---

> 💡 **TL;DR:** Chtěl jsem stáhnout data pro 15 akciových titulů přes Alpha Vantage API. Narazil jsem na free tier limit 25 requestů za den. Místo frustrace jsem se vrátil k yfinance — a při té příležitosti napsal inkrementální refresh, který je v pipelines standardem.

## API sen: data proudí sama

Jedno z největších "aha" momentů ve finančním controllingu je, když poprvé vidíš, jak data přicházejí automaticky — žádný manuální export, žádné kopírování, žádné "aktualizuji, počkej." Data prostě jsou, čerstvá, konzistentní, připravená k analýze.

REST API je základní stavební kámen tohohle světa. Napíšeš request, dostaneš JSON odpověď, zpracuješ, uložíš. Elegantní, opakovatelné, automatizovatelné. Přesně to jsem chtěl nastavit pro svoji akciovou databázi.

## Alpha Vantage a tvrdý limit

Vybral jsem **Alpha Vantage** — populární zdroj historických akciových dat s Python knihovnou a slušnou dokumentací. Free tier, bez platební karty, rychlé nastavení.

Plán byl jednoduchý: 3 sektory, 15 tickerů (Tech, Fintech, Index), denní OHLCV data od roku 2020. Spustím skript, data se stáhnou do Parquet souborů, hotovo.

Spustil jsem skript. Po chvíli — chyba. Po dalším requestu — chyba. Odpověď z API vypadala nějak takto:

```json
{
  "Note": "Thank you for using Alpha Vantage! Our standard API rate limit is 25 requests per day."
}
```

**25 requestů za den.** Pro 15 tickerů to znamená, že kompletní stažení dat trvá minimálně 2 dny — a to za předpokladu, že nic nespustím dvakrát. Což jsem samozřejmě spustil.

## Zpátky k yfinance — a tentokrát správně

Vrátil jsem se k **yfinance**, které jsem používal od začátku pro EUR/USD data. Žádné rate limity, žádná registrace, data jdou zpět přes 5 let. Jednoduché řešení bylo k dispozici od začátku — prostě jsem ho nevyužil naplno.

Tentokrát jsem ale chtěl udělat věc správně: ne jen stáhnout všechna data znovu a znovu při každém spuštění, ale napsat **inkrementální refresh** — logiku, která zkontroluje, co už na disku máme, a stáhne jen to, co chybí.

## Inkrementální refresh: základ každé pipeline

V Data Engineeringu je inkrementální zpracování dat jeden ze základních principů. Místo "smaž vše a načti znovu" (full refresh) se ptáš: "co je nové od posledního běhu?" Šetříš tím síť, čas i náklady — a v produkčním prostředí to není volba, je to standard.

Tady je jak to vypadá v praxi:

```python
import os
from datetime import datetime, timedelta
import pandas as pd
import yfinance as yf

start_date = datetime(2020, 1, 1)

tickers = {
    "Tech": ["AAPL", "MSFT", "NVDA", "GOOGL", "META", "AMZN", "AMD", "TSLA"],
    "Fintech": ["V", "MA", "PYPL", "COIN", "NU"],
    "Index": ["SPY"]
}

for sector in tickers:
    os.makedirs(f"data/stocks/{sector}", exist_ok=True)

for sector, seznam in tickers.items():
    for ticker in seznam:
        path = f"data/stocks/{sector}/{ticker}.parquet"

        # 1. Pokud soubor existuje, zjisti datum posledního záznamu
        if os.path.exists(path):
            existing_df = pd.read_parquet(path)
            last_date = existing_df.index.max()
            start_date = last_date + timedelta(days=1)  # stáhni jen nová data

        # 2. Stáhni nová data
        df = yf.download(tickers=ticker, interval="1d", start=start_date)

        # 3. Pokud nejsou nová data, přeskoč
        if df.empty:
            print(f"{ticker} — žádná nová data, přeskakuji")
            continue

        # 4. Uprav strukturu
        df.columns = df.columns.droplevel(1)
        df.index = pd.to_datetime(df.index)

        # 5. Spoj stará a nová data
        if os.path.exists(path):
            combined_df = pd.concat([existing_df, df])
        else:
            combined_df = df

        # 6. Ulož
        try:
            combined_df.to_parquet(path)
            print(f"{ticker} uloženo ✓")
        except Exception as e:
            print(f"Chyba u {ticker}: {type(e).__name__}: {e}")
```

Logika je prostá: přečti Parquet soubor → zjisti datum posledního záznamu → nastav `start_date` na den poté → stáhni jen rozdíl → spoj a ulož. Skript můžu spouštět každý den a nikdy nestahuje data znovu.

## Struktura dat: OHLCV a sektory

Výsledkem je lokální databáze 15 tickerů organizovaná do složek podle sektorů:

```
data/
  stocks/
    Tech/
      AAPL.parquet
      MSFT.parquet
      NVDA.parquet
      ...
    Fintech/
      V.parquet
      MA.parquet
      ...
    Index/
      SPY.parquet
```

Každý soubor obsahuje denní OHLCV data (Open, High, Low, Close, Volume) — klasická svíčková struktura, základ technické analýzy. Z těchto dat pak tahám co potřebuji: výkonnost sektoru, porovnání tickerů, signály pro swing trading analýzu.

## Co přijde dál: SQL nad Parquet daty

Parquet soubory jsou skvělé pro ukládání, ale pro dotazování napříč 15 tickery najednou nejsou ideální. Dalším krokem je načíst data do **PostgreSQL** lokálně a trénovat SQL dotazy — stejné SQL, které pak použiji v AWS Athena v cloudu.

SQL playground s vybranými úlohami si můžeš vyzkoušet i ty → [SQL Playground](/sql-playground/)
