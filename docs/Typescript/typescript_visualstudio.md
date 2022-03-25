Faccio riferimento a questo documento
[TypeScript tutorial in Visual Studio Code](https://code.visualstudio.com/docs/typescript/typescript-tutorial)

Visual Studio Code includes TypeScript language support but does not include the TypeScript compiler, `tsc`. You will need to install the TypeScript compiler either globally or in your workspace to transpile TypeScript source code to JavaScript (`tsc HelloWorld.ts`).

Quindi nell'esempio poi lo installa globalmente:

`npm install -g typescript`

Another option is to install the TypeScript compiler locally in your project (`npm install --save-dev typescript`) and has the benefit of avoiding possible interactions with other TypeScript projects you may have.

# Debugging

VS Code has built-in support for TypeScript debugging. To support debugging TypeScript in combination with the executing JavaScript code, VS Code relies on source maps for the debugger to map between the original TypeScript source code and the running JavaScript. You can create source maps during the build by setting "sourceMap": true in your tsconfig.json.

E così facendo, senza nemmeno fare un `launch.json` file, con il file `.ts` che volgio debuggare aperto, posso farlo!

Qua cominciamo a guardare questa pagina più di dettaglio su [Compiling](https://code.visualstudio.com/docs/typescript/typescript-compiling)

# Compiler vs language service (!)

It is important to keep in mind that VS Code's TypeScript language service is separate from your installed TypeScript compiler. You can see the VS Code's TypeScript version in the Status Bar when you open a TypeScript file.

# Transpile TypeScript into JavaScript

Praticamente visua studio permette di fare la traspilazione in watch! (senza ts-node)

Execute Run Build Task (Ctrl+Shift+B) from the global Terminal menu. If you created a tsconfig.json file in the earlier section, this should present the following picker:

Tooop

```
> Executing task: tsc -p c:\projects\typescript\HelloWorld\tsconfig.json --watch <

[19:16:25] Starting compilation in watch mode...

[19:16:25] Found 0 errors. Watching for file changes.
```


Poi manca da esplorare in dettaglio [typescript-debugging](https://code.visualstudio.com/docs/typescript/typescript-debugging)
