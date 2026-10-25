---
name: freebox-vm-creation
description: >-
  Use this when creating, sizing or starting a Linux VM on a Freebox Delta or
  Ultra.
---
# VM sur Freebox (Delta, Ultra)
Prérequis : permission vm, tunnel WireGuard actif, accord explicite.
1. Ressources : GET /api/v8/vm/info/ (total_memory, used_memory, total_cpus). Ultra : 2 Go max. Delta : selon upgrade.
2. Images : GET /api/v8/vm/distros/ ; proposer l'image demandée, sinon la dernière Ubuntu LTS.
3. Disque : télécharger l'image dans le stockage de la box, puis POST /api/v8/vm/disk/resize/ (32 Go mini, 64 Go si Plex garde ses métadonnées en local). Suivre via GET /api/v8/vm/disk/task/{id}.
4. Création : POST /api/v8/vm/ {name, disk_path (base64), disk_type qcow2, memory, vcpus, os, enable_cloudinit:true, cloudinit_hostname, cloudinit_userdata}.
5. cloud-init : utilisateur admin, clé SSH publique générée par le bot (privée en chmod 600), paquets qemu-guest-agent et zram-tools.
6. Démarrage : POST /api/v8/vm/{id}/start ; récupérer l'IP via GET /api/v8/lan/browser/pub/ et proposer un bail DHCP fixe.
7. Mémoire virtuelle OBLIGATOIRE si la VM a moins de 4 Go de RAM (Ultra toujours, Delta non upgradée).
   L'annoncer clairement AVANT la création : « Ta VM aura X Go de RAM, je vais ajouter de la mémoire virtuelle (zram + fichier swap) de façon permanente, sinon les applis planteront. »
   Mise en place permanente : zram-tools (zram 50 % de la RAM), swapfile ajouté dans /etc/fstab, vm.swappiness=10 dans /etc/sysctl.d/.
   Taille du swapfile selon les applis choisies : Plex seul 1 Go ; Plex + 2 ou 3 applis 2 Go ; suite complète 3 Go (et prévenir que ce sera lent). Réajuster à chaque nouvelle appli et le dire à la personne.
   Au-delà de 4 Go : swap facultatif (1 Go de sécurité proposé).
8. NAS : monter le disque de la box en SMB/CIFS (/mnt/freebox) pour les médias.
