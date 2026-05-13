# Ansvarsmerke (Responsibility Mark)

## Prefill Field Name

When creating an instance via multipart/form-data, use the field name: **`ansvarsmerkeData-prefill`**

## Prefill Schema

- [AnsvarsmerkeDataPrefill.schema.json](./AnsvarsmerkeDataPrefill.schema.json) — JSON Schema
- [AnsvarsmerkeDataPrefill.xsd](./AnsvarsmerkeDataPrefill.xsd) — XSD Schema

## Country Codes

Use the Swagger options endpoint to look up valid country codes:

`GET /pat/ansvarsmerke/api/options/countries?language=nb`

- Use the returned `value` in the prefill data model, not the `label`

## Email User Instantiation (Self-Identified)

For users who do not have a Norwegian national identity number or organization number, use the `username` field in the `instance` part instead of `partyId`:

```json
{"instanceOwner":{"username":"test@example.no"}}
```

To populate the Requestor with actual name and address data, include a `selfIdentifiedSubmitter` object in the prefill payload:

Notes:
- `selfIdentifiedSubmitter.customerNumber` is optional and sets the customer number directly without a SANT lookup.

## Attachments

The app supports file attachments uploaded as part of the multipart/form-data request. To find the available attachment types, allowed MIME types, size limits, and max file counts, consult the application metadata endpoint:

`GET /pat/ansvarsmerke/api/v1/applicationmetadata`

Use the attachment `id` as the multipart field name when uploading.
