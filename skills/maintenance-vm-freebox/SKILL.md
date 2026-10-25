---
name: maintenance-vm-freebox
description: >-
  Use this when checking health, updating, or rebooting a Freebox VM, its
  containers or the box itself, or when an update or alert mail arrives in the
  bot inbox.
---
# Maintenance (sans routine planifiée)
Principe : pas de vérification hebdomadaire. Chaque service envoie lui-même ses alertes par mail à l'adresse mail du bot (une variante +tag par service, ex. bot+plex@, bot+ubuntu@), et le mail sert de déclencheur.

Sources d'alertes à configurer avec l'utilisateur (SMTP de son fournisseur, mot de passe dans un secret) :
- Plex et serveur : Tautulli, Notification Agents, Email (mise à jour Plex disponible, serveur injoignable, nouveaux médias).
- Sonarr, Radarr, Lidarr, Prowlarr : Settings, Connect, Email (événement Application Update, Health Issue).
- Ubuntu : apticron (mail quotidien seulement s'il y a des paquets à mettre à jour) ou unattended-upgrades pour la sécurité avec Unattended-Upgrade::Mail, via msmtp.
- Conteneurs sans notification propre (Portainer, Overseerr, qBittorrent, Home Assistant) : conteneur léger Diun (crazymax/diun, mem_limit 64m), qui surveille les nouvelles images et envoie un mail. Portainer CE n'a pas d'alerte de mise à jour intégrée.
- Home Assistant : notification de mise à jour intégrée, relayée par mail via l'intégration SMTP.
- Freebox : les mises à jour de Freebox OS sont automatiques ; vérifier la version par l'API seulement en cas de souci.

À la réception d'un mail : résumer à l'utilisateur ce qui est disponible, appliquer après accord, une stack à la fois, avec vérif mémoire et image précédente gardée pour revenir en arrière.
Avant un redémarrage (box, VM, Docker) : annoncer ce qui sera coupé (Plex, Home Assistant, VPN du bot, téléchargements), demander accord ; après, vérifier que tunnel, VM et conteneurs reviennent.
Redémarrage VM : POST /api/vN/vm/{id}/restart. Box : POST /api/vN/system/reboot/, seulement sur demande explicite.
Torrents : voir la skill torrents-nettoyage-apres-plex.
Nettoyage : docker image prune après accord.
