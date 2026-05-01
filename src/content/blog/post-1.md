---
title: "Jak jsem postavil svoji první trading pipeline"
meta_title: ""
description: "Od nuly k automatizovanému stahování dat pro 15 akcií."
date: 2026-05-01T23:00:00Z
image: "/images/image-placeholder.png"
categories: ["Data Engineering", "Trading"]
author: "Jakub Vozar"
tags: ["python", "data engineering", "trading"]
draft: false
---

Rozhodl jsem se naučit data engineering. Ne přes kurzy, ale přes reálný projekt — pipeline pro analýzu finančních trhů. Tady je, jak to zatím vypadá.

## Proč zrovna trading pipeline?

Obchoduji forex a akcie. Vždycky jsem si dělal analýzy ručně v Excelu a Power BI. Jenže čím víc jsem se zajímal o data engineering, tím víc mi přišlo nesmyslné tahat data ručně, když to může dělat script sám.

Takže projekt: postavit end-to-end pipeline od stahování dat přes backtesting až po publikované dashboardy. Žádné kurzy, žádné tutoriály. Jen Python, reálná data a hodně chyb.

## Fáze 1 — forex data a první backtest

Začal jsem jednoduše. Script v Pythonu, který stáhne hodinová data pro EURUSD přes yfinance, uloží je do Parquetu a spustí jednoduchý SMA crossover backtest.

Výsledek byl průměrný — strategie moc nefunguje. Ale to nebyl cíl. Cílem bylo postavit pipeline a pochopit, jak data tečou od zdroje po vizualizaci.

Výsledky backtestingu jsem exportoval do CSV a postavil Power BI dashboard se čtyřmi vizuály: equity curve, drawdown, seznam obchodů a měsíční PnL. Ten dashboard teď visí live na tomhle webu.

## Fáze 2 — akciová data a Alpha Vantage API

Ve druhé fázi jsem přidal akciová data. Vybral jsem 15 tickerů — tech akcie jako AAPL, MSFT, NVDA a fintech jako Visa, Mastercard nebo Coinbase — plus SPY jako benchmark.

Pro stahování dat jsem poprvé pracoval s externím REST API. Alpha Vantage má jednoduché volání, JSON odpověď a OHLCV strukturu — stejnou, jakou znám z forexu.

Postavil jsem script, který prochází všechny tickery, stáhne data, uloží do Parquetu a přeskočí tickery, které už má aktuální. Celé to běží automaticky každý den v 23:00 přes Windows Task Scheduler.

## Co jsem se naučil

Python je mnohem přímočařejší, než jsem čekal. Největší překvapení byl způsob, jak věci do sebe zapadají — dict, loop, f-string. Jednoduché koncepty, ale když je dáš dohromady, najednou máš fungující pipeline.

Druhá věc: API rate limity jsou realita. Alpha Vantage free tier má 25 volání za den — ne 500, jak jsem původně předpokládal. Script mi spadl uprostřed stahování a musel jsem přidat error handling a logiku pro přeskakování existujících dat.

Třetí věc: `.env` soubory a `.gitignore` nejsou volitelné. API klíč nesmí nikdy skončit v Gitu. Nastavit to správně trvá 5 minut a ušetří to pozdější problémy.

## Co je dál

Data mám. Teď přijde sektorová analýza — porovnání výkonnosti tech vs fintech vs benchmark SPY. A backtesting na akciových datech, kde chci testovat stejnou SMA strategii, jakou jsem použil na forexu.

Kód je na GitHubu, dashboardy jsou živé. Jede to.