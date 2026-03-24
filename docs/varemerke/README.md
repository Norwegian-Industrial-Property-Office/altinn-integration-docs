# Varemerke (Trademark)

## Prefill Field Name

When creating an instance via multipart/form-data, use the field name: **`varemerkeData-prefill`**

## Prefill Schema

The varemerke prefill payload is sent as the `varemerkeData-prefill` multipart part with content type `application/json`.

Available schema files:

- [JSON Schema](VaremerkeDataPrefill.schema.json)
- [XSD](VaremerkeDataPrefill.xsd)

The prefill data model is documented in this repository, not in Swagger. Swagger is useful for looking up attachment types and other field names exposed by the app.

Integration notes:

- The payload is read once during instantiation and then removed after successful mapping.
- If validation fails, the payload is not partially applied.
- The XSD is provided for XML-oriented tooling and documentation. Runtime multipart instantiation still expects JSON.
- For agent-driven payloads, you may add `"agentCustomerNumber": "123456"` together with `"agentOrApplicant": "agent"`.

## Attachments

Attachments are sent as additional parts in the same `multipart/form-data` request as `varemerkeData-prefill` and `instance`.

In Postman:

- Set `varemerkeData-prefill` and `instance` to `Text` with `application/json`
- Set attachment parts to `File`
- Use the attachment field name as the multipart key
- Set `Content-Type` to the file's actual MIME type, since the app validates MIME type

Notes:

- Use Swagger as the source for attachment field names, allowed MIME types, and file-count limits
- Use this repository as the source for the prefill data model

## Country Codes

Use the Swagger options endpoint to look up valid country codes:

`GET /pat/varemerke/api/options/{optionsId}?language=nb`

- Use `countries` for address fields such as `applicants[].country`
- Use `priorityCountries` for `priorities[].countryCode` and exhibition `countryCode`
- Use the returned `value` in the prefill data model, not the `label`
