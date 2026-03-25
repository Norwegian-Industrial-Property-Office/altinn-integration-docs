# Varemerke (Trademark)

## Prefill Field Name

When creating an instance via multipart/form-data, use the field name: **`varemerkeData-prefill`**

## Prefill Schema

The varemerke prefill payload is sent as the `varemerkeData-prefill` multipart part with content type `application/json`.

Available schema files:

- [JSON Schema](VaremerkeDataPrefill.schema.json)
- [XSD](VaremerkeDataPrefill.xsd)


Notes:
- Use Swagger as the source for attachment field names, allowed MIME types, and file-count limits
- Use this repository as the source for the prefill data model

## Attachments

Attachments are sent as additional parts in the same `multipart/form-data` request as `varemerkeData-prefill` and `instance`.

Each attachment part must:

- Use the attachment data type name (from Swagger) as the multipart part name
- Include a `Content-Type` header matching the file's actual MIME type, since the app validates MIME type
- Include a `filename` in the `Content-Disposition` header
- Contain the binary file content as the part body

## Country Codes

Use the Swagger options endpoint to look up valid country codes:

`GET /pat/varemerke/api/options/{optionsId}?language=nb`

- Use `countries` for address fields such as `applicants[].country`
- Use `priorityCountries` for `priorities[].countryCode` and exhibition `countryCode`
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