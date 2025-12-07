### directory structure
```sh
mkdir -p mattermost && cd mattermost
```

### docker-compose file
```sh
sudo cat << 'EOF' > docker-compose.yml
services:
  # ----------------------------------------
  # SERVICE MATTERMOST
  # ----------------------------------------
  mattermost:
    image: mattermost/mattermost-team-edition:latest
    container_name: mattermost
    restart: unless-stopped
    depends_on:
      - postgres
    environment:
      # Connexion à la base de données (utilise les variables du fichier .env)
      - MM_SQLSETTINGS_DRIVERNAME=postgres
      - MM_SQLSETTINGS_DATASOURCE=postgres://${POSTGRES_USER}:${POSTGRES_PASSWORD}@postgres:5432/${POSTGRES_DB}?sslmode>
      # URL du site (important pour les webhooks et l'intégration)
      - MM_SERVICESETTINGS_SITEURL=${MATTERMOST_SITE_URL}
      # Options de Bleve (pour la recherche)
      - MM_BLEVESETTINGS_INDEXDIR=/mattermost/bleve-indexes
    ports:
      - "8065:8065"
    volumes:
      # Volumes persistants pour les données de l'application
      - mattermost_config:/mattermost/config:rw
      - mattermost_data:/mattermost/data:rw
      - mattermost_logs:/mattermost/logs:rw
      - mattermost_plugins:/mattermost/plugins:rw
      - mattermost_client_plugins:/mattermost/client/plugins:rw
      - mattermost_bleve-indexes:/mattermost/bleve-indexes:rw
    networks:
      - mattermost-net

  # ----------------------------------------
  # SERVICE BASE DE DONNÉES POSTGRES
  # ----------------------------------------
  postgres:
    image: postgres:15-alpine
    container_name: mattermost-postgres
    restart: unless-stopped
    environment:
      # Variables d'environnement pour l'initialisation de la base de données (utilisées par l'image Postgres)
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=${POSTGRES_DB}
    volumes:
      # Volume persistant pour les données de la base de données
      - postgres_data:/var/lib/postgresql/data
    networks:
      - mattermost-net

# ----------------------------------------
# DÉCLARATION DES VOLUMES ET RÉSEAUX
volumes:
  mattermost_config:
  mattermost_data:
  mattermost_logs:
  mattermost_plugins:
  mattermost_client_plugins:
  mattermost_bleve-indexes:
  postgres_data:

networks:
  mattermost-net:
    driver: bridge
```

### .env file
```sh
sudo cat << 'EOF' > .env
# --- Configuration de Mattermost ---

# L'URL sous laquelle Mattermost sera accessible (ex: https://chat.mondomaine.com ou http://votre_ip:8065)
MATTERMOST_SITE_URL=http://localhost:8065

# --- Configuration de la Base de Données PostgreSQL ---

# Nom d'utilisateur de la base de données Mattermost (à créer)
POSTGRES_USER=mattermost_user

# Mot de passe sécurisé pour l'utilisateur ci-dessus (TRÈS IMPORTANT de le changer !)
POSTGRES_PASSWORD=wxcFTVBGA1!*

# Nom de la base de données
POSTGRES_DB=mattermost_db
```

### commands to create users
```sh
docker exec mattermost mmctl --local roles system_admin labobga@gmail.com

docker exec -it mattermost mmctl --local user create --email email.bernard.garcia@gmail.com --username bga --password rtyFTV1
# check user/password in GUI to user added , password reset
```
