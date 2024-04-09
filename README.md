# TYPEBOT
Typebot oferece blocos poderosos para criar experiências de bate-papo únicas. Incorpore-os em qualquer lugar em seus aplicativos web/móveis e comece a coletar resultados como mágica.

## SETUP
Instale as dependencias que o sistema irá precisar
```bash
sudo apt update && sudo apt upgrade && apt install docker-compose && sudo apt update && sudo apt install nginx && sudo apt update && sudo apt install certbot && sudo apt install python3-certbot-nginx && sudo apt update
```
Clone o repositorio
```bash
git clone https://github.com/RaphaelAntunes/TypeBot-.git
```

Insira o comando para criar uma chave criptografada e salve-a
```bash
openssl rand -base64 24 | tr -d '\n' ; echo
```

## No arquivo docker-compose.yml você encontrará todas as configurações necessárias para o **TypeBot**

Substitua por sua **CHAVE CRIPTOGRAFADA**
```bash
ENCRYPTION_SECRET=SUA-CHAVE
ENCRYPTION_SECRET=LwmuxeotjassjPJ6Vv+/BDaf83tw7uKW
```
Substitua por sua URL de **PRODUÇÃO**
```bash
NEXTAUTH_URL=https://SEU-ENDERECO-IP-AQUI
NEXTAUTH_URL=https://typebot.mydomain.com.br
```
Substitua por sua URL de **VISUALIZAÇÃO**
```bash
NEXT_PUBLIC_VIEWER_URL=https://SEU-ENDERECO-IP-AQUI
NEXT_PUBLIC_VIEWER_URL=https://chat.mydomain.com.br
```
## Iniciando Docker

Inicie o arquivo docker-compose.yml, certifique-se de inserir o comando na mesma pasta do arquivo.
```bash
docker-compose up -d
```

# Configurando o Proxy Reverso com Nginx e Certbot

## Configurando o servidor para typebot.mydomain.com.br
Edite o arquivo de configuração do Nginx:
```bash
cd && sudo nano /etc/nginx/sites-available/typebot
```
Cole a seguinte configuração
```bash
server {
  server_name typebot.mydomain.com.br;

  location / {
    proxy_pass http://127.0.0.1:3001;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_cache_bypass $http_upgrade;
  }
}
```
## Configurando o servidor para chat.mydomain.com.br
Edite o arquivo de configuração do Nginx:
```bash
cd && sudo nano /etc/nginx/sites-available/chat
```
Cole a seguinte configuração
```bash
server {
  server_name chat.mydomain.com.br;

  location / {
    proxy_pass http://127.0.0.1:3002;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_cache_bypass $http_upgrade;
  }
}
```

## Criando o Link de configuração do Nginx

```bash
sudo ln -s /etc/nginx/sites-available/typebot /etc/nginx/sites-enabled
sudo ln -s /etc/nginx/sites-available/chat /etc/nginx/sites-enabled
```
## Configurando certificados SLL com CERTBOT
```bash
sudo certbot --nginx --email email@mydomain.com --redirect --agree-tos -d typebot.mydomain.com. -d chat.mydomain.com
```



