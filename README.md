# PasteDB JavaScript SDK

Official Node.js SDK for interacting with the [PasteDB](https://pastedb.netlify.app) API.

Create, retrieve, update, delete, and explore pastes, execute code, manage API keys, retrieve paste statistics, and more — directly from your Node.js application.

## Installation

```bash
npm install pastedb-js
```

## Requirements

* Node.js 18+ recommended
* A PasteDB API key for authenticated operations

> The SDK uses the built-in `fetch` API available in modern versions of Node.js.

## Quick Start

```js
const { Client } = require("pastedb-js");

const client = new Client("YOUR_API_KEY");

async function main() {
    const user = await client.me();

    console.log(user);
}

main().catch(console.error);
```

## Configuration

### Client

```js
const { Client } = require("pastedb-js");

const client = new Client(
    "YOUR_API_KEY",
    "https://pastedb-rw62.onrender.com"
);
```

Both parameters are optional:

```js
const client = new Client();
```

The default API URL is:

```text
https://pastedb-rw62.onrender.com
```

If an API key is provided, the SDK automatically sends it using both:

* `x-api-key`
* `Authorization: Bearer <API_KEY>`

## API Reference

### Get Current User

```js
const user = await client.me();
```

Equivalent endpoint:

```text
GET /api/me
```

---

### Create a Paste

```js
const paste = await client.createPaste({
    title: "My Paste",
    content: "Hello from PasteDB!",
    images:[]
});
```

Equivalent endpoint:

```text
POST /create
```

The `data` object is passed directly to the API, allowing you to provide the fields supported by PasteDB.

---

### Get a Paste

```js
const paste = await client.getPaste("paste-id");
```

Equivalent endpoint:

```text
GET /p/:pasteId
```

---

### Update a Paste

```js
const updated = await client.updatePaste("paste-id", {
    title: "Updated Title",
    content: "Updated content",
    images:["url1","url2"]
});
```

Equivalent endpoint:

```text
PUT /api/paste/:pasteId
```

---

### Explore Public Pastes

```js
const pastes = await client.explore();
```

Equivalent endpoint:

```text
GET /explore
```

---

### Run Code

```js
const result = await client.runCode(
    "javascript",
    'console.log("Hello, PasteDB!")'
);
```

Equivalent endpoint:

```text
POST /run
```

Request body:

```json
{
    "language": "javascript",
    "code": "console.log(\"Hello, PasteDB!\")"
}
```

---

### Get Paste Images

```js
const images = await client.getImages("paste-id");
```

Equivalent endpoint:

```text
GET /images/:pasteId
```

---

### Get Paste Statistics

```js
const stats = await client.pasteStats("paste-id");
```

Equivalent endpoint:

```text
GET /stats/:pasteId
```

---

### Check a Custom Paste ID

```js
const result = await client.checkCustomId("my-custom-id");
```

Equivalent endpoint:

```text
GET /check-id?id=my-custom-id
```

---

## API Key Management

### Generate an API Key

```js
const apiKey = await client.generateApiKey("My Application");
```

Equivalent endpoint:

```text
POST /generate-api-key
```

Request body:

```json
{
    "name": "My Application"
}
```

### List API Keys

```js
const keys = await client.myApiKeys();
```

Equivalent endpoint:

```text
GET /my-api-keys
```

### Delete an API Key

```js
await client.deleteApiKey("API_KEY");
```

Equivalent endpoint:

```text
DELETE /delete-api-key/:apiKey
```

> Keep API keys private and never commit them to source control.

## Direct API Methods

The SDK also exposes methods corresponding to the `/api` endpoints.

### API User

```js
const user = await client.apiMe();
```

```text
GET /api/me
```

### Create a Paste

```js
const paste = await client.apiCreatePaste({
    title: "API Paste",
    content: "Created through the API"
});
```

```text
POST /api/create
```

### Get a Paste

```js
const paste = await client.apiGetPaste("paste-id");
```

```text
GET /api/paste/:pasteId
```

### Delete a Paste

```js
await client.apiDeletePaste("paste-id");
```

```text
DELETE /api/paste/:pasteId
```

### Update a Paste

```js
const paste = await client.apiUpdatePaste("paste-id", {
    title: "Updated Paste"
});
```

```text
PUT /api/paste/:pasteId
```

### Get Your Pastes

```js
const pastes = await client.apiUserPastes();
```

```text
GET /api/pastes
```

## Error Handling

The SDK provides a custom `PasteDBError` class for API and request errors.

```js
const { Client, PasteDBError } = require("pastedb-js");

const client = new Client("YOUR_API_KEY");

try {
    const paste = await client.getPaste("invalid-id");
    console.log(paste);
} catch (error) {
    if (error instanceof PasteDBError) {
        console.error("PasteDB error:", error.message);
    } else {
        console.error("Unexpected error:", error);
    }
}
```

### Request Timeout

Requests automatically time out after **30 seconds**.

A timeout throws:

```text
PasteDBError: Request timed out.
```

HTTP errors are also converted into `PasteDBError` instances and include the HTTP status code and API response.

## Complete Example

```js
const { Client, PasteDBError } = require("pastedb-js");

const client = new Client(process.env.PASTEDB_API_KEY);

async function main() {
    try {
        // Check the authenticated user
        const user = await client.me();
        console.log("User:", user);

        // Create a paste
        const paste = await client.createPaste({
            title: "My First Paste",
            content: "Hello from pastedb-js!",
            images:[]
        });

        console.log("Created paste:", paste);

        // Retrieve the paste
        const fetched = await client.getPaste(paste.id);

        console.log("Fetched paste:", fetched);

        // Get statistics
        const stats = await client.pasteStats(paste.id);

        console.log("Stats:", stats);
    } catch (error) {
        if (error instanceof PasteDBError) {
            console.error("PasteDB error:", error.message);
        } else {
            console.error(error);
        }
    }
}

main();
```

## Exported Classes

The package exports:

```js
const {
    Client,
    PasteDBError
} = require("pastedb-js");
```

### `Client`

Main SDK client used to communicate with PasteDB.

### `PasteDBError`

Custom error class used for PasteDB request failures and timeouts.

## Project Structure

```text
pastedb-js/
├── lib/
│   └── client.js
├── index.js
├── LICENSE
├── package.json
└── README.md
```

## License

This project is licensed under the [MIT License](LICENSE).

## Author

**Aditya Sorathiya**

## Package

[**pastedb-js**](https://npmjs.com/package/pastedb-js) — Official PasteDB Node.js SDK.
