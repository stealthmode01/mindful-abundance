# Listmonk on Railway

The Railway project `mindful-abundance` is deployed with the official Railway Listmonk template. It uses a private Railway PostgreSQL service and includes a persistent volume mounted at `/listmonk/uploads`.

## Current production state

- Listmonk deployment: successful
- Railway URL: https://listmonk-production-8e7e.up.railway.app
- PostgreSQL: connected over Railway private networking; do not expose it publicly
- Upload volume: mounted at `/listmonk/uploads`
- Listmonk listens on all interfaces at port 9000 (the bind address was corrected in Railway variables)
- SMTP: still has a placeholder configuration and must be replaced with a real sending provider before campaigns can send

The initial install completed and the database schema is current. The deployment logs report that Listmonk has no superadmin user created yet. Open the Railway URL and follow the initial account setup/login flow. Then use Admin → Settings → SMTP to enter provider credentials and send a test message.

Do not paste admin or database credentials into this repository or into chat. Store them in Railway variables or Listmonk's protected settings.

## References

- Railway service and deployment docs: https://docs.railway.com/services
- Railway PostgreSQL and private connections: https://docs.railway.com/databases/postgresql
- Railway persistent volumes: https://docs.railway.com/volumes
- Listmonk official Docker setup: https://github.com/knadh/listmonk/blob/master/docker-compose.yml
