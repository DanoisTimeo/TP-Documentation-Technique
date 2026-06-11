# Changelog

## 2.1.0 - Première version

- Ajout de l'endpoint `GET /health` pour vérifier l'état du service.
- Ajout de l'endpoint `POST /api/v1/route` pour obtenir une recommandation de transport écologique basée sur la météo et le trafic.
- Mise en cache des données météo (TTL de 60 secondes).
- Authentification par clé API (`api_key`).
