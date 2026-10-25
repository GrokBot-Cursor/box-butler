---
name: torrents-nettoyage-apres-plex
description: >-
  Use this when setting up or running the automatic cleanup of finished torrents
  once the media is in Plex: stop seeding, pause or block trackers, remove the
  torrent from the client.
---
# Torrents : nettoyage après import dans Plex
Prérequis : qBittorrent (ou downloader Freebox) et Tautulli installés, accord de l'utilisateur sur ses règles (ratio ou durée de partage minimum imposés par ses trackers).

1. Déclencheur : webhook Tautulli « recently_added » (Notification Agents, Webhook, POST JSON avec titre, année, saison, épisode, chemin) vers le bot, ou vérification périodique légère si le webhook n'est pas possible.
2. Retrouver le torrent correspondant dans qBittorrent (API /api/v2/torrents/info, par nom, hash ou chemin ; Sonarr et Radarr peuvent taguer leurs torrents pour faciliter la correspondance).
3. Vérifier les conditions : téléchargement terminé, média bien visible dans Plex, ratio ou durée minimum atteints selon la règle de l'utilisateur.
4. Arrêter le partage : pause du torrent (/api/v2/torrents/stop), et si l'utilisateur le veut, blocage des annonces vers le tracker (supprimer ou désactiver l'URL du tracker via /api/v2/torrents/removeTrackers, ou règle de limite de partage globale : max_ratio, max_seeding_time, action pause ou suppression).
5. Supprimer le torrent du client (/api/v2/torrents/delete) avec deleteFiles=false par défaut : le fichier reste dans la bibliothèque Plex (hardlink). Ne supprimer les fichiers source que si l'import a fait une copie et sur demande de l'utilisateur.
6. Journal court : titre, ratio final, action faite. Prévenir l'utilisateur seulement en cas d'échec.
Alternative simple : activer dans qBittorrent les limites de partage globales (Options, BitTorrent, ratio ou temps, puis Pause ou Supprimer), et laisser Sonarr et Radarr retirer les téléchargements terminés (Completed Download Handling, Remove).
Ne jamais contourner les règles d'un tracker privé : respecter son ratio et sa durée de seed obligatoires.
