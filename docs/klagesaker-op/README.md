# Klagesaker (Claims and Complaints)

Claims and complaints to the Norwegian Industrial Property Office (NIPO/Patentstyret) and the Norwegian Board of Appeal for Industrial Property Rights (KFIR). The form also handles complaints about company names registered in the Register of Business Enterprises (Foretaksregisteret).

## Prefill Field Name

When creating an instance via multipart/form-data, use the field name: **`klagesaker-opData-prefill`**

Both `application/json` and `application/xml` content types are supported.

## Prefill Schema

- [OPDataPrefill.schema.json](./OPDataPrefill.schema.json) — JSON Schema (authoritative, includes examples and conditional validation)
- [OPDataPrefill.xsd](./OPDataPrefill.xsd) — XSD Schema (documentation/XML tooling; does not express all conditional rules)

## Domains and Claim Types

The `domain` field selects the type of case, and determines which `claimType` values are valid:

| `domain` | Valid `claimType` values |
|---|---|
| `trademark` | `reinstatement`, `protest`, `adminTransfer`, `tmAlteration`, `opposition`, `adminInvalidationPriority`, `adminDeletion`, `kfir`, `adminReview` |
| `patent` | `reinstatement`, `protest`, `opposition`, `adminLimitation`, `adminReview`, `kfir` |
| `design` | `reinstatement`, `protest`, `adminReview`, `kfir` |
| `companyNameComplaint` | — (use the `companyNameComplaint` object instead) |

- For `trademark`, `patent`, and `design`, `caseId` (the IP application/registration number) is **required**. The app looks up case details automatically.
- For `companyNameComplaint`, the `companyNameComplaint` object is **required** and `caseId`/`claimType` are not used.

## Roles: Agent vs Claimant

The `agentOrClaimant` field describes the role of the **logged-in submitter**:

| Value | Behavior |
|---|---|
| `claimant` | The submitter is a claimant. The app automatically inserts the logged-in person/organization as the **first claimant** row. Any `claimants[]` from the prefill are added after. |
| `agent` | The submitter acts as a representative. The app automatically creates the agent from the logged-in user. At least one entry in `claimants[]` is **required**. |

> **Important:** Do **not** send agent/submitter details in the payload. The submitter (requestor) and agent information is always derived from the authenticated Altinn user via the national registers (ER/DSF).

## Payment

`paymentMethod` can be `card` or `invoice`. If omitted, the user chooses in the form.

## Attachments

See the Swagger documentation for available file attachment data types, allowed MIME types, and max file counts:

- [Swagger (TT02)](https://pat.apps.tt02.altinn.no/pat/klagesaker-op/swagger/index.html)
- [Swagger (Prod)](https://pat.apps.altinn.no/pat/klagesaker-op/swagger/index.html)

Uploaded attachments are **automatically tagged** with the correct document type. Integrators do not need to set tags manually.

## Country Codes

For valid country codes, use the options endpoint:

```
GET /pat/klagesaker-op/api/options/countries?language=nb
```

Use the returned `value` (e.g. `"NO"`) in prefill data, not the `label`.

## Full Data Model Reference

See [OPDataPrefill.schema.json](./OPDataPrefill.schema.json) for the complete data model with all fields, types, conditional requirements, and inline examples.

See the [main README](../../README.md#creating-instances-with-prefill-data) for a complete multipart request example.
