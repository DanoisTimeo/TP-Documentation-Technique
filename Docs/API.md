# Documentation de l'API

La doc Swagger est disponible sur http://localhost:8000/docs

## `GET /health`

Retourne l'état du service. Aucune authentification requise.

**Sortie :**

```json
{
    "status": "healthy",
    "timestamp": 1781190555.9148142
}
```

---

## `POST /api/v1/route`

Retourne le mode de transport recommandé selon la météo et le trafic de la ville.

**Authentification requise** : passer `api_key=secret_token_3a` en paramètre query (ou la valeur configurée dans **API Key**).

```
POST /api/v1/route?api_key=secret_token_3a
```

**Entrée (JSON) :**

```json
{
    "user_id": "user_123",
    "city": "Paris",
    "destination": "Lyon"
}
```

**Sortie (JSON) :**

```json
{
    "recommended_transport": "Métro / Tram",
    "estimated_time_minutes": 35,
    "weather_condition": "Pluie",
    "carbon_footprint_g": 12.5
}
```

### Erreurs possibles

| Code | Raison                          |
| ---- | ------------------------------- |
| 401  | `Clé API invalide ou manquante` |
| 422  | `Entrée JSON invalide`          |
| 503  | `Service météo indisponible`    |
