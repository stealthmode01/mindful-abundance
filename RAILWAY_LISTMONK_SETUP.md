# Listmonk on Railway

This repository deploys the official Listmonk container alongside Railway PostgreSQL. The app service uses Railway's private network for database access. Do not expose PostgreSQL publicly.

## Railway project setup

1. In the existing Railway project, add a PostgreSQL service if one is not already present.
2. Add a service using the public Docker image `listmonk/listmonk:latest`.
3. In the Listmonk service, add these variables. Replace `<Postgres>` with the exact Railway PostgreSQL service name shown in the variable reference picker.

```env
LISTMONK_app__address=0.0.0.0:9000
LISTMONK_db__user=${{Postgres.PGUSER}}
LISTMONK_db__password=${{Postgres.PGPASSWORD}}
LISTMONK_db__database=${{Postgres.PGDATABASE}}
LISTMONK_db__host=${{Postgres.PGHOST}}
LISTMONK_db__port=${{Postgres.PGPORT}}
LISTMONK_db__ssl_mode=disable
LISTMONK_db__max_open=25
LISTMONK_db__max_idle=25
LISTMONK_db__max_lifetime=300s
TZ=America/Los_Angeles
```

Use the Railway variable picker for the Postgres references so they match the actual database service name. Keep the database private.

4. Set the Listmonk service start command to:

```sh
sh -c './listmonk --install --idempotent --yes --config "" && ./listmonk --upgrade --yes --config "" && ./listmonk --config ""'
```

This follows the official Listmonk container startup pattern: install the schema on a new database, apply migrations, then run the app.

5. Set the service's networking target port to `9000`, then generate a public domain for the Listmonk app.
6. Add a Railway volume to the Listmonk service mounted at `/listmonk/uploads` if you plan to upload media. In Listmonk Admin → Settings → Media, set the media path to `/listmonk/uploads`.
7. Deploy and inspect logs. Open the generated domain and create the initial admin account in the web interface. Do not commit admin credentials or database secrets to this repository.

## Email delivery

Listmonk does not provide email delivery by itself. Configure an SMTP provider in Admin → Settings → SMTP after the app is running. Send a test email before importing subscribers or launching a campaign. Configure your sending domain's SPF, DKIM, and DMARC records with the email provider.

## Safety notes

- Use a strong, unique admin password and enable any available account protections.
- Keep PostgreSQL private; only the Listmonk service needs database access.
- Back up PostgreSQL before upgrades or major campaign work.
- Confirm subscriber consent and suppression rules before importing or sending.
- The `latest` image can update to a new release. For predictable production upgrades, pin a reviewed Listmonk version and back up the database first.

## References

- Official Listmonk Docker setup: https://github.com/knadh/listmonk/blob/master/docker-compose.yml
- Railway Docker image services: https://docs.railway.com/services
- Railway PostgreSQL: https://docs.railway.com/databases/postgresql
- Railway volumes: https://docs.railway.com/volumes
