---
name: freebox-api-appairage
description: >-
  Use this when pairing a bot with a Freebox API, checking permissions, or
  reading the box public IP and model.
---
# Appairage API Freebox

## Tuto préalable (à donner à l'utilisateur, avant tout appel)
Sur un PC de la maison, ouvrir http://mafreebox.freebox.fr, puis Paramètres de la Freebox, Gestion des accès, onglet Paramètres :
1. Accès à Freebox OS : noter le port d'accès distant sécurisé (HTTPS) et l'adresse d'accès distant affichée.
2. Cocher « Activer l'authentification par mot de passe », avec un mot de passe administrateur solide (changer celui par défaut si besoin).
3. Applications : cocher « Permettre les nouvelles demandes d'association » le temps de l'appairage. Conseiller de la décocher une fois le bot appairé, pour bloquer toute autre demande.
4. Ne jamais envoyer le mot de passe administrateur au bot : seul le jeton d'application sert.
Après l'appairage : rappeler de vérifier l'onglet Applications (le bot y apparaît, droits limités) et l'onglet Sessions.

## Appairage
Base distante : https://<ip-publique>:<port_https> (préférer l'IP publique ou le api_domain renvoyé par la box au nom freeboxos.fr, qui peut mal se résoudre depuis le cloud). Base locale : http://mafreebox.freebox.fr.

1. Découverte : GET /api_version (api_version, api_base_url, https_port, api_domain). Utiliser le majeur renvoyé (ex. 16) dans les chemins /api/vN/.
2. IP publique : l'annoncer à l'utilisateur (service type https://api.ipify.org côté utilisateur, puis GET /api/vN/connection/ pour confirmer).
3. Demande d'autorisation : POST /api/vN/login/authorize/ {app_id, app_name, app_version, device_name}. Réponse : app_token (fichier privé, jamais affiché) et track_id.
4. Dire à l'utilisateur : « Valide sur l'écran de la box (flèche droite puis OK) dans les 2 minutes. »
5. Suivi : GET /api/vN/login/authorize/{track_id} jusqu'à status granted (sinon timeout ou denied : recommencer).
6. Session : GET /api/vN/login/ pour le challenge ; password = HMAC-SHA1(app_token, challenge) ; POST /api/vN/login/session/ {app_id, password} donne session_token, en-tête X-Fbx-App-Auth.
7. Droits : faire cocher dans Gestion des accès, Applications : settings (VPN, ports), vm, downloader si besoin, explorer (NAS). Rouvrir une session après changement.
8. Modèle : GET /api/vN/system/ (board_name, model_info).
Vérifier les endpoints sur dev.freebox.fr si une réponse surprend.

## Premier appairage : obligatoirement depuis le réseau local
Constat : depuis Internet, POST /login/authorize/ renvoie `denied_from_external_ip` (règle du firmware, aucun réglage ne la débloque). Seule la lecture de /api_version marche à distance.
Procédure novice : la personne installe l'app Grok Bot sur un PC ou un Mac connecté à la box (Wi-Fi ou câble). Le bot lance depuis cet ordinateur la demande vers http://mafreebox.freebox.fr/api/vN/login/authorize/, la personne appuie sur la flèche de l'écran de la box, et le jeton est enregistré sans jamais être affiché. Le bot crée ensuite le serveur WireGuard (avec accord) et passe par le tunnel pour tout le reste ; l'app n'est plus nécessaire.
Pour détecter l'IP publique : lien à ouvrir en Wi-Fi sur la box (pas en 4G).
