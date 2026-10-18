# Storingsmelder

Pieterpost

Een klein meldingensysteem voor storingen. Je meldt een storing via een formulier, je ziet de openstaande meldingen in een lijst, en je sluit ze af als ze opgelost zijn.

Deze applicatie is het startpunt voor **Cloud computing (B-UCLL-MGN13A)**. Je werkt er het hele semester mee. De applicatie zelf verandert bijna niet, wat verandert is hoe en waar ze draait.

---

## Wat er in deze repo zit

```text
app.py              de applicatie
requirements.txt    de pakketten die ze nodig heeft
templates/          de HTML van de webpagina
Dockerfile          om de applicatie te bouwen
compose.yaml        om de applicatie en database samen te starten
.dockerignore       bestanden die Docker bij het bouwen overslaat
.env.example        voorbeeld van de instellingen
.gitignore          bestanden die Git overslaat
```

## Wat de applicatie nodig heeft

| | |
|---|---|
| Python | 3.14 in het Docker-image |
| Pakketten | staan in `requirements.txt` |
| Poort | 8000 in de container |

## Instellingen

De applicatie gebruikt omgevingsvariabelen voor de instellingen.

| Variabele | Wat het doet |
|---|---|
| `DATABASE_URL` | Het adres van de database |
| `SQLITE_PAD` | De locatie van het SQLite-bestand als er geen PostgreSQL wordt gebruikt |
| `APP_TITEL` | De titel bovenaan de pagina |
| `PORT` | De poort waarop de applicatie luistert |
| `HOST_PORT` | De poort waarop je de applicatie op je laptop opent |
| `POSTGRES_DB` | De naam van de PostgreSQL-database |
| `POSTGRES_USER` | De PostgreSQL-gebruikersnaam |
| `POSTGRES_PASSWORD` | Het wachtwoord voor PostgreSQL |

---

## De applicatie starten

Zorg dat Docker Desktop geïnstalleerd is en draait.

Clone de repository en ga naar de projectmap:

```powershell
git clone https://github.com/RayUcll/cloud-computing.git
cd cloud-computing
```

Maak een lokaal `.env`-bestand op basis van `.env.example`:

```powershell
Copy-Item .env.example .env
notepad .env
```

Vul in `.env` de instellingen in. Gebruik voor `POSTGRES_PASSWORD` een eigen wachtwoord en gebruik hetzelfde wachtwoord in `DATABASE_URL`.

Voorbeeld:

```dotenv
DATABASE_URL=postgresql://storing:kies-een-eigen-wachtwoord@db:5432/storingen
APP_TITEL=Storingsmelder
PORT=8000
HOST_PORT=8000
POSTGRES_DB=storingen
POSTGRES_USER=storing
POSTGRES_PASSWORD=kies-een-eigen-wachtwoord
SQLITE_PAD=/data/storingen.db
```

Sla het bestand op. Het echte `.env`-bestand hoort niet in GitHub te staan.

Start de applicatie en de database:

```powershell
docker compose up -d
```

Controleer of de containers draaien:

```powershell
docker compose ps
```

De database moet `healthy` worden. Ook de applicatie wordt na het opstarten `healthy`.

Open daarna http://localhost:8000 in je browser. Je kunt nu een storing melden.

## Stoppen en opnieuw starten

Stop de containers met:

```powershell
docker compose down
```

Start alles opnieuw met:

```powershell
docker compose up -d
```

De meldingen blijven bewaard omdat PostgreSQL zijn gegevens in een Docker-volume opslaat.

**Let op:** dit commando verwijdert ook de databasegegevens:

```powershell
docker compose down -v
```

Gebruik dit alleen als je de database opnieuw wilt beginnen.

## Controleren of de applicatie werkt

De applicatie heeft ook een healthcheck op `/health`.

Open http://localhost:8000/health. Als de applicatie en database werken, krijg je een succesvolle status terug.

Als er iets misgaat, bekijk je de logs met:

```powershell
docker compose logs app
docker compose logs db
```

---

## Wat je er dit semester mee doet

| Blok | Wat je doet |
|---|---|
| Week 1 tot 4 | Je zet deze applicatie in een container en laat ze samen met een database draaien |
| Week 5 tot 9 | Je zet diezelfde applicatie op een Kubernetes-cluster en maakt ze bereikbaar in je browser |
| Week 10 tot 11 | Je laat een pipeline het image bouwen en klaarzetten |

Je begint dus nooit opnieuw. Elke opdracht bouwt verder op de vorige.

## Deze repo is van jou

Je hebt hem gemaakt vanaf het sjabloon, dus dit is jouw kopie. Werk erin, commit elke les, en push. Indienen doe je met de link naar een commit. Hoe dat precies gaat, staat in de opdrachtbundel op Toledo.