# Patent

## Prefill Field Name

When creating an instance via multipart/form-data, use the field name: **`patentData-prefill`**

## Prefill Schema

- [PatentDataPrefill.schema.json](./PatentDataPrefill.schema.json) — JSON Schema
- [PatentDataPrefill.xsd](./PatentDataPrefill.xsd) — XSD Schema

## Attachments

Attachments are included as additional parts in the multipart/form-data request. 
The part name must match a data type defined in the form's application metadata (e.g., `fileAttachment-descriptionFiles`).
