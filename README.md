# 8star Links

Een lichte, volledig self-hosted bookmarkmanager met een donkerblauwe interface, geneste mappen en Firefox-import. Gebouwd met Node.js, Express en SQLite.

## Functies

- Onbeperkt geneste mappen
- Links en mappen verslepen en ordenen
- Inklapbare mappen en zoekfunctie
- Instelbare mapkleuren
- Firefox/Netscape HTML import en export
- Automatische ontdubbeling bij importeren
- JSON-back-up downloaden en terugzetten
- Permanente SQLite-opslag
- Responsive interface voor desktop en mobiel
- Volledig lokaal te bouwen; geen externe container-image nodig

## Snel starten met Docker Compose

```bash
git clone https://github.com/JOUW-GEBRUIKERSNAAM/8star-links.git
cd 8star-links
docker compose up -d --build
```

Open daarna `http://IP-VAN-JE-SERVER:3080`.

## Portainer

Clone het repository op de Docker-host en bouw de image:

```bash
git clone https://github.com/JOUW-GEBRUIKERSNAAM/8star-links.git
cd 8star-links
docker build -t 8star-links:latest .
```

Maak daarna in Portainer een Stack met:

```yaml
services:
  8star-links:
    image: 8star-links:latest
    container_name: 8star-links
    restart: unless-stopped
    environment:
      TZ: Europe/Amsterdam
      PORT: 3000
      DATA_DIR: /app/data
    volumes:
      - 8star-links-data:/app/data
    ports:
      - "3080:3000"
    security_opt:
      - no-new-privileges:true
    cap_drop:
      - ALL

volumes:
  8star-links-data:
```

## Data en back-ups

Met de meegeleverde `docker-compose.yml` staat de database in `./data/links.sqlite`. De map `data` en alle SQLite-bestanden worden door Git genegeerd. Vanuit de webinterface kun je ook een JSON-back-up downloaden en terugzetten.

## Firefox importeren

Exporteer in Firefox je bladwijzers als HTML. Open vervolgens in 8star Links linksboven het instellingenmenu en kies **Firefox importeren**. De import behoudt de mappenstructuur, slaat bestaande links over en verwijdert dubbele URL's.

## Beveiliging

8star Links bevat bewust geen eigen gebruikersaccounts of authenticatie. Publiceer de applicatie niet rechtstreeks op internet zonder beveiligde reverse proxy, bijvoorbeeld met Authentik, Authelia, VPN of een access-list.

## Bijwerken

```bash
git pull
docker compose up -d --build
```

## Licentie

Beschikbaar onder de [MIT License](LICENSE).
