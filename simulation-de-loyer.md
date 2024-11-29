# API Simulation de Loyer

L'API Simulation de Loyer permet d'estimer le loyer du marché d'un appartement en se basant sur les caractéristiques et la localisation du bien.

# Authentification

Lorsque vous souscrivez à l'API **Simulation de Loyer**, un token vous sera fourni. Ajoutez le au Header de votre requête pour vous authentifier.

```bash
curl -X POST 'https://dimex.datatest.ch/{version}/hed-fr-rent' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic <token>' \
```

Il faut aussi renseigner la version de l'API. **La seule version disponible à ce jour est `v0`**.

# Utilisation

L'API **Simulation de Loyer** comporte deux Endpoints: un pour lister les modèles disponibles, et le deuxième pour estimer la valeur locative d'un appartement.

## Modèles disponibles

Généralement, un nouveau modèle est produit tous les semestres.

### Exemple de Requête

```bash
curl -X GET 'https://dimex.datatest.ch/{version}/hed-fr-rent-available-models' \
  -H 'Authorization: Basic <token>'
```

### Exemple de Réponse

```bash
"2024Q3"
```

## Estimation

### Exemple de Requête

```bash
curl -X 'POST' 'https://dimex.datatest.ch/{version}/hed-fr-rent' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic <token>' \
  -d '{
    "model": "2024Q2",
    "requests": [
        {
            "index": "1",
            "externalId": "12345-123a",
            "valuationDate": "2024-11-26",
            "address": "30 Rue des Carnets, 92140 Clamart",
            "constructionYear": 2018,
            "renovationYear": 2018,
            "energyGrade": "C",
            "area": {
                "usableArea": 70
            },
            "numberOfRooms": 3,
            "floor": 2,
            "numberOfFloors": 5,
            "numberOfParkingSpaces": 1,
            "hasElevator": false,
            "hasBalcony": false,
            "hasTerrace": false,
            "hasGarden": false,
            "hasFireplace": false,
            "hasAirConditioning": false
        }
    ]
}'
```

### Description des champs

| Champ                                                                    | Description                                                                        | Type      | Valeurs Acceptées                      | Exemple                             |
|:-------------------------------------------------------------------------|:-----------------------------------------------------------------------------------|:----------|:---------------------------------------|:------------------------------------|
| model<span style="color:red;"><sup>*</sup></span>                        | Modèle utilisé pour l'estimation.                                                  | `string`  | Voir la section "modèles disponibles". | *2024Q3*                            |
| requests<span style="color:red;"><sup>*</sup></span>                     | Liste des biens à estimer.                                                         | `array`   | Voir l'exemple de requête.             |                                     |
| requests[i].index                                                        | Indice utilisé pour faire correspondre les requêtes aux réponses d'une estimation. | `number`  |                                        | *1*                                 |
| requests[i].externalId                                                   | Paramètre qui peut être utilisé pour le logging ou le debugging.                   | `string`  |                                        | *12345-123a*                        |
| requests[i].valuationDate<span style="color:red;"><sup>*</sup></span>    | Date d'estimation au format aaaa-mm-jj.                                            | `string`  |                                        | *2024-09-30*                        |
| requests[i].address<span style="color:red;"><sup>*</sup></span>          | Addresse du bien à estimer.                                                        | `string`  |                                        | *30 Rue des Carnets, 92140 Clamart* |
| requests[i].constructionYear<span style="color:red;"><sup>*</sup></span> | Année de construction.                                                             | `number`  | Entre 1000 et 3000.                    | *1975*                              |
| requests[i].renovationYear                                               | Année de la rénovation la plus récente.                                            | `number`  |                                        | *2018*                              |
| requests[i].energyGrade<span style="color:red;"><sup>*</sup></span>      | Classe énergétique.                                                                | `string`  | A, B, C, D, E, F, G                    | *A*                                 |
| requests[i].area.usableArea<span style="color:red;"><sup>*</sup></span>  | Surface habitable.                                                                 | `number`  | Entre 9 et 275.                        | *60*                                |
| requests[i].numberOfRooms<span style="color:red;"><sup>*</sup></span>    | Nombre de pièces.                                                                  | `number`  | Entre 1 et 10.                         | *3*                                 |
| requests[i].floor<span style="color:red;"><sup>*</sup></span>            | Numéro de l'étage.                                                                 | `number`  | Entre -1 et 30.                        | *2*                                 |
| requests[i].numberOfFloors<span style="color:red;"><sup>*</sup></span>   | Nombre d'étages.                                                                   | `number`  | Entre 1 et 30.                         | *3*                                 |
| requests[i].numberOfParkingSpaces                                        | Nombre de places de parking.                                                       | `number`  | Supérieures à 0.                       | *1*                                 |
| requests[i].hasElevator                                                  | Le bien  est avec ascenseur.                                                       | `boolean` |                                        | *true*                              |
| requests[i].hasBalcony                                                   | Le bien  est avec balcon.                                                          | `boolean` |                                        | *true*                              |
| requests[i].hasTerrace                                                   | Le bien  est avec terrasse.                                                        | `boolean` |                                        | *true*                              |
| requests[i].hasGarden                                                    | Le bien  est avec jardin.                                                          | `boolean` |                                        | *true*                              |
| requests[i].hasFireplace                                                 | Le bien  est avec cheminée.                                                        | `boolean` |                                        | *false*                             |
| requests[i].hasAirConditioning                                           | Le bien  est climatisé.                                                       | `boolean` |                                        | *false*                             |

### Exemple de Réponse

```json
{
  "results": [
    {
      "index": "1",
      "error": null,
      "lon": 2.327675,
      "lat": 48.869687,
      "address": "1 Boulevard de la Madeleine, 75001 Paris",
      "macroLocation": {
        "code": 75101,
        "name": "Paris 1er Arrondissement"
      },
      "microLocationValue": 3,
      "marketShallIncomePerUnitYear": 34100,
      "marketShallIncomePerUnitMonth": 2840,
      "marketShallIncomePerArea": 426,
      "statisticalPriceRangeMin": 375,
      "statisticalPriceRangeMax": 486,
      "statisticalTrafficLight": "red",
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
      "index": "1",
      "messages": [
        {
          "description": "VALIDATION_ERROR",
          "level": "WARN",
          "reference": "4NyswrbaqzpeZiKpFBjY",
          "message": "The Rent FR model is unable to perform this calculation (endpoint /hed-fr-rent). Validation error: area.usableArea is not a number in range (9.000000, 275.000000)"
        }
      ]
    }
  ]
}
```

### Description des champs de la réponse

| Champ                                    | Description                                                                                                                                                                                                                                                                                  | Type     | Valeurs Possibles   | Exemple                                  |
|:-----------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------|:--------------------|:-----------------------------------------|
| results                                  | Liste des résultats d'estimation des biens.                                                                                                                                                                                                                                                  | `array`  |                     |                                          |
| results[i].index                         | Indice utilisé pour faire correspondre les requêtes aux réponses d'une estimation.                                                                                                                                                                                                           | `number` |                     |                                          |
| results[i].error                         | Information sur l'erreur en cas d'échec de la requête. Ce champ est presque toujours null. C'est plutôt le champ message qui est utile.                                                                                                                                                      | `json`   |                     |                                          |
| results[i].lon                           | Longitude de la position du bien.                                                                                                                                                                                                                                                            | `number` |                     | *2.259016*                               |
| results[i].lat                           | Latitude de la position du bien.                                                                                                                                                                                                                                                             | `number` |                     | *48.789286*                              |
| results[i].address                       | Addresse du bien                                                                                                                                                                                                                                                                             | `string` |                     | *30 Rue des Carnets, 92140 Clamart*      |
| results[i].macroLocation.code            | Code INSEE de la commune ou de l'arrondissement municipal où est situé le bien.                                                                                                                                                                                                              | `number` |                     | *92023*                                  |
| results[i].macroLocation.name            | Nom de la commune ou de l'arrondissement municipal où est situé le bien.                                                                                                                                                                                                                     | `string` |                     | *Clamart*                                |
| results[i].microLocationValue            | Score indiquant la qualité de la microlocalisation du bien par rapport au reste de la commune ou de l'arrondissement municipal où est situé le bien.                                                                                                                                         | `number` | Entre 1 et 5.       | *3.5*                                    |
| results[i].marketShallIncomePerUnitYear  | Estimation du loyer annuel du bien en Euro.                                                                                                                                                                                                                                                  | `number` |                     | *15500*                                  |
| results[i].marketShallIncomePerUnitMonth | Estimation du loyer mensuel du bien en Euro.                                                                                                                                                                                                                                                 | `number` |                     | *1300*                                   |
| results[i].marketShallIncomePerArea      | Estimation du loyer annuel par mètre carré du bien en Euro.                                                                                                                                                                                                                                  | `number` |                     | *222*                                    |
| results[i].statisticalPriceRangeMin      | Fourchette inférieure de l’estimation du loyer annuel par mètre carré du bien en Euro.                                                                                                                                                                                                                      | `number` |                     | *195*                                    |
| results[i].statisticalPriceRangeMax      | Fourchette supérieure de l’estimation du loyer annuel par mètre carré du bien en Euro.                                                                                                                                                                                                                      | `number` |                     | *253*                                    |
| results[i].statisticalTrafficLight       | Couleur indiquant l'atypicité du bien.  Ici l'atypicité mesure la similarité du bien avec les biens présents sur le marché en se basant sur des caratéristiques comme la superficie, le nombre de pièce, l'étage, …etc. green: bien typique, yellow: bien atypique, red: bien très atypique. | `string` | red, yellow, green  | *green*                                  |
| results[i].messages                      | Liste des messages d'erreur en cas d'échec de la requête.                                                                                                                                                                                                                                    | `array`  |                     | *Voir l'exemple de réponse avec erreur.* |

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
curl -X 'POST' 'https://dimex.datatest.ch/{version}/hed-fr-rent' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic <token>' \
  -d '{
    "model": "2024Q2",
    "requests": [
        {
            "index": "1",
            "externalId": "12345-123a",
            "valuationDate": "2024-11-26",
            "address": "30 Rue des Carnets, 92140 Clamart",
            "constructionYear": 2018,
            "energyGrade": "C",
            "area": {
                "usableArea": 70
            },
            "numberOfRooms": 3,
            "floor": 2,
            "numberOfFloors": 5
        }
    ]
}'
```
#### Requête 2: deux biens
```bash
curl -X 'POST' 'https://dimex.datatest.ch/{version}/hed-fr-rent' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic <token>' \
  -d '{
    "model": "2024Q2",
    "requests": [
        {
            "index": "1",
            "externalId": "12345-123a",
            "valuationDate": "2024-11-26",
            "address": "30 Rue des Carnets, 92140 Clamart",
            "constructionYear": 2018,
            "energyGrade": "C",
            "area": {
                "usableArea": 70
            },
            "numberOfRooms": 3,
            "floor": 2,
            "numberOfFloors": 5
        },
        {
            "index": "1",
            "externalId": "12345-123a",
            "valuationDate": "2024-11-26",
            "address": "30 Rue des Carnets, 92140 Clamart",
            "constructionYear": 2018,
            "energyGrade": "C",
            "area": {
                "usableArea": 70
            },
            "numberOfRooms": 3,
            "floor": 2,
            "numberOfFloors": 5
        }
    ]
}'
```
Dans cet exemple, la requête 1 sera comptabilisée comme une seule estimation alors que la requête 2 sera, quant à elle, comptabilisée comme deux estimations. 

Si le prix de la requête est de **0.2€** par exemple, la requête 1 sera facturée à **0.2€** et la requête 2 sera facturée à **0.4€**.