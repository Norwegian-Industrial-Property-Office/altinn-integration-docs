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

## Self-Identified (Email) Users

See [Self-Identified (Email) Users](../../README.md#self-identified-email-users) in the main README. This form uses the `selfIdentifiedSubmitter` object.

## Attachments

The app supports file attachments uploaded as part of the multipart/form-data request. To find the available attachment types, allowed MIME types, size limits, and max file counts, consult the application metadata endpoint:

`GET /pat/ansvarsmerke/api/v1/applicationmetadata`

Use the attachment `id` as the multipart field name when uploading.
