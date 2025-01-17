---
title: openapi-typescript
description: Quickstart
---

<img src="/assets/openapi-ts.svg" alt="openapi-typescript" width="200" height="40" />

openapi-typescript generates TypeScript types from static <a href="https://spec.openapis.org/oas/latest.html" target="_blank" rel="noopener noreferrer">OpenAPI</a> schemas quickly using only Node.js. It is fast, lightweight, (almost) dependency-free, and no Java/node-gyp/running OpenAPI servers necessary.

The code is <a href="https://github.com/drwpow/openapi-typescript/blob/main/packages/openapi-typescript/LICENSE" target="_blank" rel="noopener noreferrer">MIT-licensed</a > and free for use.

## Features

- ✅ Supports OpenAPI 3.0 and 3.1 (including advanced features like <a href="https://spec.openapis.org/oas/v3.1.0#discriminator-object" target="_blank" rel="noopener noreferrer">discriminators</a>)
- ✅ Supports YAML and JSON
- ✅ Supports loading via remote URL (simple authentication supported with the `--auth` flag)
- ✅ Supports remote references: `$ref: "external.yaml#components/schemas/User"`
- ✅ Fetches remote schemas quickly using <a href="https://www.npmjs.com/package/undici" target="_blank" rel="noopener noreferrer">undici</a>

_Note: OpenAPI 2.x is supported with versions `5.x` and previous_

## Examples

👀 <a href="https://github.com/drwpow/openapi-typescript/blob/main/packages/openapi-typescript/examples/" target="_blank" rel="noopener noreferrer">See examples</a >

## Setup

This library requires the latest version of <a href="https://nodejs.org/en" target="_blank" rel="noopener noreferrer">Node.js</a> installed (20.x or higher recommended). With that present, run the following in your project:

```bash
npm i -D openapi-typescript
```

## Basic Usage

First, generate a local type file by running `npx openapi-typescript`:

```bash
# Local schema
npx openapi-typescript ./path/to/my/schema.yaml -o ./path/to/my/schema.d.ts
# 🚀 ./path/to/my/schema.yaml -> ./path/to/my/schema.d.ts [7ms]

# Remote schema
npx openapi-typescript https://myapi.dev/api/v1/openapi.yaml -o ./path/to/my/schema.d.ts
# 🚀 https://myapi.dev/api/v1/openapi.yaml -> ./path/to/my/schema.d.ts [250ms]
```

> ⚠️ Be sure to <a href="(https://apitools.dev/swagger-cli/" target="_blank" rel="noopener noreferrer">validate your schemas</a>! openapi-typescript will err on invalid schemas.

Then, import schemas from the generated file like so:

```ts
import { paths, components } from "./path/to/my/schema"; // <- generated by openapi-typescript

// Schema Obj
type MyType = components["schemas"]["MyType"];

// Path params
type EndpointParams = paths["/my/endpoint"]["parameters"];

// Response obj
type SuccessResponse = paths["/my/endpoint"]["get"]["responses"][200]["content"]["application/json"]["schema"];
type ErrorResponse = paths["/my/endpoint"]["get"]["responses"][500]["content"]["application/json"]["schema"];
```

> ✨ **Tip**
>
> Using TypeScript’s bracket notation (`obj["property"]`) is a safe way to access all names in your OpenAPI schema, even the ones that aren’t “TypeScript-safe”

From here, you can use these types for any of the following (but not limited to):

- Using an OpenAPI-supported fetch client (like [openapi-fetch](/openapi-fetch))
- Asserting types for other API requestBodies and responses
- Building core business logic based on API types
- Validating mock test data is up-to-date with the current schema
- Packaging API types into any npm packages you publish (such as client SDKs, etc.)
