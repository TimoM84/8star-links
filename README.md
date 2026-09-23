# 8star Links

A lightweight, fully self-hosted bookmark manager with nested folders, drag-and-drop sorting, Firefox import, and locally cached website icons.

Built with Node.js, Express, and SQLite.

## Features

- Unlimited nested folders
- Drag-and-drop sorting for bookmarks and folders
- Collapsible folders and instant search
- Custom folder colours
- Automatic website icons with a letter fallback
- Website icons cached locally as PNG files
- Firefox/Netscape HTML import and export
- Duplicate detection during import
- JSON backup and restore
- Persistent SQLite storage
- Responsive desktop and mobile interface
- Docker and Portainer support
- No external database required

## Quick start

Create a `compose.yml` file:

```yaml
services:
  8star-links:
    image: ghcr.io/timom84/8star-links:latest
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

Start the application:

```bash
docker compose up -d
```

Open `http://YOUR-SERVER-IP:3080`.

## Portainer

1. Open **Stacks** and select **Add stack**.
2. Paste the Compose configuration from the Quick start section.
3. Select **Deploy the stack**.
4. Open `http://YOUR-SERVER-IP:3080`.

The named volume `8star-links-data` keeps the database and cached website icons persistent when the container is replaced or updated.

## Build from source

```bash
git clone https://github.com/TimoM84/8star-links.git
cd 8star-links
docker compose up -d --build
```

## Import from Firefox

1. Export your Firefox bookmarks as an HTML file.
2. Open the settings menu in the upper-left corner of 8star Links.
3. Select **Import from Firefox**.
4. Choose the exported HTML file.

The folder structure is preserved, existing bookmarks are skipped, and duplicate URLs are removed.

## Data and backups

Application data is stored in `/app/data` inside the container:

- `links.sqlite` contains bookmarks, folders, colours, and ordering.
- `favicons/` contains locally cached PNG website icons.

The web interface can download and restore a JSON backup. Creating a backup before major updates is recommended.

Do not remove the Docker volume if you want to keep your data.

## Updating

```bash
docker compose pull
docker compose up -d
```

## Security

8star Links intentionally does not include user accounts or authentication. Do not expose it directly to the public internet without protection such as a VPN, an authenticated reverse proxy, Authentik, Authelia, or an access list.

## License

8star Links is available under the [MIT License](LICENSE).
