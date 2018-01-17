El Bot Builder SDK para Node.js es un marco para el desarrollo de bots. Es fácil de usar y conectar con otras bibliotecas Node, como Express & Restify.

Este tutorial le ayudará a configurar su entorno y construir un bot simple mediante el SDK de Bot Builder a Node.js. Usted puede probar el bot en una ventana de consola y con el Bot Framework Emulator.

Una cuenta de Microsoft Azure es necesaria para poder usar  el servicio de Azure Bot Service. Si aún no tiene una cuenta activa, es posible registrarse para una en [**Trial Gratuito**](https://aka.ms/bots-azure-free).

## Pre-requisitos:
* Instalar  [Node.js](https://nodejs.org/) em sua máquina
* Instalar  [Bot Emulator](http://emulator.botframework.com/)
* Instalar  [Visual Studio Code](code.visualstudio.com)
* Crear una carpeta para su bot y abrirla en Visual Studio Code.
* En la terminal de Visual Studio Code, debe ejecutar el comando siguiente:
```javascript=
npm init
```

Siga los pasos que aparecerán en el indicador y añada la información a npm para crear el archivo ** package.json **, que contendrá la información de su bot..

## Instalando SDK
A continuación, instale el SDK de Bot Builder a Node.js ejecutando el siguiente comando npm:
```javascript
npm install --save botbuilder
```

Después de tener el SDK instalado, usted está listo para escribir su primer bot.

Para su primer bot, usted creará un bot que contesta cualquier entrada del usuario. Para la creación de este, siga estos pasos:

* En el mismo proyecto que está trabajando en Visual Studio Code, cree un nuevo archivo llamado app.js.
* Con el app.js abierto, agregue el código siguiente al archivo:

```javascript
var builder = require('botbuilder');

var connector = new builder.ConsoleConnector().listen();
var bot = new builder.UniversalBot(connector, function (session) {
    session.send("Você disse: %s", session.message.text);
});
```

Salve o arquivo. Agora você está pronto para correr e testar seu bot.

## Inicio del bot
Vuelva a abrir la terminal de Visual Studio Code e inicie su bot con el siguiente comando:

```javascript
node app.js
```

Su bot ahora se está ejecutando localmente. Prueba tu bot escribiendo algunos mensajes en la ventana de la consola. Usted debe ver que el bot responde a cada mensaje que usted envía haciendo eco de su mensaje con el prefijo con el texto "Usted dijo:".

## Instalando Restify
Los bots de consola son buenos clientes basados en texto, pero para usar cualquiera de los canales de Bot Framework (o ejecutar su bot en el emulador), su bot debe ejecutarse en un puerto de servidor. Para poder ejecutar su bot en el emulador o publicarlo en algún canal, vamos a utilizar Restify. En su terminal, escriba el siguiente comando npm:

```javascript
npm install --save restify
```

Una vez que tenga Restify instalado, usted está listo para hacer algunos cambios en su bot.

## Editando su bot
Usted necesitará hacer algunos cambios en su archivo ** app.js **.

* Agregue una línea de require al módulo de restify
* Cambie la llamada `` `ConsoleConnector``` a```ChatConnector` ``. 
* Incluya sus credenciales de Microsoft App ID y App Password (puede dejar en blanco cuando se está probando localmente).
* Para instanciar el servicio, debera agregar el siguiente código:

```javascript
var restify = require('restify');
var builder = require('botbuilder');

// Setup Restify Server
var server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
   console.log('%s listening to %s', server.name, server.url); 
});

// Crie um chat conector para se comunicar com o Bot Framework Service
var connector = new builder.ChatConnector({
    appId: process.env.MICROSOFT_APP_ID,
    appPassword: process.env.MICROSOFT_APP_PASSWORD
});

// Endpoint que irá monitorar as mensagens do usuário
server.post('/api/messages', connector.listen());

// Recebe as mensagens do usuário e responde repetindo cada mensagem (prefixado com 'Você disse:')
var bot = new builder.UniversalBot(connector, function (session) {
    session.send("Você disse: %s", session.message.text);
});
```

Guarde el archivo. Ahora usted está listo para ejecutar y probar su bot en el emulador.

## Prueba tu bot
Después de instalar [Bot Emulator] (https://docs.microsoft.com/es-ES/bot-framework/bot-service-debug-emulator), a través de la consola, navegue hasta el directorio de su bot y, a continuación, siguiente comando:

```javascript
node app.js
```

Agora, seu bot está sendo executado localmente.

### Probar el bot en el emulador
Después de iniciar su bot, conéctate a tu bot en el emulador:

* Escriba `` `http: // localhost: 3978 / api / messages` `` en la barra de direcciones de Bot Emulator. (Este es el punto final predeterminado que su bot cuando se está ejecutando localmente.)
* Haga clic en Connect.


Ahora que su bot se está ejecutando localmente y está conectado al emulador, pruebe su bot escribiendo algunos mensajes en el emulador. Usted debe ver que el bot responde a cada mensaje que usted envía haciendo eco de su mensaje con el prefijo con el texto "Usted dijo:".
