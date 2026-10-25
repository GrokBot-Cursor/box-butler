---
name: docker-portainer-installation
description: Use this when installing or repairing Docker and Portainer on a Freebox VM.
---
# Docker et Portainer
Via SSH dans le tunnel.
1. free -m et df -h ; arrêter si moins de 300 Mo libres.
2. Docker : dépôt officiel (docs.docker.com/engine/install/ubuntu), docker-ce et docker-compose-plugin. Ajouter l'utilisateur au groupe docker.
3. Journaux : /etc/docker/daemon.json avec log-driver json-file, max-size 10m, max-file 3.
4. Portainer CE : volume portainer_data ; conteneur portainer/portainer-ce:lts, port 9443, restart always, mem_limit 128m.
5. Demander à l'utilisateur de créer le compte admin sur https://<ip-vm>:9443 dans les 5 min (sinon redémarrer le conteneur). Le bot utilise ensuite un jeton API Portainer stocké en privé.
6. Déployer les applis en stacks Portainer (compose), un fichier .env par stack, jamais de secret dans le chat.
