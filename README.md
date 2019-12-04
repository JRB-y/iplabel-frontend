# Exercice évaluation recrutement **R&D ip-label** (vue.js)

Cet exercice a pour sujet la réalisation d’une application permettant la
recherche de films dans l’api http://www.omdbapi.com/ (api key **\*\*\*\*\*\*\***)

Cette application web doit être réalisée en vue.js en utilisant
exclusivement les composants de https://vuetifyjs.com/en/

### Table of Contents

**[Technology](#1-technology)**<br>
**[Node dependencies](#2-node-dependencies)**<br>
**[Hosting](#3-hosting)**<br>
**[Mongodb hosting](#4-mongodb-hosting)**<br>
**[Installation Instructions](#5-installation-Instructions)**<br>
**[API description](#6-api-description)**<br>

## 1. Technology

1. vue.js
2. vuetify

## 2. Node dependencies

- core-js: 3.4.3,
- vue: 2.6.10,
- vue-router: 3.1.3,
- vuetify: 2.1.0

## 3. Hosting

This vue application is hosted on [HEROKU](https://www.heroku.com)

Link: http://iplabel-backend.herokuapp.com/

## 5. Installation Instructions

1. Clone the github repository.

   `> git clone https://github.com/JRB-y/iplabel-backend.git`

2. Inside the folder install the node modules dependecies:

   `> npm install`

3. Rename the `.env.example` to `.env` and configure your port and url.

   ```
   PORT= { your port }
   DB_CONNECTION={your mongoDB connection string }
   ```

   :warning: It's not secure to expose the DB_CONNECTION string!

## 6. API description

### 1. NUMBERS

List of numbers of the API.

    number {
        n: Number,
        created_at: Date
    }

#### Available methods

1. `GET /numbers` return all numbers
2. `POST /numbers` create a new number with the value of `created_at = Date().now`
3. `GET /numbers/:n` return a specific number
4. `DELETE /numbers/:n` delete a specific number

### 2. MATHS

Do somme maths on the numbers

#### Available methods

1.  `GET /maths/somme` Get the sum of all numbers.
2.  `GET /maths/moyenne` Get the average of all numbers.
3.  `GET /maths/mediane` Get the median of all numbers
4.  `POST /maths/custom` Make a custom math operation

    In the Request Body we need to have an `operator` and `nbrToExec`.

        1. opeartor: Can be [ '+', '-', '*', '/' ]
        2. nbrToExec: Have to be a Number
