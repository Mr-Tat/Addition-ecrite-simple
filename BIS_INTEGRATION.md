# Addition écrite simple — Intégration Math'as BIS (23/10/2026)

Ce dépôt est le **jeu réel**. Le HTML public main ne doit pas être modifié pendant la construction.
Branche : bis/integration-v1. Dépôt HUB : Mr-Tat/Mathas, branche bis/architecture-v1.
Contrat de référence : docs/BIS_ARCHITECTURE_V1.md; module de validation pur : bis/core/contract.js.

## Responsabilités
Le jeu conserve toutes ses règles : calculs, points, records, trophées, déblocages, modes classique/mort subite/HARD et Master Mode.
La passerelle commune Math'as ne comprend aucune règle du jeu et n'effectue aucune fusion de records.
Le jeu expose un adaptateur réduit :
- completedActivity() -> {eventId, activityKey, mode, levelKey, score, scoreMax?, points?, scoringVersion} ;
- exportSave() -> {schemaVersion, data} avec l'état nécessaire pour une restauration ;
- importSave(data, conflictContext?) qui décide comment utiliser les données distantes, éventuellement après conflit de révision.

Les identifiants métier d'élève et les groupes sont attribués par le service de session; jamais par l'adaptateur du jeu.
La passerelle protège les écritures concurrentes avec revision + resetEpoch, et une queue locale cloisonnée par élève/application.
Ne jamais intercepter globalement Storage.prototype dans la version BIS.
Ne jamais envoyer les parties Master Mode comme résultats élèves.
Ouvrir le jeu sans session approuvée doit continuer à fonctionner librement, sans enregistrement nominatif.
Ne pas importer automatiquement les anciens scores du mode libre dans un compte élève.

## Étapes d'intégration avant merge public
1. Observer précisément les événements de fin de partie et la persistance existants ; rédiger l'adaptateur sans toucher au moteur du jeu.
2. Vérifier hors réseau qu'il n'existe aucun résultat envoyé depuis le mode libre / Master Mode.
3. Vérifier passage d'élève A à B, absence de fuite des records, restauration de l'état propre à chacun et conflit revision.
4. Tester uniquement contre Supabase-Test, valider permissions et idempotence.
5. Vérifier les résultats dans l'espace professeur BIS et le tableau BIS, puis revue de publication.

## Interdits explicites
Pas de copie du jeu dans Mathas/lab-comptes ou autre sous-dossier du HUB.
Pas de dépendance au SDK expérimental qui pointe vers Supabase production.
Pas de nouveau barème ni de recalcul de points par le HUB.
Pas de modification de main ou Supabase production avant feu vert final.
