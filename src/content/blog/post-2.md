---
title: "API rate limity a jiné věci, které mě překvapily"
meta_title: ""
description: "Konkrétní problémy, na které jsem narazil při práci s Alpha Vantage API, a jak jsem je vyřešil."
date: 2026-05-01T23:00:00Z
image: "/images/image-placeholder.png"
categories: ["Data Engineering", "Trading"]
author: "Jakub Vozar"
tags: ["python", "api", "data engineering"]
draft: false
---

Když jsem začínal pracovat s Alpha Vantage API, myslel jsem, že to bude jednoduché. Zavolám API, dostanu data, uložím. Trvalo mi to déle, než jsem čekal — a ne kvůli složitosti kódu.

## Rate limity nejsou to, co si myslíš

Alpha Vantage free tier má 25 volání za den. Ne 500, jak jsem si přečetl v dokumentaci jednoho staršího článku. Zjistil jsem to v nejhorší možný moment — uprostřed stahování dat, když mi script spadl s chybou `KeyError: 'Time Series (Daily)'`.

Chyba nevypadala jako rate limit. Vypadala jako problém s daty. Přidal jsem `print(data)` před problematický řádek a teprve tehdy jsem uviděl skutečnou odpověď od API: *"We have detected your API key and our standard rate limit is 25 requests per day."*

Lekce: když API vrátí neočekávanou odpověď, vždycky nejdřív vypiš celou odpověď. Nesnaž se hádat.

## API klíč jako None

Druhý problém byl hloupější. Script hlásil, že API klíč je `None`. Měl jsem `.env` soubor, měl jsem `load_dotenv()`, měl jsem `os.getenv()`. Vše správně.

Přidal jsem debug řádek hned po načtení klíče:

```python
print(f"API klíč načten: {API_KEY}")
```

Vypsal `None`. Problém byl v `.env` souboru — špatný formát. Správný formát je:

```
ALPHA_VANTAGE_KEY=abc123xyz
```

Bez uvozovek, bez mezer kolem `=`, bez ničeho navíc. Jednoduchá věc, ale snadno se přehlédne.

## Script, který nespadne

Po těchto dvou zkušenostech jsem přidal do scriptu dvě věci.

První — kontrola odpovědi před parsováním:

```python
if "Time Series (Daily)" not in data:
    print(f"Chyba u {ticker}: {data}")
    continue
```

`continue` přeskočí problematický ticker a pokračuje dalším. Script nespadne, jen zaloguje chybu.

Druhá — přeskočení aktuálních dat:

```python
if os.path.exists(path):
    existing_df = pd.read_parquet(path)
    last_date = existing_df.index.max()
    if last_date.date() >= pd.Timestamp.today().date():
        print(f"{ticker} je aktuální, přeskakuji")
        continue
```

Takhle script při každém spuštění stáhne jen data, která skutečně chybí. Šetří API volání a čas.

## Co si z toho odnáším

Robustní pipeline není o tom, aby fungovala za ideálních podmínek. Je o tom, aby zvládla selhání — rate limity, špatné odpovědi, chybějící soubory — a pokračovala dál.

Tohle je věc, která se v tutoriálech moc neučí. Naučil jsem se ji tím, že mi to dvakrát spadlo.
