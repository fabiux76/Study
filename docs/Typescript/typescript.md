# Code Editor Setup 
From O'Reilly **Programming TypeScript**

**TSC** is a command-line application written in TypeScript. This is the compiler
**TSCLint** this is a linter

## Setup inside existing npm project

`npm intall --save-dev typescript tslint @types/node`

## Configuration

### tsconfig.json
File `tsconfig.json` da creare nella root del progetto (`touch tsconfig.json`)
Tra i parametri principali:

- `include`: dir con sorgenty ts
- `lib`: es `["es2015"]`
- `module`: specifica il module system da usare, es `commonjs`
- `outDir`
- `strict`
- `target`: "es2015"

### tslint.json
Configurazione stylistic conventions (es tab vs spaces, trailing commas, etc)

## Run

### Basic
Sicuramente non l'approccio migliore (poi vedremo modi migliori) ma nel libro fa due passaggi

`./node_modules/.bin/tsc` 
`node ./dist/index.js`

## Shortcuts

### ts-node
Use it to compile and run your typescript with a single command

### typescript-node-starter
Skaffolding tool


# Tools

Sia comandi e plugin di Visual Studio Code ad esempio

# Doubts\Questions

1. Debugging using typescript types
2. Non c'era un modo per creare un `tsconfig.json` di default?

