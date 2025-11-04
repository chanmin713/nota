# manifest.json 스키마(초안)

```json
{
  "name": "string",
  "version": "semver",
  "permissions": ["clipboard", "file", "network", "ai-call", "ontology", "editor", "sidebar"],
  "entrypoints": { "main": "string" },
  "commands": [{ "id": "string", "title": "string" }],
  "sidebar": [{ "id": "string", "title": "string" }],
  "settingsSchema": { }
}
```
