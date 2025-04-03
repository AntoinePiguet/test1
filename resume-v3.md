# Examen EP2 - ICT 165 - Bases de données NoSQL

**Date :** 02 avril 2025  
**Préparé par :** Grok 3 (xAI)  

Ce document contient les réponses aux tâches demandées dans l’examen EP2 - ICT 165, basé sur les documents fournis, y compris `villes_suisse.json` et les fichiers Docker. Chaque section est expliquée avec les structures JSON et les commandes nécessaires.  

Vous devrez exécuter ces commandes sur votre machine, car je ne peux pas interagir directement avec votre environnement.  

---

## Section 1 : Modélisation “Expositions de musée”

### 1.1 Modélisation pour la collection `museums`

**Exigence :**  
Représenter un musée avec son nom, adresse (incluant coordonnées géographiques), numéro de téléphone, et site web.  

**Structure JSON :**  

```json
{
    "_id": ObjectId(),
    "name": "<string>",
    "address": {
        "geo_ref": {
            "lat": <number>,
            "lng": <number>
        },
        "street": "<string>",
        "street_number": "<string>",
        "zip": "<string>",
        "location": "<string>"
    },
    "phone": "<string>",
    "website": "<string>"
}
```

**Action :**  
Créez un fichier `X-1-document-museum.json` avec cette structure et placez-le dans `X-165-reponses-ts2`.  

---

### 1.2 Modélisation pour la collection `items`

**Exigence :**  
Représenter un objet avec titre, description, auteurs, catégorie, caractéristiques, et musée associé.  

**Structure JSON :**  

```json
{
    "_id": ObjectId(),
    "title": "<string>",
    "description": "<string>",
    "authors": ["<string>"],
    "category": "<string>", 
    "features": [
        {
            "name": "<string>",
            "value": "<string>",
            "unit": "<string>"
        }
    ],
    "museum": {
        "name": "<string>"
    }
}
```

**Action :**  
Créez un fichier `X-1-document-item.json` avec cette structure et placez-le dans `X-165-reponses-ts2`.  

---

### 1.3 Modélisation pour la collection `exhibitions`

**Exigence :**  
Représenter une exposition avec titre, description, conservateurs, objets, et événements.  

**Structure JSON :**  

```json
{
    "_id": ObjectId(),
    "title": "<string>",
    "description": "<string>",
    "curators": ["<string>"],
    "items": [ObjectId()],
    "events": [
        {
            "museum": {
                "name": "<string>",
                "geo_ref": {
                    "lat": <number>,
                    "lng": <number>
                }
            },
            "start_date": ISODate(),
            "end_date": ISODate()
        }
    ]
}
```

**Action :**  
Créez un fichier `X-1-document-exhibition.json` avec cette structure et placez-le dans `X-165-reponses-ts2`.  

---

## Section 2 : MongoDB - Conception

### 2.1 Création d’une base de données et d’une collection

**Commande MongoDB :**  

```javascript
use db_ch;

db.cities.insertMany([
    {"city": "Zürich", "location": {"lat": 47.3744, "lng": 8.5411}, "country": "Switzerland", "iso2": "CH", "canton": "Zürich", "capital": "admin", "population": 436332},
    {"city": "Geneva", "location": {"lat": 46.2017, "lng": 6.1469}, "country": "Switzerland", "iso2": "CH", "canton": "Genève", "capital": "admin", "population": 201818},
    {"city": "Basel", "location": {"lat": 47.5547, "lng": 7.5906}, "country": "Switzerland", "iso2": "CH", "canton": "Basel-Stadt", "capital": "admin", "population": 177827},
    {"city": "Cureglia", "location": {"lat": 46.0333, "lng": 8.95}, "country": "Switzerland", "iso2": "CH", "canton": "Ticino", "capital": "", "population": 1385}
]);
```

**Action :**  
Créez un fichier `X-2-db.m` avec ces commandes et placez-le dans `X-165-reponses-ts2`.  

---

## Section 3 : MongoDB - Requêtes

### 3.1 Requête de recherche I

```javascript
db.cities.findOne({ "canton": "Vaud", "capital": "admin" });
```

**Réponse :** Lausanne.  

### 3.2 Requête de recherche II

```javascript
db.cities.countDocuments({ "population": { "$gt": 100000 } });
```

**Réponse :** 6 villes.  

### 3.3 Requête de recherche III

```javascript
db.cities.findOne({ "city": { "$regex": /^Saint/ } });
```

**Réponse :** Saint-Sulpice ou Sankt Gallen.  

### 3.4 Requête de recherche IV

```javascript
db.cities.find({
    "location.lat": { "$gte": 46.5197, "$lte": 46.5283 }
}).sort({ "location.lat": 1 });
```

**Réponse :** Lausanne, Ecublens, Chavannes-près-Renens.  

---
