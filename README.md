# 🌍 Travel AI Assistant

Assistente di viaggio intelligente basato su AI che aiuta a pianificare viaggi personalizzati.

## 📁 Struttura del Progetto

```
ai/
├── 📄 run.py                      # Entry point CLI
├── 📄 run_with_login.py           # Entry point CLI con autenticazione
├── 📄 api_flask.py                # API REST Flask
│
├── 📂 src/                    
│   ├── 📂 agents/                 # Agenti AI specializzati
│   |   ├── base_agent.py
│   |   ├── data_collector.py
│   |   ├── plan_generator.py
|   |   ├── query_parser.py
│   |   └── rag_manager.py
│   |
|   ├── 📂 core/                  # Logica centrale
│   |   ├── orchestrator.py        # Coordinatore degli agenti
│   |   ├── session_manager.py     # Gestione sessioni utente
│   |   └── config.py              # Configurazioni
│   |
│   ├── 📂 auth/                  # Sistema di autenticazione
│   │   ├── database.py            # Gestione database SQLite
│   │   ├── auth_manager.py        # Autenticazione utenti
│   │   ├── trip_manager.py        # Gestione viaggi
│   │   └── auth_cli.py            # CLI per login
│   │
│   └── 📂 utils/                 # Per exports
│
├── 📂 data/                      # File dei codici iata
│   └── airports_iata.json
|
├── 📂 tests/                     # File di test ed esempi
│   ├── test_api.py
│   ├── test_api_simple.py
│   ├── frontend_example_react.jsx
│   ├── example_integration.py
│   └── test_login.py
│
├── 📂 frontend/                  # File del sito web
│   ├── app.js
│   ├── config.js
│   ├── index.html
│   └── styles.css
│
├── 📂 documentation/             # Documentazione
│   ├── API_README.md
│   └── GUIDA_FRONTEND.md
│
└── 📄 travel_assistant.db        # Database SQLite

```














## 🚀 Quick Start

### 1. Installazione

```bash
# Installa le dipendenze
pip install -r requirements.txt
```

### 2. Configurazione

Crea un file `.env` nella root del progetto:

```env
# === OpenAI ===
OPENAI_API_KEY=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

# === Flight Data (Amadeus) ===
VOLI_API_KEY=hdPf3R8swBDbJdc7Ryxp07tlb2ELHUYo
VOLI_API_SECRET=0mrPDoSeElf9lUHg

# === Weather ===
OPENWEATHER_API_KEY=8944cfbb82118ca79d84a44bbdb9bf3c

# === Monuments (Google Places) ===
MONUMENTS_API_KEY=AIzaSyBW9zBZjUV67q1KKJDxhDPwAQDAlNok_IQ

# === Events (Ticketmaster) ===
TICKETMASTER_API_KEY=LQuDjOy9WlbtgcWN5WuPASpIYWyGUGWd

# === GitHub (Optional, for higher rate limits) ===
GITHUB_TOKEN=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

# === Model Configuration (Optional) ===
OPENAI_MODEL=gpt-3.5-turbo
OPENAI_TEMPERATURE=0.7
REQUEST_TIMEOUT=15

# === RAG Configuration (Optional) ===
CHUNK_SIZE=800
CHUNK_OVERLAP=100
RAG_TOP_K=5

PYTHONIOENCODING=utf-8
```

### 3. Utilizzo

#### Opzione A: Web Interface (Consigliato)

```bash
# Avvia il server
python api_flask.py

# Apri il browser su: http://localhost:5000
```

#### Opzione B: CLI con Login

```bash
python run_with_login.py
```

#### Opzione C: CLI Semplice

```bash
python run.py
```

## 🌐 Web Interface

L'interfaccia web offre:
- ✅ Registrazione e login utenti
- ✅ Chat interattiva con l'AI
- ✅ Cronologia viaggi
- ✅ Design responsive e moderno
- ✅ Gestione sessioni sicura

**Accedi a:** `http://localhost:5000`

## 📡 API REST

Il server Flask espone API REST complete:

### Endpoints Principali

```
GET  /api/health              - Health check
POST /api/auth/register       - Registrazione
POST /api/auth/login          - Login
POST /api/auth/logout         - Logout
GET  /api/auth/status         - Stato autenticazione
POST /api/travel/query        - Nuovo viaggio
POST /api/travel/interact     - Interazione con piano
POST /api/travel/finalize     - Finalizza viaggio
GET  /api/history             - Cronologia viaggi
GET  /api/trip/:id            - Dettagli viaggio
```

📖 **Documentazione completa:** `document/API_README.md`

## 🗄️ Database

Il progetto usa **SQLite** con 4 tabelle:

1. **users** - Utenti registrati
2. **trips** - Viaggi pianificati
3. **plans** - Versioni dei piani
4. **interactions** - Interazioni utente

## 🧪 Test

Esegui i test dell'API:

```bash
# Test completo
python test/test_api.py

# Test semplice (senza AI)
python test/test_api_simple.py
```

## 🏗️ Architettura

### Frontend → API → Business Logic → AI/Database

```
┌─────────────┐
│  Browser    │
│ (HTML/JS)   │
└──────┬──────┘
       │ HTTP
       ↓
┌─────────────┐
│ Flask API   │
│ (REST)      │
└──────┬──────┘
       │
       ↓
┌──────────────────┐
│ SessionManager   │
│ (Business Logic) │
└──────┬───────────┘
       │
   ┌───┴───┬──────────┬──────────┐
   ↓       ↓          ↓          ↓
┌──────┐┌────┐┌────────┐┌──────────┐
│SQLite││Auth││TripMgr ││Orchestr. │
└──────┘└────┘└────────┘└────┬─────┘
                             │
                ┌────────────┴────┐
                ↓                 ↓
         ┌──────────┐      ┌──────────┐
         │ OpenAI   │      │ Tavily   │
         │ (GPT)    │      │ (Search) │
         └──────────┘      └──────────┘
```

## 📚 Funzionalità

### Agenti AI

1. **QueryParser** - Analizza la richiesta dell'utente
2. **DataCollector** - Raccoglie informazioni online
3. **RAGManager** - Gestisce la knowledge base
4. **PlanGenerator** - Genera il piano di viaggio

### Interazioni Intelligenti

L'AI classifica automaticamente le richieste in:
- 🔧 **Modification** - Modifica al piano
- ℹ️ **Information** - Richiesta informazioni
- 🆕 **New Trip** - Nuovo viaggio
- ✅ **Done** - Finalizza

## 🔒 Sicurezza

- Password hashate con SHA-256
- Token di sessione sicuri
- Validazione input lato server
- SQL injection protection
- CORS configurato

## 📦 Deploy

### Docker

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "api_flask.py"]
```

### Heroku

```bash
echo "web: python api_flask.py" > Procfile
heroku create travel-ai-app
git push heroku main
```

## 🤝 Contribuire

1. Fork del progetto
2. Crea un branch (`git checkout -b feature/AmazingFeature`)
3. Commit delle modifiche (`git commit -m 'Add feature'`)
4. Push al branch (`git push origin feature/AmazingFeature`)
5. Apri una Pull Request

## 📄 Licenza

MIT License - Sentiti libero di usare questo progetto!

## 🐛 Troubleshooting

### API non raggiungibile
```bash
# Verifica che il server sia attivo
curl http://localhost:5000/api/health
```

### Errore "Non autenticato"
- Assicurati di accedere tramite `http://localhost:5000` (non file://)
- Apri la console browser (F12) per vedere i log di debug

### Database locked
```bash
# Chiudi tutte le istanze dell'app
# Elimina il file .db-journal se esiste
```

## 📞 Supporto

Per domande o problemi:
- 📖 Controlla la documentazione in `document/`
- 🧪 Esegui i test in `test/`
- 💬 Apri un issue su GitHub

---

**Fatto con ❤️ da:**
- [Gaia Cassinelli](https://github.com/gaiacassinelli1)
- [Sergio Ghezzi](https://github.com/sergioghez)
- [Benedetta Milossevich](https://github.com/benedettami)
- [Barbara Geroli](https://github.com/BarbaraGeroli)
- [Sharon Burgo](https://github.com/sharonburg)
- [Mattia Stefanizzi](https://github.com/luxmattiastef)





