[Qui](https://www.serverless.com/plugins/serverless-offline) la documentazione

# Installazione ed uso

Facilissimo:

1 `npm install serverless-offline --save-dev`
2 Then inside your project's serverless.yml file add following entry to the plugins section: serverless-offline. If there is no plugin section you will need to add it to the file. 
```yml
plugins:
  - serverless-offline
```
3 Run with `serverless offline` or `sls offline`.


Ci sono un sacco di opzioni (per vederle `sls offline --help`) che possono essere aggiunte al comando cli, oppure nel `serverless.yml`

Per invocarla poi da cli:

```	
aws lambda invoke /dev/null \
  --endpoint-url http://localhost:3002 \
  --function-name myServiceName-dev-invokedHandler
```

Anche se non ho capito a cosa server... Se voglio invocare la lambda in locale (senza gateway) posso pur sempre usare `serverless invoke local`, no?)

# Debug

Sarebbe interessante capire se effettivamente riesco a fare debug come spiegato, perchè con `serverless invoke local` ci ero riuscito... MA SI E' uguale, ma devo aggiungere qualche parametro. [Qui](https://www.youtube.com/watch?v=xaBAZm2jfXQ&t=98s) è spiegato tutto perfettamente!!!!


Guardare anche il [video youtube](https://www.youtube.com/watch?v=ul_85jfM0oo)!!!
In realtò questo è molto incetrnato su `serverless dynamodb local` 
Per il **debug** quello da guardare è [questo](https://www.youtube.com/watch?v=xaBAZm2jfXQ) 