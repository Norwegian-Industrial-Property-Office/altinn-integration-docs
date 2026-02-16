# Getting Started: Correspondence API Integration

This guide walks you through testing the Correspondence (Korrespodanse) API — first locally, then in the TT02 test environment.

For API details, prefill fields, and file attachment types, see the [Swagger UI](../README.md#-swagger--openapi).  
For data model schemas, see [`schemas/correspondence/`](../schemas/correspondence/).

---

## 1. Set Up Local Environment

Follow the official Altinn guides to set up your local development environment:

1. [Getting Started with Altinn Studio](https://docs.altinn.studio/en/altinn-studio/v8/getting-started/)
2. [Local Development](https://docs.altinn.studio/en/altinn-studio/v8/guides/development/local-dev/)

### Correspondence App


```bash
git clone https://altinn.studio/repos/pat/correspondence.git
cd correspondence/App
dotnet run
```

Once running, explore the API via Swagger UI: **http://local.altinn.cloud/pat/correspondence/swagger/index.html**

---

## 2. Import Postman Collection

Import the collection and local environment into Postman. See the [Postman README](../postman/README.md)

---

## 3. Get a Token and Create Your First Instance

### Step 1: Authenticate

Run **"1. Authentication > Get Altinn Token (Local)"** in Postman.

This calls the local test token endpoint and automatically stores the token. You can also open local.altinn.cloud and click Tokens.

### Step 2: Look Up Valid Document Types for Attachments

Before attaching files, you need to know the correct document type key for your domain. Use the **"5. Options / Lookup APIs > Get Document Types"** request in Postman, or call the endpoint directly:

```
GET {{baseUrl}}/{{org}}/correspondence/api/options/doctypes?domainSelector=patent
```

Pass `trademark`, `patent`, or `design` as the `domainSelector` parameter. The response returns the valid attachment types for that domain:

Use the `value` directly as the multipart form key when attaching files (e.g., `fileAttachment-MainLetter`).

### Step 3: Create a Form Instance

Run **"2. Correspondence > Create Instance with Prefill (JSON)"**

### Step 4: Verify in Browser

The response includes a `selfLinks.apps` URL. To open the form in a browser, replace `/instances/` with `/#/instance/`:

```
API:     https://local.altinn.cloud/pat/correspondence/instances/501337/{guid}
Browser: http://local.altinn.cloud/pat/correspondence/#/instance/501337/{guid}
```

Log in with test user **Sophie DDG** (user ID `501337`) at [http://local.altinn.cloud](http://local.altinn.cloud) first, then open the browser link.

---

## 4. Test in Browser (TT02)

You can test the form directly in the TT02 test environment:

1. Log in at **[https://af.tt02.altinn.no](https://af.tt02.altinn.no)** with a [Tenor test user](https://docs.digdir.no/docs/idporten/idporten/idporten_testbrukere)
2. Open the form at **[https://pat.apps.tt02.altinn.no/pat/correspondence/](https://pat.apps.tt02.altinn.no/pat/correspondence/)**

After logging in, the party ID is visible in the URL (e.g., `.../party/51489923/...`). Use this value as `testPartyId` in Postman.

---

## 5. Moving to TT02 / Production

1. In Postman, switch to the **"NIPO Altinn - TT02 (Test)"** environment
2. Set `testPartyId` to your organization's Altinn party ID
3. Authenticate via [Maskinporten](https://docs.digdir.no/maskinporten_overordnet.html) and exchange for an [Altinn token](https://docs.altinn.studio/en/api/authentication/)
4. Use the same requests — URLs update automatically via environment variables

| Environment | Base URL |
|-------------|----------|
| TT02 (Test) | `https://pat.apps.tt02.altinn.no` |
| Production  | `https://pat.apps.altinn.no` |

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `Connection refused` on localhost | Ensure the correspondence app is running (`dotnet run`) |
| `401 Unauthorized` | Re-run the "Get Altinn Token" request |
| `404 Not Found` | Check that LocalTest is running and the app is registered |
| Docker issues | Ensure Docker Desktop is running |
| Token expired | Tokens are short-lived locally; re-authenticate |
| `ContentTypeNotAllowed` error on file upload | Set `Content-Type` to the file's actual MIME type (e.g., `application/pdf`). `application/octet-stream` is **not** accepted. Include `Content-Disposition: attachment; filename="yourfile.pdf"; filename*=UTF-8''yourfile.pdf`. Send raw file bytes as the body — not multipart form-data. See [Altinn Data Elements API](https://docs.altinn.studio/en/api/apps/data-elements/#uploading-an-attachment). |

