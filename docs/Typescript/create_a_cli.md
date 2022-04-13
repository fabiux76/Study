Seguo questo articolo

https://itnext.io/how-to-create-your-own-typescript-cli-with-node-js-1faf7095ef89


In realtà alla base di tutto c'è questo:
https://www.npmjs.com/package/commander


Cmq in realtà è molto carini il primo link, permette proprio di isntallare il comand globalmente con npm (ciè la parte `bin` nel `package.json`) e poi lo ritrovo tra i comandi (posso fare da shell semplicemente `sssusers`) e lo me lo ritrovo anche nei packages:

`npm list -g --depth=0`

+-- @aws-amplify/cli@6.3.1
+-- UNMET PEER DEPENDENCY @types/node@*
+-- serverless@3.10.0
+-- sssusers@1.0.0 -> C:\projects\SSSUsers
+-- ts-node@10.4.0
`-- typescript@4.4.4

Domande
1 Come faccio a fare Shebang per windows? Cioè fare un executable da un file?
