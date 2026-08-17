# Klagesaker (Claims and Complaints)

Claims and complaints to the Norwegian Industrial Property Office (NIPO/Patentstyret) and the Norwegian Board of Appeal for Industrial Property Rights (KFIR), including complaints about company names in the Register of Business Enterprises.

## Prefill Field Name

When creating an instance via multipart/form-data, use the field name: **`klagesaker-opData-prefill`**

## Prefill Schema

- [OPDataPrefill.schema.json](./OPDataPrefill.schema.json) — JSON Schema
- [OPDataPrefill.xsd](./OPDataPrefill.xsd) — XSD Schema

## Domains and Claim Types

| `domain` | Valid `claimType` values |
|---|---|
| `trademark` | `reinstatement`, `protest`, `adminTransfer`, `tmAlteration`, `opposition`, `adminInvalidationPriority`, `adminDeletion`, `kfir`, `adminReview` |
| `patent` | `reinstatement`, `protest`, `opposition`, `adminLimitation`, `adminReview`, `kfir` |
| `design` | `reinstatement`, `protest`, `adminReview`, `kfir` |
| `companyNameComplaint` | — use the `companyNameComplaint` object instead |

- For `trademark`, `patent` and `design`, `caseId` is required and case details are looked up automatically.
- For `companyNameComplaint`, the `companyNameComplaint` object is required; `caseId` and `claimType` are not used.

## Roles

`agentOrClaimant` describes the role of the logged-in submitter:

| Value | Behavior |
|---|---|
| `claimant` | The logged-in user is inserted as the first claimant. Any `claimants[]` entries are added after. |
| `agent` | The agent is created from the logged-in user. At least one entry in `claimants[]` is required. |

Do not send agent or submitter details in the payload — they are always derived from the authenticated Altinn user.

## Payment

`paymentMethod` can be `card` or `invoice`. If omitted, the user chooses in the form.

## Country Codes

Use the Swagger options endpoint to look up valid country codes:

`GET /pat/klagesaker-op/api/options/countries?language=nb`

- Use the returned `value` in the prefill data model, not the `label`

## Attachments

The app supports file attachments uploaded as part of the multipart/form-data request. To find the available attachment types, allowed MIME types, size limits, and max file counts, consult the application metadata endpoint:

`GET /pat/klagesaker-op/api/v1/applicationmetadata`

Use the attachment `id` as the multipart field name when uploading. Uploaded attachments are tagged with the correct document type automatically.

See the [main README](../../README.md#creating-instances-with-prefill-data) for a complete multipart request example.
