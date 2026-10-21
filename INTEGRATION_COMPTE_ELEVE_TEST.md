# Intégration scolaire — Addition écrite simple (branche de test)

## État
Le jeu réel enregistre déjà ses résultats dans le navigateur. Cette branche ajoute un **événement de fin de partie** et un adaptateur `mathas-school-bridge.js`.

Aucun résultat n'est actuellement envoyé à Supabase depuis cette application : le SDK scolaire n'est pas encore branché à une session de lancement sécurisée, et l'API serveur des scores officiels n'est pas encore disponible.

## Déclencheurs couverts
- Classique : score / nombre de calculs (4, 10, 20).
- Mort subite : score avant première erreur. La valeur `score + 1` transmise à l'affichage local n'est **pas** un maximum ; l'adaptateur envoie `scoreMax: null`.
- Hard 3 calculs / série de 5 : score sur 5.
- Hard mort subite : score avant première erreur, maximum inconnu.

## Séparation des modes
- Ouverture par URL directe ou HUB partagé : **mode libre**, aucun envoi vers le serveur.
- HUB classe : le futur SDK devra prouver un ticket de lancement lié à l'application avant tout enregistrement.
- La persistance locale, les récompenses et les déblocages historiques sont préservés.
- Les scores des parties libres déjà sauvegardés ne seront pas importés automatiquement sous une identité d'élève.

## Étapes avant un vrai test classe
1. Identifier et valider un ticket de lancement coté serveur, associé à une application de référence `application_id=1` dans Mathas-Test.
2. Mettre en service une API d'enregistrement des tentatives avec contrôle de session, idempotence, limitation de débit, RLS et validation de score.
3. Raccorder le SDK à cette API et vérifier les tests sur deux sessions Juju distinctes.
4. Synchroniser progression et trophées avec un schéma/version adaptés à cette application.
5. Revoir la sécurité, les sauvegardes et la séparation des modes avant toute mise en production.

Projet GitHub : `Mr-Tat/Addition-ecrite-simple`, branche `feature/mathas-comptes-eleves-integration-20261021`.
Ne pas fusionner automatiquement dans `main`.
