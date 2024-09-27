# Authentification

Lorsque vous souscrivez à l'API **Estimation de Bien**, un token vous sera fourni. Ajoutez le au Header de votre requête pour vous authentifier.

```bash
curl -X POST 'https://dimex.datatest.ch/{version}/{endpoint}' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic <token>' \
```

# Utilisation

L'API **Estimation de Bien** comporte trois Endpoints: un pour les modèle disponibles, le deuxième pour estimer les maisons individuelles et le dernier pour estimer les appartements. 

**La seule version d'API dispinible à ce jour est `v0`**.

## Modèles disponibles

### Exemple de Requête

```bash
curl -X GET 'https://dimex.datatest.ch/{version}/hed-fr-available-models' \
  -H 'Authorization: Basic <token>'
```

### Exemple de Réponse

```bash
"2024Q2"
```

## Estimation

### Exemple de Requête: Maisons individuelles

```bash
curl -X POST 'https://dimex.datatest.ch/{version}/hed-fr-sfh' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic <token>' \
  -d '{
    "model": "2024Q2",
    "requests": [
        {
            "index": 1,
            "externalId": "12345-123a",
            "valuationDate": "2024-09-26",
            "address": "30 Rue des Carnets, 92140 Clamart",
            "constructionYear": 1990,
            "area": {
                "usableArea": 120
            },
            "numberOfRooms": 5,
            "conditionManualValue": 2.5,
            "standardManualValue": 2.5,
            "houseType": "CHALET",
            "landPlotArea": 200,
            "numberOfOutdoorParkingSpaces": 0,
            "numberOfGarageParkingSpaces": 0,
            "numberOfFloors": 2,
            "firstOccupancy": false,
            "energyGrade": "G",
            "hasBalcony": false,
            "cellarType": "NONE",
            "atticType": "NONE",
            "hasTerrace": false,
            "hasSeparateRoomSecondaryUse": false,
            "glazingType": "OTHER"
        }
    ]
}'
```

### Description des champs: Maisons individuelles

| Champ                                    | Type    | Obligatoire   | Exemple                             | Description                                                                                   | Valeurs Acceptées                                                                                                    |
|:-----------------------------------------|:--------|:--------------|:------------------------------------|:----------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------|
| **model**                                | string  | Oui           | *2024Q2*                            | Modèle utilisé pour l'estimation.                                                             | Voir la section "modèles disponibles".                                                                               |
| **requests**                             | array   | Oui           | **                                  | Liste des biens à estimer.                                                                    | Voir l'exemple de requête.                                                                                           |
| requests[i].index                        | number  | Non           | *1*                                 | Indexe utilisé pour correspondre les requêtes et les réponses d'une estimation.               |                                                                                                                      |
| requests[i].externalId                   | string  | Non           | *12345-123a*                        | Paramètre qui peut être utilisé pour le logging ou le debugging.                              |                                                                                                                      |
| **requests[i].valuationDate**            | string  | Oui           | *2024-09-30*                        | Date d'estimation au format aaaa-mm-jj.                                                       |                                                                                                                      |
| **requests[i].address**                  | string  | Oui           | *30 Rue des Carnets, 92140 Clamart* | Addresse du bien à estimer.                                                                   |                                                                                                                      |
| **requests[i].constructionYear**         | number  | Oui           | *1995*                              | Année de construction.                                                                        | Entre 1000 et 3000.                                                                                                  |
| **requests[i].area.usableArea**          | json    | Oui           | *90*                                | Surface habitable.                                                                            | Entre 9 et 500.                                                                                                      |
| **requests[i].numberOfRooms**            | number  | Oui           | *3*                                 | Nombre de pièces.                                                                             | Entre 1 et 20.                                                                                                       |
| **requests[i].conditionManualValue**     | numbre  | Oui           | *2.5*                               | Score indiquant l'état du bien, plus élevé est le score meilleur est l'état du bien.          | Entre 1 et 5.                                                                                                        |
| requests[i].standardManualValue          | numbre  | Non           | *2.5*                               | Score indiquant la qualité du bien, plus élevé est le score meilleure est la qualité du bien. | Entre 1 et 5.                                                                                                        |
| **requests[i].houseType**                | string  | Oui           | *CHALET*                            | Type de maison.                                                                               | CHALET, DIVERS, FERME, ESTATE, HOTEL PARTICULIER, MASTERS_HOUSE, VILLAGE_HOUSE, DETACHED_HOUSE, MAISON RURALE, VILLA |
| **requests[i].landPlotArea**             | number  | Oui           | *200*                               | Surface du terrain.                                                                           |                                                                                                                      |
| requests[i].numberOfOutdoorParkingSpaces | number  | Non           | *1*                                 | Nombre de places de parking extérieures.                                                      | Supérieures à 0.                                                                                                     |
| requests[i].numberOfGarageParkingSpaces  | number  | Non           | *1*                                 | Nombre de places de garage.                                                                   | Supérieures à 0.                                                                                                     |
| **requests[i].numberOfFloors**           | number  | Oui           | *3*                                 | Nombre d'étages.                                                                              | Entre 1 et 5.                                                                                                        |
| requests[i].firstOccupancy               | boolean | Non           | *true*                              | Booléen qui indique si le est neuf.                                                           |                                                                                                                      |
| **requests[i].energyGrade**              | string  | Oui           | *A*                                 | Classe énergétique.                                                                           | A, B, C, D, E, F, G                                                                                                  |
| requests[i].hasBalcony                   | boolean | Non           | *true*                              | Le bien est avec balcon.                                                                      |                                                                                                                      |
| requests[i].cellarType                   | string  | Non           | *STOREROOM*                         | Type de cave.                                                                                 | STOREROOM, CELLAR, BASEMENT, NONE                                                                                    |
| requests[i].atticType                    | string  | Non           | *ATTIC*                             | Type de grenier.                                                                              | ATTIC, CONVERTABLE_ATTIC_SPACE, NONE                                                                                 |
| requests[i].hasTerrace                   | boolean | Non           | *true*                              | Les bien est avec terrace.                                                                    |                                                                                                                      |
| requests[i].hasSeparateRoomSecondaryUse  | boolean | Non           | *true*                              | Le bien a une pièce séparée.                                                                  |                                                                                                                      |
| requests[i].glazingType                  | string  | Non           | *DOUBLE*                            | Type de vitrage.                                                                              | TRIPLE, DOUBLE, OTHER                                                                                                |

### Exemple de Requête: Appartements

```bash
curl -X POST 'https://dimex.datatest.ch/{version}/hed-fr-con' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic <token>' \
  -d '{
    "model": "2024Q2",
    "requests": [
        {
            "index": 1,
            "externalId": "12345-123a",
            "valuationDate": "2024-09-26",
            "address": "1 Boulevard de la Madeleine, 75001 Paris",
            "constructionYear": 1975,
            "area": {
                "usableArea": 60
            },
            "numberOfRooms": 3,
            "conditionManualValue": 2.5,
            "standardManualValue": 2.5,
            "flatType": "STANDARD",
            "floor": 2,
            "numberOfFloors": 5,
            "terraceArea": 0,
            "numberOfGarageParkingSpaces": 4,
            "numberOfOutdoorParkingSpaces": 4,
            "buildingMaterialType": "BRICK",
            "gardenArea": 0,
            "glazingType": "OTHER",
            "hasCellar": false,
            "hasAttic": false,
            "balconyArea": 0,
            "firstOccupancy": false,
            "energyGrade": "G",
        }
    ]
}'
```

### Description des champs: Appartement

| Champ                                    | Type    | Obligatoire   | Exemple                                    | Description                                                                                   | Valeurs Acceptées                                         |
|:-----------------------------------------|:--------|:--------------|:-------------------------------------------|:----------------------------------------------------------------------------------------------|:----------------------------------------------------------|
| **model**                                | string  | Oui           | *2024Q2*                                   | Modèle utilisé pour l'estimation.                                                             | Voir la section "modèles disponibles".                    |
| **requests**                             | array   | Oui           | **                                         | Liste des biens à estimer.                                                                    | Voir l'exemple de requête.                                |
| requests[i].index                        | number  | Non           | *1*                                        | Indexe utilisé pour correspondre les requêtes et les réponses d'une estimation.               |                                                           |
| requests[i].externalId                   | string  | Non           | *12345-123a*                               | Paramètre qui peut être utilisé pour le logging ou le debugging.                              |                                                           |
| **requests[i].valuationDate**            | string  | Oui           | *2024-09-30*                               | Date d'estimation au format aaaa-mm-jj.                                                       |                                                           |
| **requests[i].address**                  | string  | Oui           | *1 Boulevard de la Madeleine, 75001 Paris* | Addresse du bien à estimer.                                                                   |                                                           |
| **requests[i].constructionYear**         | number  | Oui           | *1975*                                     | Année de construction.                                                                        | Entre 1000 et 3000.                                       |
| **requests[i].area.usableArea**          | json    | Oui           | *60*                                       | Surface habitable.                                                                            | Entre 9 et 275.                                           |
| **requests[i].numberOfRooms**            | number  | Oui           | *3*                                        | Nombre de pièces.                                                                             | Entre 1 et 10.                                            |
| **requests[i].conditionManualValue**     | numbre  | Oui           | *2.5*                                      | Score indiquant l'état du bien, plus élevé est le score meilleur est l'état du bien.          | Entre 1 et 5.                                             |
| requests[i].standardManualValue          | numbre  | Non           | *2.5*                                      | Score indiquant la qualité du bien, plus élevé est le score meilleure est la qualité du bien. | Entre 1 et 5.                                             |
| **requests[i].flatType**                 | string  | Oui           | *STANDARD*                                 | Type d'appertement.                                                                           | STANDARD, DUPLEX, TRIPLEX, STUDIO, SMALL_STUDIO_APARTMENT |
| **requests[i].floor**                    | number  | Oui           | *2*                                        | Numéro de l'étage.                                                                            | Entre -1 et 30.                                           |
| requests[i].numberOfFloors               | number  | Non           | *3*                                        | Nombre d'étages.                                                                              | Entre 1 et 30.                                            |
| **requests[i].terraceArea**              | number  | Oui           | *2.5*                                      |                                                                                               | Supérieures à 0.                                          |
| requests[i].numberOfOutdoorParkingSpaces | number  | Non           | *1*                                        | Nombre de places de parking extérieures.                                                      | Supérieures à 0.                                          |
| requests[i].numberOfGarageParkingSpaces  | number  | Non           | *1*                                        | Nombre de places de garage.                                                                   | Supérieures à 0.                                          |
| requests[i].buildingMaterialType         | string  | Non           | *BRICK*                                    | Matériau de construction.                                                                     | BRICK, BETON, EARTH, OTHER                                |
| requests[i].gardenArea                   | number  | Non           | *8*                                        | Surface de jardin.                                                                            | Supérieures à 0.                                          |
| requests[i].glazingType                  | string  | Non           | *DOUBLE*                                   | Type de vitrage.                                                                              | TRIPLE, DOUBLE, OTHER                                     |
| requests[i].hasCellar                    |         |               | **                                         |                                                                                               |                                                           |
| requests[i].hasAttic                     |         |               | **                                         |                                                                                               |                                                           |
| requests[i].balconyArea                  |         |               | **                                         |                                                                                               | Supérieures à 0.                                          |
| **requests[i].firstOccupancy**           | boolean | Oui           | *1*                                        | Booléen indiquent si le est neuf.                                                             |                                                           |
| **requests[i].energyGrade**              | string  | Oui           | *A*                                        | Classe énergétique.                                                                           | A, B, C, D, E, F, G                                       |

### Exemple de Réponse

```json
{
  "results": [
    {
      "index": 1,
      "error": null,
      "lon": 2.259016,
      "lat": 48.789286,
      "address": "30 Rue des Carnets, 92140 Clamart",
      "marketValueBeforeCorrection": 562000,
      "macroLocation": {
        "code": 92023,
        "name": "Clamart"
      },
      "microLocationValue": 3,
      "statisticalPriceRangeMin": 480000,
      "statisticalPriceRangeMax": 640000,
      "statisticalTrafficLight": {
        "trafficLight": 2,
        "classificationText": "YELLOW"
      },
      "messages": []
    }
  ]
}
```

### Exemple de Réponse avec erreur

```json
{
  "results": [
    {
      "index": 1,
      "messages": [
        {
          "description": "VALIDATION_ERROR",
          "level": "WARN",
          "reference": "2zLwdUuwOjDYh8hzlwTF",
          "message": "The HED FR model is unable to perform this calculation (endpoint /hed-fr-con). Validation error: floor is not an integer in range (-1, 30)"
        }
      ]
    }
  ]
}
```

### Description des champs de la réponse

| Champ                                                 | Type   | Exemple                                    | Description                                                                                                                                                                                                                                                                                                                                       | Valeurs Possibles   |
|:------------------------------------------------------|:-------|:-------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------|
| Results                                               | array  |                                          | Liste des résultats d'estimation des biens.                                                                                                                                                                                                                                                                                                       |                     |
| Results[i].index                                      |        |                                          |                                                                                                                                                                                                                                                                                                                                                   |                     |
| Results[i].error                                      | json   |                                          | Information sur l'erreur, en cas d'échec de la requête. Ce champ est presque toujours null, c'est plutôt le champ message qui est utile.                                                                                                                                                                                                          |                     |
| Results[i].lon                                        | number | *2.327675*                                 | Longitude de la position du bien.                                                                                                                                                                                                                                                                                                                 |                     |
| Results[i].lat                                        | number | *48.869687*                                | Latitude de la position du bien.                                                                                                                                                                                                                                                                                                                  |                     |
| Results[i].address                                    | string | *1 Boulevard de la Madeleine, 75001 Paris* | Addresse du bien                                                                                                                                                                                                                                                                                                                                  |                     |
| Results[i].marketValueBeforeCorrection                | number | *626000*                                   | Estimation du prix du bien en Euro.                                                                                                                                                                                                                                                                                                               |                     |
| Results[i].macroLocation.code                         | number | *75101*                                    | Code INSEE de la commune ou de l'arrodissement municipal où est situé le bien.                                                                                                                                                                                                                                                                    |                     |
| Results[i].macroLocation.name                         | string | *Paris 1er Arrondissement*                 | Nom de la commune ou de l'arrodissement municipal où est situé le bien.                                                                                                                                                                                                                                                                           |                     |
| Results[i].microLocationValue                         | number | *3.5*                                      | Score indiquant la qualité de la microlocalisation du bien par rapport au reste de la commune ou de l'arrondissement municipal où est situé le bien.                                                                                                                                                                                              | Entre 1 et 5.       |
| Results[i].statisticalPriceRangeMin                   | number | *530000*                                   | Estimation minimale du prix du bien.                                                                                                                                                                                                                                                                                                              |                     |
| Results[i].statisticalPriceRangeMax                   | number | *720000*                                   | Estimation maximale du prix du bien.                                                                                                                                                                                                                                                                                                              |                     |
| Results[i].statisticalTrafficLight.trafficLight       | number | *2*                                        | Indicateur d'atypicité du bien. Ici l'atypicité mesure la similarité du bien avec les biens présents sur le marché en se basant sur des caratéristiques comme la superficie, le nombre de pièce, l'étage, …etc. Généralement, moins le bien est atypique, plus fiable est l'estimation. 1: bien typique, 2: bien atypique, 3: bien très atypique. | 1, 2, 3             |
| Results[i].statisticalTrafficLight.classificationText | string | *RED*                                      | Couleur indiquant l'atypicité. GREEN: bien typique, YELLOW: bien atypique, RED: bien très atypique.                                                                                                                                                                                                                                               | RED, YELLOW, GREEN. |
| Results[i].messages                                   | array  | *Voir l'exemple de réponse avec erreur.*   | Liste des messages d'erreur en cas d'échec de la requête.                                                                                                                                                                                                                                                                                         |                     |

### Codes erreur

|   Code | Description          |
|-------:|:---------------------|
|    200 | Successful operation |
|    401 | Unauthorized         |
|    403 | Forbidden            |
|    404 | Not Found            |