**Deploying on Arweave: Bringing Your Projects to the Decentralized Web Made Easy**

Are you new to deploying on Arweave? Don't worry! We're here to show you just how straightforward it can be. Whether you're experienced with web development or just starting out, deploying on Arweave is as easy as pie. Here's what you need to know:

**Not Familiar with Deploying on Arweave? No Problem!**

If you've never deployed on Arweave before, we've got your back. Join us at our event, and we'll walk you through the process step by step. Whether you have questions or need hands-on assistance, we're here to help you deploy your projects on Arweave smoothly.

**How Easy is it to Deploy on Arweave?**

It's as simple as 1-2-3! Let's break it down:

1. **Install Arweave.js:** Start by installing Arweave.js, a JavaScript/TypeScript SDK for interacting with the Arweave network. This SDK allows you to easily connect your projects to Arweave, enabling decentralized storage and permanent hosting.

   ```bash
   npm install --save arweave
   ```

2. **Initialize Arweave:** Once you've installed Arweave.js, it's time to initialize it. This step sets up Arweave to connect to a gateway, which acts as your entry point to the Arweave network. Don't worry; it's just a few lines of code!

   ```javascript
   const Arweave = require('arweave');

   const arweave = Arweave.init({
       host: 'arweave.net', // Or your preferred node
       port: 443,           // Standard HTTPS port
       protocol: 'https'    // Use 'http' for local development
   });
   ```

3. **Create and Sign Transactions:** With Arweave.js initialized, you can start creating and signing transactions to store your project data on the Arweave network. These transactions can include HTML, JSON, or any other type of data you want to store permanently.

   ```javascript
   const wallet = JSON.parse(fs.readFileSync('path/to/wallet.json'));

   const transaction = await arweave.createTransaction({
       data: 'Hello, Arweave!',
   }, wallet);

   transaction.addTag('Content-Type', 'text/plain');

   await arweave.transactions.sign(transaction, wallet);

   const response = await arweave.transactions.post(transaction);
   console.log(response.status); // Should log 200 if successful
   ```

**Advanced Tools:**

For more sophisticated interactions and additional functionalities, consider using advanced tools like **@permaweb/aoconnect** and **@ardriveapp/turbo-sdk**. These libraries provide additional features for interacting with the Arweave network, giving you more options and flexibility in your development workflow.

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

##Weavers YT##

[Turbo SDK 101 with Stephen (aka Bobinstein) from Ario: Dive into the basics of the Turbo SDK and learn how to harness its power for your Arweave projects.](https://youtu.be/3F95opFzZ7Q?si=gwxQnp32SMza9tgS)

[Building an Arena Game on AO with Bots by Tom Rakis: Explore how to build interactive games on the Arweave network using the AO protocol.](https://youtu.be/BPHzWywrBLo?si=B-pj026YC7NDgurE)

[Gather Chat Walkthrough Video: Discover how to create decentralized chat applications using Arweave and Gather.](https://youtu.be/zF9d5Y5L2iY?si=KeR0U0_IegvnPaG_)

##aoVenture Sessions##

[Ao Ventures Keynote #6 by Tom Wilson](https://www.crowdcast.io/c/aoventureskeynote6)

[Building an Arena Game on AO with Bots by Tom Rakis: Explore how to build interactive games on the Arweave network using the AO protocol.](https://youtu.be/BPHzWywrBLo?si=B-pj026YC7NDgurE)

[Gather Chat Walkthrough Video: Discover how to create decentralized chat applications using Arweave and Gather.](https://youtu.be/zF9d5Y5L2iY?si=KeR0U0_IegvnPaG_)

These walkthroughs provide valuable insights and practical demonstrations that can help you take your Arweave development skills to the next level. Happy coding!

At our event, we'll provide guidance, answer your questions, and offer hands-on assistance to ensure you can deploy your projects on Arweave smoothly. Join us to discover just how easy and powerful deploying on Arweave can be. See you there!


## Examples Built with Arweave and AO ##

[Gather Chat](https://gatherchat.g8way.io/#/)
| [Repo](https://github.com/elliotsayes/gatherchat)

[TYPR - AO Twitter Clone](https://www.typr.day/) | [Repo](https://github.com/iamgamelover/ao-twitter)


## AO Documentation ##

[ao Cookbook](https://ao.g8way.io/)

[ao Cookbook](https://ao.g8way.io/)

[ao Cookbook](https://ao.g8way.io/)

[ao Cookbook](https://ao.g8way.io/)

[ao Cookbook](https://ao.g8way.io/)

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

[ArIO SDK](https://github.com/ar-io/ar-io-sdk) - This SDK provides functionality for interacting with the ar.io ecosystem of services (e.g. gateways and observers) and protocols (e.g. ArNS). It is available for both NodeJS and Web environments.

## Permaweb Cookbook ##

[Permaweb Cookbook Official](https://ao.g8way.io/) - The Permaweb Cookbook is a developer resource that provides the essential concepts and references for buiding applications on the Permaweb. Each concept and reference will focus on specific aspects of the Permaweb development ecosystem while providing additional details and usage examples

[Get a webpage on to the permaweb](https://cookbook.arweave.dev/getting-started/quick-starts/hw-code.html) - This guide walks you through a quick way to get a static HTML, CSS and JavaScript webpage on to the permaweb using a few lines of code and a command-line interface (CLI)

[Webpage uploade with Arweave.js and Irys](https://cookbook.arweave.dev/getting-started/quick-starts/hw-nodejs.html) - simple way to get data on to the permaweb using arweave-js and irys

[Querying Transactions - Irys & GraphQl](https://cookbook.arweave.dev/guides/posting-transactions/arweave-js.html) -Arweave native transactions can be posted directly to a node or gateway using the arweave-js package.

[Fetching Transaction Data](https://cookbook.arweave.dev/guides/http-api.html) - A Transaction data caching service is offered by most gateways though a set of HTTP endpoints. Any HTTP client/package can be used to request transaction data from these endpoints. For example Axios or Fetch for JavaScript, Guzzle for PHP, etc.

[Posting Transactions using arweave-js](https://cookbook.arweave.dev/guides/posting-transactions/arweave-js.html) -Arweave native transactions can be posted directly to a node or gateway using the arweave-js package.

[Tags - Transaction Metadata](https://cookbook.arweave.dev/concepts/tags.html) - Arweave can be thought of as a permanent append-only hard drive where each entry on the drive is its own unique transaction. Transactions have a unique ID, signature, and owner address for the address that signed and paid for the transaction to be posted. 

[Path Manifests](https://cookbook.arweave.dev/concepts/manifests.html) - Path Manifests are a way to link multiple transactions together under a single base transaction ID and give them human readable file names. 

## Starter Kit and Getting Started ##

[React Starter Kits](https://cookbook.arweave.dev/kits/react/index.html)

[Svelte Starter Kits](https://cookbook.arweave.dev/kits/svelte/index.html)

[Vue Starter Kits](https://cookbook.arweave.dev/kits/vue/index.html)

[AOS Release v1.0.25 ](https://www.crowdcast.io/c/aoventureskeynote6)

[AOS-Sqlite Workshop by Tom Wilson](https://hackmd.io/@ao-docs/rkM1C9m40)

## Recommended VS Code Extensions ##

[ES-Lint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) |
[Editor-Config](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig) |
[Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) | 
[ZipFS](https://marketplace.visualstudio.com/items?itemName=arcanis.vscode-zipfs) |
[LUA\AO lua-language-server](https://marketplace.visualstudio.com/items?itemName=sumneko.lua)



---




This guide now includes information on both AoConnect and the Turbo SDK, providing developers with options for advanced functionalities when deploying on Arweave.