# Motion MCP

Connects [Motion](https://www.usemotion.com) to Claude Code through the Model Context Protocol, so an agent working in this repo can read and act on the studio's tasks, projects, and calendar without leaving the editor.

Motion sits alongside n8n in the automation layer: n8n runs unattended pipelines, Motion holds the scheduling layer a human still steers.

## Configuration

Registered project-wide in [`.mcp.json`](../.mcp.json) at the repo root:

```json
{
  "mcpServers": {
    "motion": {
      "type": "http",
      "url": "https://mcp.motion.so/mcp"
    }
  }
}
```

`type: "http"` means this is a **remote** server — Motion hosts it, nothing runs locally, and there is no package to install. The only thing the repo stores is the URL.

## Connecting

Because the config is project-scoped, it applies to anyone who opens this repository:

1. Open the repo in Claude Code. On first launch it will prompt to approve the `motion` server. Approve it once per machine.
2. Run `/mcp` and select **motion**.
3. Choose **Authenticate**. A browser window opens for Motion's OAuth flow — sign in and grant access.
4. Return to Claude Code. `/mcp` should now show `motion` as **connected**.

Authentication is **per user, not per repo**. Each person authenticates against their own Motion account, and the token lives in their local Claude Code credential store. No key, token, or account identifier belongs in this repository — if you find yourself wanting to commit one, something has gone wrong.

To disconnect, run `/mcp`, select **motion**, and choose **Clear authentication**.

## Verifying

`/mcp` → **motion** → **View tools** lists exactly what the server exposes to this account. That listing is the source of truth for what Motion can actually do here.

> **Not verified from this repo.** The sandbox that added this config could not reach `mcp.motion.so` — outbound requests to it were refused by egress policy (`CONNECT mcp.motion.so:443` → 403). The URL and transport are recorded as supplied; the handshake, the OAuth flow, and the tool list have not been exercised. Treat the first local `/mcp` run as the real test, and correct this page from what **View tools** reports.

Deliberately, this page does not list tool names. Guessing them would produce a doc that reads authoritative and is wrong, and MCP servers change their surface between releases anyway. Read the live listing.

## Where it fits

The content lines in `/content` — Eclipse, Luna, Tarot, Numerología — each run on their own cadence, and the work of turning a template in `/templates` into a scheduled post is mostly coordination. Once connected, that coordination becomes addressable from inside the repo: an agent that just generated a carousel can also file the task that ships it, against the same context that produced the asset.

The useful boundary is roughly:

| Layer | Owns |
| --- | --- |
| `/automation/n8n` | Recurring, unattended pipelines — no human in the loop |
| Motion MCP | Scheduling and task state a person reviews and reprioritizes |
| Claude Code | The bridge — reads repo context, acts on both |

## Troubleshooting

**Server does not appear in `/mcp`** — Claude Code reads `.mcp.json` at startup. Restart the session. If it still does not appear, confirm you are running from the repo root and that `.mcp.json` parses as valid JSON.

**Stuck at "needs authentication"** — the OAuth callback needs a browser on the same machine as Claude Code. Over SSH or in a remote container that redirect has nowhere to land; connect from a local session instead.

**Connects, but no tools** — the account authenticated may lack access to the workspace you expect. Check which Motion account the browser flow actually used; a personal account and a studio account look identical at the prompt.

**Blocked in a sandboxed or CI environment** — remote MCP servers need outbound HTTPS to their host. In a network-restricted session, `mcp.motion.so` must be allowed by policy, and no client-side change substitutes for that. This is the failure the note above describes.

## Notes

- Being in `.mcp.json` means every collaborator gets the approval prompt. That is the point of project scope, but it does mean adding a server here is a decision made on other people's behalf — worth a mention in review rather than a silent commit.
- A remote MCP server sees whatever the connected account can see. Scope the Motion account accordingly if the studio workspace holds client material.
- The server is Motion's, not ours. Its available tools, rate limits, and auth behavior can change without a commit to this repo, so this page can drift without anything here changing. Re-check **View tools** when something stops behaving.
