---
type: Wiki Page
title: Install on Azure
description: Run the Commonplace container on Azure App Service.
timestamp: '2026-07-19T10:00:54.000Z'
---

On Azure, run Commonplace as a container on App Service. The app is stateless, so no database or storage account is needed.

## Steps

1. Create a **Web App** in the Azure Portal:
   * **Publish**: Container
   * **Image source**: Docker Hub, image `commonplacewiki/commonplace`
   * Any Linux plan works; the smallest tiers are sufficient for a team wiki.
2. Under **Settings → Environment variables**, add:

   | Name | Value |
   | --- | --- |
   | `GIT_REPO` | URL of your [wiki repository](/Installation/git-repository.md) |
   | `GITHUB_CLIENT_ID` | from your [GitHub App](/Installation/install-github-app.md) |
   | `GITHUB_CLIENT_SECRET` | from your GitHub App |
   | `SESSION_SECRET` | output of `openssl rand -hex 32` |
   | `WEBSITES_PORT` | `3000` |

3. Restart the Web App and note its URL (e.g. `https://your-wiki.azurewebsites.net`).
4. Update the GitHub App: **Homepage URL** = the Web App URL, **Callback URL** = `https://your-wiki.azurewebsites.net/api/auth/callback`.
5. Open the URL and sign in with GitHub.

## Equivalent CLI setup

```bash
az webapp create \
  --resource-group my-rg --plan my-plan --name my-wiki \
  --container-image-name commonplacewiki/commonplace

az webapp config appsettings set \
  --resource-group my-rg --name my-wiki \
  --settings GIT_REPO=https://github.com/owner/wiki \
             GITHUB_CLIENT_ID=... \
             GITHUB_CLIENT_SECRET=... \
             SESSION_SECRET=$(openssl rand -hex 32) \
             WEBSITES_PORT=3000
```

## Secrets

For production, store `GITHUB_CLIENT_SECRET` and `SESSION_SECRET` in Azure Key Vault and reference them from the app settings instead of pasting the values directly.
