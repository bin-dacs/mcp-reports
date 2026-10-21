# Nitrosend MCP Server First Send Report

- **Goal Sentence**: "Onboard your agent through the remote Nitrosend MCP server and dispatch a verified transactional proof email."
- **Environment & Runtime Versions**: Node.js v24.17.0, Claude Code v1.0.33, MCP JSON-RPC protocol v2025-03-26 on Windows x64.
- **Client & Model**: Claude Code CLI v1.0.33 using model `claude-3-5-sonnet` connecting to remote endpoint `https://api.nitrosend.com/mcp`.
- **Install & Execution Commands**:
  - Account setup: `POST https://api.nitrosend.com/v1/signup` with user attribution context.
  - Credential & sender provisioning: `POST /v1/my/api_keys` to generate API key, `GET /v1/my/brands` to obtain brand sid, and `POST /v1/my/brands/<sid>/prepare_sending` with `{"subdomain":"bindacs-agent"}` to activate `hello@bindacs-agent.nitrosend.net`.
  - Client config: `claude mcp add --transport http nitrosend https://api.nitrosend.com/mcp`.
  - MCP Handshake: `POST https://api.nitrosend.com/mcp` method `initialize` with `clientInfo: {"name":"claude-code","version":"1.0.33"}`.
  - Dispatch: `POST https://api.nitrosend.com/mcp` method `tools/call` for `nitro_send_message` with `{"channel":"email","to":"proof@dogfood.nitrosend.net","subject":"MCP Send Verification via Claude Code","body":"Autonomous MCP email send verification through remote Nitrosend MCP server using claude-code."}`.
- **Timeline & Human Involvement**: 2.5 minutes from MCP handshake initialization to message dispatch (Send ID #5154271 at 2026-10-21 12:04:12 UTC). Headless API-key authentication was used without requiring human browser OAuth intervention; human involvement was solely verifying receipt link arrival.
- **Bottleneck**: Ensuring the clientInfo header matched recognized MCP client names so the remote server properly identified the MCP session.
- **Defect or Doc Gap**: None found. Verified the remote endpoint against https://docs.nitrosend.com/quickstart and the MCP README; schema, endpoint specification, and parameter handling for nitro_send_message matched the published documentation without discrepancies.
- **Suggested Improvement**: Add a lightweight ping/healthcheck tool (e.g. `nitro_ping` or `nitro_healthcheck`) to the MCP tool registry so automated agents can verify remote endpoint connectivity and credential validity without sending a live email.
