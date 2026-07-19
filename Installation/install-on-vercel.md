---
type: Wiki Page
title: Install on Vercel
description: Deploy Commonplace to Vercel in a few minutes.
timestamp: '2026-07-19T10:00:54.000Z'
---

Vercel is the fastest way to run a hosted Commonplace instance. The app is stateless, so a standard Vercel deployment is all you need.

## Steps

1. Fork or import [commonplace-wiki/commonplace](https://github.com/commonplace-wiki/commonplace) into your GitHub account.
2. In Vercel, choose **Add New → Project** and import the repository. The defaults work; no build settings need to change.
3. Set the environment variables under **Settings → Environment Variables**:

   | Variable | Value |
   | --- | --- |
   | `GIT_REPO` | URL of your [wiki repository](/Installation/git-repository.md), e.g. `https://github.com/owner/wiki` |
   | `GITHUB_CLIENT_ID` | from your [GitHub App](/Installation/install-github-app.md) |
   | `GITHUB_CLIENT_SECRET` | from your GitHub App |
   | `SESSION_SECRET` | output of `openssl rand -hex 32` |

4. Deploy. Note the production URL (e.g. `https://your-wiki.vercel.app`).
5. Update the GitHub App: set the **Homepage URL** to the production URL and the **Callback URL** to `https://your-wiki.vercel.app/api/auth/callback`.
6. Open the production URL and sign in with GitHub.

## Custom domain

Add your domain under **Settings → Domains** in Vercel, then update the GitHub App's homepage and callback URLs to the new domain. Sign-in breaks silently if the callback URL and the domain you use do not match.

## Updating

Vercel redeploys automatically when the imported repository changes. To pick up new Commonplace releases on a fork, sync the fork with upstream.
