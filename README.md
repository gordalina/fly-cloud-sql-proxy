# Cloud SQL Proxy on Fly.io

This repository provisions Cloud SQL Proxy on fly.io as a private service

## Setup

Create an app out of this template.

```sh
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

And allocate a private IPv6.

```sh
fly ips allocate-v6 --private
```
