# Nuxt-TS: Nuxt application powered by TypeScript

[Nuxt 2.4.0 release (Jan-2019)](https://dev.to/nuxt/nuxtjs-v240-is-out-typescript-smart-prefetching-and-more-18d)
has pushed one step forward TypeScript integration into Nuxt. **Kudos to Nuxt team**.

Half tutorial, half exploration, I want to check out how far I can get
with Nuxt+TypeScript in a full application from scratch.

- [Initialise Project](#initialise-project)
- [Switch to TypeScript](#switch-to-typescript)

## Initialise project

Create a NuxtJS project as usual ([Nuxt doc](https://nuxtjs.org/guide/installation)):

```sh
yarn create nuxt-app {your project name}
```

For the sake of the tutorial, my _nuxt-ts_ project is bare metal: no
plugins, no CSS framework, default server framework, no test
framework. I selected _Yarn_ as my package manager.

![Project creation](screenshots/01.01_create_project.png)

The Nuxt project structure is explained in [Nuxt doc](https://nuxtjs.org/guide/directory-structure):

![Project structure](screenshots/01.02_project_structure.png)

Run the scaffolded project:

```sh
cd {your project folder}
yarn dev
```

Nuxt should not encounter any error:

![Project run](screenshots/01.03_run.png)

And the application should be available on <http://localhost:3000>:

![Project is working](screenshots/01.04_project_is_working.png)

## Switch to TypeScript

Before adding anything, let switch to TypeScript first. I was
guided by the two following links:

- [Nuxt-ts helloworld](https://codesandbox.io/s/github/nuxt/nuxt.js/tree/dev/examples/typescript)
- [Hackernews on nuxt-ts](https://github.com/nuxt-community/hackernews-nuxt-ts)

### Adding Nuxt-ts

```sh
yarn add nuxt-ts
yarn add --dev nuxt-property-decorator
yarn remove nuxt
```

- `nuxt-ts` is TypeScript counterpart of `nuxt`
- [`nuxt-property-decorator`](https://github.com/nuxt-community/nuxt-property-decorator/) is
  Nuxt counter part of [`vue-property-decorator`](https://github.com/kaorun343/vue-property-decorator)

### Update configuration

Few configurations have to be added or changed:

Update _package.json_ by using `nuxt-ts` instead of `nuxt`:

```json
{
  "scripts": {
    "dev": "nuxt-ts",
    "build": "nuxt-ts build",
    "start": "nuxt-ts start",
    "generate": "nuxt-ts generate"
  }
}
```

Add a tsconfig.json:

```json
{
  "extends": "@nuxt/typescript",
  "compilerOptions": {
    "baseUrl": ".",
    "types": ["@types/node", "@nuxt/vue-app"],
    "experimentalDecorators": true,
    "resolveJsonModule": true,
    "esModuleInterop": true,
    "paths": {
      "@/*": ["*"],
      "~/*": ["*"]
    }
  }
}
```

- `experimentalDecorators` is required when using Vue decorators
- `resolveJsonModule` and `esModuleInterop` are required when
  [importing JSON in TypeScript](https://hackernoon.com/import-json-into-typescript-8d465beded79)
- `paths` are Nuxt convention and [it rocks with VS Code](https://medium.com/@caludio/how-to-use-module-path-aliases-in-visual-studio-typescript-and-javascript-e7851df8eeaa)

Renaming _nuxt.config.js_ into _nuxt.config.ts_. I simply change the first line:

```diff
- var pkg = require('./package')
+ import pkg from "./package.json";
```

and commented out the `build: { ... }` content to avoid having a _"declared but never used"_
and _"implicitly has an 'any' type"_ error

## Update existing code

The scaffolded application is not much, only _pages/index.vue_ has to
be updated:

```vue
<script lang="ts">
import { Component, Vue } from "nuxt-property-decorator";

import Logo from "@/components/Logo.vue";

@Component({
  components: {
    Logo
  }
})
export default class Index extends Vue {}
</script>
```

> Don't forget `lang="ts"`

At this stage, I rely on [VS Code Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
to format my code. We will define Prettier later on.