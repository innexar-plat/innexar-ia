# OpenClaw no Innexar (Coolify / Docker)

Configuração para rodar o Gateway OpenClaw integrado ao workspace Innexar (API: api.innexar.com.br, App: app.innexar.com.br).

## 1. Variáveis de ambiente (Coolify ou `.env`)

Crie `.env` (não commitar; já está no `.gitignore`) com:

```env
OPENCLAW_GATEWAY_TOKEN=<gere com: openssl rand -hex 32>
```

Opcional: chaves de provedores de modelo (OpenAI, Anthropic, etc.) conforme `.env.example`.

## 2. Config (Control UI)

O arquivo `openclaw.json.example` já está ajustado para o workspace:

- `gateway.controlUi.basePath`: `/api/workspace/openclaw-ui`
- `gateway.controlUi.allowedOrigins`: `https://app.innexar.com.br`, `https://api.innexar.com.br`

O `docker-compose.yml` monta esse arquivo em `/data/openclaw.json` no container. Se quiser usar um arquivo próprio (por exemplo com mais opções), copie `openclaw.json.example` para `openclaw.json`, edite e no `docker-compose.yml` troque o volume para `./openclaw.json:/data/openclaw.json:ro`.

## 3. Backend do workspace

No ambiente do **backend** (innexar-workspace), defina:

```env
OPENCLAW_GATEWAY_URL=http://<host-do-openclaw>:18789
OPENCLAW_GATEWAY_WS_URL=ws://<host-do-openclaw>:18789/ws
```

Em Coolify, se o OpenClaw e o backend estiverem na mesma rede, use o nome do serviço como host (ex.: `http://openclaw:18789`).

## 4. Subir

```bash
cp openclaw.json.example openclaw.json   # opcional, só se for customizar
# Crie .env com OPENCLAW_GATEWAY_TOKEN
docker compose up -d
```

No Coolify: use este repositório como fonte do projeto, configure a variável `OPENCLAW_GATEWAY_TOKEN` e faça o deploy.
