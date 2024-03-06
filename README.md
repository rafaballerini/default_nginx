# Configuração do arquivo default para NGINX do vídeo sobre APIs em VPS do Youtube
Link do vídeo: https://youtu.be/pIvHp7sk2UA

Como seu arquivo deve parecer após as configurações com NGINX e Certbot: 
```
server {
    server_name apicafes.online www.apicafes.online; # Adicionar o seu domínio, tanto sozinho quanto com www.
    location / {
        proxy_pass http://localhost:3000; # Supondo que sua API esteja rodando na porta 3000
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
}
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/apicafes.online/fullchain.pem; # mana>
    ssl_certificate_key /etc/letsencrypt/live/apicafes.online/privkey.pem; # ma>
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}
server {
    if ($host = www.apicafes.online) {
        return 301 https://$host$request_uri;
    } # managed by Certbot
    if ($host = apicafes.online) {
        return 301 https://$host$request_uri;
    } # managed by Certbot
    listen 80;
    server_name apicafes.online www.apicafes.online;
    return 404; # managed by Certbot
}
```
