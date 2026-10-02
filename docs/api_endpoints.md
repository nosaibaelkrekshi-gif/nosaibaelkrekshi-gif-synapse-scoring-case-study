# Points d'accès de l'API

Préfixe commun : `/api/scoring` · documentation interactive générée automatiquement par FastAPI (Swagger) sur `/docs`.

| US | Méthode | Endpoint | Rôle | Réponses |
|---|---|---|---|---|
| US-01 | `POST` | `/ingestion/ingestData` | Soumettre un fichier (PDF, TXT, JSON, CSV) | 201 Created · 413 si > 5 Mo |
| US-02 | `POST` | `/configWeights` | Enregistrer les pondérations d'un domaine | 200 OK · 400 si la somme ≠ 100 % |
| US-03 | `POST` | `/calculate-global-score` | Calculer un score (cosinus ou BM25) | 200 OK + ID de transaction |
| US-04 | `GET` | `/top-k?k=5` | Classement normalisé des meilleurs résultats | 200 OK |
| US-05 | `GET` | `/explain/{transactionId}` | Justification détaillée d'un score | 200 OK · 404 si ID inconnu |
| US-06 | `GET` | `/rag-context?domaine=RH&limit=5` | Contexte filtré pour un module RAG | 200 OK |
| US-07 | `POST` | `/compare` | Compatibilité entre deux sources | 200 OK + verdict métier |
| US-08 | `GET` | `/logs?domaine=RH&statut=ERREUR` | Journaux d'audit filtrés | 200 OK |

## Exemples de requêtes (tests Postman)

**Configuration des pondérations (US-02)**
```json
{
  "domaine": "RH",
  "poids": { "finance_weight": 0.4, "rh_weight": 0.3, "formation_weight": 0.3 }
}
```
→ `200 OK`. Avec une somme de 120 %, l'API renvoie `400 Bad Request`.

**Calcul du score global (US-03)**
```json
{
  "query": "Recherche de compétences Python",
  "domaine": "RH",
  "type_donnee": "Vecteurs",
  "seuil_confiance": 0.8
}
```
→ `200 OK`. Le moteur choisit la similarité cosinus, calcule le score brut et génère un ID de transaction.

**Explicabilité (US-05)** : `GET /api/scoring/explain/{transaction_id}` avec l'ID obtenu ci-dessus
→ `200 OK` : score global, confiance de vectorisation (≥ 80 %), version des règles et contributions de chaque critère supérieures à 5 %.
