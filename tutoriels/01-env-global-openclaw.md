# Tutoriel — Gérer tes tokens API avec `~/.env.global` pour OpenClaw

Ce guide te permet de stocker tes clés API **une seule fois** dans `~/.env.global`, puis de les utiliser partout avec `$NOM_DU_TOKEN`.

## 1) Ajouter tes tokens dans `~/.env.global`

```bash
nano ~/.env.global
```

Exemple :

```env
EXA_MCP_TOKEN=ton_token_exa
BRAVE_API_KEY=ta_cle_brave
OPENAI_API_KEY=ta_cle_openai
```

## 2) Script de chargement (launchd)

```bash
mkdir -p ~/bin
nano ~/bin/load-global-env.sh
```

Colle :

```bash
#!/usr/bin/env bash
set -euo pipefail
ENV_FILE="$HOME/.env.global"
[ -f "$ENV_FILE" ] || exit 0

while IFS='=' read -r key value; do
  [[ -z "${key:-}" || "$key" =~ ^# ]] && continue
  key="$(echo "$key" | xargs)"
  value="${value%\"}"; value="${value#\"}"
  value="${value%\'}"; value="${value#\'}"
  launchctl setenv "$key" "$value"
done < "$ENV_FILE"
```

Puis :

```bash
chmod +x ~/bin/load-global-env.sh
~/bin/load-global-env.sh
openclaw gateway restart
```

## 3) Vérifier

```bash
echo $EXA_MCP_TOKEN
launchctl getenv EXA_MCP_TOKEN
```

## 4) Quand tu ajoutes un nouveau token

```bash
~/bin/load-global-env.sh
openclaw gateway restart
```

## 5) Sécurité

```bash
chmod 600 ~/.env.global
```

Ne partage jamais ce fichier et régénère une clé si elle fuite.
