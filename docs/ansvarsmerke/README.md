# Ansvarsmerke (Hallmark / Responsibility Mark)

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

To populate the Requestor with actual name and address data, include a `submitter` object in the prefill payload:

Notes:
- The `submitter` object is optional. Without it, the Requestor fields will be blank for self-identified users.
- `submitter.customerNumber` is optional and sets the customer number directly without a SANT lookup.
- The `submitter` uses the same field structure as applicants (role, name, address) — see the JSON schema for details.
