# YumaPlayer Chat no Render Free, sem Blueprint

Este pacote usa o Render somente para hospedar o servidor Node/WebSocket. O banco persistente continua no Neon. Não crie PostgreSQL, Redis, Key Value ou qualquer outro recurso no Render.

## 1. Preparar os arquivos

Extraia `YumaPlayer-render-free.zip` no computador. O pacote não contém `.env.local`, API keys ou senhas.

O Render normalmente publica a partir de um repositório GitHub, e não por upload direto de ZIP. Se a sua tela não aceitar ZIP, extraia o pacote e envie o conteúdo para um repositório GitHub novo ou para `Testando0/Playeryz`.

## 2. Criar o serviço gratuito

1. Abra https://dashboard.render.com/.
2. Clique em **New**.
3. Escolha **Web Service**. Não escolha **Blueprint**.
4. Conecte o GitHub e selecione o repositório que contém o pacote.
5. Configure:

   - **Name:** `yumaplayer-chat`
   - **Region:** qualquer região disponível
   - **Branch:** `main`
   - **Runtime:** `Docker`
   - **Root Directory:** deixe vazio se o `Dockerfile` estiver na raiz do pacote
   - **Dockerfile Path:** `./Dockerfile`
   - **Docker Context:** `.`
   - **Instance Type:** `Free`

6. Não adicione banco de dados.
7. Em **Health Check Path**, use `/health`.
8. Clique em **Create Web Service**.

## 3. Cadastrar as variáveis

Antes ou depois do primeiro deploy, abra **Environment → Add Environment Variable** e cadastre as variáveis do Neon. Os nomes são:

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

Use os valores privados do `.env.local` criado pelo Neon. O valor de `JWT_SECRET` precisa ter pelo menos 32 caracteres. Para `CORS_ORIGIN`, deixe vazio durante o primeiro teste ou informe os domínios autorizados.

Não coloque os valores no GitHub, no APK, em mensagens públicas ou dentro do ZIP. Eles devem ficar somente nas variáveis privadas do serviço Render.

## 4. Verificar o deploy

Depois que o Render terminar o deploy, abra:

```text
https://yumaplayer-chat.onrender.com/health
```

A resposta esperada começa com:

```json
{"ok":true,"service":"yumaplayer-chat"
```

Se aparecer `503`, revise `DATABASE_URL` e `JWT_SECRET`. Se aparecer `404`, o serviço ainda não foi criado com o nome `yumaplayer-chat` ou está usando outro domínio.

## 5. Testar cadastro e login

```bash
curl -i -X POST https://yumaplayer-chat.onrender.com/v1/auth/register \
  -H 'Content-Type: application/json' \
  -d '{"username":"teste123","displayName":"Teste","password":"senha-forte-123"}'
```

Depois, use o APK `YumaPlayer-v1.2.0-RC.4`. Ele já aponta para `https://yumaplayer-chat.onrender.com`.

## Observação importante sobre o plano Free

O Web Service Free pode dormir quando fica sem tráfego e pode levar aproximadamente um minuto para acordar. Isso não apaga as mensagens, porque o histórico fica no Neon. O Render Free é adequado para teste e uso pessoal; limites de tráfego e suspensão podem ser aplicados pela plataforma.

Documentação oficial: https://render.com/docs/free
