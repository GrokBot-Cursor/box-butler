---
name: alertes-par-mail-smtp
description: >-
  Use this when a service (Tautulli, Sonarr, Radarr, Lidarr, Prowlarr, Ubuntu,
  Diun...) must send its alerts by mail to the bot instead of a webhook or a
  scheduled check, when configuring its email notification, or when triaging
  those alert mails.
---
# Alertes par mail via SMTP (remplace webhooks et vérifications planifiées)

But : chaque service de la maison envoie lui-même ses alertes par mail vers l'adresse du bot. Le mail sert de déclencheur : pas de routine hebdomadaire.

## Paramètres SMTP (ceux de la personne)
- Serveur et port de son fournisseur mail (exemples : smtp.free.fr 465 SSL ou 587 STARTTLS ; smtp.gmail.com 587 avec mot de passe d'application ; smtp.orange.fr 465).
- Identifiant et expéditeur : l'adresse de la personne, demandée à elle.
- Mot de passe : saisi par la personne dans un formulaire sécurisé ou directement dans le champ du programme. Jamais dans le chat, jamais affiché, stocké seulement en secret ou fichier chmod 600.
- Destinataire : l'adresse mail du bot, avec une variante +tag par service si possible (bot+plex@, bot+ubuntu@...).
- Usage réservé à l'envoi d'alertes.

## Configurer un programme
- Sonarr, Radarr, Lidarr, Prowlarr : Settings, Connect, ajouter Email, serveur, port, SSL, identifiant, mot de passe, expéditeur, destinataire. Cocher Application Update et Health Issue (et On Import si voulu), puis Test.
- Tautulli : Settings, Notification Agents, Email, mêmes paramètres ; déclencheurs Recently Added, Plex Server Down, Plex Update Available, Tautulli Update Available ; Test Notification.
- Ubuntu : installer msmtp-mta (fichier /etc/msmtprc en chmod 600), puis apticron (mail seulement s'il y a des paquets à mettre à jour) ou unattended-upgrades avec Unattended-Upgrade::Mail.
- Conteneurs sans alerte propre (Portainer CE, Overseerr, qBittorrent, Home Assistant) : Diun en option (crazymax/diun, mem_limit 64m, notif mail). Le proposer, ne pas l'imposer.
- Home Assistant : intégration SMTP pour relayer ses notifications de mise à jour.

## Tri à l'arrivée d'un mail d'alerte
- Mise à jour disponible : résumer à la personne, appliquer seulement après accord (skill maintenance-vm-freebox).
- Nouveau média dans Plex : informer si la personne le souhaite ; déclencher le nettoyage torrent (skill torrents-nettoyage-apres-plex).
- Santé ou erreur : diagnostiquer et proposer une correction.
- Mail « Test » : ignorer. Ne jamais répondre à ces mails.

## Règles
- Ne jamais mettre comme destinataire l'adresse d'une autre personne sans accord.
- Aucune mise à jour ni action sans accord.
