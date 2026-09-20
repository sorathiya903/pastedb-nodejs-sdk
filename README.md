# PasteDB Node.js SDK

> Official Node.js SDK for interacting with the PasteDB API.

Create, manage, and retrieve pastes from your Node.js applications with a simple and developer-friendly interface.

Features

- 🚀 Create new pastes
- 📄 Retrieve existing pastes
- ✏️ Update pastes
- 🗑️ Delete pastes
- 📊 View paste statistics
- 🔍 Explore public pastes
- 🖼️ Retrieve paste images
- 🔑 Manage API keys
- ⚡ Run supported code snippets
- 🔒 API Key authentication

---

Installation

npm install pastedb-js

---

Getting Started

const { Client } = require("pastedb-js");

const client = new Client("YOUR_API_KEY");

If you're only accessing public endpoints, an API key is optional:

const { Client } = require("pastedb");

const client = new Client();

---

Create a Paste

const result = await client.createPaste({
    title: "Hello World",
    content: "console.log('Hello World');",
    language: "javascript"
});

console.log(result);

---

Get a Paste

const paste = await client.getPaste("PASTE_ID");

console.log(paste);

---

Explore Public Pastes

const pastes = await client.explore();

console.log(pastes);

---

Run Code

const result = await client.runCode(
    "python",
    "print('Hello from PasteDB')"
);

console.log(result);

---

API Methods

Public

- "createPaste(data)"
- "getPaste(pasteId)"
- "updatePaste(pasteId, data)"
- "explore()"
- "runCode(language, code)"
- "getImages(pasteId)"
- "pasteStats(pasteId)"
- "checkCustomId(customId)"

Authenticated

- "me()"
- "generateApiKey(name)"
- "myApiKeys()"
- "deleteApiKey(apiKey)"

REST API

- "apiMe()"
- "apiCreatePaste(data)"
- "apiGetPaste(pasteId)"
- "apiUpdatePaste(pasteId, data)"
- "apiDeletePaste(pasteId)"
- "apiUserPastes()"

---

Error Handling

try {
    const paste = await client.getPaste("PASTE_ID");
    console.log(paste);
} catch (err) {
    console.error(err.message);
}

---

Requirements

- Node.js 18 or later

---

Documentation

Visit the official documentation:

https://pastedb.netlify.app/jsdocs

---

Issues

Found a bug or have a feature request?

Please open an issue on the GitHub repository.

---

License

MIT License

---

Made with ❤️ for developers using PasteDB.
