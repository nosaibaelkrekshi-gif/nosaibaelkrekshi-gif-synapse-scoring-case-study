# User stories : EPIC Scoring (US-01 à US-08)

**User story principale**
> En tant qu'acteur de l'écosystème SYNAPSE (Producteur, Orchestrateur ou Consommateur),
> je veux m'appuyer sur un moteur de scoring **centralisé, hybride et paramétrable**,
> afin de calculer, normaliser et classer des données (Top-K) de manière cohérente selon des règles métier,
> tout en garantissant une **transparence totale** grâce à l'explicabilité (XAI) de chaque décision.

| US | Titre | Règles métier et critères d'acceptation | Statut |
|---|---|---|---|
| US-01 | Ingestion multi-sources et validation | Fichiers PDF, TXT, JSON, CSV · taille max **5 Mo** (sinon 413) · empreinte **SHA-256** pour l'intégrité · vectorisation rejetée si confiance < 80 % | ✅ |
| US-02 | Configuration des pondérations métier | Poids par critère et par domaine (RH, Finance, Formation) · la somme doit être **égale à 100 %** (sinon 400) · configuration versionnée | ✅ |
| US-03 | Sélection et exécution du scoring | Routage automatique : **similarité cosinus** pour les vecteurs, **BM25** pour le texte · précision à 4 décimales · ID de transaction | ✅ |
| US-04 | Normalisation et classement Top-K | Normalisation **Min-Max sur 0-100** · seuil de pertinence **≥ 80** · tri décroissant · K entre 1 et 50 | ✅ |
| US-05 | Explicabilité (XAI) | Décomposition du score par critère · seuls les impacts **> 5 %** sont affichés · mots-clés influents · version des règles | ✅ |
| US-06 | Contexte optimisé pour le RAG | Seuls les segments **≥ 80** et au statut « prêt pour scoring » · Top-K pour respecter la fenêtre de contexte du LLM | ✅ |
| US-07 | Aide à la décision (matching) | Comparaison de deux sources · **≥ 95 : doublon détecté** · **≥ 80 : match pertinent** · sinon écart significatif | ✅ |
| US-08 | Audit, traçabilité et performance | Chaque opération journalisée (ID, horodatage UTC, source, méthode) · alerte si confiance < 80 % · filtres par domaine et statut | ✅ |

**Définition du « terminé » (DoD)** : les 8 user stories implémentées, code organisé en couches (modèles, services, routeurs), tests unitaires et d'intégration au vert, réponse de l'API en moins de 5 secondes.
