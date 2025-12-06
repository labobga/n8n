### directory structure
```sh
mkdir -p website/nginx/conf.d/ && cd website
```

### docker-compose file
```sh
sudo cat << 'EOF' > docker-compose.yml
services:
  website:
    image: nginx:latest
    container_name: website
    ports:
      - "8090:80"
    volumes:
      # Monte votre index.html local dans le répertoire html de Nginx
      - ./index.html:/usr/share/nginx/html/index.html:ro
      # Monte votre fichier de configuration Nginx local dans le répertoire de configuration de Nginx
      - ./nginx/conf.d/default.conf:/etc/nginx/conf.d/default.conf:ro
EOF
```

### index.html file
```sh
sudo cat << 'EOF' > index.html
<h1>netbga website test n8n</h1>
<p>Ceci est le site de test via docker pour tester n8n</p>
EOF
```

### index.html file
```sh
sudo cat << 'EOF' >  nginx/conf.d/default.conf
server {
    listen 80;
    server_name localhost;

    location / {
        # Pointe vers le répertoire où nous allons monter nos fichiers
        root /usr/share/nginx/html;
        index index.html;
    }
}
EOF
```
