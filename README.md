# EcoRide

EcoRide est une API qui aide les utilisateurs à choisir le mode de transport le plus adapté selon:

- **Condition météo** (pluie, nuageux, soleil)
- **Situation du trafic** (fluide, saturé)

### Modes de transport disponibles

| Transport                    | Conditions                   |
| ---------------------------- | ---------------------------- |
| Métro / Tram                 | Pluie + Trafic saturé        |
| Voiture Électrique (Partage) | Pluie + Trafic fluide        |
| Vélo Électrique              | Pas de pluie + Trafic saturé |
| Vélo Standard                | Pas de pluie + Trafic fluide |

## Technologies

- FastAPI
- Pydantic
- AsyncIO
- Uvicorn

## Installation

### Prérequis

- Python 3.8+
- pip

### Étapes

```bash
# Créer l'environnement virtuel
python -m venv venv
venv\Scripts\activate   # Windows
source venv/bin/activate  # Linux / macOS

# Installer les dépendances
pip install fastapi uvicorn pydantic

# Lancemer l'api
python main.py
```

> Le serveur démarre sur `http://localhost:8000`

## Diagramme de séquence

```mermaid
sequenceDiagram
    actor Client
    participant API as API EcoRide
    participant Cache as Cache météo
    participant Meteo as Service météo
    participant Trafic as Service trafic

    Client->>API: POST /api/v1/route (api_key, user, city, destination)
    API->>API: Vérifier l'api_key

    alt Clé invalide
        API-->>Client: 401 Clé API invalide ou manquante
    else Clé valide
        API->>Cache: Vérifier le cache météo
        alt Cache valide (< 60s)
            Cache-->>API: météo en cache
        else Cache expiré ou absent
            API->>Meteo: Récupérer la météo
            alt Service indisponible
                Meteo-->>API: Erreur
                API-->>Client: 503 Service météo indisponible
            else Succès
                Meteo-->>API: météo
                API->>Cache: Mettre à jour le cache
            end
        end

        API->>Trafic: Récupérer le trafic
        Trafic-->>API: trafic

        API->>API: Calculer le meilleur transport (météo + trafic)
        API-->>Client: 200 (transport, durée, météo, CO2)
    end
```

## Documentation

- [API.md](API.md) : détails des endpoints
- [CHANGELOG.md](CHANGELOG.md) : historique des versions
- La doc Swagger est disponible sur http://localhost:8000/docs
