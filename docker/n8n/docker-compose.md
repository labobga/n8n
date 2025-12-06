1- docker-compose.yml \
2- fichier .env \
3- fichier configuration nginx \
4- générer certificats \
5- Appliquer droits sur sous répertoires, correspondants aux volumes

## 1- docker-compose.yml
```sh
mkdir -p docker/n8n && cd docker/n8n
```
```sh
sudo cat << 'EOF' > docker-compose.yml
services:
  n8n:
    image: n8nio/n8n:latest
    container_name: n8n
    restart: unless-stopped
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=${POSTGRES_DB}
      - DB_POSTGRESDB_USER=${POSTGRES_USER}
      - DB_POSTGRESDB_PASSWORD=${POSTGRES_PASSWORD}
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=${N8N_BASIC_AUTH_USER}
      - N8N_BASIC_AUTH_PASSWORD=${N8N_BASIC_AUTH_PASSWORD}
      - N8N_HOST=${N8N_HOST}
      - WEBHOOK_URL=https://${N8N_HOST}
      - N8N_PROTOCOL=https
      - GENERIC_TIMEZONE=${GENERIC_TIMEZONE}
    volumes:
      - ./n8n_data:/home/node/.n8n
    networks:
      - n8n-network
    depends_on:
      - postgres

  postgres:
    image: postgres:15-alpine
    container_name: postgres
    restart: unless-stopped
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    volumes:
      - ./postgres_data:/var/lib/postgresql/data
    networks:
      - n8n-network

  nginx:
    image: nginx:latest
    container_name: nginx
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/conf.d:/etc/nginx/conf.d:ro
      - ./nginx/certs:/etc/nginx/certs:ro
    networks:
      - n8n-network

networks:
  n8n-network:
    driver: bridge
EOF
```

## 2- fichier .env
```sh
sudo cat << 'EOF' > .env
POSTGRES_DB=n8n
POSTGRES_USER=n8nuser
POSTGRES_PASSWORD=rtyftvbga1!*
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=rtyftvbga1!*
N8N_HOST=n8n.netbga.org
GENERIC_TIMEZONE=Europe/Paris
EOF
```

## 3- fichier configuration nginx
```sh
mkdir -p nginx/conf.d
sudo cat << 'EOF' >  ./nginx/conf.d/n8n.conf
server {
    listen 80;
    server_name n8n.netbga.org;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name n8n.example.com;

    ssl_certificate /etc/nginx/certs/fullchain.pem;
    ssl_certificate_key /etc/nginx/certs/privkey.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;
    ssl_ciphers HIGH:!aNULL:!MD5;

    location / {
        proxy_pass http://n8n:5678;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 3600s;
    }
}
EOF
```

## 4- générer certificats
```sh
mkdir -p ./nginx/certs
openssl req -x509 -nodes -days 730 -newkey rsa:2048 -keyout ./nginx/certs/privkey.pem -out ./nginx/certs/fullchain.pem -subj "/C=FR/ST=IDF/L=Paris/O=Labobga/OU=Labo/CN=netbga.org"
```

## 5- changer droits sur sous-répertoires 
```sh
sudo chown -R 1000:1000 n8n_data/
sudo chown -R 1000:1000 postgres_data/
```
