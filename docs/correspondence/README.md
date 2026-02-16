# Correspondence

## Prefill Field Name

When creating an instance via multipart/form-data, use the field name: **`correspondenceData-prefill`**

## Prefill Schema

- [CorrespondenceDataPrefill.schema.json](./CorrespondenceDataPrefill.schema.json) — JSON Schema
- [CorrespondenceDataPrefill.xsd](./CorrespondenceDataPrefill.xsd) — XSD Schema

## Document Types

Correspondence uses domain-specific document types. Provide the `domain` from your prefill data as the `domainSelector` parameter:

```http
GET {{basePath}}/pat/correspondence/api/options/doctypes?domainSelector={{domain}}
```

Valid domains: `trademark`, `patent`, `design`, `other`
