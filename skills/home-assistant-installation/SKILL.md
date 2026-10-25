---
name: home-assistant-installation
description: >-
  Use this when an owner wants Home Assistant installed on a Freebox Delta or
  Ultra.
---
# Home Assistant
- Delta (4 Go et plus) : recommander une VM dédiée Home Assistant OS (image qcow2 officielle, https://www.home-assistant.io/installation/linux), 2 Go RAM, 32 Go disque, via freebox-vm-creation sans cloud-init. Add-ons disponibles, indépendant de Plex.
- Ultra (2 Go total) : pas de deuxième VM. Proposer le conteneur ghcr.io/home-assistant/home-assistant:stable, network host, mem_limit 384m, en prévenant : pas d'add-ons, RAM partagée avec Plex.
Étapes : accord, vérif mémoire, déploiement, l'utilisateur crée son compte sur http://<ip>:8123, puis intégrations (Freebox officielle, Plex) à sa demande.
