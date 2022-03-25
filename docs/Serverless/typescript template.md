# Intro

In realtà esiste anche il template `aws-nodejs-typescript`

Avevo provato ad usarlo ed avevo visto che tra le altre cose generava anche il `serverless.ts`, qundi non più `yaml`
Potrebbe essere interessante approfondire.

Non sono mai riuscito a farlo andare, dopo aver fatto `npm install` se faccio `serverless deploy` mi da questo errore:

```
λ serverless deploy
Serverless: Running "serverless" installed locally (in service node_modules)

 Error ---------------------------------------------------

  Error: Cannot find module 'esbuild'
  Require stack:
  - c:\projects\Manning\LiveProjects\HelloWorldTS\node_modules\serverless-esbuild\dist\index.js
  - c:\projects\Manning\LiveProjects\HelloWorldTS\node_modules\serverless\lib\classes\PluginManager.js
...  
```

Peccato che [questo link](https://github.com/serverless/serverless/tree/master/lib/plugins/create/templates/aws-nodejs-typescript) non è più funzionante.

C'è [quest'altra risorsa](https://github.com/andrenbrandao/serverless-typescript-boilerplate) che però forse può aiutarci a capire come funziona e le librerie che vengono usate


