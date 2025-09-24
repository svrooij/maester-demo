---
layout: image-right
theme: quantum
image: .demo/slides/app-registration.png
---

# 1. App registration

1. Open Entra Portal
1. Create new App Registration
1. Add API permissions (see website)
1. Add Federated credentials
1. Note Application (client) ID
1. Note Directory (tenant) ID

---
layout: image
image: .demo/slides/permissions-1.png
transition: slideUp
title: Permissions
---


---
layout: image
image: .demo/slides/permissions-2.png
transition: slideUp
title: Admin consent 🥇
---

---
layout: image
image: .demo/slides/federated-credentials.png
transition: slideUp
title: Federated credentials
---

---
layout: image-right
image: .demo/slides/add-federated-credentials.png
transition: slideUp
title: Federated credentials
---

# Federated credentials

1. Add new credential
1. Select GitHub Actions
1. Organization => your github user/org
1. Repository => your repo
1. branch => main (or your branch)
1. Subject (generated) => repo:your-user/your-repo:ref:refs/heads/main

---
layout: two-columns
transition: slideUp
title: Create workflow file
---

# Configure Github Actions

1. Create `.github/workflows/maester.yml`
1. Copy content from [maester.dev](https://maester.dev/docs/monitoring/github)
   Monitoring -> GitHub


::right::

## When to run

```yml
name: Daily tests 🧪

on:
  push:
    branches: ["main"]
  # Run once a day at midnight
  schedule:
    - cron: "30 7 * * 1"
  # Allows to run this workflow manually from the Actions tab
  workflow_dispatch:

permissions:
      id-token: write
      contents: read
      checks: write
...
```

---
layout: two-columns
transition: slideUp
title: Create workflow file
---

# Configure Github Actions

1. Create `.github/workflows/maester.yml`
1. Copy content from [maester.dev](https://maester.dev/docs/monitoring/github)
   Monitoring -> GitHub


::right::

## Do what

```yml
...
jobs:
  run-maester-tests:
    name: Maester 🔥
    runs-on: ubuntu-latest
    steps:
    - name: 🔥Run Maester 
      id: maester-action
      uses: maester365/maester-action@v1.0.1
      with:
        client_id: ${{ secrets.AZURE_CLIENT_ID }}
        tenant_id: ${{ secrets.AZURE_TENANT_ID }}
        include_public_tests: true
        include_private_tests: false
        include_exchange: false
        include_teams: false
...
```

---
layout: image-right
image: .demo/slides/github-action-secrets.png
transition: slideUp
---

# Configure Github Actions 

1. Create `.github/workflows/maester.yml`
1. Copy content from [maester.dev](https://maester.dev/docs/monitoring/github)
   Monitoring -> GitHub
1. Add Actions-secrets to your repo
   - `AZURE_CLIENT_ID` = Application (client) ID
   - `AZURE_TENANT_ID` = Directory (tenant) ID

---
layout: section
transition: slideUp
---

# Demo time 🧑‍💻