---
layout: image-right
theme: quantum
image: .demo/slides/swa-contributor.png
---

# Publish results to Static Web Apps

1. Create Static Web App
1. Grant service principal `contributor` access to the SWA (there is no "least privilege" role available for this 🤕)
1. Add `SWA_NAME` secret to your Github Actions secrets

---
layout: two-columns
transition: slideUp
---

# Publish results to Static Web Apps

1. Create Static Web App
1. Grant service principal `contributor` role to the SWA
1. Add `SWA_NAME` secret to your Github Actions secrets
4. Add `config/staticwebapp.config.json` file to your repo

::right::

## staticwebapp.config.json

```json
{
  "routes": [
    
    {
      "route": "/.auth/login/github",
      "statusCode": 404
    },
    {
      "route": "/*",
      "allowedRoles": ["admin"]
    }
  ],
  "responseOverrides": {
    "401": {
      "statusCode": 302,
      "redirect": "/.auth/login/aad"
    }
  }
}
```

---
layout: two-columns
transition: slideUp
---

# Publish results to Static Web Apps

1. Create Static Web App
1. Grant service principal `contributor` role to the SWA
1. Add `SWA_NAME` secret to your Github Actions secrets
1. Add `config/staticwebapp.config.json` file to your repo
1. Modify workflow to publish to SWA

::right::

## maester.yml

```yml
...
- name: 📦 Move files
  shell: pwsh
  run: |
    if (-not (Test-Path -Path "dist")) {
      New-Item -ItemType Directory -Path "dist"
    }
    Copy-Item -Path "test-results/*" -Destination "dist" -Recurse -Force
    if (Test-Path -Path "dist/test-results.html") {
      Rename-Item -Path "dist/test-results.html" -NewName "index.html"
    }
...
```

---
layout: two-columns
transition: slideUp
---

# Publish results to Static Web Apps

1. Create Static Web App
1. Grant service principal `contributor` role to the SWA
1. Add `SWA_NAME` secret to your Github Actions secrets
1. Add `config/staticwebapp.config.json` file to your repo
1. Modify workflow to publish to SWA

::right::

## maester.yml

```yml
...
- name: 🚀 Deploy results to SWA
  uses: svrooij/azure-static-web-app-deploy-action@main
  with:
    tenant_id: ${{ secrets.AZURE_TENANT_ID }}
    client_id: ${{ secrets.AZURE_CLIENT_ID }}
    static_web_app_name: ${{ secrets.SWA_NAME }}
    app_location: 'dist'
    api_location: ''
    swa_environment: 'production'
    swa_config_directory: 'config'
```