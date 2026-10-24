# Onboarding Brief: github/github-mcp-server

Rotation: MCP Server integration team. This is a summary of research on the upstream project
[github/github-mcp-server](https://github.com/github/github-mcp-server), captured so the team can follow along.

## Findings

### Upstream repo snapshot

- **Repo:** https://github.com/github/github-mcp-server — "GitHub's official MCP Server"
- **Primary language:** Go; **License:** MIT; **Stars:** ~33.8k; **Forks:** ~5.3k; **Open issues/PRs:** ~380
- **Default branch:** `main`

### Where the `get_me` tool is implemented

The `get_me` tool lives in the Go package `pkg/github`:

- `pkg/github/context_tools.go` — primary implementation. Contains the `GetMe` tool factory function that registers the
  `get_me` tool (name, description, annotations, JSON schema), plus its input type (`GetMeInput`), its companion
  `get_teams` / `get_team_members` context tools, and `GetMeUIResourceURI` (`ui://github-mcp-server/get-me`).
- `pkg/github/tools.go` — context/toolset wiring where the context tools (including `get_me`) are registered.
- `pkg/github/ui_resources.go` — MCP App UI resource backing the `get_me` tool.
- Supporting/related: `pkg/github/context_tools_test.go` (tests), `pkg/errors`, `pkg/ifc`, `pkg/inventory`, `pkg/scopes`,
  `pkg/translations`, and `pkg/utils` packages it depends on.

### Branches

- **Default branch:** `main`
- The repo has roughly **300 branches**, all protected. Besides `main`, the notable long-lived ones are:
  - `enterprise` — a distinct long-lived integration line
  - `release/v0.27.0-hotfix` — release hotfix line
  - `actions-consolidated` / `actions-consolidation-backport` — CI/Actions workstreams
  - Numerous `SamMorrowDrums/*` (e.g. `oauth-scopes`, phases 2–4, `add-icons-to-tools-rebased`, `optimize-file-contents`),
    `copilot/*`, `sammorrowdrums/*`, `tommy/*`, `tonytrg/*`, and `dependabot/*` branches — mostly feature/spike/dependency
    branches owned by individual maintainers.
- When in doubt, target `main` for all new work.

## Good first issues

**No open issues are currently labeled "good first issue".** As of this research the label exists on the repo, but every
issue that has carried it is already closed:

- #420 — "Failed API calls should not be failures, not errors" (closed Dec 2025; also labeled `user-experience`)
- #199 — "Update readme to include docker-less instructions" (closed Jun 2025; also labeled `documentation`, `enhancement`)

So there's no warm-up task to pick from that label right now — watch the
[issues page](https://github.com/github/github-mcp-server/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
for new ones, or consider `documentation`-labeled issues as an alternative entry point.

## CI artifacts

**Main CI workflow:** "Build and Test Go Project" — `.github/workflows/go.yml`

- **Most recent completed run on `main`:** run #11664 (run ID 38808623916), triggered by push of commit
  `e3809fe` ("Add cursor pagination to list_label (#3505)"), ran 2026-10-15 21:16 UTC, conclusion **success**.
  Run URL: https://github.com/github/github-mcp-server/actions/runs/38808623916
- **Artifacts:** this run published **none** (0 artifacts), so there is nothing to download from it. If you need
  downloadable build outputs, look instead at the "GoReleaser Release" (`.github/workflows/goreleaser.yml`) or
  "Docker" (`.github/workflows/docker-publish.yml`) workflows / the repo's Releases page.

## Team contacts

My GitHub account belongs to one organization: **mcpmark-eval-liuhezi**. Within that org I'm not a member of any
organization teams, so there are no team rosters to pull contacts from — I'd reach out in the org directly, or to the
upstream maintainers via issues/discussions on github/github-mcp-server.
