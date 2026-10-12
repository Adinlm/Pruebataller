# Explicit signing key and debug configuration

Set a strong, private `DJANGO_SECRET_KEY` in the environment; the application intentionally refuses to start without it. There is no hardcoded fallback. Generate a new key locally using Python's `secrets.token_urlsafe(64)` and store it in your deployment's environment manager. The sample `.env.example` documents names only; it is not loaded automatically. Never commit a real `.env` file.

For local development set `DJANGO_DEBUG=true` explicitly and `DJANGO_ALLOWED_HOSTS=localhost,127.0.0.1`. Run Django migrations before starting a clean clone. Debug defaults to false. Production must set its actual hosts. Existing database deployments use `DATABASE_URL` through `dj_database_url`; without it, the existing local SQLite configuration remains in use. The `DJANGO_DB_*` variables used by another project do not configure this application.

Before merging/deploying, configure the environment and review whether an application is still active. Changing an active signing key can invalidate sessions, password-reset tokens and other signatures. This source patch does not revoke historical keys or update a running service. Historical values remain in Git and must be rotated externally where still in use.

Local SQLite databases are excluded. No database rows, users, deployments or running sessions were modified.
