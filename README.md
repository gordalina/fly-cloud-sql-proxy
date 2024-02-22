# Cloud SQL Proxy on Fly.io

This repository provisions Cloud SQL Proxy on fly.io as a private service

## Setup

Create an app out of this template.

```sh
git clone https://github.com/gordalina/fly-cloud-sql-proxy.git fly-cloud-sql-proxy
cd fly-cloud-sql-proxy

fly config save -a <your-app-name>
fly launch --no-deploy
```

Then we set the service account key secret.

```sh
fly secrets set GOOGLE_APPLICATION_CREDENTIALS_JSON=<base64_of_service_account_key.json>
```

Then we deploy it.

```sh
fly deploy
```

Make sure to release all public IPs.

```sh
fly ips list
fly ips release <ip_address>
```

Now you're ready to connect to it!
Make sure you are not connecting using SSL.
