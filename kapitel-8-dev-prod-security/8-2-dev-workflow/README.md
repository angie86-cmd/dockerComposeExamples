# Übung 8.2 – Docker Development and Production Workflow

This example demonstrates a Docker-based development workflow with:

- environment-specific configuration
- PostgreSQL persistence
- database migration service
- automated test service
- Docker image tagging via environment variable
- branch-friendly resource configuration

The Compose file uses environment variables such as `NODE_ENV`, `TAG`, `PORT`, `DB_USER`, `DB_PASSWORD`, `DB_NAME` and `DB_VOLUME` to support different development, test and production scenarios.