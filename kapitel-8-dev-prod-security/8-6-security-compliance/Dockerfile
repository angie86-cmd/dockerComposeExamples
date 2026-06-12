# Stage 1: Build-Stage
# Verwendet ein vollständiges Node.js-Image, um die Anwendung zu bauen.
FROM node:18 AS build

# Arbeitsverzeichnis im Container.
WORKDIR /app

# Kopiert zuerst nur package-Dateien, damit npm-Abhängigkeiten gecacht werden können.
COPY package*.json ./

# Installiert exakt die in package-lock.json definierten Abhängigkeiten.
RUN npm ci

# Kopiert den restlichen Quellcode in den Build-Container.
COPY . .

# Baut die Anwendung, z. B. TypeScript/React/Vue/Node-Build.
RUN npm run build


# Stage 2: Runtime-Stage
# Schlankeres Alpine-Image für den produktiven Betrieb.
FROM node:18-alpine

# Erstellt einen unprivilegierten Benutzer für mehr Sicherheit.
RUN addgroup -S appuser && adduser -S appuser -G appuser

# Arbeitsverzeichnis im finalen Runtime-Container.
WORKDIR /app

# Kopiert nur die gebauten Artefakte aus der Build-Stage.
# --chown setzt den Besitzer direkt auf appuser.
COPY --from=build --chown=appuser:appuser /app/dist ./dist

# Kopiert package-Dateien, um Produktionsabhängigkeiten installieren zu können.
COPY --from=build --chown=appuser:appuser /app/package*.json ./

# Installiert nur Produktionsabhängigkeiten und löscht den npm-Cache.
RUN npm ci --only=production && npm cache clean --force

# Ab hier läuft der Container nicht mehr als root, sondern als appuser.
USER appuser

# Startbefehl für die Anwendung.
CMD ["node", "dist/server.js"]