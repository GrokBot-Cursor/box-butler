---
name: nas-externe-detection-appairage
description: >-
  Use this when a Freebox has no internal NAS or VM (Pop, Revolution, Mini 4K)
  and the owner may have an external NAS (Synology, QNAP, Asustor...) on the
  local network to pair and use for media.
---
# NAS externe (box sans NAS ni VM)
Prérequis : tunnel WireGuard actif, accord explicite de l'utilisateur pour scanner son réseau local.

1. Scan léger du LAN : d'abord GET /api/vN/lan/browser/pub/ (liste des appareils connus de la box, nom, fabricant, IP). Ensuite seulement, si besoin, scan ciblé des ports NAS courants sur le sous-réseau (5000/5001 Synology DSM, 8080/443 QNAP, 8000/8001 Asustor, 445 SMB, 2049 NFS). Pas de scan agressif.
2. Présenter les NAS trouvés (modèle, IP) et demander lequel utiliser.
3. Appairage, par ordre de préférence :
   - Synology : créer côté utilisateur un compte dédié au bot (pas l'admin), droits limités aux dossiers médias, 2FA conseillée sur l'admin. API DSM : GET /webapi/query.cgi?api=SYNO.API.Info&version=1&method=query&query=all, puis SYNO.API.Auth (login, session sid). QNAP / Asustor : API ou compte dédié de la même façon.
   - Sinon identifiant et mot de passe du compte dédié, saisis par l'utilisateur dans un formulaire privé (jamais dans le chat), stockés en fichier chmod 600.
4. Selon le NAS : Docker natif (Synology Container Manager, QNAP Container Station) pour Portainer et la stack média avec les mêmes règles (mémoire, mem_limit, accord), ou simple partage SMB/NFS des médias si l'utilisateur préfère.
5. Aucun NAS trouvé : s'arrêter au VPN et proposer un disque USB sur la box ou un futur NAS.
Ne jamais exposer l'interface du NAS sur Internet : accès uniquement par le tunnel.
