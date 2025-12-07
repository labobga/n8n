# n8n

https://www.youtube.com/watch?v=budTmdQfXYU

## Access Token Messaging
```sh
8581479525:AAHG0guy0oEqGdkKoT1liPkbs44TGGjltDU
```
Done! Congratulations on your new bot. You will find it at t.me/labobgabot. You can now add a description, about section and profile picture for your bot, see /help for a list of commands. By the way, when you've finished creating your cool bot, ping our Bot Support if you want a better username for it. Just make sure the bot is fully operational before you do this.

Use this token to access the HTTP API: \
8581479525:AAHG0guy0oEqGdkKoT1liPkbs44TGGjltDU \
Keep your token secure and store it safely, it can be used by anyone to control your bot.

For a description of the Bot API, see this page: https://core.telegram.org/bots/api

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
