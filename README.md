# NIPO System Integration APIs (Altinn)

Postman collection for integrating systems with the Norwegian Industrial Property Office's (Patentstyret) Altinn 3 forms.

## 🚀 Quick Start

1. **Download** the Postman collection and environment files from the [`postman/`](./postman/) folder
2. **Import** into Postman
3. **Configure** your environment (see [setup guide](./postman/README.md))
4. **Get a token** and start making API calls

For a step-by-step walkthrough (local setup → TT02), see the [Getting Started guide](./docs/getting-started.md).


## 📐 Data Model Schemas

Prefill data model schemas are available in [`schemas/`](./schemas/) for client-side validation and code generation:

| Form | JSON Schema | XSD |
|------|-------------|-----|
| Correspondence | [CorrespondenceDataPrefill.schema.json](./schemas/correspondence/CorrespondenceDataPrefill.schema.json) | [CorrespondenceDataPrefill.xsd](./schemas/correspondence/CorrespondenceDataPrefill.xsd) |

## 🔍 Swagger / OpenAPI

Interactive API documentation is available via Swagger UI:

| App | Environment | Swagger URL |
|-----|-------------|-------------|
| Correspondence | Local | http://local.altinn.cloud/pat/correspondence/swagger/index.html |
| Correspondence | TT02 (Test) | https://pat.apps.tt02.altinn.no/pat/correspondence/swagger/index.html |


## 🔐 Authentication

Systems authenticate using the **System User + Maskinporten** pattern — the standard Altinn 3 flow. You use your own Maskinporten client, and the Norwegian organization you act on behalf of grants access via a System User in Altinn.

Patentstyret does not provide tokens or API keys.
