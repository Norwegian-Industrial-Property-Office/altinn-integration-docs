# Correspondence

## Prefill Field Name

When creating an instance via multipart/form-data, use the field name: **`correspondenceData-prefill`**

## Prefill Schema

- [CorrespondenceDataPrefill.schema.json](./CorrespondenceDataPrefill.schema.json) — JSON Schema
- [CorrespondenceDataPrefill.xsd](./CorrespondenceDataPrefill.xsd) — XSD Schema

## Attachments

Attachments are included as additional parts in the multipart/form-data request. The part name must match a data type defined in the form's application metadata (e.g., `fileAttachment-MainLetter`).

### Document Types

Correspondence uses domain-specific document types. To retrieve available document types, use the options and select doctypes for optionsId and set queryParams to 
```
{
  "domainSelector": "{{domain}}"
}
```
Valid {{domain}}: `trademark`, `patent`, `design`, `other`

### Example

```http
--boundary
Content-Disposition: form-data; name="fileAttachment-MainLetter"; filename="document.pdf"
Content-Type: application/pdf

<binary-file-content>
--boundary--
```

See the [main README](../../README.md#creating-instances-with-prefill-data) for a complete multipart request example.

### Options
To get valid countries use the countries optionsId and select langauge, either `nb` or `en`