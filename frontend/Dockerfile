# frontend/Dockerfile

# --- Stufe 1: Build-Umgebung ---
# Verwende ein offizielles Node.js Image als Basis für den Build
FROM node:18-alpine AS builder

# Setze das Arbeitsverzeichnis im Container
WORKDIR /app/frontend

# Kopiere package.json und package-lock.json (oder yarn.lock)
COPY package.json ./
# Wenn du yarn verwendest:
# COPY yarn.lock ./
COPY package-lock.json ./

# Installiere Projekt-Abhängigkeiten
RUN npm install
# Wenn du yarn verwendest:
# RUN yarn install

# Kopiere den Rest des Frontend-Quellcodes
COPY . .

# Baue die Produktions-App
RUN npm run build
# Wenn du yarn verwendest:
# RUN yarn build

# --- Stufe 2: Produktions-Umgebung ---
# Verwende ein leichtes Nginx Image, um die statischen Dateien auszuliefern
FROM nginx:stable-alpine

# Entferne die Standard-Nginx-Konfiguration
RUN rm /etc/nginx/conf.d/default.conf

# Kopiere eine benutzerdefinierte Nginx-Konfiguration (optional, aber empfohlen für SPAs)
COPY nginx.conf /etc/nginx/conf.d/default.conf
# (Erstelle unten die nginx.conf Datei)

# Kopiere die gebauten statischen Assets von der Build-Stufe
COPY --from=builder /app/frontend/dist /usr/share/nginx/html

# Exponiere Port 80 (Standard-HTTP-Port für Nginx)
EXPOSE 80

# Standardbefehl, um Nginx im Vordergrund zu starten, wenn der Container läuft
CMD ["nginx", "-g", "daemon off;"]
