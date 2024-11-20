---
title: Fleek
---
To deploy to Fleek, use [`@fleek-platform/svelte-adapter`](https://github.com/fleek-platform/fleek-svelte-adapter).

Adding it to your project allows you to specify Fleek-specific options.


## Usage

Install with `nnpm install -D @fleek-platform/svelte-adapter`, then add the adapter to your `svelte.config.js`:


```js
// @errors: 2307 2345
/// file: svelte.config.js
import adapter from '@fleek-platform/svelte-adapter';

export default {
  kit: {
    adapter: adapter({
      // optional configuration
      outDir: '.fleek', // Defaults to '.fleek'
    }),
  },
};
```


### Configuration Options

- `outDir`: File path to output the production build. Defaults to `.fleek` if not specified.


### **Build Output**
When running `npm run build`, the adapter performs a series of steps to prepare your SvelteKit app for deployment on Fleek. Here's a breakdown of the process:

1. **Production Build of SvelteKit App**:
   - The build process optimizes your app for production by compiling Svelte components into efficient JavaScript.
   - It includes minification, tree-shaking, and bundling of JavaScript, CSS, and other assets to ensure a smaller and faster app.

2. **Static Assets**:
   - All static assets, such as images, stylesheets, fonts, and pre-rendered HTML, are stored in the `.fleek/static` directory.
   - These assets are made available through IPFS for decentralized distribution.

3. **Server-Side Functions**:
   - The server-side logic of your app, such as API endpoints or server-rendered pages, is bundled and stored in `.fleek/dist`.
   - The output is optimized for Fleek's serverless environment to handle requests efficiently.


### **Deployment**
Once the build process is complete, the deployment to Fleek Functions involves the following steps:

1. **Command Explanation**:
   ```bash
   fleek functions deploy --bundle=false --path .fleek/dist/index.js --assets .fleek/static
   ```
   - `--bundle=false`: Indicates that the files are already bundled, and Fleek doesn’t need to re-bundle them.
   - `--path .fleek/dist/index.js`: Specifies the entry point for the server-side functions.
   - `--assets .fleek/static`: Specifies the directory containing static assets for the app.

2. **IPFS Integration**:
   - Fleek integrates directly with IPFS, allowing your static assets to be hosted on a decentralized network. This ensures content integrity and global availability.

3. **Scalable Hosting**:
   - Your server-side functions run on Fleek’s serverless infrastructure, providing scalability and efficient resource utilization.


### **Notes**
To ensure a smooth build and deployment process, consider the following:

#### **1. Fleek CLI**
   - The [Fleek CLI](https://fleek.xyz/docs/cli/) is required for deployment. Install it with the following command:
     ```bash
     npm install -g @fleek/cli
     ```
   - Authenticate and configure it with your Fleek account to deploy.

#### **2. Automatic Handling by the Adapter**
   - **Static Asset Serving**:
     - Static files in `.fleek/static` are served through IPFS. This ensures your app assets are distributed across a decentralized network.

   - **Pre-rendered Pages**:
     - Pages defined as static in your SvelteKit routes are pre-rendered and included in `.fleek/static`.

   - **Server-Side Rendering (SSR)**:
     - Dynamic pages and API routes are bundled into `.fleek/dist`, allowing the serverless infrastructure to execute them on demand.

   - **Client/Server Routing**:
     - The app supports client-side navigation for a smooth user experience while handling server-side routing for dynamic pages or APIs.

   - **TypeScript Support**:
     - If your project uses TypeScript, the build process automatically compiles your TypeScript code into JavaScript.

   - **Base Path Configuration**:
     - The adapter handles base paths, making it easier to deploy apps in subdirectories or with custom URL structures.


## Support
This adapter is compatible with:

- Node.js: 18.x and later
- SvelteKit: 2.0 and later
