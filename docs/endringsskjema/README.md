# Endringsskjema (Changes, Assignment, Lien and License)

Notification of changes to existing IP rights at the Norwegian Industrial Property Office (NIPO/Patentstyret) — change of name/address, transfer of ownership, change of representative, lien/pledge, license, limitation of goods and services, and withdrawal/deletion.

## Prefill Field Name

When creating an instance via multipart/form-data, use the field name: **`endringsskjemaData-prefill`**

## Prefill Schema

- [EndringsskjemaDataPrefill.schema.json](./EndringsskjemaDataPrefill.schema.json) — JSON Schema
- [EndringsskjemaDataPrefill.xsd](./EndringsskjemaDataPrefill.xsd) — XSD Schema

## Domains and Request Types

`domain` determines which `requestType` values are valid:

| `requestType` | Description | Patent | Trademark | Design |
|---|---|:---:|:---:|:---:|
| `nameAddress` | Change of name or address for applicant/owner | ✅ | ✅ | ✅ |
| `fusion` | Transfer of ownership (assignment, merger, division) | ✅ | ✅ | ✅ |
| `agentChange` | Change of representative/agent | ✅ | ✅ | ✅ |
| `lien` | Registration of lien/pledge | ✅ | ✅ | ❌ |
| `license` | Registration of a license agreement | ✅ | ✅ | ✅ |
| `goodsAndServices` | Limitation of goods/services | ❌ | ✅ | ❌ |
| `deleteOrDraw` | Withdrawal of application or deletion of registration | ✅ | ✅ | ✅ |
| `other` | Other changes | ✅ | ✅ | ✅ |

> **PPH is not supported via API prefill.** The form has a `pph` request type (patent only), but it cannot be prefilled — omit it and let the user fill it in, or submit it through a different channel.

Each request type has a matching detail object of the same name. `license` and `lien` are **required** when the corresponding `requestType` is used, because they carry a mandatory `role`. The other detail objects are optional but recommended — without them the user has to fill in that page manually.

## Roles

`license.role` and `lien.role` describe the role of the logged-in submitter and are **required**. Submitter and agent details are never sent in the payload — they are always derived from the authenticated Altinn user.

### `license.role`

| Value | Behavior |
|---|---|
| `licensor` | The submitter grants the license. Supply `licensees[]`. |
| `licensee` | The logged-in user is inserted as the **first** row of licensees. Any `licensees[]` entries follow after it. |
| `agent` | Agent details are derived from the logged-in user. `representsInLicenseCase` is **required** (`licensor` and/or `licensee`). |

### `lien.role`

| Value | Behavior |
|---|---|
| `lienee` | The submitter owns the pledged right. `lienHolder` is **required**. |
| `lienholder` | The submitter holds the lien; `lienHolder` is derived from the logged-in user and any supplied `lienHolder` is **ignored**. |
| `agent` | Agent details are derived from the logged-in user. `lienHolder` and `representsInLienCase` (`lienee` and/or `lienholder`) are **required**. |

## Fields Validated Against the IP Case

These fields are checkbox selections in the form, and only values registered on the referenced IP case are selectable. The app resolves them against the case registry and **silently drops values that match nothing**:

| Field | Description |
|---|---|
| `license.licensor` | The licensor(s) |
| `lien.lienees` | The party/parties pledging the right |
| `nameAddress.changeNameAddress` | Existing applicants/owners the change applies to |

All three are comma-separated. You can send either the full registry label (`"Hydro Extruded Solutions AS; Postboks 980 Skøyen; 0240; OSLO; NO"`) or just the name part (`"Hydro Extruded Solutions AS"`) — both match.

## Participants

A participant is either a person or an organization, never both:

| Type | Required fields |
|---|---|
| `person` | `firstName` **and** `lastName` |
| `organization` | `companyName` and/or `companyNumber` (9 digits) |

`participantType` is optional — if omitted it is inferred from which fields are present. Mixing person and organization fields in the same participant is rejected.

## Country Codes

Use the options endpoint to look up valid country codes:

`GET /pat/endringsskjema/api/options/countries?language=nb`

- Use the returned `value` in the prefill data model, not the `label`

## Attachments

The app supports file attachments uploaded as part of the multipart/form-data request. To find the available attachment types, allowed MIME types, size limits, and max file counts, consult the application metadata endpoint:

See the [main README](../../README.md#creating-instances-with-prefill-data) for a complete multipart request example.
