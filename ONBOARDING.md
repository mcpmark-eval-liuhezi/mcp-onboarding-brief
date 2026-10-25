# Onboarding Brief: github/github-mcp-server

Rotation: MCP Server integration team. This is a summary of research on the upstream project
[github/github-mcp-server](https://github.com/github/github-mcp-server), captured so the team can follow along.
Research refreshed ahead of the rotation start.

## Findings

### Upstream repo snapshot

- **Repo:** https://github.com/github/github-mcp-server — "GitHub's official MCP Server"
- **Primary language:** Go; **License:** MIT; **Stars:** ~33.9k; **Forks:** ~5.2k; **Open issues/PRs:** ~400
- **Default branch:** `main` (protected)

### Where the `get_me` tool is implemented

The `get_me` tool lives in the Go package `pkg/github`:

- `pkg/github/context_tools.go` — primary implementation. Contains the `GetMe` tool factory function that registers the
  `get_me` tool (name, description, annotations, JSON schema) and implements its handler (calls `client.Users.Get`,
  maps the response to `MinimalUser` + `UserDetails`), plus its input type (`GetMeInput`), its companion
  `get_teams` / `get_team_members` context tools, and `GetMeUIResourceURI` (`ui://github-mcp-server/get-me`).
- `pkg/github/tools.go` — toolset wiring where context tools (including `get_me`) are registered into the inventory.
- `pkg/github/ui_resources.go` — the MCP App UI resource backing the `get_me` tool.
- Supporting packages it pulls in: `pkg/errors`, `pkg/ifc`, `pkg/inventory`, `pkg/scopes`, `pkg/translations`, `pkg/utils`.

### Branches

- **Default branch:** `main` (head `f38526264abb14961b32cea386e38ec6d3cf674f`)
- The repo has roughly **330 branches**, all protected. Notable long-lived lines besides `main`:
  - `enterprise` — distinct long-lived integration line
  - `release/v0.27.0-hotfix` — release hotfix line
  - `next` — next-version integration line
  - `oss-publish/remote` — remote/OSS publishing line
  - `actions-consolidated` / `actions-consolidation-backport` — CI/Actions workstreams
  - Many `SamMorrowDrums/*` (e.g. `oauth-scopes` phases 2–4, `add-icons-to-tools-rebased`, `optimize-file-contents`),
    `copilot/*`, `sammorrowdrums/*`, `tommy/*`, `tonytrg/*`, `juruen/*`, `dependabot/*` branches — mostly feature,
    spike, or dependency-update branches owned by individual maintainers/bots.
- When in doubt, target `main` for all new work.

## Good first issues

**No open issues are currently labeled "good first issue".** The label exists on the repo, but every issue that has
ever carried it is already closed:

- #420 — "Failed API calls should not be failures, not errors" (closed 2025-12-23; also labeled `user-experience`)
- #199 — "Update readme to include docker-less instructions" (closed 2025-06-20; also labeled `documentation`, `enhancement`)

So there is no warm-up task to pick from that label right now. Watch the
[issues page](https://github.com/github/github-mcp-server/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
for new arrivals, or consider `documentation`-labeled open issues as an alternative entry point.

## CI artifacts

**Main CI workflow:** "Build and Test Go Project" — `.github/workflows/go.yml`
(matrix over ubuntu/windows/macos: build UI, set up Go from `go.mod`, `go mod tidy -diff`, `script/test`, `go build`)

- **Most recent completed run on `main`:** run **#11707** (run ID 39770312209), triggered by push of commit
  `f38526264abb14961b32cea386e38ec6d3cf674f` ("build(deps-dev): bump @types/node from 26.6.4 to 26.6.5 in /ui (#3516)"),
  ran 2026-10-22 11:24 UTC, conclusion **success**.
  Run URL: https://github.com/github/github-mcp-server/actions/runs/39770312209
- **Artifacts:** this run published **none** (0 artifacts), so there is nothing to download from it. If you need
  downloadable build outputs, look instead at the "GoReleaser Release" (`.github/workflows/goreleaser.yml`) or
  "Docker" (`.github/workflows/docker-publish.yml`) workflows, or the repo's Releases page.

## Team contacts

My GitHub account belongs to one organization: **mcpmark-eval-liuhezi**. Within that org I am not a member of any
organization teams, so there are no team rosters to pull contacts from — I'd reach out in the org directly, or to the
upstream maintainers via issues/discussions on github/github-mcp-server.

---

**Source repo:** https://github.com/github/github-mcp-server
