---
title: "Astro, GitHub a vlastní web: jak jsem přestal dělat věci na koleni"
meta_title: ""
description: "Astro framework, Cloudflare Pages, GitHub a doména za 200 Kč. Jak vznikl tento web a proč ne WordPress."
date: 2026-05-23T00:00:00Z
image: "/images/image-placeholder.png"
categories: ["Web Development", "Tooling"]
author: "Jakub Vozar"
tags: ["astro", "github", "cloudflare", "web-dev", "tooling"]
draft: false
---

> 💡 **TL;DR:** Web na Astro frameworku, hosting zdarma přes Cloudflare Pages, doména za 200 Kč/rok a celý kód na GitHubu. Žádný WordPress, žádný drag-and-drop editor — a tím spíš žádné další placené subscription.

## Web jako součást portfolia, ne jako blog

Hned v úvodu chci říct jednu věc: **tento web není jen blog.** Je to živá součást portfolia — místo, kde je vidět, co umím, jak přemýšlím a jak postupuju. Každý krok projektu, každý naučený koncept, každý vyřešený problém tu má svůj otisk.

Přemýšlel jsem o jednoduchém blog webu — ale pak mi došlo, že samotnému mě takové weby moc neříkají. Chtěl jsem něco, kde je za každou stránkou skutečná práce, ne jen text.

## Proč ne WordPress (ani Wix, ani Squarespace)

Mohl jsem si otevřít WordPress, vybrat šablonu a mít web za odpoledne. Ale to by mi nedalo nic — ani zkušenost, ani kontrolu, ani zájem.

Místo toho jsem sáhl po **Astro** — moderním static site generatoru, který generuje čisté HTML bez zbytečného JavaScriptu. Je rychlý, výborně se integruje s Tailwindem a komunita kolem něj roste. A co je důležité: naučil jsem se při tom pracovat se skutečným vývojovým workflow — terminál, Git, deploy pipeline.

Hosting běží na **Cloudflare Pages** — zdarma, automatický deploy při každém `git push` do main větve, HTTPS bez nastavování. Jediná věc, za kterou jsem zaplatil, je doména `jakubvozar.cz` — cca 200 Kč ročně.

## GitHub: od pbix souboru ke skutečnému verzování

Přiznávám: GitHub jsem doteď prakticky nepotřeboval. Sdílení Power BI reportů probíhalo přes OneDrive, Sharepoint, nebo emailem — co je na tom špatného?

Všechno. Jakmile na jednom pbix souboru pracují dva lidi, nebo potřebuješ zjistit, co se změnilo mezi verzemi, nebo chceš vrátit jednu konkrétní úpravu zpátky — zjistíš, že to jednoduše nefunguje. Přitom Power BI dnes už má nástroje pro Git integraci, verzování šablon i spolupráci na projektech. Svět se posunul.

GitHub mě naučil myslet jinak: **každá změna má důvod, každý commit je zpráva pro budoucí já** (nebo pro budoucího klienta, kolegu, recruiter). To je mindset profesionála — ne skrývání toho, co neumím, ale otevřené dokumentování toho, jak se učím.

Celý zdrojový kód webu je veřejně dostupný — to je záměr, ne náhoda.

## Interaktivní dashboardy přímo na webu

Největší technické rozhodnutí bylo, jak zobrazit výsledky backtestů na webu. Původní plán byl Power BI embed — jenže to vyžaduje Power BI Service licenci a work/school email, což je zbytečná překážka pro osobní projekt.

Řešení bylo elegantnější: **Plotly** v Pythonu generuje interaktivní HTML soubory, které se jednoduše vloží do Astro stránky. Žádný server, žádná licence, žádná závislost na externí službě. Equity curve, drawdown, tabulka obchodů, monthly PnL heatmap — vše je živé, interaktivní a běží čistě v prohlížeči.

## Sekce Games: učení hrou

Jednou z věcí, na které jsem nejvíce hrdý, je sekce **Games** — miniaplikace, kde si můžeš vyzkoušet Python nebo SQL znalosti formou kvízů a cvičení. Inspirace Duolingem: opakování v malých dávkách funguje lépe než maratónské studium.

Hry vznikly jako vedlejší produkt mého vlastního učení — když jsem se naučil nový koncept, říkal jsem si: "jak bych z toho udělal cvičení pro ostatní?" A najednou z toho byl obsah pro web.

Do budoucna plánuji rozšíření o SQL cvičení a pokročilejší Python úlohy. Vše přibývá postupně, jak jde projekt dopředu.

## Shrnutí: stack a náklady

| Co | Nástroj | Cena |
|---|---|---|
| Web framework | Astro + Tailwind | zdarma |
| Hosting | Cloudflare Pages | zdarma |
| Doména | jakubvozar.cz | ~200 Kč/rok |
| Verzování | GitHub (private repo) | zdarma |
| Dashboardy | Plotly (Python → HTML) | zdarma |

Celková roční investice: **200 Kč.** Zbytek je čas a chuť se učit.
