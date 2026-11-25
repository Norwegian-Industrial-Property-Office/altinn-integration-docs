# Altinn Integration Docs

This documentation is for integrators who want to programmatically create forms in the Norwegian Industrial Property Office (Patentstyret) Altinn 3 system.

## Table of Contents

- [Altinn 3 Platform](#altinn-3-platform)
- [Environments](#environments)
  - [Testing in TT02](#testing-in-tt02)
- [Available Forms](#available-forms)
  - [Downloading the Prefill Schema from the Form](#downloading-the-prefill-schema-from-the-form)
- [Creating Instances with Prefill Data](#creating-instances-with-prefill-data)
  - [Example: Creating a Correspondence Instance](#example-creating-a-correspondence-instance)
  - [Self-Identified (Email) Users](#self-identified-email-users)
  - [Attachments](#attachments)
  - [Responses and Errors](#responses-and-errors)
- [After Instance Creation](#after-instance-creation)
  - [Validate](#validate)
  - [Open the form in Browser](#open-the-form-in-browser)
- [Further Reading](#further-reading)

These forms run on the **Altinn 3 Apps platform**. This repository documents the **Patentstyret-specific prefill data models** and form-specific details. For general platform concepts such as authentication, instance lifecycle, and the complete API reference, see the [Altinn 3 documentation](#altinn-3-platform).

## Altinn 3 Platform

These forms are standard Altinn 3 applications. For general integration patterns, API details, and authentication, refer to the official Altinn documentation:

- [End User Systems Guide](https://docs.altinn.studio/en/api/guides/endusersystems/submitdata/) - Overview of the integration flow from authentication to instantiation
- [Authentication](https://docs.altinn.studio/en/api/authentication/) - ID-porten and Maskinporten setup
- [Instances API](https://docs.altinn.studio/en/api/apps/instances/) - Creating and managing instances
- [Data Elements API](https://docs.altinn.studio/en/api/apps/data-elements/) - Uploading and managing attachments
- [Validation API](https://docs.altinn.studio/en/api/apps/validation/) - Validating data before submission

**Authentication flow:** Obtain an ID-porten or Maskinporten access token, then [exchange it for an Altinn token](https://docs.altinn.studio/en/authorization/getting-started/authentication/id-porten/). The Altinn token is used in the `Authorization: Bearer` header for all API calls.

## Available Forms

Each form has a unique prefill data model used when creating instances. The prefill data is automatically mapped to the actual form fields.

| Form                  | `{{app}}`         | Swagger                                                                                                                                                                            | Prefill Schema Field Name           | Prefill Schema                                                                                                            | Details                          |
| --------------------- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------| ---------------------------------|
| Correspondence        | `correspondence`  | [TT02](https://pat.apps.tt02.altinn.no/pat/correspondence/swagger/index.html) / [Prod](https://pat.apps.altinn.no/pat/correspondence/swagger/index.html)                           | `correspondenceData-prefill` | [JSON](docs/correspondence/CorrespondenceDataPrefill.schema.json) ([XSD](docs/correspondence/CorrespondenceDataPrefill.xsd))     | [Docs](docs/correspondence/)     |
| Patent                | `patent`          | [TT02](https://pat.apps.tt02.altinn.no/pat/patent/swagger/index.html) / [Prod](https://pat.apps.altinn.no/pat/patent/swagger/index.html)                                           | `patentData-prefill`         | [JSON](docs/patent/PatentDataPrefill.schema.json) ([XSD](docs/patent/PatentDataPrefill.xsd))                                     | [Docs](docs/patent/README.md)    |
| Design                | `design`          | [TT02](https://pat.apps.tt02.altinn.no/pat/design/swagger/index.html) / [Prod](https://pat.apps.altinn.no/pat/design/swagger/index.html)                                           | `designData-prefill`         | [JSON](docs/design/DesignDataPrefill.schema.json) ([XSD](docs/design/DesignDataPrefill.xsd))                                     | [Docs](docs/design/)             |
| Trademark             | `varemerke`       | [TT02](https://pat.apps.tt02.altinn.no/pat/varemerke/swagger/index.html) / [Prod](https://pat.apps.altinn.no/pat/varemerke/swagger/index.html)                                     | `varemerkeData-prefill`      | From the form: JSON [TT02](https://pat.apps.tt02.altinn.no/pat/varemerke/api/jsonschema/varemerkeData-prefill) / [Prod](https://pat.apps.altinn.no/pat/varemerke/api/jsonschema/varemerkeData-prefill), XSD [TT02](https://pat.apps.tt02.altinn.no/pat/varemerke/api/xsdschema/varemerkeData-prefill). Copy: [JSON](docs/varemerke/VaremerkeDataPrefill.schema.json) ([XSD](docs/varemerke/VaremerkeDataPrefill.xsd)) | [Docs](docs/varemerke/README.md) |
| Ansvarsmerke          | `ansvarsmerke`    | [TT02](https://pat.apps.tt02.altinn.no/pat/ansvarsmerke/swagger/index.html) / [Prod](https://pat.apps.altinn.no/pat/ansvarsmerke/swagger/index.html)                               | `ansvarsmerkeData-prefill`   | [JSON](docs/ansvarsmerke/AnsvarsmerkeDataPrefill.schema.json) ([XSD](docs/ansvarsmerke/AnsvarsmerkeDataPrefill.xsd))             | [Docs](docs/ansvarsmerke/README.md) |
| Endringsskjema        | `endringsskjema`  | [TT02](https://pat.apps.tt02.altinn.no/pat/endringsskjema/swagger/index.html) / [Prod](https://pat.apps.altinn.no/pat/endringsskjema/swagger/index.html)                           | `endringsskjemaData-prefill` | [JSON](docs/endringsskjema/EndringsskjemaDataPrefill.schema.json) ([XSD](docs/endringsskjema/EndringsskjemaDataPrefill.xsd))     | [Docs](docs/endringsskjema/README.md) |
| EP-Validering         | `ep-validering`   | [TT02](https://pat.apps.tt02.altinn.no/pat/ep-validering/swagger/index.html) / [Prod](https://pat.apps.altinn.no/pat/ep-validering/swagger/index.html)                             | `epValideringData-prefill`   | [JSON](docs/ep-validation/EpValideringDataPrefill.schema.json) ([XSD](docs/ep-validation/EpValideringDataPrefill.xsd))         | [Docs](docs/ep-validation/README.md) |
| Klagesaker            | `klagesaker-op`   | [TT02](https://pat.apps.tt02.altinn.no/pat/klagesaker-op/swagger/index.html) / [Prod](https://pat.apps.altinn.no/pat/klagesaker-op/swagger/index.html)                             | `klagesaker-opData-prefill`  | [JSON](docs/klagesaker-op/OPDataPrefill.schema.json) ([XSD](docs/klagesaker-op/OPDataPrefill.xsd))                               | [Docs](docs/klagesaker-op/README.md) |

> **Note:** the **JSON Schema is the authoritative source**. The XSD is provided for documentation and XML-oriented tooling but does not express all conditional validation rules (e.g., role-dependent required fields). Always validate against the JSON Schema.

### Downloading the Prefill Schema from the Form

Forms that list "From the form" in the table above serve their prefill schema themselves, so you always get the version that is deployed in that environment. No token is needed:

```
GET {{basePath}}/pat/{{app}}/api/jsonschema/{{prefillFieldName}}
GET {{basePath}}/pat/{{app}}/api/xsdschema/{{prefillFieldName}}
```

- `{{prefillFieldName}}` is the "Prefill Schema Field Name" from the table, the same name you use for the multipart part. Example: `GET https://pat.apps.altinn.no/pat/varemerke/api/jsonschema/varemerkeData-prefill`.
- In Swagger, open the definition "End user app API for pat/{{app}}". The section `FormData {{prefillFieldName}}` links to the JSON Schema. The same endpoints are also listed as `GET /{org}/{app}/api/jsonschema/{id}` and `GET /{org}/{app}/api/xsdschema/{id}` under `Resource` in the "Altinn App API" definition.
- The `Schemas` list in Swagger also shows a model with the same fields, generated from the code. It does not show the required fields or the allowed text values, so use the JSON Schema.
- The JSON Schema is returned with `Content-Type: text/plain`. Save the body as a `.schema.json` file.
- Production does not serve the XSD yet. Use the copy in this repository until it does.

For the other forms, use the copies in this repository. They will move to "From the form" over time.

## Environments

| Environment | Base URL                          |
|-------------|-----------------------------------|
| Test (TT02) | `https://pat.apps.tt02.altinn.no` |
| Production  | `https://pat.apps.altinn.no`      |

### Testing in TT02

Do **not** use your real personal number or organization number in TT02. TT02 is connected to the [Tenor test population](https://www.skatteetaten.no/skjema/testdata/) — a shared synthetic dataset that simulates the Norwegian population.

To get a test user:

1. Open a form's login page in TT02.
2. Choose **TestID** as the login method.
3. Click **Find random user** (for a person) or **Find random CEO** (for an organization).
4. Copy the displayed national identity number, then click **Authenticate**.

**`partyId` in TT02 vs. Production**

The `partyId` you receive in the Altinn token is the party associated with the authenticated user. In TT02, when you act on behalf of a Tenor user or company, the relevant `partyId` is the one for that test party — not necessarily the one in your personal token.

After opening the form in the browser, the `partyId` appears in the URL just before the instance GUID:

```
https://pat.apps.tt02.altinn.no/pat/patent#/instance/51841311/2320f06c-47a1-4b52-83b4-5c4dc13c65b7/Task_FillOutForm/01_ContactInfo
                                                       ^^^^^^^^ ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                                                       partyId  instanceGuid
```

Forms on Altinn v9 have the same path without `#`: `https://pat.apps.tt02.altinn.no/pat/patent/instance/51841311/...`.

Example test credentials:

| Field   | Value                                  |
|---------|----------------------------------------|
| TestID  | `04857194662`                          |
| partyId | `51841311`                             |

Use that `partyId` when creating instances or calling APIs on behalf of the test user. In **production**, use the `partyId` from your Altinn token as normal.

## Creating Instances with Prefill Data

To create an instance with prefill data, use a `multipart/form-data` request. This follows the standard Altinn 3 instantiation pattern (see [Altinn Instances API docs](https://docs.altinn.studio/en/api/apps/instances/#create-instance)).

**Template variables used in examples:**

| Variable | Description |
|----------|-------------|
| `{{basePath}}` | The environment base URL (e.g., `https://pat.apps.tt02.altinn.no`) |
| `{{app}}` | See table for form names |
| `{{instanceOwnerPartyId}}` | The party ID from the instance creation response |
| `{{instanceGuid}}` | The instance GUID from the instance creation response |
| `{{altinnToken}}` | Your Altinn Runtime Token (see Authentication section above) |


The `{{app}}Data-prefill` and `instance` parts must have `Content-Type: application/json`. They can be sent either as inline text or as a file

### Example: Creating a Correspondence Instance

The following example creates a Correspondence form instance using multipart instantiation with prefill data and attachments:

```http
POST {{basePath}}/pat/correspondence/instances
Authorization: Bearer {{altinnToken}}
Content-Type: multipart/form-data; boundary=boundary

--boundary
Content-Disposition: form-data; name="instance"
Content-Type: application/json

{"instanceOwner":{"partyId":"51109100"}}

--boundary
Content-Disposition: form-data; name="correspondenceData-prefill"
Content-Type: application/json

{
    "domain": "trademark",
    "inquiryMessage": "Status on my application",
    "contactEmail": "test@example.com",
    "ipCases": ["202200912"]
}

--boundary
Content-Disposition: form-data; name="fileAttachment-MainLetter"; filename="document.pdf"
Content-Type: application/pdf

<binary-file-content>
--boundary--
```

**What's standard Altinn:** The multipart structure, the `instance` part with owner information, and the general HTTP pattern are standard Altinn 3 instantiation. You can also provide identity numbers instead of `partyId` (see [Altinn docs](https://docs.altinn.studio/en/api/apps/instances/#create-instance)). For self-identified users without a Norwegian national ID, see [Self-Identified (Email) Users](#self-identified-email-users).

**What's Patentstyret-specific:**
- The prefill field name (`{{app}}Data-prefill`) identifies which form's data model to prefill
- The prefill JSON structure follows the form's schema (see Available Forms table above)
- Attachment field names (e.g., `fileAttachment-MainLetter`) correspond to data types defined in each form's application metadata, they are found on the swagger page

### Self-Identified (Email) Users

For users who do not have a Norwegian national identity number or organization number, use the `username` field in the `instance` part instead of `partyId`:

```json
{"instanceOwner":{"username":"test@example.no"}}
```

Since no national register data is available for these users, the Requestor fields are blank unless you provide them. To populate the Requestor with name and address data, include a submitter object in the prefill payload:

| Form | Prefill object |
|------|----------------|
| Patent (`patent`) | `submitter` |
| Correspondence (`correspondence`) | `submitter` |
| Trademark (`varemerke`) | `submitter` |
| Design (`design`) | `submitter` |
| Ansvarsmerke (`ansvarsmerke`) | `selfIdentifiedSubmitter` |
| EP-Validering (`ep-validering`) | `submitter` |

Notes:
- The submitter object is optional. Without it, the Requestor fields will be blank for self-identified users.
- Provide either a person name (`firstName` + `lastName`) or a `companyName`, plus address fields (`streetAddress`, `postalCode`, `city`, `country`). See each form's JSON Schema for the exact structure.
- `customerNumber` (where supported) is optional. If you already have a Patentstyret customer number, providing it links the submission to your existing customer record, so no lookup in our system is needed.
- For users identified by national identity number or organization number, register data takes precedence over the submitter object.
- Correspondence: the search for the user's own cases is not available for self-identified users, but cases can still be prefilled with `ipCases`. If `contactName` is omitted, the contact name is set from the submitter's `firstName` + `lastName`.

### Attachments

Attachments are uploaded as additional parts in the multipart request. You can also upload them after instance creation using the [Data Elements API](https://docs.altinn.studio/en/api/apps/data-elements/).

**Finding available attachment types:**

Each form's Swagger page lists available data types. The ones you can upload attachments to are those with the prefix `fileAttachment-` (e.g., `fileAttachment-designs`, `fileAttachment-powerOfAttorney`, `fileAttachment-MainLetter`).

To see the full list for a form, open its Swagger page (linked in the [Available Forms](#available-forms) table) and look at the data types section. It shows:

| Column | Description |
|--------|-------------|
| DataTypeId | The name to use in the multipart `name` field |
| Allowed number | Min and max number of files (e.g., `0-200`) |
| MimeTypes | Accepted content types for that attachment type |

**Multipart format for attachments:**

```
--boundary
Content-Disposition: form-data; name="{{dataTypeId}}"; filename="myfile.pdf"
Content-Type: application/pdf

<binary-file-content>
--boundary
```

**Key rules:**
- The `name` field must exactly match a `fileAttachment-*` data type ID from the Swagger page
- The `Content-Type` must be one of the allowed MIME types for that data type
- The `filename` must include a valid file extension matching the content type
- Filenames must be unique across all attachments in the instance
- You can upload multiple files to the same data type (up to the max allowed number) by repeating the part with different filenames
- Uploading to a data type not prefixed with `fileAttachment-` or exceeding the allowed count will be rejected

### Responses and Errors

| Status | Meaning | What to do |
|--------|---------|------------|
| `201 Created` | The instance was created. The body is the instance JSON. | Save `id` (`{{instanceOwnerPartyId}}/{{instanceGuid}}`). |
| `400 Bad Request` | The request or the prefill data is invalid: it does not match the schema, required fields are missing, or an attachment has the wrong data type, MIME type or count. | Fix the request and send it again. |
| `401` / `403` | The token is missing or invalid, or the party has no access to the form. | Check the token and the `instanceOwner`. |
| `500 Internal Server Error` | The instance could not be created or set up. | On Altinn v9, read `recommendedAction` (see below). Otherwise, try again later or contact Patentstyret. |

#### Instance creation on Altinn v9

Patentstyret's forms are moving to [Altinn v9](https://docs.altinn.studio/nb/altinn-studio/v9/new-in-v9/). The request format and the prefill schemas stay the same. What changes is how instance creation can fail: on v9, the app stores the instance first and then sets it up in a separate workflow engine. If the setup fails, the `500` response is [Problem Details](https://www.rfc-editor.org/rfc/rfc9457) with fields that tell you whether you can safely send the request again:

```json
{
  "title": "Instance initialization failed.",
  "status": 500,
  "detail": "Runtime created the instance, but the initial workflow was not accepted by the workflow engine. The created instance was deleted, so the client can safely retry instance creation.",
  "initializationState": "workflowNotAccepted",
  "recommendedAction": "retryInstanceCreation",
  "instanceDeleted": true,
  "instanceId": "51109100/2320f06c-47a1-4b52-83b4-5c4dc13c65b7"
}
```

| `recommendedAction` | What happened | What to do |
|---------------------|---------------|------------|
| `retryInstanceCreation` | The instance was deleted (`instanceDeleted: true`). | Send the request again. |
| `inspectInstance` | An instance may exist (`instanceId`). | Look it up with `GET {{basePath}}/pat/{{app}}/instances/{instanceId}` before you send the request again, so you don't create a duplicate. Delete it if you don't want to use it. |
| `resumeCurrentTask` | The instance exists, but the setup failed. | Do not create a new instance. Call `resumeEndpoint` (`POST .../process/resume`) later. If it keeps failing, contact Patentstyret with the `instanceId`. |

Instance creation can also take longer on v9, because the request waits for the setup to finish. If your HTTP client times out, the instance may still have been created. List the party's active instances with `GET {{basePath}}/pat/{{app}}/instances/{{instanceOwnerPartyId}}/active` before you try again.


## After Instance Creation

Creating an instance initializes the form data with your prefill values. You can then:

 ### Validate
 Validate the instance using the [Altinn Validation API](https://docs.altinn.studio/en/api/apps/validation/) to check that all required data is present and valid: `GET {{basePath}}/pat/{{app}}/instances/{{instanceOwnerPartyId}}/{{instanceGuid}}/validate`. An empty array means the instance is valid. Use this endpoint rather than `.../data/{dataGuid}/validate`, which Altinn v9 removes.

### Open the form in Browser

Open the form in the browser to complete and submit it. The user must open the form in a browser to review, complete, and submit it. After a successful instance creation you'll get the instance JSON back.
   **Use the `selfLinks.apps` URL from the response**

   In the instance JSON response, find the `selfLinks.apps` value:
   ```
   "selfLinks": {
       "apps": "{{basePath}}/pat/{{app}}/instances/{{instanceOwnerPartyId}}/{{instanceGuid}}"
   }
   ```

   Replace `instances` with `#/instance` to get the browser URL:
   ```
   {{basePath}}/pat/{{app}}/#/instance/{{instanceOwnerPartyId}}/{{instanceGuid}}
   ```

   Forms on Altinn v9 use the same path without `#`: `{{basePath}}/pat/{{app}}/instance/{{instanceOwnerPartyId}}/{{instanceGuid}}`. They redirect `#/instance` links to the new address, so the link above works on both versions.


## Further Reading

- [Altinn Studio Instance Documentation](https://docs.altinn.studio/nb/api/apps/instances/)
- [Altinn App API OpenAPI Spec](https://docs.altinn.studio/en/api/apps/spec/)

## Getting Started with Postman

A sample Postman collection is available to demonstrate a typical flow using the Correspondence schema. This collection serves as a reference and includes pre-configured requests for:

- Authentication (ID-porten and Maskinporten token exchange)
- Creating instances with prefill data
- Uploading attachments
- Validating instances

**To use the collection:**

1. Download the Correspondence Postman collection from the [postman/](postman) directory
2. Import it into Postman
3. Set up environment variables:
    - `basePath` - Your environment URL (`https://pat.apps.tt02.altinn.no` for test)
    - `altinnToken` - Your Altinn Runtime Token
    - `instanceOwnerPartyId` - Your party ID
4. Run requests in sequence to create and manage instances


