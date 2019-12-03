# .NET CORE 3.0 (DOTNET CORE)

---

## INSTALACION

[descargar el binario SDK de aqui](https://dotnet.microsoft.com/download/dotnet-core) y descomprimirlo donde queramos (no hace falta ser root)

```sh
// como usuario normal
nano $HOME/.bashrc 
nano $HOME/.profile 

// añadir a cada uno

# DotNet
export PATH=$PATH:$HOME/Documentos/dotnet
# Estas mas adelante las explico
# Add .NET Core SDK tools
export PATH="$PATH:$HOME/.dotnet/tools"
export DOTNET_ROOT="$(dirname $(which dotnet))"
```

Para recargar la configuracion  
`source ~/.profile`  
`source ~/.bashrc`  

* **Formatear codigo a mi gusto**  

Al instalar el plugin para C# de VSCode crea una carpeta $HOME/.omnisharp.  
Dentro ponemos un archivo `omnisharp.json`

[Mas informacion aqui](https://github.com/OmniSharp/omnisharp-vscode/issues/313)

```json
{
  "FormattingOptions": {
    "NewLinesForBracesInLambdaExpressionBody": false,
    "NewLinesForBracesInAnonymousMethods": false,
    "NewLinesForBracesInAnonymousTypes": false,
    "NewLinesForBracesInControlBlocks": false,
    "NewLinesForBracesInTypes": false,
    "NewLinesForBracesInMethods": false,
    "NewLinesForBracesInProperties": false,
    "NewLinesForBracesInAccessors": false,
    "NewLineForElse": false,
    "NewLineForCatch": false,
    "NewLineForFinally": false
  }
}
```

---

## COMANDOS

`dotnet --info`  

`dotnet run`  

`dotnet watch run`

---

