# Cloud SQL Proxy on Fly.io

This repository allows you to provision a Cloud SQL Proxy on Fly.io as a private service, enabling secure access to your Google Cloud SQL instances without the need for public IP addresses.

## Prerequisites

Before you start, ensure you have the following:

- A Google Cloud Platform account with billing enabled.
- A Cloud SQL instance created on Google Cloud Platform.
- A service account with the appropriate permissions for accessing your Cloud SQL instance, i.e. `roles/cloudsql.client`
- A service account key in JSON format
- The fly CLI installed on your machine. Visit [Fly.io documentation](https://fly.io/docs/hands-on/install-flyctl) for installation instructions.

## Setup

Start by cloning this repository to your local machine and navigate into the directory:

```sh
git clone https://github.com/gordalina/fly-cloud-sql-proxy.git fly-cloud-sql-proxy
cd fly-cloud-sql-proxy
```

Use the fly CLI to create a new application on Fly.io. This step copies the existing fly.toml configuration but does not deploy the app yet:

```sh
fly launch --copy-config --no-deploy
```

Encode your Google Cloud service account key file to base64 and set it as a secret in your Fly.io application:

```sh
fly secrets set "GOOGLE_APPLICATION_CREDENTIALS_JSON=$(base64 < service_account_key.json)"
```

Update the `fly.toml` configuration file to include the connection name of your Cloud SQL instance. You can add multiple instances by incrementing the suffix number (_0, _1, etc.):

```toml
CSQL_PROXY_INSTANCE_CONNECTION_NAME_0 = 'project:region:instance?port=5432'
```

Finally, deploy your Cloud SQL Proxy on Fly.io:

```sh
fly deploy
```

Ensure your proxy service remains private by releasing any public IPs associated with it:

```sh
fly ips list
fly ips release <ip_address>
```

## Connecting to your SQL instance

With the setup complete, your services within the same Fly.io organization can connect to your Cloud SQL instance. Ensure your application connects without SSL as the proxy provides secure access:

```
postgres://user:pwd@your-app-name.internal:port/dbname?ssl=false
```

## Additional configuration

All command line flags of Cloud SQL Proxy can be injected via environmental variables with the prefix `CSQL_PROXY_`.
To get a list of all flags run:

```sh
docker run --rm gcr.io/cloud-sql-connectors/cloud-sql-proxy --help
```

## Contributing

Contributions are welcome! Feel free to open issues or submit pull requests to improve the documentation or functionality of this project.

## License

This project is open source and available under the MIT License.
