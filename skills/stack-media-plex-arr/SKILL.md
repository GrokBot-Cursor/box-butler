---
name: stack-media-plex-arr
description: >-
  Use this when installing Plex alone or with Prowlarr, Sonarr, Radarr, Lidarr,
  Overseerr, Tautulli or qBittorrent on a Freebox VM.
---
# Stack média
Avant chaque appli : free -m, docker stats --no-stream. Refuser si la somme des mem_limit dépasse la RAM moins 300 Mo (prévenir, proposer zram/swap ou retirer une appli).
Images linuxserver.io ou officielles, PUID/PGID de l'utilisateur, TZ du fuseau de l'utilisateur, restart unless-stopped.
mem_limit indicatifs (Ultra / Delta) :
- Plex (plexinc/pms-docker, network host, claim via plex.tv/claim) : 768m / 2g
- Prowlarr 192m / 384m ; Sonarr 256m / 512m ; Radarr 256m / 512m ; Lidarr 192m / 384m
- Overseerr 256m / 384m ; Tautulli 128m / 256m ; qBittorrent 256m / 512m
Ultra : conseiller Plex plus 2 ou 3 applis maximum.
## Proposer chaque appli, une par une, en expliquant pourquoi
Ne jamais tout installer d'office. Après Plex, proposer chaque appli séparément, avec sa RAM, et attendre oui ou non :
- Prowlarr : gère en un seul endroit les sources de recherche (indexeurs) et les transmet aux autres applis. Utile seulement si tu prends Sonarr, Radarr ou Lidarr.
- Sonarr : suit tes séries, récupère les nouveaux épisodes et les range automatiquement dans Plex.
- Radarr : même chose pour les films (liste de souhaits, qualité voulue, rangement).
- Lidarr : même chose pour la musique (albums d'artistes suivis).
- Overseerr : une page simple où toi ou ta famille demandez un film ou une série, envoyé ensuite à Sonarr ou Radarr.
- Tautulli : statistiques de Plex (qui regarde quoi) et surtout alertes par mail (mises à jour Plex, nouveau média) qui déclenchent le bot.
- qBittorrent : le client de téléchargement utilisé par Sonarr, Radarr et Lidarr.
Sur une box < 4 Go, rappeler la RAM restante et l'ajustement du swap avant chaque oui.

Chemins : /mnt/freebox/medias/{films,series,musique} et /mnt/freebox/telechargements, identiques dans tous les conteneurs (hardlinks).
Liaisons : Prowlarr vers Sonarr, Radarr, Lidarr (clé API) ; client qBittorrent ou downloader Freebox ; Overseerr vers Plex, Sonarr et Radarr.
Indexeurs : après l'installation de Prowlarr, demander « As-tu des indexeurs dont tu as le droit d'usage ? ». Le bot n'en suggère aucun et ne crée jamais de compte sur un tracker. La clé API est saisie dans un formulaire sécurisé, puis le bot l'ajoute à Prowlarr, la teste et synchronise Sonarr, Radarr et Lidarr.

## Secrets et connexions
Chaque fois qu'un identifiant, un mot de passe ou une clé est nécessaire : proposer un formulaire sécurisé (jamais dans le chat). Quand un service n'a pas d'API : proposer une connexion automatique avec les identifiants enregistrés dans le coffre de l'utilisateur.

## TMDB et abonnements streaming
1. Proposer de créer un compte gratuit sur themoviedb.org (avec l'email du bot si la personne le veut, sinon le sien) et de récupérer la clé API gratuite (formulaire sécurisé ou récupération par le bot après accord).
2. Demander quels abonnements la personne possède (Netflix, Prime Video, Disney+, Canal+, Max, Apple TV+, Paramount+, OQEE/Free…) et les mémoriser.
3. Avant toute demande Overseerr ou Radarr/Sonarr : interroger TMDB watch/providers (région FR). Si le média est sur un abonnement déjà payé, prévenir « Disponible sur X, inutile de le demander » et ne lancer la demande que si la personne insiste.
4. Dans Overseerr, activer les infos de disponibilité streaming (région FR) quand c'est possible.
Transcodage : Plex Pass requis pour l'accélération matérielle ; sur Ultra, conseiller la lecture directe.

## Accès distant Plex et ouverture de port
Avant toute redirection (accord explicite obligatoire) :
1. Lire la plage de ports disponible : GET /api/vN/connection/config/ et /api/vN/connection/ (IPv4 full-stack ou IPv4 partagée). Certaines lignes Free ont une IPv4 partagée avec seulement une plage de ports utilisable ; d'autres ont toute la plage 1 à 65535.
2. Port Plex par défaut 32400. S'il est hors de la plage autorisée ou déjà redirigé (GET /api/vN/fw/redir/), choisir un port externe libre dans la plage et le rediriger vers l'IP de la VM port 32400 (POST /api/vN/fw/redir/), puis le régler dans Plex, Paramètres, Accès à distance, « port public manuel ».
3. Proposer en alternative IPv6 ou UPnP désactivé avec port manuel ; ne jamais ouvrir d'autres ports (Portainer, *arr) vers Internet : ils restent accessibles par le tunnel WireGuard.
4. Tester depuis l'extérieur (Plex, Accès à distance doit afficher « Entièrement accessible »).

## TV Free et enregistrements (optionnel, sur proposition)
1. Guide des programmes : utiliser l'API de la box (tv/epg, chaînes françaises) pour répondre « qu'est-ce qui passe ce soir », retrouver un film ou une série à la TV et éviter une demande Overseerr inutile.
2. Proposer, si la personne le veut, la gestion des enregistrements selon ses goûts : identifiant et mot de passe tv.free.fr / OQEE via formulaire sécurisé (connexion automatique ensuite, jamais dans le chat).
3. Demander le quota d'enregistrement de son abonnement (ex. jusqu'à 320 h sur Freebox Ultra), suivre l'espace utilisé et prévenir avant d'atteindre la limite ; ne jamais supprimer un enregistrement sans accord.
4. Lieu d'enregistrement selon le matériel : cloud Free (enregistrements réseau), disque ou SSD interne de la box, ou NAS local si présent. Proposer le choix et l'expliquer.
5. Chaque enregistrement programmé est annoncé à la personne ; les programmations automatiques par profil restent désactivables à tout moment.
