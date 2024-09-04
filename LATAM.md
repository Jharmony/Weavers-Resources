[![Join our Weavers Discord](https://arweave.net/osw8PJbK1J6clwOj41SHasp_IIdigSPYEuBc9AgSgRM)](https://discord.gg/gvZTg53zuJ)

## Guía Rápida con NextJS / ArDrive Turbo ##

1. **Ejecuta create-next-app** Comienza creando una aplicación de Next.js escalada para trabajar desde ella.

   ```bash
   npx create-next-app
   ```

2. **Instala las dependencias:** Ahora necesitamos instalar las herramientas necesarias para interactuar con Arweave. Utilizaremos el SDK ArDrive TURBO para esto usando npm o yarn.
   ```bash
   npm install @ardrive/turbo-sdk
   ```
   o
   ```bash
   yarn add @ardrive/turbo-sdk
   ```

3. **Crea el script para interactuar con Arweave** Comienza cargando tu archivo de clave JWK para autenticación o proporcionando tu propio firmador. Recupera y muestra tu saldo actual de Arweave para asegurar fondos suficientes. Define las variables necesarias para la carga de archivos y calcula el costo de carga usando TURBO. Compara tu saldo de TURBO con el costo estimado de carga para asegurar que tienes suficientes fondos. Si es necesario, implementa una ventana de recarga para saldo adicional. Finalmente, carga tu archivo a Arweave, completando el proceso de manera fluida y eficiente. 

   ```javascript
   import { TurboFactory, ArweaveSigner } from '@ardrive/turbo-sdk';

   // Autenticación con un JWK
   const myKeyFile = fs.readFileSync('./my-secret-key.json');
   const myWalletAddress = arweave.wallets.jwkToAddress(myKeyFile);
   const turboClient = TurboFactory.authenticated({ privateKey: myKeyFile });
   
   // Alternativamente, proporcionando un firmador
   const mySigner = new ArweaveSigner(myKeyFile);
   const turboClient = TurboFactory.authenticated({ signer: mySigner });
   
   // Recuperando el saldo de la billetera
   const { winc: myBalance } = await turboClient.getBalance();
   
   // Preparando el archivo para la carga
   const myFilePath = path.join(__dirname, './my-photo.png');
   const myFileSize = fs.statSync(myFilePath).size;
   
   // Estimando el costo de carga
   const [{ winc: myUploadCost }] = await turboClient.getUploadCosts({
     bytes: [myFileSize],
   });
   
   // Verificando el saldo antes de la carga
   if (myBalance < myUploadCost) {
     const { url } = await turboClient.createCheckoutSession({
       amount: myUploadCost,
       owner: myWalletAddress,
     });
     open(url); // Solicita al usuario recargar saldo
     return;
   }
   
   // Cargando el archivo
   try {
     const { id, owner, dataCaches, fastFinalityIndexes } = await turboClient.uploadFile   (() => {
       fileStreamFactory => () => fs.createReadStream(myFilePath),
       fileSizeFactory => () => myFileSize,
     });
     // Archivo cargado exitosamente
     console.log('Data item uploaded successfully!', { id, owner, dataCaches,    fastFinalityIndexes });
   } catch (error) {
     // Manejo de errores
     console.error('Failed to upload data item!', error);
   } finally {
     // Verificando el nuevo saldo después de la carga
     const { winc: myNewBalance } = await turboClient.getBalance();
     console.log('New balance:', myNewBalance);
   }
   ```
Lee la documentación de ArDrive [Turbo SDK](https://docs.ardrive.io/) para más detalles. 

## Otros Paquetes NPM: ##

Para interacciones más sofisticadas y funcionalidades adicionales, considera usar herramientas avanzadas como @permaweb/aoconnect y @ardriveapp/turbo-sdk. Estas bibliotecas proporcionan características adicionales para interactuar con la red Arweave, brindándote más opciones y flexibilidad en tu flujo de trabajo de desarrollo.

**Turbo SDK:**

```bash
npm install @ardrive/turbo-sdk
```

Aquí tienes un ejemplo rápido de cómo usar el Turbo SDK para cargar un archivo en Arweave:

```javascript
import { TurboFactory } from '@ardrive/turbo-sdk';

const turbo = TurboFactory.authenticated({ privateKey: walletPrivateKey });

const filePath = 'path/to/your/file';
const fileSize = fs.statSync(filePath).size;

const uploadResponse = await turbo.uploadFile({
  fileStreamFactory: () => fs.createReadStream(filePath),
  fileSizeFactory: () => fileSize,
});

console.log('Carga completa:', uploadResponse);
```

Para recorridos más detallados y tutoriales, te recomendamos encarecidamente revisar nuestras sesiones de hackathon anteriores:

**AoConnect:**

```bash
npm install --save @permaweb/aoconnect
```

Aquí tienes un ejemplo rápido de cómo usar AoConnect para enviar un mensaje en la red Arweave:

```javascript
const wallet = JSON.parse(fs.readFileSync('path/to/wallet.json'));

// Enviando un mensaje
await message({
    process: "process-id",  // ID del proceso
    tags: [
        { name: "Your-Tag-Name-Here", value: "your-tag-value" },  // Etiqueta y valor personalizados
        { name: "Another-Tag", value: "another-value" },  // Otra etiqueta y valor
    ],
    signer: createDataItemSigner(wallet),  // Firmador del elemento de datos
    data: "any data",  // Datos que se enviarán
})
.then(console.log)  // Mostrar el resultado en la consola
.catch(console.error);  // Mostrar errores en la consola
```

## Al metal con Arweave.js ##

1. **Instalar Arweave.js::** Comienza instalando Arweave.js, un SDK de JavaScript/TypeScript para interactuar con la red Arweave. Este SDK te permite conectar fácilmente tus proyectos a Arweave, habilitando almacenamiento descentralizado y hosting permanente.
   
   ```bash
   npm install --save arweave
   ```

2. **Inicializar Arweave** Una vez que hayas instalado Arweave.js, es hora de inicializarlo. Este paso configura Arweave para conectar a un gateway, que actúa como tu punto de entrada a la red Arweave. ¡No te preocupes; son solo unas pocas líneas de código!

   ```javascript
   const Arweave = require('arweave');

  const arweave = Arweave.init({
  host: 'arweave.net', // O otro nodo
  port: 443,           // Puerto HTTPS
  protocol: 'https'    // Desarrollo local
});
   ```

3. **Create and Sign Transactions:** Con Arweave.js inicializado, puedes comenzar a crear y firmar transacciones para almacenar los datos de tu proyecto en la red Arweave. Estas transacciones pueden incluir HTML, JSON o cualquier otro tipo de dato que quieras almacenar permanentemente.

   ```javascript
   const myWallet = JSON.parse(fs.readFileSync('path/to/wallet.json'));

   const myTransaction = await arweave.createTransaction({
     data: '¡Bienvenido a Colombia!',
   }, myWallet);
   
   myTransaction.addTag('Content-Type', 'text/plain');
   
   await arweave.transactions.sign(myTransaction, myWallet);
   
   const myResponse = await arweave.transactions.post(myTransaction);
   console.log(myResponse.status);
   ```

## Videos en Weavers YT ##
- [Turbo SDK 101 con Stephen (alias Bobinstein) de Ario: Sumérgete en los conceptos básicos del Turbo SDK y aprende a aprovechar su poder para tus proyectos en Arweave.](https://youtu.be/3F95opFzZ7Q?si=gwxQnp32SMza9tgS)

- [Construyendo un Juego de Arena en AO con Bots por Tom Rakis: Explora cómo construir juegos interactivos en la red Arweave utilizando el protocolo AO.](https://youtu.be/BPHzWywrBLo?si=B-pj026YC7NDgurE)

- [Video de Tutorial de Gather Chat: Descubre cómo crear aplicaciones de chat descentralizadas usando Arweave y Gather.](https://youtu.be/zF9d5Y5L2iY?si=KeR0U0_IegvnPaG_)

## Sesiones aoVenture ##

- [Ao Ventures Keynote #1 por Phil Mataras - Ar.io / ArDrive](https://www.youtube.com/watch?v=PJWHKA90UfU)

- [Ao Ventures Keynote #2 por Tom Wilson - Memeframes](https://www.crowdcast.io/c/aoventureskeynote2)

- [Ao Ventures Keynote #3 por Marton - herramientas ao](https://www.crowdcast.io/c/aoventureskeynote3)

- [Ao Ventures Keynote #6 por Tom Wilson](https://www.crowdcast.io/c/aoventureskeynote6)

- [Construyendo un Juego de Arena en AO con Bots por Tom Rakis: Explora cómo construir juegos interactivos en la red Arweave utilizando el protocolo AO.](https://youtu.be/BPHzWywrBLo?si=B-pj026YC7NDgurE)

- [GVideo de Tutorial de Gather Chat: Descubre cómo crear aplicaciones de chat descentralizadas usando Arweave y Gather.](https://youtu.be/zF9d5Y5L2iY?si=KeR0U0_IegvnPaG_)

## Otros Videos ##

- [Almacenamiento de Archivos Permanente para Aplicaciones Web3 con Arweave, Bundlr (ahora Irys) y Next.js (Guía Completa)](https://www.youtube.com/watch?v=aUU-eHCB6j8&t=613s)

- [Almacena tus datos NFT en Arweave](https://www.youtube.com/watch?v=MTSPjmCmdqs&t=40s)

## Ejemplos Construidos con Arweave y AO ##

[Gather Chat](https://gatherchat.g8way.io/#/)
| [Repo](https://github.com/elliotsayes/gatherchat)

[TYPR - Clon de Twitter AO](https://www.typr.day/) | [Repo](https://github.com/iamgamelover/ao-twitter)

[Pet or Rekt](https://dumdum.g8way.io/)

[BetterIDEa](https://ide.betteridea.dev/)

[ArCode](https://arcode.g8way.io/)

[AOWW](https://aoww.net/)

[Helix](https://helix.g8way.io)

## Kit de Inicio y Primeros Pasos ##

[Kits de Inicio para React](https://cookbook.arweave.dev/kits/react/index.html)

[Kits de Inicio para Svelte](https://cookbook.arweave.dev/kits/svelte/index.html)

[Kits de Inicio para Vue](https://cookbook.arweave.dev/kits/vue/index.html)

[Lanzamiento de AOS v1.0.25](https://www.crowdcast.io/c/aoventureskeynote6)

[Taller AOS-Sqlite por Tom Wilson](https://hackmd.io/@ao-docs/rkM1C9m40)

[Repo de Kwil-db](https://github.com/kwilteam/kwil-db)

## Documentación de AO ##

[Implementación de Activos Atómicos AO](https://github.com/permaweb/ao-atomic-asset) - Los activos atómicos son elementos digitales únicos almacenados en Arweave, donde tanto los datos del activo como el proceso basado en AO (en lugar del tradicional SmartWeave) se suben en una única transacción inseparable.

[Especificaciones de AO](https://ao.arweave.dev/#/spec) - El computador AO es un entorno de computación unificado (una Imagen de Sistema Único), alojado en un conjunto heterogéneo de nodos en una red distribuida.

[Repo Oficial de AO](https://github.com/permaweb/ao) - El computador AO es la máquina orientada a actores que emerge de la red de nodos que se adhieren a su protocolo de datos central, ejecutándose en la red Arweave.

[Recetario de AO](https://ao.g8way.io/) - El computador AO es un mundo donde innumerables procesos paralelos interactúan dentro de un entorno de computación cohesivo, interconectados a través de una capa de mensajería nativa.

[Módulo de AO](https://cookbook_ao.g8way.io/references/ao.html) - La comunicación de procesos AO se maneja mediante mensajes; cada proceso recibe mensajes en forma de ANS-104 DataItems, y debe ser capaz de realizar las siguientes operaciones comunes.

[Comenzar en 5 Min](https://cookbook_ao.g8way.io/welcome/getting-started.html) - Configura AO en tu entorno de desarrollo local en menos de 5 minutos.

[Tutoriales de AO](https://cookbook_ao.g8way.io/tutorials/begin/index.html) - Tutoriales que te guiarán a través de una serie de pasos interactivos para profundizar tu conocimiento y comprensión del entorno AO.

[Introducción a AOS](https://cookbook_ao.g8way.io/guides/aos/intro.html) - AOS es un enfoque diferente para construir Procesos o Contratos, el computador AO es una red de computación descentralizada que permite que el cómputo se ejecute en cualquier lugar y AOS en un shell interactivo único.

[Acceso a Datos desde Arweave con AO](https://cookbook_ao.g8way.io/references/data.html) - Para solicitar datos desde Arweave, simplemente llama a Assign con una lista de procesos a los que te gustaría asignar los datos, y un Mensaje que es el txid de un Mensaje.

[Carga en el Proceso AO](https://cookbook_ao.g8way.io/guides/aos/load.html) - Esta característica permite cargar código Lua desde un archivo fuente en tu máquina local, proporcionando una experiencia de desarrollo agradable para trabajar con procesos AOS.

[Instalando AO Connect](https://cookbook_ao.g8way.io/guides/aoconnect/connecting.html) - Instala AO Connect en tu aplicación Node.js y navegador.

[Conectando a nodos específicos de AO](https://cookbook_ao.g8way.io/guides/aos/load.html) - Al incluir AO Connect en tu código, tienes la capacidad de conectarte a un MU y CU específicos, así como a una puerta de enlace Arweave.

[Enviando un Mensaje a un Proceso](https://cookbook_ao.g8way.io/guides/aoconnect/sending-messages.html) - Enviar un mensaje es la forma central en la que tu aplicación puede interactuar con AO. Un mensaje es la entrada a un proceso.

[Lectura de resultados de un Proceso AO](https://cookbook_ao.g8way.io/guides/aoconnect/reading-results.html) - Una llamada a resultados también puede proporcionarte una lista paginada de múltiples resultados.

[Generación de un Proceso](https://cookbook_ao.g8way.io/guides/aoconnect/spawning-processes.html) - Para generar un Proceso debes tener el TXID de un Módulo AO que se haya cargado en Arweave.

[Llamada a DryRun](https://cookbook_ao.g8way.io/guides/aoconnect/calling-dryrun.html) - DryRun es el proceso de enviar un objeto de mensaje a un proceso específico y obtener el objeto de Resultado de vuelta, pero la memoria no se guarda, es perfecto para crear un mensaje de lectura para devolver el valor actual de la memoria.

[Monitoreo de Cron](https://cookbook_ao.g8way.io/guides/aoconnect/monitoring-cron.html) - Configurar etiquetas cron significa que tu proceso comenzará a producir resultados de cron en su buzón de salida, pero necesitas monitorear estos resultados si quieres que los mensajes de esos resultados se envíen a través de la red.

[Enviando una Asignación a un Proceso](https://cookbook_ao.g8way.io/guides/aoconnect/assign-data.html) - Las asignaciones se pueden usar para cargar Datos desde otro Mensaje a un Proceso. O para no duplicar Mensajes.

[Conceptos Básicos de AO](https://cookbook_ao.g8way.io/concepts/index.html) - AO tiene un par de conceptos básicos incorporados en el diseño.

## Documentación de ArDrive / TURBO ##

[ArDrive Core](https://docs.ardrive.io/docs/core-sdk.html#overview) - una biblioteca TypeScript que contiene las características esenciales del backend para soportar el CLI y las aplicaciones de escritorio de ArDrive, como la gestión de archivos, la carga/descarga en la Permaweb, la gestión de carteras y otras funciones comunes.

[Publicación de Transacciones con ArDrive Turbo](https://cookbook.arweave.dev/guides/posting-transactions/turbo.html) - Publicar transacciones utilizando Turbo se puede lograr mediante el paquete JavaScript @ardrive/turbo-sdk.

[CLI de ArDrive & ArFS](https://docs.ardrive.io/docs/cli/#arfs) - ofrece operaciones de utilidad para interactuar de forma segura con las carteras de Arweave e inspeccionar varias condiciones de la cadena de bloques Arweave.

[ArDrive CLI Primeros Pasos](https://docs.ardrive.io/docs/cli/getting-started.html#prerequisites) - la forma más rápida de comenzar es instalar globalmente la última versión del CLI de ArDrive en tu sistema local a través de NPM.

[Protocolo ArFS](https://docs.ardrive.io/docs/arfs/#key-features) - El Sistema de Archivos Arweave, o “ArFS” es un protocolo de modelado, almacenamiento y recuperación de datos diseñado para emular operaciones comunes del sistema de archivos y proporcionar aspectos de mutabilidad a tu jerarquía de datos en el almacenamiento de datos permanente e inmutable de Arweave.

[API de Pago de TURBO](https://docs.ardrive.io/docs/turbo/api/payment.html) - ArDrive ofrece varios puntos finales de API para ayudar a gestionar y determinar los costos asociados con la conversión de monedas en créditos Turbo.

[API de Carga de TURBO](https://docs.ardrive.io/docs/turbo/api/upload.html) - El Servicio de Carga Turbo admite el pago de elementos de datos firmados para upload.ardrive.io utilizando Créditos Turbo.

[TURBO SDK](https://docs.ardrive.io/docs/turbo/turbo-sdk/) - Este SDK proporciona funcionalidad para interactuar con los Servicios de Carga y Pago de Turbo y está disponible tanto para entornos NodeJS como para la web.
[EthAReum](https://docs.ardrive.io/docs/misc/ethareum/#overview) - EthAReum es un nuevo protocolo de derivación de claves que permite la generación de claves privadas para una cartera Arweave utilizando una firma de una cartera Ethereum. Esto permite a los usuarios crear una cartera Arweave directamente a través de un proveedor de carteras Ethereum como MetaMask.

[Permasites](https://docs.ardrive.io/docs/misc/permasite.html#overview) - ArDrive ofrece la capacidad de guardar copias de trabajo de sitios web estáticos permanentemente en Arweave.

## Documentación de ArIO ##

[Arquitectura de Gateway](https://docs.ar.io/gateways/) - El papel principal de un gateway en el ecosistema Arweave es actuar como un puente entre la red Arweave y el mundo exterior.

[Documentación de ArIO](https://docs.ar.io) - El ecosistema AR.IO está dedicado a cultivar productos y protocolos para mantener el acceso a la permanencia digital, haciendo que el permaweb esté disponible para todos.

[ArIO SDK](https://github.com/ar-io/ar-io-sdk) - Este SDK proporciona funcionalidad para interactuar con el ecosistema de servicios (por ejemplo, gateways y observadores) y protocolos (por ejemplo, ArNS). Está disponible para entornos NodeJS y Web.

## Permaweb Cookbook ##

[Permaweb Cookbook Oficial](https://cookbook.arweave.dev/) - El Permaweb Cookbook es un recurso para desarrolladores que proporciona los conceptos esenciales y referencias para construir aplicaciones en el Permaweb. Cada concepto y referencia se centra en aspectos específicos del ecosistema de desarrollo del Permaweb y ofrece detalles adicionales y ejemplos de uso.

[Coloca una página web en el permaweb](https://cookbook.arweave.dev/getting-started/quick-starts/hw-code.html) - Esta guía te muestra una forma rápida de subir una página web estática HTML, CSS y JavaScript al permaweb utilizando unas pocas líneas de código y una interfaz de línea de comandos (CLI).

[Página web subida con Arweave.js e Irys](https://cookbook.arweave.dev/getting-started/quick-starts/hw-nodejs.html) - Forma sencilla de subir datos al permaweb usando arweave-js e Irys.

[Consulta de Transacciones - Irys & GraphQl](https://cookbook.arweave.dev/guides/posting-transactions/arweave-js.html) - Las transacciones nativas de Arweave se pueden publicar directamente en un nodo o gateway utilizando el paquete arweave-js.

[Obtención de Datos de Transacciones](https://cookbook.arweave.dev/guides/http-api.html) - Un servicio de almacenamiento en caché de datos de transacciones es ofrecido por la mayoría de los gateways a través de un conjunto de endpoints HTTP. Se puede utilizar cualquier cliente/paquete HTTP para solicitar datos de transacciones desde estos endpoints, como Axios o Fetch para JavaScript, Guzzle para PHP, etc.

[Publicación de Transacciones usando arweave-js](https://cookbook.arweave.dev/guides/posting-transactions/arweave-js.html) - Las transacciones nativas de Arweave se pueden publicar directamente en un nodo o gateway usando el paquete arweave-js.

[Etiquetas - Metadatos de Transacciones](https://cookbook.arweave.dev/concepts/tags.html) - Arweave se puede considerar como un disco duro permanente de solo anexado donde cada entrada en el disco es una transacción única. Las transacciones tienen una ID única, firma y dirección del propietario que firmó y pagó por la transacción.

[Manifiestos de Ruta](https://cookbook.arweave.dev/concepts/manifests.html) - Los Manifiestos de Ruta son una forma de vincular múltiples transacciones bajo una sola ID de transacción base y darles nombres de archivos legibles por humanos.

## Extensiones Recomendadas para VS Code ##

[ES-Lint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) |
[Editor-Config](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig) |
[Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) |
[ZipFS](https://marketplace.visualstudio.com/items?itemName=arcanis.vscode-zipfs) |
[LUA\AO lua-language-server](https://marketplace.visualstudio.com/items?itemName=sumneko.lua)

## Otros Recursos ##

[Instalación de paquetes en procesos AO usando APM](https://mirror.xyz/0xCf673b87aFBed6091617331cC895376209d3b923/M4XoQFFCAKBH54bwIsCFT3Frxd575-plCg2o4H1Tujs) - APM es un gestor de paquetes para procesos AO.

[Construyendo en Arweave: Herramientas para Acceso con Token para Protocolos Web3](https://www.aoventures.io/blog/building-on-arweave-tools-for-token-gated-access-for-web3-protocols) - El concepto de acceso con token gira en torno a restringir el acceso a ciertos contenidos como el código fuente solo a aquellos individuos/entidades que posean una cierta cantidad de tokens $U o tokens AR.

[Comprendiendo Lua](https://cookbook_ao.g8way.io/references/lua.html) - Lua es un lenguaje de programación ligero, de alto nivel y multi-paradigma diseñado principalmente para sistemas embebidos y clientes. Es conocido por su eficiencia, simplicidad y flexibilidad.

[Documentación de Lua](https://www.lua.org/docs.html) - La definición oficial del lenguaje Lua es su manual de referencia, que describe la sintaxis y la semántica de Lua, las bibliotecas estándar y la API de C.

[AOP-5: WeaveDrive](https://hackmd.io/@ao-docs/H1JK_WezR) - Arweave es un disco duro permanente. AO, una supercomputadora descentralizada, vive sobre él. Este AOP añade soporte para un sistema de archivos virtual para pasar (grandes) datos leídos desde Arweave directamente a procesos AO de manera eficiente.

[Aprender X en Y minutos - Donde X = Lua](https://learnxinyminutes.com/docs/lua/) - Guía de inicio para principiantes en Lua.

[Documentación de ArweaveKit](https://docs.arweavekit.com/wallets/introduction) - ArweaveKit tiene como objetivo reducir la barrera de entrada y construcción en Arweave creando una biblioteca bien documentada y única.

[Orbit Playground](https://playground.0rbit.co/) - Prueba 0rbit en tu navegador.

[Documentación de Orbit](https://docs.0rbit.co/) - Documentación de 0rbit.

[Repositorio del Taller AOV de Lorimer Jenkins](https://github.com/lorimerjenkins/aov-workshop)

[¿Qué es Irys?](https://irys.xyz/what-is-irys)

[ao Tokens](https://github.com/labscommunity/ao-tokens) - Esta biblioteca facilita la interacción con tokens y la operación con cantidades de tokens.

[Presentación de Gamma](https://gamma.app/docs/Deploying-on-Arweave-Build-on-the-Decentralized-Web-xj30isp8bok5dep)

---

Estos tutoriales proporcionan valiosas perspectivas y demostraciones prácticas que pueden ayudarte a llevar tus habilidades de desarrollo en Arweave al siguiente nivel. ¡Feliz codificación!

En nuestro evento, proporcionaremos orientación, responderemos tus preguntas y ofreceremos asistencia práctica para garantizar que puedas desplegar tus proyectos en Arweave sin problemas. ¡Únete a nosotros para descubrir lo fácil y poderoso que puede ser desplegar en Arweave!

Esta guía ahora incluye información tanto sobre AoConnect como sobre el SDK Turbo, proporcionando a los desarrolladores opciones para funcionalidades avanzadas al desplegar en Arweave.

[![Join our Weavers Discord](https://arweave.net/osw8PJbK1J6clwOj41SHasp_IIdigSPYEuBc9AgSgRM)](https://discord.gg/gvZTg53zuJ)
