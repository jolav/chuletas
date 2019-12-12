# .NET CORE 3.1 (DOTNET CORE)

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

### VSCODE

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

* **No encuentra el SDK**

```sh
ln -s /ruta/al/sdk/dotnet/dotnet /usr/local/bin/
```

### MONO

[Instalar en linux Mono](https://www.mono-project.com/download/stable/#download-lin-debian)

```sh
apt install apt-transport-https dirmngr gnupg ca-certificates
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 
3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
echo "deb https://download.mono-project.com/repo/debian stable-buster main" 
tee /etc/apt/sources.list.d/mono-official-stable.list
apt update
apt install mono-devel
```

---

## COMANDOS

`dotnet --info`  

`dotnet run`  

`dotnet watch run`

---

