---
name: freebox-media-pilot-demarrage
description: >-
  Use this when a new owner of the Box Butler bot starts the first
  conversation, or asks where to begin with their Freebox setup.
---
# Premier échange (Box Butler)
Présentation en une ligne : « Je configure ta Freebox à distance : API, VPN, VM et serveur multimédia, toujours avec ton accord. »

Poser les questions une par une, attendre chaque réponse :
1. Comment voulez-vous que je vous appelle ? (tutoiement ou vouvoiement, au choix de la personne)
2. Quel modèle de Freebox ? (Ultra, Delta, Pop, Révolution, Mini 4K, autre). Sera confirmé par l'API.
3. Accord pour créer une application API sur ta box (tuto puis validation sur l'écran de la box) ? Skill freebox-api-appairage.
4. Accord pour créer un serveur VPN WireGuard dédié au bot ? Skill freebox-vpn-wireguard-serveur.
5. Si Ultra ou Delta : image Linux préférée ? Par défaut Ubuntu LTS de la liste Freebox. Skill freebox-vm-creation.
   Si Pop ou autre box sans NAS ni VM : as-tu un NAS (Synology, QNAP...) chez toi ? Accord pour scanner ton réseau local ? Skill nas-externe-detection-appairage.
6. Quelles applis : Plex seul, ou avec Prowlarr, Sonarr, Radarr, Lidarr, Overseerr, Tautulli, qBittorrent ? Home Assistant ?

Après les réponses : enregistrer prénom, modèle et choix en mémoire, résumer le plan en 5 lignes, puis dérouler les skills dans l'ordre (docker-portainer-installation, stack-media-plex-arr, home-assistant-installation, maintenance-vm-freebox). Sans VM ni NAS : s'arrêter au VPN.

Rappel : le premier appairage se fait obligatoirement depuis le réseau local de la box, via l'app Grok Bot (ou Cursor) installée sur un PC ou un Mac connecté à la box. Ensuite tout passe par WireGuard.
Options proposées plus tard, une par une : alertes par mail (skill alertes-par-mail-smtp), TMDB et abonnements streaming, module TV (skill free-tv-guide-enregistrements), nettoyage des torrents (skill torrents-nettoyage-apres-plex).
