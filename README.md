# 🗺️ Google Maps Seller

A fully automated B2B lead generation and outreach system — scrapes businesses from Google Maps, generates a personalized website for each lead, creates a video offer, and sends it via WhatsApp. No manual intervention required.

---

## 🏗️ System Architecture

```
Google Maps
    │
    ▼
┌─────────────────────────────┐
│        scraper.py           │
│  Selenium-based scraper     │
│  Extracts business data     │
│  (name, phone, address,     │
│   category, city, photos)   │
└────────────┬────────────────┘
             │  Saves to SQLite DB
             ▼
┌─────────────────────────────┐
│    script1_preparador.py    │
│  Cleans and normalizes data │
│  Filters duplicates         │
│  Prepares outreach queue    │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│      gerador_sites.py       │
│  Generates a unique HTML    │
│  website per business using │
│  their own data & photos    │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│  script2_agenda_ofertas.py  │
│  Schedules outreach queue   │
│  Controls send rate/limits  │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│      script5_sender.py      │
│  Sends WhatsApp message     │
│  via Evolution API          │
│  Includes website link      │
│  + video offer              │
└────────────┬────────────────┘
             │  Client replies
             ▼
┌─────────────────────────────┐
│  script3_webhook.py         │
│  script4_reply_builder.py   │
│  Handles incoming replies   │
│  Builds automated responses │
│  Routes to human if needed  │
└─────────────────────────────┘
```

---

## ✨ Features

### Scraping
- Selenium-based Google Maps scraper with scroll automation
- Filters by business type, city, and radius
- Extracts: name, phone, address, category, rating, photos
- Pre-filter by visual listing to avoid low-quality leads
- Supports manual city list or coordinates from CSV (5,000+ Brazilian cities)

### Site Generation
- Generates a unique HTML website per business automatically
- Uses the business's own name, photos, address, and category
- Hosted and served per lead — each gets their own URL
- Site expiration system (`expirar_sites.py`) for link urgency

### Outreach Automation
- WhatsApp message delivery via **Evolution API**
- Rate limiting and queue management to avoid bans
- Personalized message with business name and website link
- Video offer generation per lead (`test_video_343.py`)
- Payment redirect integration (`script7_redirect_pagamento.py`)

### Reply Handling
- Webhook receiver for incoming WhatsApp replies
- Automated reply builder based on conversation stage
- Yampi e-commerce webhook integration for purchase events

### Dashboard
- Full web dashboard (`dashboard.py`) for monitoring:
  - Leads scraped, sites generated, messages sent
  - Reply rates and conversion tracking
  - Queue status and send schedules

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Scraping | Python, Selenium, Chromium |
| Site Generation | Python, HTML/CSS templating |
| Database | SQLite |
| Outreach | Evolution API (WhatsApp) |
| Webhook Server | Python (Flask/fastAPI) |
| Dashboard | Python web framework |
| Infrastructure | Ubuntu VPS, Docker Compose |
| Scheduling | systemctl services |

---

## 📁 Project Structure

```
├── scraper.py                   # Google Maps scraper (main)
├── script1_preparador.py        # Data cleaning and queue prep
├── script2_agenda_ofertas.py    # Outreach scheduler
├── script3_webhook.py           # Incoming WhatsApp webhook
├── script4_reply_builder.py     # Automated reply logic
├── script5_sender.py            # WhatsApp message sender
├── script6_webhook_yampi.py     # E-commerce purchase webhook
├── script7_redirect_pagamento.py# Payment redirect handler
├── gerador_sites.py             # Per-lead website generator
├── expirar_sites.py             # Site expiration system
├── primeira_oferta.py           # First message template builder
├── responder.py                 # Reply handler
├── setup_login.py               # WhatsApp session setup
├── dashboard.py                 # Monitoring dashboard
├── database.py                  # SQLite interface
├── docker-compose.yml           # Evolution API + services
├── latitude-longitude-cidades.csv # 5000+ Brazilian cities
└── documentacao.md              # Internal documentation
```

---

## ⚙️ Configuration

Environment variables via `.env`:

```env
EVOLUTION_API_URL=https://your-evolution-instance
EVOLUTION_API_KEY=your_key
INSTANCE_NAME=your_instance
DB_PATH=./scraperbot.db
SITES_BASE_URL=https://your-domain.com/sites
```

---

## 🚀 Usage

**1. Start infrastructure**
```bash
docker-compose up -d
```

**2. Run scraper**
```bash
python scraper.py
# Configure: business type, city, radius, max results
```

**3. Prepare outreach queue**
```bash
python script1_preparador.py
```

**4. Generate sites**
```bash
python gerador_sites.py
```

**5. Start sender**
```bash
python script5_sender.py
```

**6. Monitor via dashboard**
```bash
python dashboard.py
```

---

## ⚠️ Disclaimer

This tool is intended for legitimate B2B outreach to businesses with publicly listed contact information. Users are responsible for complying with local regulations regarding automated messaging and data collection.

---

## 👤 Author

**Hilton Paz** — [github.com/hiltontrip](https://github.com/hiltontrip) · [linkedin.com/in/hilton-júnior-b1544927](https://linkedin.com/in/hilton-júnior-b1544927)
