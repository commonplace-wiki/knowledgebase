---
type: Wiki Page
title: Install on Kubernetes
description: Plain manifests to run Commonplace on any Kubernetes cluster.
timestamp: '2026-07-19T10:00:54.000Z'
---

Commonplace runs as a single stateless Deployment. No volumes or databases are required; all state lives in the [Git repository](/Installation/git-repository.md).

## Manifests

Create a Secret with the credentials:

```bash
kubectl create secret generic commonplace \
  --from-literal=GITHUB_CLIENT_ID=... \
  --from-literal=GITHUB_CLIENT_SECRET=... \
  --from-literal=SESSION_SECRET=$(openssl rand -hex 32)
```

Then apply the Deployment and Service:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: commonplace
spec:
  replicas: 1
  selector:
    matchLabels:
      app: commonplace
  template:
    metadata:
      labels:
        app: commonplace
    spec:
      containers:
        - name: commonplace
          image: commonplacewiki/commonplace
          ports:
            - containerPort: 3000
          env:
            - name: GIT_REPO
              value: https://github.com/owner/wiki
          envFrom:
            - secretRef:
                name: commonplace
---
apiVersion: v1
kind: Service
metadata:
  name: commonplace
spec:
  selector:
    app: commonplace
  ports:
    - port: 80
      targetPort: 3000
```

Expose the Service with your usual Ingress or Gateway setup and TLS termination.

## GitHub App

Once the public URL is known, set the GitHub App's **Homepage URL** to it and the **Callback URL** to `https://<your-host>/api/auth/callback`. See [Install GitHub App](/Installation/install-github-app.md).

## Notes

* The app is stateless, so multiple replicas are fine behind a standard load balancer.
* Pin the image to a release tag instead of `latest` if you want controlled upgrades.
