# Notes For Me

## Setting up Astro:

`bun create astro MY-PROJECT`
Add Dependencies by selecting option or `bun install`

## Editing package.json to run using Bun:

{

    "dev": "bun --bun astro dev",
    "build": "bun --bun astro build",
    "preview": "bun --bun astro preview"

}

## Changing Folder Structure:

Create a client/ folder in src.
Edit astro.config.mjs to point to a new srcDir: `srcDir: './src/client'`

## Setting up TailwindCSS:

`bunx astro add tailwind`

## Setting up daisyUI:

`bun add -d daisyui@latest`

Add `@plugin "daisyui";` to global.css file

## Setting up Prettier

`bun add -d --save-exact prettier prettier-plugin-astro`

For TailwindCSS:
`bun add -d prettier-plugin-tailwindcss`

Add the .prettierrc file to root:
`{
  "plugins": ["prettier-plugin-astro", "prettier-plugin-tailwindcss"],
  "overrides": [
    {
      "files": "*.astro",
      "options": {
        "parser": "astro"
      }
    }
  ]
}
`

Add a script to run Prettier formatting in package.json:
`"format": "prettier --write ."`

Add a script to check Prettier formatting in package.json:
`"format:check": "prettier --check ."`

## Setting up ESLint:

`bun add -d eslint eslint-plugin-astro`

For TypeScript:
`bun add -d @typescript-eslint/parser`

Create eslint.config.js file:

```
import eslintPluginAstro from "eslint-plugin-astro";
export default [
  ...eslintPluginAstro.configs.recommended,
  {
    files: ["*.astro", "*.ts", "*.tsx"],
    processor: "astro/client-side-ts",
    rules: {},
  },
];
```

Add lint script to package.json:
`"lint": "eslint 'src/**/*.{js,ts,tsx,astro}'"`

Add to .vscode/settings.json:

```
{
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "astro",
    "typescript",
    "typescriptreact"
  ]
}
```

## Add React

`bunx astro add react`

## For good icons: add lucide-react

`bun add lucide-react`

## JSX vs HTML:

Use HTML when building static parts. Use JSX when building dynamic (data-driven) parts.

## Take a look at Card.astro and index.astro for a way to reuse code/components

## Adding in Hono Server

Install Hono `bun add hono`
Add a server/ folder in src/

Add basic Hono code:

```
import { Hono } from "hono";
const app = new Hono();

app.get("/", (c) => c.text("Hono!"));

export default app;
```

## Add Wrangler for Cloudflare tools

`bun add wrangler`

Then in package.json add worker script:
`"worker": "wrangler dev"`

Add wrangler.jsonc file for config:

```
{
  "name": "cloudflare-hono-template",
  "main": "src/server/index.ts",
  // Set this to today's date
  "compatibility_date": "2026-07-15",
}

```

Note: Have to use Node 22 with this because its not directly compatible with Bun. So we must run `nvm use 22` before `bun run worker`

Trailing comma fix for wrangler.jsonc in .pretterrc

## Config proxy for local dev:

Edit astro.config.mjs in vite block under plugins:

```
server: {
      proxy: {
        "/api": {
          target: "http://localhost:8787"
        }
      }
    }
```

## Setting up Staging and Production Environments in Wrangler:

Edit wrangler.jsonc:

```
"env": {
    "staging": {
      "vars": {
        "ENVIRONMENT": "staging",
        "CF_ACCESS_DOMAIN": "https://ayushkulkarni714.cloudflareaccess.com",
        "POLICY_AUD": "c81c4ef4a81123000f970fd4e249c1999c85a544dfe6bee29a171428b148b1a1"
      }
    },
    "production": {
      "vars": {
        "ENVIRONMENT": "production"
      }
    }
  }
```

