# Altinn Integration Docs

This documentation is for integrators who want to programmatically create forms in the Norwegian Industrial Property Office (Patentstyret) Altinn 3 system.

These forms run on the **Altinn 3 Apps platform**. This repository documents the **Patentstyret-specific prefill data models** and form-specific details. For general platform concepts such as authentication, instance lifecycle, and the complete API reference, see the [Altinn 3 documentation](#altinn-3-platform).

## Altinn 3 Platform

These forms are standard Altinn 3 applications. For general integration patterns, API details, and authentication, refer to the official Altinn documentation:

- [End User Systems Guide](https://docs.altinn.studio/en/api/guides/endusersystems/submitdata/) - Overview of the integration flow from authentication to instantiation
- [Authentication](https://docs.altinn.studio/en/api/authentication/) - ID-porten and Maskinporten setup
- [Instances API](https://docs.altinn.studio/en/api/apps/instances/) - Creating and managing instances
- [Data Elements API](https://docs.altinn.studio/en/api/apps/data-elements/) - Uploading and managing attachments
- [Validation API](https://docs.altinn.studio/en/api/apps/validation/) - Validating data before submission

**Authentication flow:** Obtain an ID-porten or Maskinporten access token, then [exchange it for an Altinn token](https://docs.altinn.studio/en/authorization/getting-started/authentication/id-porten/). The Altinn token is used in the `Authorization: Bearer` header for all API calls.

## Environments

| Environment | Base URL                          |
|-------------|-----------------------------------|
| Test (TT02) | `https://pat.apps.tt02.altinn.no` |
| Production  | `https://pat.apps.altinn.no`      |

## Available Forms

Each form has a unique prefill data model used when creating instances. The prefill data is automatically mapped to the actual form fields.

| Form                  | `{{app}}`         | Swagger                                                                                                                                                                            | Prefill Schema Field Name           | Prefill Schema                                                                                                               | Details                       |
| --------------------- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------|
| Correspondence        | `correspondence`  | [TT02](https://pat.apps.tt02.altinn.no/pat/correspondence/swagger/index.html) / [Prod](https://pat.apps.altinn.no/pat/correspondence/swagger/index.html)                         | `correspondenceData-prefill` | [JSON](docs/correspondence/CorrespondenceDataPrefill.schema.json) ([XSD](docs/correspondence/CorrespondenceDataPrefill.xsd)) | [Docs](docs/correspondence/)  |
| Patent                | `patent`          | [TT02](https://pat.apps.tt02.altinn.no/pat/patent/swagger/index.html) / [Prod](https://pat.apps.altinn.no/pat/patent/swagger/index.html)                                       | `patentData-prefill`         | Coming soon                                                                                                                  | [Docs](docs/patent/)          |
| Design                | `design`          | [TT02](https://pat.apps.tt02.altinn.no/pat/design/swagger/index.html) / [Prod](https://pat.apps.altinn.no/pat/design/swagger/index.html)                                       | `designData-prefill`         | Coming soon                                                                                                                  | [Docs](docs/design/)          |
| Trademark             | `varemerke`       | [TT02](https://pat.apps.tt02.altinn.no/pat/varemerke/swagger/index.html) / [Prod](https://pat.apps.altinn.no/pat/varemerke/swagger/index.html)                                 | `varemerkeData-prefill`      | [JSON](docs/varemerke/VaremerkeDataPrefill.schema.json) ([XSD](docs/varemerke/VaremerkeDataPrefill.xsd))                   | [Docs](docs/varemerke/README.md)       |

> **Note:** the **JSON Schema is the authoritative source**. The XSD is provided for documentation and XML-oriented tooling but does not express all conditional validation rules (e.g., role-dependent required fields). Always validate against the JSON Schema.

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

**What's standard Altinn:** The multipart structure, the `instance` part with owner information, and the general HTTP pattern are standard Altinn 3 instantiation. You can also provide identity numbers instead of `partyId` (see [Altinn docs](https://docs.altinn.studio/en/api/apps/instances/#create-instance)). For self-identified users without a Norwegian national ID, use `{"instanceOwner":{"username":"user@example.no"}}`

**What's Patentstyret-specific:**
- The prefill field name (`{{app}}Data-prefill`) identifies which form's data model to prefill
- The prefill JSON structure follows the form's schema (see Available Forms table above)
- Attachment field names (e.g., `fileAttachment-MainLetter`) correspond to data types defined in each form's application metadata, they are found on the swagger page


## After Instance Creation

Creating an instance initializes the form data with your prefill values. You can then:

 ### Validate
 Validate the instance using the [Altinn Validation API](https://docs.altinn.studio/en/api/apps/validation/) to check that all required data is present and valid

### Open the form in Browser

Open the form in the browser to complete and submit it. The user must open the form in a browser to review, complete, and submit it. After a successful instance creation you'll get the instance JSON back.\n
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



