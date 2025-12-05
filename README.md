# n8n

## Start a simple web container to test
```sh
# Create a simple website container for demonstrations
# Using port 8090 to avoid conflicts with n8n/Traefik
docker run -d --name website -p 8090:80 nginx
docker exec website sh -c 'echo "<h1>netbga website test n8n</h1>" > /usr/share/nginx/html/index.html'
```
