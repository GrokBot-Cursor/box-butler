---
name: freebox-vpn-wireguard-serveur
description: >-
  Use this when creating or checking a Freebox WireGuard server and the bot's
  peer, or when local box access fails.
---
# Serveur WireGuard sur la Freebox
Prérequis : session API avec permission settings, accord explicite de l'utilisateur.
1. Lister : GET /api/v8/vpn/ ; repérer le serveur wireguard (état, port).
2. Activer : PUT /api/v8/vpn/{id}/config/ {enabled:true, conf_wireguard:{port, mtu}}. Annoncer le port UDP utilisé.
3. Utilisateur VPN : POST /api/v8/vpn/user/ {login:"boxbutler-bot", mot de passe aléatoire, ip_reservation}. Le mot de passe reste en fichier privé.
4. Config pair : GET /api/v8/vpn/download_config/{server}/{login} ; stocker en chmod 600, jamais affiché.
5. Split tunnel : AllowedIPs limité au LAN de la box (lu via GET /api/v8/lan/config/) et à l'IP VPN du serveur ; pas de 0.0.0.0/0.
6. Monter : wg-quick up (ou wireguard-go en userspace). Tester : ping de la box, GET /api_version en local.
7. Ensuite toujours passer par ce tunnel pour l'API locale et SSH vers la VM.
Panne : vérifier le handshake (wg show), une IP publique changée, le serveur toujours actif.
