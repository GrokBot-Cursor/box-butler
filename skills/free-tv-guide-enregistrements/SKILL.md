---
name: free-tv-guide-enregistrements
description: >-
  Use this when the owner wants the optional TV module: TV guide (EPG) through
  the Freebox API, Free TV / OQEE recordings by profile, recording quota, or
  checking whether a film is on TV before an Overseerr request.
---
# Module TV Free (optionnel, sur proposition)

## Quand l'utiliser
La personne parle du guide TV, de ce qui passe ce soir, de ses enregistrements, du quota, d'OQEE Ciné, ou accepte le module TV proposé après la stack média.

## Prérequis
- Accord explicite pour activer le module.
- Guide : session API Freebox par le tunnel WireGuard (pas besoin d'identifiants Free TV).
- Enregistrements via tv.free.fr : identifiant et mot de passe saisis par la personne dans un formulaire sécurisé (jamais dans le chat), puis connexion automatique avec le coffre.
- La lecture vidéo depuis un ordinateur hors réseau Free est souvent géobloquée : ne pas promettre le direct, ne jamais contourner le géoblocage.

## Guide des programmes
1. Utiliser l'API de la box (tv/epg, chaînes françaises ; vérifier les endpoints sur dev.freebox.fr).
2. Répondre court : chaîne, titre, horaire.
3. Avant une demande Overseerr : regarder si le film ou la série passe bientôt à la TV ou est en replay, et le signaler.

## Enregistrements par profil
1. Demander les goûts de chaque profil (genres, séries suivies, chaînes).
2. Proposer des enregistrements ; chaque programmation est annoncée. Les programmations automatiques restent désactivables à tout moment.
3. Lieu d'enregistrement selon le matériel : cloud Free (enregistrement réseau), disque ou SSD interne de la box, ou NAS local. Expliquer et laisser choisir.
4. Ne jamais supprimer un enregistrement sans accord explicite.

## Quota
- Demander le quota de l'abonnement (ex. jusqu'à 320 h sur Freebox Ultra, selon l'offre).
- Suivre l'espace utilisé et prévenir avant la limite (par exemple à 80 % puis 95 %).

## URLs utiles
- Guide : https://tv.free.fr/home/tv_guide?time=0
- Ma liste et enregistrements : https://tv.free.fr/home/my_list
- OQEE Ciné (A-VOD incluse) : https://tv.free.fr/home/oqee_cine

## Règles
- Aucun identifiant dans la skill ni dans le chat.
- Confirmation avant toute suppression.
- Pour « où regarder » hors Free : TMDB watch/providers région FR.
