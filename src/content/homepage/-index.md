---
# Banner
banner:
  title: "Data Engineer & Trading Research"
  content: "Jsem nadšenec pro data engineering a data science, kterou kombinuji s analýzou finančních trhů. Stavím end-to-end pipeline od stahování dat přes backtesting trading strategií až po publikované dashboardy."
  image: "/images/banner.png"
  button:
    enable: true
    label: "Zobrazit Dashboard 📊"
    link: "/dashboard"

# Features
features:
  - title: "Trading Pipeline"
    image: "/images/service-1.png"
    content: "End-to-end datová pipeline pro backtesting trading strategií na forexových a akciových datech."
    bulletpoints:
      - "Stahování dat z yfinance a Alpha Vantage API"
      - "Backtesting SMA crossover strategie na EURUSD 1H datech"
      - "Interaktivní vizualizace v Plotly — equity curve, drawdown, trades"
      - "Automatický export výsledků do HTML dashboardu"
    button:
      enable: true
      label: "Zobrazit Dashboard"
      link: "/dashboard"

  - title: "Data Engineering Stack"
    image: "/images/service-2.png"
    content: "Projekt je postaven na moderním DE stacku s důrazem na reprodukovatelnost a rozšiřitelnost."
    bulletpoints:
      - "Python + pandas pro ingest a transformace"
      - "Parquet jako sloupcový formát pro efektivní ukládání"
      - "Astro + Cloudflare Workers pro statický web deploy"
      - "Git + konvenční commity pro verzování"
    button:
      enable: false
      label: ""
      link: ""

  - title: "Co chystám dál"
    image: "/images/service-3.png"
    content: "Projekt aktivně roste — každý týden přibývají nová data, strategie a funkce."
    bulletpoints:
      - "Alpha Vantage ingest pro US akcie z mého watchlistu"
      - "AWS S3 + Glue pro cloudovou migraci pipeline"
      - "Walk-forward analýza a Monte Carlo simulace"
      - "Blog o data engineeringu a trading research metodologii"
    button:
      enable: false
      label: ""
      link: ""
---