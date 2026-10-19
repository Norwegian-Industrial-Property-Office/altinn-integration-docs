# Varemerke (Trademark)

## Prefill Schema

The varemerke prefill payload is sent as the `varemerkeData-prefill` multipart part with content type `application/json`.

Available schema files:

- [JSON Schema](VaremerkeDataPrefill.schema.json)
- ( [XSD](VaremerkeDataPrefill.xsd) )

## Attachments

Attachments are sent as additional parts in the same `multipart/form-data` request as `varemerkeData-prefill` and `instance`.

Each attachment part must:

- Use the attachment data type name (from Swagger) as the multipart part name
- Include a `Content-Type` header matching the file's actual MIME type, since the app validates MIME type
- Include a `filename` in the `Content-Disposition` header
- Contain the binary file content as the part body
>Note: Use Swagger as the source for attachment field names, allowed MIME types, and file-count limits

## Country Codes

Use the Swagger options endpoint to look up valid country codes:

`GET /pat/varemerke/api/options/{optionsId}?language=nb`

- Use `countries` for address fields such as `applicants[].country`
- Use `priorityCountries` for `priorities[].countryCode` and exhibition `countryCode`
- Use the returned `value` in the prefill data model, not the `label`

## Self-Identified (Email) Users

See [Self-Identified (Email) Users](../../README.md#self-identified-email-users) in the main README. This form uses the `submitter` object.