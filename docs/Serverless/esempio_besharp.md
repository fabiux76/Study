# Repo

Faccio riferimento a [questo repo](https://github.com/besharpsrl/blog-serverless-backend-nodejs) 
[Qui](https://www.proud2becloud.com/it/costruiamo-un-backend-serverless-con-typescript-nodejs-e-aws-lambda/) invece l'articolo in cui lo presentano

Cose interessanti

# docker-compose
docker-compose usato per far partire un'istanza locale di postgres per i test in locale

# strumenti\librerie utilizzate
- **express**. La lamda è in realtà un'istanza di express, quindi la stessa lambda in realtà serve molte rotte, a differenza della soluzione del liveproject in cui ogni lambda era per una rotta separata)
- **tsoa**. PEr fare documentazione swagger e definire rotte... (?)
- **sequelize**. Dovrebbe essere un ORM
- **serverless-offline**. Per fare test in locale
- **serverless-http**. Questo secondo me è quello che ci fa collegare il mondo lambda al mondo express
- uso dei labmda layers


# comandi di script
```json
  "scripts": {
    "generate-swagger": "./node_modules/tsoa/dist/cli.js swagger",
    "generate-routes": "./node_modules/tsoa/dist/cli.js routes",
    "build": "./node_modules/typescript/bin/tsc",
    "prebuild": "npm run generate-routes && ./node_modules/tslint/bin/tslint -c tslint.json -p tsconfig.json --fix && rm -rf dist/",
    "prestart": "npm install && npm run build",
    "start": "isLocal=true NODE_ENV=dev sls offline start",
    "pre-deploy": "rm -rf node_modules .serverless && npm install --production && npm run build && ./prepare-layer.sh",
    "deploy-dev": "npm run pre-deploy && NODE_ENV=dev sls deploy",
    "migrate-db-local": "NODE_ENV=local ./node_modules/sequelize-cli/lib/sequelize db:migrate",
    "migrate-db-dev": "NODE_ENV=dev ./node_modules/sequelize-cli/lib/sequelize db:migrate"
  },
```

Interessante:
- il `build` fa la compilazione del typescript in javascript. Prima però il `prebuild fa diverse cose`:
    - `./node_modules/tsoa/dist/cli.js routes`
    - `./node_modules/tslint/bin/tslint -c tslint.json -p tsconfig.json --fix`
    - `rm -rf dist/`
- lo `start` e `prestart` servono per fare il deploy in locale. 
    - installazione dei pacchetti: `npm install`
    - step di build: `npm run build`
    - start del plugin sls offline: `isLocal=true NODE_ENV=dev sls offline start`
- il `pre-deploy` e `deploy-dev` fanno invece il deploy su ambiente dev. Fondamente fanno:
    - clean = `rm -rf node_modules .serverless`    
    - installazione dei pacchetti di produzione = `npm install --production`
    - step di build: `npm run build`
    - Preparazione del layer: `./prepare-layer.sh`
    - deploy vero e proprio serverless: `NODE_ENV=dev sls deploy`

Quindi secondo me, per fare il deploy.
In locale:
- `docker-compose up`
- `npm run migrate-db-local`
- `npm run start`

PEr depolyare su ambiente dev:
- `npm run deploy-dev`
Poi però devo fare 
- `npm run migrate-db-dev` da una macchina (Bastion host??? ma non ho cpaito come può funzionare... , sul bastion host mica c'è quel codice...)

# tsoa

**tsoa**, ovvero *OpenAPI-compliant REST APIs using TypeScript and Node* è [qui](https://tsoa-community.github.io/docs/)

tsoa is a framework with integrated OpenAPI compiler to build Node.js serve-side applications using TypeScript. It can target express, hapi, koa and more frameworks at runtime. tsoa applications are type-safe by default and handle runtime validation seamlessly

Fondamentalmente possiamo interpretare questo progetto di besharp come:
- progetto di rest **tsoa** basato su **express**
- che per lo storage dei dati usa **postgress** attraverso la libreria **sequelize**
- invece che essere esposto come server a se stante usa **serverless** per essere deployato come lambda, e **serverless-http** come strumento per linkare il modello di invocazione delle lambda a quello di express
- usa **serverless-offline** per permettere di fare i test in locale 

Nel progetto vengono usati questi due comandi rispettivamente per la generazione della codumentazione swagger ed il codice delle rotte:
```
"generate-swagger": "./node_modules/tsoa/dist/cli.js swagger",
"generate-routes": "./node_modules/tsoa/dist/cli.js routes",
```

Mentre nella [documentazione](https://tsoa-community.github.io/docs/generating.html#using-cli) di tsoa vengono usati questi:

```
# generate OAS
tsoa spec

# generate routes
tsoa routes
```

Forse loro usavano una versione un po' vecchia

Approfondisco questo tema molto interessante [qui](../Typescript/tsoa.md)

# serverless-offline

Qui il [link](https://www.serverless.com/plugins/serverless-offline). È un plugin.

This Serverless plugin emulates AWS λ and API Gateway on your local machine to speed up your development cycles. To do so, it starts an HTTP server that handles the request's lifecycle like APIG does and invokes your handlers.

Approfondisco [qui](serverless-offline.md)

# Layers

Usano questo approccio

```yml
package:
  exclude:
    - node_modules/**
    - src/**

layers:
  nodeModules:
    path: layer
    compatibleRuntimes:
      - nodejs12.x
```

Cioè tutte le dipendenze node le sposta nella directory `layer`.... Ma questo secondo me non è il corretto uso

`prepare-layer.sh` fa questo:

```
mkdir -p layer/nodejs
cp -r ./node_modules ./layer/nodejs
cp package.json ./layer/nodejs
```

NElla docuemntazione scrivono:
Con questa configurazione, infatti, andremo a creare un Lambda Layer contenente tutti i node modules. Così facendo, alleggeriremo notevolmente le dimensioni della nostra funzione ottenendo vantaggi in performance durante la sua esecuzione.

Quindi comq forse ha senso valutare... Sicoome è stta fatta una lambda con express forse effettivamente è un po' ciccia


# serverless-http
[This module](https://www.npmjs.com/package/serverless-http) allows you to 'wrap' your API for serverless use. No HTTP server, no ports or sockets. Just your code in the same execution pipeline you are already familiar with.

# AWS Secret Manager
Usa secret manager pee recuperare le credenziali di accesso al DB


# PROVA

PRovo a vedere se effettivamente funziona e cosa bisogna cambiare
Steps:
1 `npm install`
2 `npm run build` fallisce su pc per discorso di percorsi, ma funziona su mac. Continuo le prove su mac
3 `docker-compose up`
4 `npm run migrate-db-local`
5 `npm run start` (che poi in realtà i primi 2 step)

E funziona. Riesco a fare la chiamata con postman in locale !
(verificare se riesco anche a fare il debug locale!!!!)

6 `npm run deploy-dev`. 
Cioè, provo fare deploy su aws. Fa effettivamente un sacco di pasaggi come il caricamento dei zip sia per lambda che per i layer, ma poi fallisce perchè nel file yaml è specificato vpc esplicito
Dopo aver corretto vpc e subnet, mettendo quelle del mio account sono effettivamente riuscito a fare il deploy. [Qui](https://eu-west-1.console.aws.amazon.com/lambda/home?region=eu-west-1#/functions/express-serverless-dev-app?tab=code) i dettagli della lambda deployata.
Effettivamente c'è un layer, doe sono i node_modules (che non sono nl codice della lambda)

