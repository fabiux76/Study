
[Questa](https://www.youtube.com/playlist?list=PLmexTtcbIn_gP8bpsUsHfv-58KsKPsGEo) è  la playlisti di 52 video
Qui metto le info, i punti più significativi da ricordare


# 1) Setting UP the Serverless Framework with AWS - Configure AWS Credentials

Niente di nuovo. 
- Creare utente with programmatic access and administration rights
- Install serverless:
    `npm install -g serverless`
- Setup credentials
    `serverless config credentials --provider aws -- key <ID> --secret <SECRET> -- profile servelessUser`
    Immagino questo comando abbia creato il profilo direttamente nel .aws config files

# 2) Creating a new Serverless Project and deploying a Lambda

- `serverless create --template aws-nodejs -path myServerlessProject`
    Questo ha creato `handler.js` e `serverless.yaml`
- add `profile` to `provider` section
- `serverless deploy`

## Cose da ricordare, anche se non vengono dette 

I comandi serverless possono prendere come opzioni:
- `--stage` or `-s` The stage in your service you want to display information about.
- `--region` or `-r` The region in your stage that you want to display information

Come funziona il discorso degli stage (spiegato anche [qui](https://serverless-stack.com/chapters/stages-in-serverless-framework.html))
Perchè poi il discorso si intreccia anche con gli stage di Api Gateway.
In realtà, come è spiegato in quell'articilo, Serverless usa separate API for each stage (cioè non usa il meccanismo di staging di API Gateway)

As mentioned above, a new stage is a new API Gateway project. To deploy to a specific stage, you can either specify the stage in the serverless.yml.

```yaml
service: service-name

provider:
  name: aws
  stage: dev
```

Or you can specify the stage by passing the `--stage` option to the `serverless deploy` command.

```
serverless deploy --stage dev
```

Se stage non viene specificato secondo me il default è `dev`

Non ho poi capito se in realtà l'override di stage da cli rispetto a quello nel file è automatico o no, perchè ad esempio spesso c'è qualcosa di questo tipo

```yaml
custom:
  myStage: ${opt:stage, self:provider.stage}
```
This is telling Serverless Framework to use the `--stage` CLI option if it exists. And if it does not, then use the default stage specified by `provider.stage`
E molto spesso viene usata questa modalità per gestire la cosa.

Anche alcune risorse che vengono create poi presentano il valore di stage in parte del nome (per esempio questo succede per le funzioni, ma non ad esempio per i nomi dei bucket)

# 3) How to Deploy an S3 bucket and Upload Data

- Aggiungere le risorse
```yaml
resources:
    Resources:
        DemoBucketUpload:
            Type: AWS::S3::Bucket
            Properties:
                BucketName: UniqueBucketName
```
- Di nuovo `sls deploy`
    In questo caso il nome del bucket sarà proprio solo `UniqueBucketName` senza riferimento allo stage
- Aggiugna il plugin `serverless-s3-sync`
    Questo server per sincronizzare il contentuo di un file locale dentro il bucket     
```yaml
plugins:
    - serverless-s3-sync

custom:
    s3Sync:
        - bucketName: UniqueBucketName
          localDir: UploadData
```
- Ora quando faccio `sls-deploy` farà anche la sincronizzazione del bucket

# 4) Creating an API with Serverless

Niente di nuovo. 
- Fa una lamda. Interessante il moldulono che scrive per gestire le risposte (con header, cors, stringify)
- Aggiunge evento `http`

# 5) Adding Serverless Webpack to your Project

Obiettivo: deployare solo il codice che ci serve

- Aggiungo `serverless.webpack` plugin
-   ```
    package:
        individually: true
    ```
- `npm install --sabe serverless-webpack`
- `npm install --save webpack`
- Aggiungere `webpack.config.js`
- ```javascript
    module.exports = {
        target: 'node',
        mode: 'production'
    }
  ```
- `sls deploy` ora usa webpack per comprimere i sorgenti
- Se non vogliamo il minifier, pasta cambiare `mode: 'node'`. In questo modo abbiamo cmq solo il codice della funzione, ma non compressa cambiando i nomi delle variabile, ecc...

# 6) Create a Serverless Database - DynamoDB with the Serverless Framework

- Aggiungere la risorsa
```yaml
resources:
    Resources:
        MyDinamoTable:
            Type: AWS::DynamoDB::Table
            Properties:
                TableName: ${self.custom.tableName}
                AttributeDefinitions:
                    - AttributeName: ID
                      AttributeType: S
                KeySchema:
                    - AttributeName: ID
                      KeyType: HASH
                BillingMode: PAY_PER_REQUEST 
```
`PAY_PER_REQUEST` è una scelta che ha fatto lui, non è detto che sia la migliore in tutti i contesti

- Per il nome della tabella
```yaml
custom:
    tableName: player-points
```