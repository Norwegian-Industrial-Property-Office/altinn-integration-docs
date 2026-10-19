# Design

## Prefill Field Name

When creating an instance via multipart/form-data, use the field name: **`designData-prefill`**

## Prefill Schema

- [DesignDataPrefill.schema.json](./DesignDataPrefill.schema.json) — JSON Schema
- [DesignDataPrefill.xsd](./DesignDataPrefill.xsd) — XSD Schema

## Country Codes

Use the Swagger options endpoint to look up valid country codes:
`GET /pat/design/api/options/countries?language=nb`
- Use the returned `value` in the prefill data model, not the `label`

## Self-Identified (Email) Users

See [Self-Identified (Email) Users](../../README.md#self-identified-email-users) in the main README. This form uses the `submitter` object.

## Attachments

The app supports file attachments uploaded as part of the multipart/form-data request. To find the available attachment types, allowed MIME types, size limits, and max file counts, consult the application metadata endpoint:
`GET /pat/design/api/v1/applicationmetadata`
Use the attachment `id` as the multipart field name when uploading.

### Attachment Types

| Data Type | Content Types | Max Count | Max Size |
|-----------|---------------|-----------|----------|
| `fileAttachment-designs` | `image/jpeg`, `image/png`, `video/mp4` | 200 | 50 MB |
| `fileAttachment-powerOfAttorney` | `application/pdf`, `.docx`, `.odt` | 10 | 50 MB |

### Image Filename Mapping

Each product in the prefill payload must include an `imageFileNames` array with at least one filename. These filenames must match the filenames of files uploaded as `fileAttachment-designs` in the same multipart request. The app maps each filename to the corresponding uploaded attachment.

Example: if a product has `"imageFileNames": ["design-1-front.png"]`, a file named `design-1-front.png` must be uploaded as a `fileAttachment-designs` part in the same request.
