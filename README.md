# ChatRM — YumaPlayer Chat Server

Backend independente do Together para o chat do YumaPlayer, com cadastro, login, conversas privadas e em grupo, histórico PostgreSQL e WebSocket.

## Deploy no Render Free, sem Blueprint

Este repositório não precisa de Blueprint e não cria banco no Render.

1. No Render, escolha **New → Web Service**.
2. Conecte este repositório e a branch `main`.
3. Use estas configurações:

```text
Name: yumaplayer-chat
Runtime: Docker
Root Directory: vazio
Dockerfile Path: ./Dockerfile
Docker Context: .
Instance Type: Free
Health Check Path: /health
```

4. Não adicione PostgreSQL, Redis, Key Value ou qualquer outro recurso.
5. Em **Environment**, cadastre as variáveis listadas abaixo usando os valores privados do Neon.

O banco persistente é o PostgreSQL do Neon. O Render hospeda apenas o servidor Node/WebSocket.

## Variáveis do Neon

```text
DATABASE_URL
JWT_SECRET
CORS_ORIGIN
NEON_AUTH_BASE_URL
NEON_AUTH_JWKS_URL
AWS_ENDPOINT_URL_S3
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_REGION
NEON_OBJECT_STORAGE_BUCKET
NEON_FUNCTION_HOST_SUFFIX
NEON_BRANCH_ID
NEON_REALTIME_URL
NEON_REALTIME_SECRET
```

`JWT_SECRET` precisa ter pelo menos 32 caracteres. Nunca publique `.env.local`, API keys ou senhas no GitHub. O arquivo `.env.example` contém apenas nomes e valores de exemplo.

## Teste após o deploy

```text
https://yumaplayer-chat.onrender.com/health
```

O serviço Free pode dormir após um período sem tráfego e levar aproximadamente um minuto para acordar. Isso não apaga o histórico, pois as mensagens ficam no Neon.

Leia o [tutorial detalhado](TUTORIAL-RENDER-FREE.md) para o passo a passo e o teste de cadastro.
