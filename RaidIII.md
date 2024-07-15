## Deploying on Arweave: Build on the Decentralized Web ##

Are you new to deploying on Arweave? Don't worry! We're here to show you just how straightforward it can be. Whether you're experienced with web development or just starting out, deploying on Arweave is as easy as pie. Here's what you need to know:

**Not Familiar with Deploying on Arweave? No Problem!**

If you've never deployed on Arweave before, we've got your back. Join us at our event, and we'll walk you through the process step by step. Whether you have questions or need hands-on assistance, we're here to help you deploy your projects on Arweave smoothly.

## Raid III Weavers Event: Unleashing Creativity on the Arweave Blockchain ##

Welcome to the Raid III Weavers event, a dynamic two-week virtual hackathon from July 24th to August 7th, tailored for the Arweave ecosystem. With $10,000 in bounties, this inclusive event encourages creativity and collaboration among developers and creatives alike. No application is needed—simply join our Weavers Discord, attend the Twitter Space kick-off on July 24th, and start building. 

Access comprehensive resources in the WeaveWorld Documentation section to support your projects. We look forward to seeing your innovative contributions to the Arweave blockchain!

## WeaveWorld Protocols (Verse, Schema, Chat Protocol) ##

**Introduction to WeaveWorld**
WeaveWorld is our suite of protocols designed to enhance your experience with the Arweave ecosystem. Below are some of the protocols you can explore:

- Verse: A protocol for creating decentralized content and applications.
- Schema: A protocol for structuring and organizing data.
- Chat Protocol: A protocol for building decentralized chat applications.

For more detailed documentation and resources, check out the WeaveWorld documentation maintained by Elliot:

***WeaverWorld***

- [WeaveWorld Documentation](https://github.com/elliotsayes/WeaveWorld/tree/main/docs)
- [WeaveWorld GitHub](https://github.com/elliotsayes/WeaveWorld/tree/main)

***Agents & AI Agents***
- [Verse Guide](https://github.com/elliotsayes/WeaveWorld/tree/main)
- [Agent Guide](https://github.com/elliotsayes/WeaveWorld/tree/main)
- [Llama Complainer Agent](https://github.com/elliotsayes/WeaveWorld/blob/main/process/npc/LlamaComplainer.lua)
- [Llama Runner Agent](https://github.com/elliotsayes/WeaveWorld/blob/main/process/npc/LlamaRunner.lua)

***Protcols***
- [Verse Protocol](https://github.com/elliotsayes/WeaveWorld/blob/main/docs/Verse.md)
- [Schema Protcol](https://github.com/elliotsayes/WeaveWorld/blob/main/docs/Schema.md)
- [Chat Protocol](https://github.com/elliotsayes/WeaveWorld/blob/main/docs/Chat.md) 



**How Easy is it to Deploy on Arweave?**

## Quick Start with NextJS / ArDrive Turbo##

1. **Run create-next-app** Start by building a scafolded nextjs app to work from.

   ```bash
   npx create-next-app
   ```

2. **Install dependencies:** Now we need to install the tools needed for us to interact with Arweave. We will leverage the ArDrive TURBO SDK for this using npm or yarn.

   ```bash
   npm install @ardrive/turbo-sdk
   ```
   or
   ```bash
   yarn add @ardrive/turbo-sdk
   ```

3. **Create the script to interact with Arweave** Begin by loading your JWK keyfile for authentication or providing your own signer. Retrieve and display your current Arweave balance to ensure sufficient funds. Define the necessary variables for file upload and calculate the upload cost using TURBO. Compare your TURBO balance with the estimated upload cost to ensure you have enough funds. If necessary, implement a top-up window for additional balance. Finally, upload your file to Arweave, completing the process smoothly and efficiently. 

   ```javascript
   import { TurboFactory, ArweaveSigner } from '@ardrive/turbo-sdk';

   // Authenticating with a JWK
   const myKeyFile = fs.readFileSync('./my-secret-key.json');
   const myWalletAddress = arweave.wallets.jwkToAddress(myKeyFile);
   const turboClient = TurboFactory.authenticated({ privateKey: myKeyFile });
   
   // Alternatively, providing a signer
   const mySigner = new ArweaveSigner(myKeyFile);
   const turboClient = TurboFactory.authenticated({ signer: mySigner });
   
   // Retrieving wallet balance
   const { winc: myBalance } = await turboClient.getBalance();
   
   // Preparing file for upload
   const myFilePath = path.join(__dirname, './my-photo.png');
   const myFileSize = fs.statSync(myFilePath).size;
   
   // Estimating upload cost
   const [{ winc: myUploadCost }] = await turboClient.getUploadCosts({
     bytes: [myFileSize],
   });
   
   // Checking balance before upload
   if (myBalance < myUploadCost) {
     const { url } = await turboClient.createCheckoutSession({
       amount: myUploadCost,
       owner: myWalletAddress,
     });
     open(url); // Prompt user to top-up balance
     return;
   }
   
   // Uploading the file
   try {
     const { id, owner, dataCaches, fastFinalityIndexes } = await turboClient.uploadFile   (() => {
       fileStreamFactory => () => fs.createReadStream(myFilePath),
       fileSizeFactory => () => myFileSize,
     });
     // File uploaded successfully
     console.log('Data item uploaded successfully!', { id, owner, dataCaches,    fastFinalityIndexes });
   } catch (error) {
     // Error handling
     console.error('Failed to upload data item!', error);
   } finally {
     // Checking new balance after upload
     const { winc: myNewBalance } = await turboClient.getBalance();
     console.log('New balance:', myNewBalance);
   }
   ```
Read the ArDrive [Turbo SDK](https://docs.ardrive.io/) docs for more details. 

## Other NPM Packages: ##

For more sophisticated interactions and additional functionalities, consider using advanced tools like **@permaweb/aoconnect** and **@ardriveapp/turbo-sdk**. These libraries provide additional features for interacting with the Arweave network, giving you more options and flexibility in your development workflow.

**Turbo SDK:**

```bash
npm install @ardrive/turbo-sdk
```

Here's a quick example of how to use the Turbo SDK to upload a file to Arweave:

```javascript
import { TurboFactory } from '@ardrive/turbo-sdk';

const turbo = TurboFactory.authenticated({ privateKey: walletPrivateKey });

const filePath = 'path/to/your/file';
const fileSize = fs.statSync(filePath).size;

const uploadResponse = await turbo.uploadFile({
  fileStreamFactory: () => fs.createReadStream(filePath),
  fileSizeFactory: () => fileSize,
});

console.log('Upload complete:', uploadResponse);
```

For more in-depth walkthroughs and tutorials, we highly recommend checking out our previous hackathon sessions:

**AoConnect:**

```bash
npm install --save @permaweb/aoconnect
```

Here's a quick example of how to use AoConnect to send a message on the Arweave network:

```javascript
import { message, createDataItemSigner } from "@permaweb/aoconnect";

const wallet = JSON.parse(fs.readFileSync('path/to/wallet.json'));

await message({
    process: "process-id",
    tags: [
        { name: "Your-Tag-Name-Here", value: "your-tag-value" },
        { name: "Another-Tag", value: "another-value" },
    ],
    signer: createDataItemSigner(wallet),
    data: "any data",
})
.then(console.log)
.catch(console.error);
```

## To the metal with Arweave.js ##

1. **Install Arweave.js:** Start by installing Arweave.js, a JavaScript/TypeScript SDK for interacting with the Arweave network. This SDK allows you to easily connect your projects to Arweave, enabling decentralized storage and permanent hosting.

   ```bash
   npm install --save arweave
   ```

2. **Initialize Arweave:** Once you've installed Arweave.js, it's time to initialize it. This step sets up Arweave to connect to a gateway, which acts as your entry point to the Arweave network. Don't worry; it's just a few lines of code!

   ```javascript
   const Arweave = require('arweave');

   const arweave = Arweave.init({
       host: 'arweave.net', // Or other node
       port: 443,           // HTTPS port
       protocol: 'https'    // Local development
   });
   ```

3. **Create and Sign Transactions:** With Arweave.js initialized, you can start creating and signing transactions to store your project data on the Arweave network. These transactions can include HTML, JSON, or any other type of data you want to store permanently.

   ```javascript
   const myWallet = JSON.parse(fs.readFileSync('path/to/wallet.json'));

   const myTransaction = await arweave.createTransaction({
     data: 'Welcome to Berlin!',
   }, myWallet);
   
   myTransaction.addTag('Content-Type', 'text/plain');
   
   await arweave.transactions.sign(myTransaction, myWallet);
   
   const myResponse = await arweave.transactions.post(myTransaction);
   console.log(myResponse.status);
   ```

## Weavers YT ##
- [Turbo SDK 101 with Stephen (aka Bobinstein) from Ario: Dive into the basics of the Turbo SDK and learn how to harness its power for your Arweave projects.](https://youtu.be/3F95opFzZ7Q?si=gwxQnp32SMza9tgS)

- [Building an Arena Game on AO with Bots by Tom Rakis: Explore how to build interactive games on the Arweave network using the AO protocol.](https://youtu.be/BPHzWywrBLo?si=B-pj026YC7NDgurE)

- [Gather Chat Walkthrough Video: Discover how to create decentralized chat applications using Arweave and Gather.](https://youtu.be/zF9d5Y5L2iY?si=KeR0U0_IegvnPaG_)

## aoVenture Sessions ##

- [Ao Ventures Keynote #1 by Phil Mataras - Ar.io / ArDrive](https://www.youtube.com/watch?v=PJWHKA90UfU)

- [Ao Ventures Keynote #2 by Tom Wilson - Memeframes](https://www.crowdcast.io/c/aoventureskeynote2)

- [Ao Ventures Keynote #3 by Marton - ao tooling](https://www.crowdcast.io/c/aoventureskeynote3)

- [Ao Ventures Keynote #6 by Tom Wilson](https://www.crowdcast.io/c/aoventureskeynote6)

- [Building an Arena Game on AO with Bots by Tom Rakis: Explore how to build interactive games on the Arweave network using the AO protocol.](https://youtu.be/BPHzWywrBLo?si=B-pj026YC7NDgurE)

- [Gather Chat Walkthrough Video: Discover how to create decentralized chat applications using Arweave and Gather.](https://youtu.be/zF9d5Y5L2iY?si=KeR0U0_IegvnPaG_)

## Other Videos ##
- [Permanent File Storage for Web3 Applications with Arweave, Bundlr(now Irys), and Next.js (Full Stack Guide)](https://www.youtube.com/watch?v=aUU-eHCB6j8&t=613s)

- [Store your NFT data on Arweave](https://www.youtube.com/watch?v=MTSPjmCmdqs&t=40s)

## Examples Built with Arweave and AO ##

[Gather Chat](https://gatherchat.g8way.io/#/)
| [Repo](https://github.com/elliotsayes/gatherchat)

[TYPR - AO Twitter Clone](https://www.typr.day/) | [Repo](https://github.com/iamgamelover/ao-twitter)

[Pet or Rekt](https://dumdum.g8way.io/)

[BetterIDEa](https://ide.betteridea.dev/)

[ArCode](https://arcode.g8way.io/)

[AOWW](https://aoww.net/)

[Helix](https://helix.g8way.io)


## Starter Kit and Getting Started ##

[React Starter Kits](https://cookbook.arweave.dev/kits/react/index.html)

[Svelte Starter Kits](https://cookbook.arweave.dev/kits/svelte/index.html)

[Vue Starter Kits](https://cookbook.arweave.dev/kits/vue/index.html)

[AOS Release v1.0.25 ](https://www.crowdcast.io/c/aoventureskeynote6)

[AOS-Sqlite Workshop by Tom Wilson](https://hackmd.io/@ao-docs/rkM1C9m40)

[Kwil-db Repo](https://github.com/kwilteam/kwil-db)


## AO Documentation ##

[ao SPEC](https://ao.arweave.dev/#/spec) - The ao computer is a single, unified computing environment (a Single System Image), hosted on a heterogenous set of nodes in a distributed network. 

[ao Official Repo](https://github.com/permaweb/ao) - The ao computer is the actor oriented machine that emerges from the network of nodes that adhere to its core data protocol, running on the Arweave network.

[ao Cookbook](https://ao.g8way.io/) - The ao computer is a world where countless parallel processes interact within a single, cohesive computing environment, seamlessly interlinked through a native message-passing layer.

[ao Module](https://cookbook_ao.g8way.io/references/ao.html) - ao process communication is handled by messages, each process receives messages in the form of ANS-104 DataItems, and needs to be able to do the following common operations.

[Get Started in 5 Min](https://cookbook_ao.g8way.io/welcome/getting-started.html) - Get ao up and running in your local dev enviorment in under 5 minutes. 

[ao Tutorials](https://cookbook_ao.g8way.io/tutorials/begin/index.html) - tutorials that will walk you through an interactive set of steps that will help you deepen your knowledge and understanding of the aos environment.

[aos Introduction](https://cookbook_ao.g8way.io/guides/aos/intro.html) - aos is a different approach to building Processes or Contracts, the ao computer is a decentralized computer network that allows compute to run anywhere and aos in a unique interactive shell.

[Accessing Data from Arweave with ao](https://cookbook_ao.g8way.io/references/data.html) - In order, to request data from arweave, you simply call Assign with a list of processes you would like to assign the data to, and a Message which is the txid of a Message.

[.load in ao process](https://cookbook_ao.g8way.io/guides/aos/load.html) - This feature allows you to load lua code from a source file on your local machine, this simple feature gives you a nice DX experience for working with aos processes.

[Installing ao connect](https://cookbook_ao.g8way.io/guides/aoconnect/connecting.html) - Install ao connect in to your Nodejs & Browser app.

[Connecting to specific ao nodes](https://cookbook_ao.g8way.io/guides/aos/load.html) - When including ao connect in your code you have the ability to connect to a specific MU and CU, as well as being able to specifiy an Arweave gateway. 

[Sending a Message to a Process](https://cookbook_ao.g8way.io/guides/aoconnect/sending-messages.html) - Sending a message is the central way in which your app can interact with ao. A message is input to a process.

[Reading results from an ao Process](https://cookbook_ao.g8way.io/guides/aoconnect/reading-results.html) - A call to results can also provide you paginated list of multiple results.

[Spawning a Process](https://cookbook_ao.g8way.io/guides/aoconnect/spawning-processes.html) - In order to spawn a Process you must have the TXID of an ao Module that has been uploaded to Arweave.

[Calling DryRun](https://cookbook_ao.g8way.io/guides/aoconnect/calling-dryrun.html) - DryRun is the process of sending a message object to a specific process and getting the Result object back, but the memory is not saved, it is perfect to create a read message to return the current value of memory.

[Monitoring Cron](https://cookbook_ao.g8way.io/guides/aoconnect/monitoring-cron.html) - Setting cron tags means that your process will start producing cron results in its outbox, but you need to monitor these results if you want messages from those results to be pushed through the network.

[Sending an Assignment to a Process](https://cookbook_ao.g8way.io/guides/aoconnect/assign-data.html) - Assignments can be used to load Data from another Message into a Process. Or to not duplicate Messages.

[Core ao Concepts](https://cookbook_ao.g8way.io/concepts/index.html) -ao has a couple of core concepts built into the design.

## ArDrive / TURBO Documentation ##

[ArDrive Core](https://docs.ardrive.io/docs/core-sdk.html#overview) - a TypeScript library that contains the essential back end application features to support the ArDrive CLI and Desktop apps, such as file management, Permaweb upload/download, wallet management, and other common functions.

[Posting Transactions with Ardrive Turbo](https://cookbook.arweave.dev/guides/posting-transactions/turbo.html) - Posting transactions using Turbo can be accomplished using the @ardrive/turbo-sdk JavaScript package.

[ArDrive CLI & ArFS](https://docs.ardrive.io/docs/cli/#arfs) - offers utility operations for securely interacting with Arweave wallets and inspecting various Arweave (opens new window)blockchain conditions.

[ArDrive CLI Getting Started](https://docs.ardrive.io/docs/cli/getting-started.html#prerequisites) - the fastest way to get up and running is to globally install the latest version of the ArDrive CLI to your local system via NPM

[ArFS Protocol](https://docs.ardrive.io/docs/arfs/#key-features) - Arweave File System, or “ArFS” is a data modeling, storage, and retrieval protocol designed to emulate common file system operations and to provide aspects of mutability to your data hierarchy on Arweave (opens new window)'s otherwise permanent, immutable data storage blockweave.

[TURBO Payment API](https://docs.ardrive.io/docs/turbo/api/payment.html) - ArDrive offers several API endpoints to help manage and determine costs associated with converting currencies into Turbo credits.

[TURBO Upload API](https://docs.ardrive.io/docs/turbo/api/upload.html) - The Turbo Upload Service supports payment for signed data-items to upload.ardrive.io using Turbo Credits.

[TURBO SDK](https://docs.ardrive.io/docs/turbo/turbo-sdk/) - This SDK provides functionality for interacting with the Turbo Upload and Payment Services and is available for both NodeJS and Web environments.

[EthAReum](https://docs.ardrive.io/docs/misc/ethareum/#overview) - EthAReum is a new key derivation protocol that enables the generation of private keys for an Arweave wallet using a signature from an Ethereum wallet. This allows users to create an Arweave wallet directly through an Ethereum wallet provider like MetaMask.

[Permasites](https://docs.ardrive.io/docs/misc/permasite.html#overview) - ArDrive offers the ability to save working copies of static websites permanently on Arweave.

## ArIO Documentation ##

[Gateway Architechture](https://docs.ar.io/gateways/) - A gateway’s primary role in the Arweave ecosystem is to act as a bridge between the Arweave network and the outside world. 

[ArIO Docs](https://docs.ar.io) - The AR.IO ecosystem is dedicated to cultivating products and protocols for sustaining access to digital permanence, making the permaweb available to everyone.

[ArIO SDK](https://github.com/ar-io/ar-io-sdk) - This SDK provides functionality for interacting with the ar.io ecosystem of services (e.g. gateways and observers) and protocols (e.g. ArNS). It is available for both NodeJS and Web environments.

## Permaweb Cookbook ##

[Permaweb Cookbook Official](https://cookbook.arweave.dev/) - The Permaweb Cookbook is a developer resource that provides the essential concepts and references for buiding applications on the Permaweb. Each concept and reference will focus on specific aspects of the Permaweb development ecosystem while providing additional details and usage examples

[Get a webpage on to the permaweb](https://cookbook.arweave.dev/getting-started/quick-starts/hw-code.html) - This guide walks you through a quick way to get a static HTML, CSS and JavaScript webpage on to the permaweb using a few lines of code and a command-line interface (CLI)

[Webpage uploade with Arweave.js and Irys](https://cookbook.arweave.dev/getting-started/quick-starts/hw-nodejs.html) - simple way to get data on to the permaweb using arweave-js and irys

[Querying Transactions - Irys & GraphQl](https://cookbook.arweave.dev/guides/posting-transactions/arweave-js.html) -Arweave native transactions can be posted directly to a node or gateway using the arweave-js package.

[Fetching Transaction Data](https://cookbook.arweave.dev/guides/http-api.html) - A Transaction data caching service is offered by most gateways though a set of HTTP endpoints. Any HTTP client/package can be used to request transaction data from these endpoints. For example Axios or Fetch for JavaScript, Guzzle for PHP, etc.

[Posting Transactions using arweave-js](https://cookbook.arweave.dev/guides/posting-transactions/arweave-js.html) -Arweave native transactions can be posted directly to a node or gateway using the arweave-js package.

[Tags - Transaction Metadata](https://cookbook.arweave.dev/concepts/tags.html) - Arweave can be thought of as a permanent append-only hard drive where each entry on the drive is its own unique transaction. Transactions have a unique ID, signature, and owner address for the address that signed and paid for the transaction to be posted. 

[Path Manifests](https://cookbook.arweave.dev/concepts/manifests.html) - Path Manifests are a way to link multiple transactions together under a single base transaction ID and give them human readable file names. 

## Recommended VS Code Extensions ##

[ES-Lint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) |
[Editor-Config](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig) |
[Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) | 
[ZipFS](https://marketplace.visualstudio.com/items?itemName=arcanis.vscode-zipfs) |
[LUA\AO lua-language-server](https://marketplace.visualstudio.com/items?itemName=sumneko.lua)

## Other Resources ##

[Installing packages on ao processes using APM](https://mirror.xyz/0xCf673b87aFBed6091617331cC895376209d3b923/M4XoQFFCAKBH54bwIsCFT3Frxd575-plCg2o4H1Tujs) - APM is  package mananger for ao process

[Building on Arweave: Tools for Token-Gated Access for Web3 Protocols](https://www.aoventures.io/blog/building-on-arweave-tools-for-token-gated-access-for-web3-protocols) - The concept of token-gated access revolves around restricting access to certain content such as source code to only those individuals/entities that hold a certain amount of $U tokens or AR tokens.

[Understanding Lua](https://cookbook_ao.g8way.io/references/lua.html) - Lua is a lightweight, high-level, multi-paradigm programming language designed primarily for embedded systems and clients. It's known for its efficiency, simplicity, and flexibility

[Lua Documentation](https://www.lua.org/docs.html) - The official definition of the Lua language is its reference manual, which describes the syntax and the semantics of Lua, the standard libraries, and the C API

[AOP-5: WeaveDrive](https://hackmd.io/@ao-docs/H1JK_WezR) - Arweave is a permanent hard drive. AO, a decentralized supercomputer lives on top of it. This AOP adds virtual file system support to pass (large) data read from Arweave directly into AO processes efficiently.

[Learn X in Y minutes Where X = Lua](https://learnxinyminutes.com/docs/lua/) - Starter guide for lua beginnersS

[ArweaveKit Docs](https://docs.arweavekit.com/wallets/introduction) - ArweaveKit aims to lower the barrier of onboarding and building on Arweave by creating a well documented one-stop library.

[Orbit Playground](https://playground.0rbit.co/) - Test 0rbit in your browser.

[Orbit Documentation](https://docs.0rbit.co/) - 0rbit Documentation.

[Lorimer Jenkins AOV Workshop Repo](https://github.com/lorimerjenkins/aov-workshop)

[What is Irys](https://irys.xyz/what-is-irys)

[ao Tokens](https://github.com/labscommunity/ao-tokens) - This library makes it easy to interact with tokens and operate with token quantities.

[Gamma Presentation](https://gamma.app/docs/Deploying-on-Arweave-Build-on-the-Decentralized-Web-xj30isp8bok5dep)

---

These walkthroughs provide valuable insights and practical demonstrations that can help you take your Arweave development skills to the next level. Happy coding!

At our event, we'll provide guidance, answer your questions, and offer hands-on assistance to ensure you can deploy your projects on Arweave smoothly. Join us to discover just how easy and powerful deploying on Arweave can be. See you there!

This guide now includes information on both AoConnect and the Turbo SDK, providing developers with options for advanced functionalities when deploying on Arweave.

[![protocol.land](https://arweave.net/eZp8gOeR8Yl_cyH9jJToaCrt2He1PHr0pR4o-mHbEcY)](https://protocol.land/#/repository/14428ded-7107-4d78-ae81-7ff37973520e)
