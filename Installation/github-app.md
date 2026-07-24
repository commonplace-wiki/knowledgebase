---
type: Wiki Page
title: GitHub App
description: Create a GitHub App for sign-in and grant it access to the wiki repository.
timestamp: '2026-07-24T07:02:48.407Z'
---

Commonplace uses a GitHub App for two things: users sign in with their GitHub account, and the app reads and writes the wiki repository on their behalf.

## Create the App

The easiest way is the guided setup: when you start Commonplace without `GITHUB_CLIENT_ID` set, the setup screen generates a GitHub App with the correct settings for you.

To create it manually instead, go to _https://github.com/organizations/<YOUR_ORGANIZATION>/settings/apps_, add a new GitHub App, and configure:

* **Homepage URL**: the URL of your Commonplace instance, e.g. [`https://wiki.example.com`](https://wiki.example.com) (or `http://localhost:3000` for local development).
* **Callback URL**: `<your instance URL>/api/auth/callback`
* **Repository permissions**: Contents → Read and write.
* **Where can this app be installed**: your account or organization is enough; there is no need to make it public.

![Create GitHub App form with homepage and callback URL](/assets/Screenshot-2026-07-19-at-11.09.43.png)

Under repository permissions, grant Contents read and write:

![Contents permission set to Read and write](/assets/Screenshot-2026-07-19-at-11.09.57.png)

After creating the App, generate a client secret and set both values as environment variables:

```
GITHUB_CLIENT_ID=...
GITHUB_CLIENT_SECRET=...
```

## Install the App

Now select the organization and repository that should store the data (create a new repository first, if you don't have one — see [Git Repository](/Installation/git-repository.md)).

Install the GitHub App for this repository with read and write permissions.

![](/assets/Screenshot-2026-07-19-at-10.28.56.png)

## Verify

Start Commonplace and open it in the browser. You should be redirected to GitHub to sign in, and after authorizing you should see the wiki homepage. If sign-in loops or fails, check that the callback URL exactly matches your instance URL, including the scheme.
