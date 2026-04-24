# Patent

## Prefill Field Name

When creating an instance via multipart/form-data, use the field name: **`patentData-prefill`**

## Prefill Schema

- [PatentDataPrefill.schema.json](./PatentDataPrefill.schema.json) — JSON Schema
- [PatentDataPrefill.xsd](./PatentDataPrefill.xsd) — XSD Schema

## Country Codes

Use the options endpoints to fetch valid codes before building prefill payloads.

### Endpoints

- `GET /pat/patent/api/options/countries?language=nb`
- `GET /pat/patent/api/options/priorityCountries?language=nb`

### Which endpoint to use

| Prefill field | Use codes from |
|---|---|
| `applicantsAndInventors[].country` | `countries` |
| `submitterIdentity.country` | `countries` |
| `earlierApplications[].country` | `priorityCountries` |

### Important

- Use the returned `value` in prefill data.
- `label` is display text only.
- `language` changes labels, not values.

### Example

If the API returns:

```json
[
  { "value": "NO", "label": "Norge" },
  { "value": "SE", "label": "Sverige" }
]
```

then prefill must use `"country": "NO"` (not `"Norge"`).

For `earlierApplications[].country`, the code list may include authorities (for example WIPO) in addition to countries.

## Attachments

Attachments are included as additional parts in the multipart/form-data request. 
The part name must match a data type defined in the form's application metadata (e.g., `fileAttachment-descriptionFiles`).
