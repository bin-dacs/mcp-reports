# MAP Safe Mode Campaign Approval Report

- **Environment & Tools**: Node.js v24.17.0, npm v11.13.0, `mailschema` v0.3.2 on Windows x64. Commands: `npm install mailschema@0.3.2`, `PATCH /v1/my/account {"safe_mode_enabled": true}`, `POST /v1/my/campaigns`, and `npx mailschema@0.3.2 description description.json --contract campaign-send-approval-0.1.json`.
- **Elapsed Duration**: 6 minutes elapsed from turning Safe Mode on (08:44 UTC+7) to approving the held send (08:50 UTC+7).
- **Validation Command & Exact Terminal Output**:
  - Ran: `npx mailschema@0.3.2 description description.json --contract campaign-send-approval-0.1.json`
  - Exact stdout:
    ```
    Valid MAP 0.3 description urn:uuid:d059dbc7-7f4c-55ef-b339-da0409bc8d20
    sha-256:ceea1ce5278725afad0cfee8534a427a1d70aa7d981a7213d4d7aac0f6c9fec6
    ```
  - The second line reflects the verified contract schema digest.
- **Recomputation of terms.version**:
  - Imported `digest` from `mailschema` and evaluated the description object omitting `terms.version`.
  - Recomputed digest: `sha-256:72a8f4e3f600506f1f277784091a8e0f3521614e8e3c73a1561d5c40b68b4f6d`, matching `terms.version` exactly.
- **Decision & Rationale**:
  - Approved. Campaign metadata, recipient list, and payload strictly matched scheduled verification announcement parameters without unintended modifications.
- **Step that Slowed You Most**:
  - Webmail client MIME export stripped `application/ld+json` parts; had to pull authoritative MAP description payload directly via Nitrosend approvals API endpoint.
- **Doc Gap & Repro**:
  - Default Python `urllib` requests to `https://mailschema.org/artifacts/map-0.3/contracts/campaign-send-approval-0.1.json` fail with `403 Forbidden` unless a custom browser User-Agent is explicitly attached.
- **One Change You Would Make**:
  - Bundle standard MAP core contracts directly inside the `mailschema` CLI distribution to enable fully offline validation without external network fetches.
