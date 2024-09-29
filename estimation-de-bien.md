# API Estimation de Bien

L'API Estimation de Bien permet d'estimer le prix d'un appartement ou d'une maison individuelle en se basant sur les caratéristiques et la localisation du bien.

# Authentification

Lorsque vous souscrivez à l'API **Estimation de Bien**, un token vous sera fourni. Ajoutez le au Header de votre requête pour vous authentifier.

```bash
curl -X POST 'https://dimex.datatest.ch/{version}/{endpoint}' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic <token>' \
```

# Utilisation

L'API **Estimation de Bien** comporte trois Endpoints: un pour les modèle disponibles, le deuxième pour estimer les maisons individuelles et le dernier pour estimer les appartements. 

**La seule version d'API disponible à ce jour est `v0`**.

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

| Champ                                                                        | Description                                                                                   | Type      | Valeurs Acceptées                                                                                                    | Exemple                             |
|:-----------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------|:----------|:---------------------------------------------------------------------------------------------------------------------|:------------------------------------|
| model<span style="color:red;"><sup>*</sup></span>                            | Modèle utilisé pour l'estimation.                                                             | `string`  | Voir la section "modèles disponibles".                                                                               | *2024Q2*                            |
| requests<span style="color:red;"><sup>*</sup></span>                         | Liste des biens à estimer.                                                                    | `array`   | Voir l'exemple de requête.                                                                                           |                                     |
| requests[i].index                                                            | Indexe utilisé pour correspondre les requêtes et les réponses d'une estimation.               | `number`  |                                                                                                                      | *1*                                 |
| requests[i].externalId                                                       | Paramètre qui peut être utilisé pour le logging ou le debugging.                              | `string`  |                                                                                                                      | *12345-123a*                        |
| requests[i].valuationDate<span style="color:red;"><sup>*</sup></span>        | Date d'estimation au format aaaa-mm-jj.                                                       | `string`  |                                                                                                                      | *2024-09-30*                        |
| requests[i].address<span style="color:red;"><sup>*</sup></span>              | Addresse du bien à estimer.                                                                   | `string`  |                                                                                                                      | *30 Rue des Carnets, 92140 Clamart* |
| requests[i].constructionYear<span style="color:red;"><sup>*</sup></span>     | Année de construction.                                                                        | `number`  | Entre 1000 et 3000.                                                                                                  | *1995*                              |
| requests[i].area.usableArea<span style="color:red;"><sup>*</sup></span>      | Surface habitable.                                                                            | `number`  | Entre 9 et 500.                                                                                                      | *90*                                |
| requests[i].numberOfRooms<span style="color:red;"><sup>*</sup></span>        | Nombre de pièces.                                                                             | `number`  | Entre 1 et 20.                                                                                                       | *3*                                 |
| requests[i].conditionManualValue<span style="color:red;"><sup>*</sup></span> | Score indiquant l'état du bien, plus élevé est le score meilleur est l'état du bien.          | `numbre`  | Entre 1 et 5.                                                                                                        | *2.5*                               |
| requests[i].standardManualValue                                              | Score indiquant la qualité du bien, plus élevé est le score meilleure est la qualité du bien. | `numbre`  | Entre 1 et 5.                                                                                                        | *2.5*                               |
| requests[i].houseType<span style="color:red;"><sup>*</sup></span>            | Type de maison.                                                                               | `string`  | CHALET, DIVERS, FERME, ESTATE, HOTEL PARTICULIER, MASTERS_HOUSE, VILLAGE_HOUSE, DETACHED_HOUSE, MAISON RURALE, VILLA | *CHALET*                            |
| requests[i].landPlotArea<span style="color:red;"><sup>*</sup></span>         | Surface du terrain.                                                                           | `number`  |                                                                                                                      | *200*                               |
| requests[i].numberOfOutdoorParkingSpaces                                     | Nombre de places de parking extérieures.                                                      | `number`  | Supérieures à 0.                                                                                                     | *1*                                 |
| requests[i].numberOfGarageParkingSpaces                                      | Nombre de places de garage.                                                                   | `number`  | Supérieures à 0.                                                                                                     | *1*                                 |
| requests[i].numberOfFloors<span style="color:red;"><sup>*</sup></span>       | Nombre d'étages.                                                                              | `number`  | Entre 1 et 5.                                                                                                        | *3*                                 |
| requests[i].firstOccupancy                                                   | Booléen qui indique si le est neuf.                                                           | `boolean` |                                                                                                                      | *true*                              |
| requests[i].energyGrade<span style="color:red;"><sup>*</sup></span>          | Classe énergétique.                                                                           | `string`  | A, B, C, D, E, F, G                                                                                                  | *A*                                 |
| requests[i].hasBalcony                                                       | Le bien est avec balcon.                                                                      | `boolean` |                                                                                                                      | *true*                              |
| requests[i].cellarType                                                       | Type de cave.                                                                                 | `string`  | STOREROOM, CELLAR, BASEMENT, NONE                                                                                    | *STOREROOM*                         |
| requests[i].atticType                                                        | Type de grenier.                                                                              | `string`  | ATTIC, CONVERTABLE_ATTIC_SPACE, NONE                                                                                 | *ATTIC*                             |
| requests[i].hasTerrace                                                       | Les bien est avec terrace.                                                                    | `boolean` |                                                                                                                      | *true*                              |
| requests[i].hasSeparateRoomSecondaryUse                                      | Le bien a une pièce séparée.                                                                  | `boolean` |                                                                                                                      | *true*                              |
| requests[i].glazingType                                                      | Type de vitrage.                                                                              | `string`  | TRIPLE, DOUBLE, OTHER                                                                                                | *DOUBLE*                            |

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

| Champ                                                                        | Description                                                                                   | Type      | Valeurs Acceptées                                         | Exemple                                    |
|:-----------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------|:----------|:----------------------------------------------------------|:-------------------------------------------|
| model<span style="color:red;"><sup>*</sup></span>                            | Modèle utilisé pour l'estimation.                                                             | `string`  | Voir la section "modèles disponibles".                    | *2024Q2*                                   |
| requests<span style="color:red;"><sup>*</sup></span>                         | Liste des biens à estimer.                                                                    | `array`   | Voir l'exemple de requête.                                |                                            |
| requests[i].index                                                            | Indexe utilisé pour correspondre les requêtes et les réponses d'une estimation.               | `number`  |                                                           | *1*                                        |
| requests[i].externalId                                                       | Paramètre qui peut être utilisé pour le logging ou le debugging.                              | `string`  |                                                           | *12345-123a*                               |
| requests[i].valuationDate<span style="color:red;"><sup>*</sup></span>        | Date d'estimation au format aaaa-mm-jj.                                                       | `string`  |                                                           | *2024-09-30*                               |
| requests[i].address<span style="color:red;"><sup>*</sup></span>              | Addresse du bien à estimer.                                                                   | `string`  |                                                           | *1 Boulevard de la Madeleine, 75001 Paris* |
| requests[i].constructionYear<span style="color:red;"><sup>*</sup></span>     | Année de construction.                                                                        | `number`  | Entre 1000 et 3000.                                       | *1975*                                     |
| requests[i].area.usableArea<span style="color:red;"><sup>*</sup></span>      | Surface habitable.                                                                            | `number`  | Entre 9 et 275.                                           | *60*                                       |
| requests[i].numberOfRooms<span style="color:red;"><sup>*</sup></span>        | Nombre de pièces.                                                                             | `number`  | Entre 1 et 10.                                            | *3*                                        |
| requests[i].conditionManualValue<span style="color:red;"><sup>*</sup></span> | Score indiquant l'état du bien, plus élevé est le score meilleur est l'état du bien.          | `numbre`  | Entre 1 et 5.                                             | *2.5*                                      |
| requests[i].standardManualValue                                              | Score indiquant la qualité du bien, plus élevé est le score meilleure est la qualité du bien. | `numbre`  | Entre 1 et 5.                                             | *2.5*                                      |
| requests[i].flatType<span style="color:red;"><sup>*</sup></span>             | Type d'appertement.                                                                           | `string`  | STANDARD, DUPLEX, TRIPLEX, STUDIO, SMALL_STUDIO_APARTMENT | *STANDARD*                                 |
| requests[i].floor<span style="color:red;"><sup>*</sup></span>                | Numéro de l'étage.                                                                            | `number`  | Entre -1 et 30.                                           | *2*                                        |
| requests[i].numberOfFloors                                                   | Nombre d'étages.                                                                              | `number`  | Entre 1 et 30.                                            | *3*                                        |
| requests[i].terraceArea<span style="color:red;"><sup>*</sup></span>          | Surface de la terrace.                                                                        | `number`  | Supérieures à 0.                                          | *2.5*                                      |
| requests[i].numberOfOutdoorParkingSpaces                                     | Nombre de places de parking extérieures.                                                      | `number`  | Supérieures à 0.                                          | *1*                                        |
| requests[i].numberOfGarageParkingSpaces                                      | Nombre de places de garage.                                                                   | `number`  | Supérieures à 0.                                          | *1*                                        |
| requests[i].buildingMaterialType                                             | Matériau de construction.                                                                     | `string`  | BRICK, BETON, EARTH, OTHER                                | *BRICK*                                    |
| requests[i].gardenArea                                                       | Surface de jardin.                                                                            | `number`  | Supérieures à 0.                                          | *8*                                        |
| requests[i].glazingType                                                      | Type de vitrage.                                                                              | `string`  | TRIPLE, DOUBLE, OTHER                                     | *DOUBLE*                                   |
| requests[i].hasCellar                                                        | Le bien  est avec cave.                                                                       |           |                                                           | *true*                                     |
| requests[i].hasAttic                                                         | Le bien  est avec grenier.                                                                    |           |                                                           | *false*                                    |
| requests[i].balconyArea                                                      | Surface du balcon.                                                                            |           | Supérieures à 0.                                          | *1.5*                                      |
| requests[i].firstOccupancy<span style="color:red;"><sup>*</sup></span>       | Booléen indiquent si le est neuf.                                                             | `boolean` |                                                           | *true*                                     |
| requests[i].energyGrade<span style="color:red;"><sup>*</sup></span>          | Classe énergétique.                                                                           | `string`  | A, B, C, D, E, F, G                                       | *A*                                        |

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

| Champ                                                 | Description                                                                                                                                                                                                                                                                                                                                       | Type     | Valeurs Possibles   | Exemple                                    |
|:------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------|:--------------------|:-------------------------------------------|
| Results                                               | Liste des résultats d'estimation des biens.                                                                                                                                                                                                                                                                                                       | `array`  |                     |                                            |
| Results[i].index                                      |                                                                                                                                                                                                                                                                                                                                                   |          |                     |                                            |
| Results[i].error                                      | Information sur l'erreur, en cas d'échec de la requête. Ce champ est presque toujours null, c'est plutôt le champ message qui est utile.                                                                                                                                                                                                          | `json`   |                     |                                            |
| Results[i].lon                                        | Longitude de la position du bien.                                                                                                                                                                                                                                                                                                                 | `number` |                     | *2.327675*                                 |
| Results[i].lat                                        | Latitude de la position du bien.                                                                                                                                                                                                                                                                                                                  | `number` |                     | *48.869687*                                |
| Results[i].address                                    | Addresse du bien                                                                                                                                                                                                                                                                                                                                  | `string` |                     | *1 Boulevard de la Madeleine, 75001 Paris* |
| Results[i].marketValueBeforeCorrection                | Estimation du prix du bien en Euro.                                                                                                                                                                                                                                                                                                               | `number` |                     | *626000*                                   |
| Results[i].macroLocation.code                         | Code INSEE de la commune ou de l'arrodissement municipal où est situé le bien.                                                                                                                                                                                                                                                                    | `number` |                     | *75101*                                    |
| Results[i].macroLocation.name                         | Nom de la commune ou de l'arrodissement municipal où est situé le bien.                                                                                                                                                                                                                                                                           | `string` |                     | *Paris 1er Arrondissement*                 |
| Results[i].microLocationValue                         | Score indiquant la qualité de la microlocalisation du bien par rapport au reste de la commune ou de l'arrondissement municipal où est situé le bien.                                                                                                                                                                                              | `number` | Entre 1 et 5.       | *3.5*                                      |
| Results[i].statisticalPriceRangeMin                   | Estimation minimale du prix du bien.                                                                                                                                                                                                                                                                                                              | `number` |                     | *530000*                                   |
| Results[i].statisticalPriceRangeMax                   | Estimation maximale du prix du bien.                                                                                                                                                                                                                                                                                                              | `number` |                     | *720000*                                   |
| Results[i].statisticalTrafficLight.trafficLight       | Indicateur d'atypicité du bien. Ici l'atypicité mesure la similarité du bien avec les biens présents sur le marché en se basant sur des caratéristiques comme la superficie, le nombre de pièce, l'étage, …etc. Généralement, moins le bien est atypique, plus fiable est l'estimation. 1: bien typique, 2: bien atypique, 3: bien très atypique. | `number` | 1, 2, 3             | *2*                                        |
| Results[i].statisticalTrafficLight.classificationText | Couleur indiquant l'atypicité. GREEN: bien typique, YELLOW: bien atypique, RED: bien très atypique.                                                                                                                                                                                                                                               | `string` | RED, YELLOW, GREEN. | *RED*                                      |
| Results[i].messages                                   | Liste des messages d'erreur en cas d'échec de la requête.                                                                                                                                                                                                                                                                                         | `array`  |                     | *Voir l'exemple de réponse avec erreur.*   |

### Codes erreur

|   Code | Description          |
|-------:|:---------------------|
|    200 | Successful operation |
|    401 | Unauthorized         |
|    403 | Forbidden            |
|    404 | Not Found            |

# Règles de facturation
Sauf cas spécial, nous entendons par "facturation par requête" une **facturation par estimation**. Une requête avec deux biens à estimer est comptabilisée comme deux estimations.
### Exemple
#### Requête 1: un seul bien
```bash
curl -X 'POST' \
  'https://dimex.datatest.ch/v0/hed-fr-sfh' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic <token>' \
  -d '{
  "model": "2024Q2",
  "requests": [
    {
      "valuationDate": "2024-09-29",
      "address": "47 Rue Carnot, 92150 Suresnes",
      "constructionYear": 1990,
      "area": {
        "usableArea": 80
      },
      "numberOfRooms": 4,
      "conditionManualValue": 3.5,
      "houseType": "CHALET",
      "landPlotArea": 200,
      "numberOfFloors": 3,
      "energyGrade": "A"
    }
  ]
}'
```
#### Requête 2: deux biens
```bash
curl -X 'POST' \
  'https://dimex.datatest.ch/v0/hed-fr-sfh' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic <token>' \
  -d '{
  "model": "2024Q2",
  "requests": [
    {
      "valuationDate": "2024-09-29",
      "address": "47 Rue Carnot, 92150 Suresnes",
      "constructionYear": 1990,
      "area": {
        "usableArea": 80
      },
      "numberOfRooms": 4,
      "conditionManualValue": 3.5,
      "houseType": "CHALET",
      "landPlotArea": 200,
      "numberOfFloors": 3,
      "energyGrade": "A"
    },
    {
      "valuationDate": "2024-09-29",
      "address": "29 Rue des Carnets, 92140 Clamart",
      "constructionYear": 2003,
      "area": {
        "usableArea": 120
      },
      "numberOfRooms": 5,
      "conditionManualValue": 3.5,
      "houseType": "CHALET",
      "landPlotArea": 200,
      "numberOfFloors": 3,
      "energyGrade": "A"
    }
  ]
}'
```
Dans cette exemple, la requête 1 sera comptabilisée comme une seule estimation alors que la requête 2 sera, quant à elle, comptabilisée comme deux estimations. 

Si le prix de la requête est par exemple **0.2€**, le requête 1 sera facturée à **0.2€** et la requête 2 sera facturée à **0.4€**.