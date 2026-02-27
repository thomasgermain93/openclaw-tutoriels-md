# Tutoriel — Configurer les variables Google Ads API pour OpenClaw

Ce guide te montre **où trouver chaque valeur** à mettre dans `~/.env.global`.

## Variables à remplir

```env
GOOGLE_ADS_DEVELOPER_TOKEN=
GOOGLE_ADS_CLIENT_ID=
GOOGLE_ADS_CLIENT_SECRET=
GOOGLE_ADS_REFRESH_TOKEN=
GOOGLE_ADS_LOGIN_CUSTOMER_ID=
GOOGLE_ADS_CUSTOMER_ID=
```

---

## 1) `GOOGLE_ADS_DEVELOPER_TOKEN`

### Où le trouver
Dans ton **compte manager Google Ads (MCC)** :

- Ouvre Google Ads
- Va dans **Admin** (ou **Tools & Settings**) > **API Center**
- Tu verras le **Developer token**

### Notes
- C’est obligatoire pour l’API.
- Son niveau d’accès (test/explorer/basic/standard) limite ce que tu peux faire.

---

## 2) `GOOGLE_ADS_CLIENT_ID` + `GOOGLE_ADS_CLIENT_SECRET`

### Où les créer
Dans **Google Cloud Console** :

1. Crée (ou choisis) un projet GCP
2. Active l’API **Google Ads API**
3. Va dans **APIs & Services > Credentials**
4. Crée un **OAuth 2.0 Client ID** (type Desktop App ou Web App)
5. Copie :
   - Client ID → `GOOGLE_ADS_CLIENT_ID`
   - Client Secret → `GOOGLE_ADS_CLIENT_SECRET`

---

## 3) `GOOGLE_ADS_REFRESH_TOKEN`

### Comment l’obtenir
Il faut faire un flow OAuth une fois, puis récupérer le refresh token.

Le plus simple :
- Utiliser un petit script OAuth (Python) via la lib Google Ads
- Autoriser le compte Google qui a accès aux comptes Ads
- Copier le `refresh_token` retourné

> Important: le compte Google utilisé doit avoir accès au MCC / comptes clients.

---

## 4) `GOOGLE_ADS_LOGIN_CUSTOMER_ID`

C’est l’ID du **MCC** (manager account) utilisé comme compte de connexion API.

- Format attendu: chiffres uniquement, sans tirets
- Exemple visuel Google Ads: `123-456-7890` → mettre `1234567890`

---

## 5) `GOOGLE_ADS_CUSTOMER_ID`

C’est l’ID du **compte client** à piloter.

- Aussi sans tirets
- Tu peux changer cette valeur pour passer d’un client à l’autre

---

## Exemple complet dans `~/.env.global`

```env
# Google Ads API (Hungry Nuggets)
GOOGLE_ADS_DEVELOPER_TOKEN=xxxxxxxxxxxxxxxxxxxxxx
GOOGLE_ADS_CLIENT_ID=1234567890-xxxxx.apps.googleusercontent.com
GOOGLE_ADS_CLIENT_SECRET=GOCSPX-xxxxxxxx
GOOGLE_ADS_REFRESH_TOKEN=1//0gxxxxxxxxxxxxx
GOOGLE_ADS_LOGIN_CUSTOMER_ID=1234567890
GOOGLE_ADS_CUSTOMER_ID=9876543210
```

---

## Appliquer les changements

Après modification de `~/.env.global` :

```bash
~/bin/load-global-env.sh
openclaw gateway restart
```

Puis vérifier:

```bash
python3 /Users/thomas/.openclaw/workspace/skills/google-ads-ops/scripts/google_ads_ops.py env-check
```

Si tout est bon, tu verras: `✅ Environnement Google Ads prêt`
