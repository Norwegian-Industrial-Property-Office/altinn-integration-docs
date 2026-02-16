# Correspondence

## Prefill Field Name

When creating an instance via multipart/form-data, use the field name: **`correspondenceData-prefill`**

## Prefill Schema

- [CorrespondenceDataPrefill.schema.json](./CorrespondenceDataPrefill.schema.json) — JSON Schema
- [CorrespondenceDataPrefill.xsd](./CorrespondenceDataPrefill.xsd) — XSD Schema

## Attachments

Attachments are included as additional parts in the multipart/form-data request. The part name must match a data type defined in the form's application metadata (e.g., `fileAttachment-MainLetter`).

### Document Types

Correspondence uses domain-specific document types. To retrieve available document types, provide the `domain` from your prefill data as the `domainSelector` parameter:

```http
GET {{basePath}}/pat/correspondence/api/options/doctypes?domainSelector={{domain}}
```

Valid domains: `trademark`, `patent`, `design`, `other`

### Example

```http
--boundary
Content-Disposition: form-data; name="fileAttachment-MainLetter"; filename="document.pdf"
Content-Type: application/pdf

<binary-file-content>
--boundary--
```

See the [main README](../../README.md#creating-instances-with-prefill-data) for a complete multipart request example.
