# bun-plugin-lingui-macro

> bun plugin to compile [Lingui](https://lingui.dev) macro

## Description

The plugin will add a babel with `@lingui/babel-plugin-lingui-macro` to your bun setup.
Supports Lingui v5 and v6.

Plugin will process only files where macro import is found.

If you already have babel or SWC in your pipeline consider to use `@lingui/babel-plugin-lingui-macro` or
`@lingui/swc-plugin` directly.

## Installation

```sh
bun add --dev bun-plugin-lingui-macro
# npm install --save-dev bun-plugin-lingui-macro
# yarn add --dev bun-plugin-lingui-macro
```

If your project uses Lingui v6, also add the matching macro package so the v6 transformer is selected:

```sh
bun add --dev @lingui/babel-plugin-lingui-macro@6
```

## Usage

### Macro Transformation

```ts
import { pluginLinguiMacro } from "bun-plugin-lingui-macro";

await Bun.build({
  plugins: [pluginLinguiMacro()],
});
```

To disable the `.po` file loader:

```ts
await Bun.build({
  plugins: [pluginLinguiMacro({ loader: false })],
});
```

The plugin will transform Lingui macros in your `.js`/`.ts` files:

```typescript
import { Trans } from "@lingui/macro";

// This will be compiled at build time
<Trans>Hello World</Trans>
```

### .po File Loading

The plugin also supports direct imports of `.po` catalog files, eliminating the need for a separate `lingui compile` step:

```typescript
import { messages as enMessages } from "./locales/en/messages.po";
import { messages as fiMessages } from "./locales/fi/messages.po";

i18n.load({
  en: enMessages,
  fi: fiMessages,
});
```

The plugin automatically:

1. Loads the `.po` file using your `lingui.config.js` configuration
2. Compiles it to JavaScript using Lingui's compiler
3. Returns an ES module: `export const messages = {...}`

**TypeScript Support:**

Create a `lingui.d.ts` file in your project for proper type checking:

```typescript
declare module "*.po" {
  import type { Messages } from "@lingui/core";
  export const messages: Messages;
}
```

This provides type safety and autocomplete for your `.po` imports.

## License

This package is licensed under MIT license.
