# Postman Collection

Postman collection and environments for the NIPO Altinn 3 integration APIs.

## Import into Postman

1. Open Postman
2. Click **Import** → Select files from this folder:
   - `collections/NIPO-System-Integration-APIs.postman_collection.json`
   - Choose one environment from `environments/`:
     - `local.postman_environment.json` — Local development
     - `tt02.postman_environment.json` — Test environment
     - `production.postman_environment.json` — Production

## Environment Variables

| Variable | Description | Local | TT02 | Prod |
|----------|-------------|-------|------|------|
| `baseUrl` | Altinn app base URL | `http://local.altinn.cloud` | `https://pat.apps.tt02.altinn.no` | `https://pat.apps.altinn.no` |
| `tokenEndpoint` | Token service URL | Local test endpoint | N/A | N/A |
| `maskinportenToken` | Your Maskinporten token | N/A | Your token | Your token |
| `testPartyId` | Altinn party ID for testing | `501337` | Your org's party ID | Your org's party ID |
| `org` | Organization short name | `pat` | `pat` | `pat` |
| `bearerToken` | Auto-populated token | Auto | Auto | Auto |

For Swagger, schemas, authentication, and a step-by-step walkthrough, see the [main README](../README.md).

