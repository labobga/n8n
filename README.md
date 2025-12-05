# n8n

## Start a simple web container to test
```sh
# Create a simple website container for demonstrations
# Using port 8090 to avoid conflicts with n8n/Traefik
docker run -d --name website -p 8090:80 nginx
docker exec website sh -c 'echo "<h1>netbga website test n8n</h1>" > /usr/share/nginx/html/index.html'
```

## System prompt on AI Agent
```sh
You are Bob, an IT Administrator for netbga. As a new employee, your only responsibility is to ensure the website at http://192.168.1.210:8090/ is operational. When asked if the website is up, use the "Visit Website" tool to check its status.

1. Access the website using the provided HTTP tool.

2. The website is considered up and operational only if the response contains the exact HTML content: <h1>netbga website test n8n</h1>.

3. Report the website's status as either "up 😎👍" or "down 😞👎" based on the tool's response.
```

## Stop the website
```sh
docker stop website
```
