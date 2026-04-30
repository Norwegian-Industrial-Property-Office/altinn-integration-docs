# EP Validation

## Prefill Field Name
When creating an instance via multipart/form-data, use the field name: **`ep-validering-prefill`**

## Prefill Schema
- [EpValideringDataPrefill.schema.json](./EpValideringDataPrefill.schema.json) — JSON Schema
- [EpValideringDataPrefill.xsd](./EpValideringDataPrefill.xsd) — XSD Schema

## Request Types
`requestType` must be one of:
| Value | Description |
|---|---|
| `A1/A2` | Publication of EP application |
| `T3` | Publication of claims |
| `T4-opposition` | Revalidation after opposition at EPO |
| `T4-administrativeLimitation` | Revalidation after administrative limitation at EPO |
| `T9` | Correction |

## Country Codes
Use options endpoint values (not labels) for country fields:
- `GET /pat/ep-validering/api/options/countries?language=nb`
Use returned `value` (example: `NO`, `US`) in prefill payload.

## Title and Applicants (EPO Autofetch)
When EP number is entered, the app fetches title and applicants from EPO.
- `norwegianTranslatedTitle` is optional; if omitted or empty, title is auto-filled from EPO.
- `additionalApplicantsBeyondEpoFetched` can be used to append applicants beyond EPO-fetched applicants; these are preserved.
### `additionalApplicantsBeyondEpoFetched[].personOrOrg`
Use:
- `person`
- `org`
If omitted, applicant type is inferred from `companyName`:
- `companyName` present -> `org`
- otherwise -> `person`
### `additionalApplicantsBeyondEpoFetched[].companyNumber` rule for Norway
For organization applicants with `country = "NO"`, `companyNumber` is required.

## Contact Name
- `contactName` can be provided for contact person name (for example when submitting as an organization).

## Submitter (Self-Identified Users)
For email/username-based instance owner (self-identified users), you can provide:
- `submitter.firstName` + `submitter.lastName`, or `submitter.companyName`
- and address fields (`streetAddress`, `postalCode`, `city`, `country`)
This is used to populate requestor/contact identity in the form flow.

## Contact Reference
`contactReference` is the earlier reference from NIPO (if available).

## Attachments
Attachments are sent as additional multipart parts in the same request as `instance` + `ep-validering-prefill`.
- `fileAttachment-epClaims` is required in current form validation flow.
