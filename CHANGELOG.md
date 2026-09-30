# Changelog

## 1.2.3 — 2026-09-30 (manifest schema fix)

Clears the ClawHub "Needs review" flag `manifest-unknown-fields` ("manifest uses unsupported top-level fields"). No behavior changes.

- **Fix**: removed `homepage`, `license` and `keywords` from `openclaw.plugin.json`. They are not part of OpenClaw's `PluginManifest` type (`docs/plugins/manifest.md`, "Top-level field reference"; "Only documented manifest fields are read by the manifest loader"). They already live in `package.json`; the manifest-only keywords (`clawhub`, `ccpa`, `lgpd`, `pipl`, `tn-ved`, `anti-hallucination`) were merged there.
- **Fix**: replaced the `setup.providers` entry (no `id`, undocumented `name`/`description`/`url`/`envVar` keys, silently dropped by the OpenClaw loader) with `uiHints` for `CLEO_LEGAL_API_KEY` (label, signup link, placeholder, `sensitive`) and `CLEO_LEGAL_MCP_URL` (advanced).
- **Chore**: version bumped to 1.2.3 across `package.json`, `openclaw.plugin.json`, `.claude-plugin/plugin.json` and the `X-Client` header in `.mcp.json`.

## 1.2.2 — 2026-05-22 (publish workflow fix)

Re-cuts 1.2.1 after the publish workflow failed on both jobs.

- **Fix**: npm scope reverted to `@alexbloch-ia/legal-data` (the previous `@alexblochia` rename did not match the npm user owning `NPM_TOKEN`, causing `404 PUT` on first publish).
- **Fix**: ClawHub auth in CI now uses `clawhub login --token "$CLAWHUB_TOKEN"` (the old `echo > ~/.clawhub/credentials` wrote to a path the CLI does not read; correct config lives at `~/.config/clawhub/config.json` and the CLI writes it itself on `login`).
- **Chore**: added `npm whoami` debug step before `npm publish` so any future scope mismatch surfaces immediately.

## 1.2.1 — 2026-05-21 (audit-driven fix release)

Addresses the ruthless audit dated 2026-05-21. No behavior changes; corrects packaging, install command, runtime compatibility, and visible URLs.

- **Fix**: install command now uses the scoped form `clawhub:@alexbloch-ia/legal-data` everywhere (README, install docs, all launch copy, post-launch checklist). The unscoped `clawhub:legal-data` returns 404 on ClawHub.
- **Fix**: `openclaw.compat.pluginApi` and `openclaw.build.openclawVersion` migrated from the legacy `"1.x"` to OpenClaw's date-based scheme (`">=2026.3.24-beta.2"`); adds `minGatewayVersion`. The plugin now installs on current OpenClaw runtimes (2026.4.x / 2026.5.x).
- **Fix**: 4 broken pricing/quota links — removed the duplicated subdomain in `legaldata-public.legaldata-public.cleolabs.co/pricing` (`commands/cleo-search.md`, `commands/cleo-setup.md`, `docs/troubleshooting.md`, `docs/pricing.md`).
- **Fix**: ClawHub listing URLs corrected from `clawhub.ai/plugins/cleo-legal` (and `/legal-data`) to `clawhub.ai/plugins/@alexbloch-ia/legal-data` (README badge, all launch copy, publish script, post-launch checklist).
- **Chore**: version aligned across `package.json`, `openclaw.plugin.json`, `.claude-plugin/plugin.json`, and the `X-Client` header in `.mcp.json` (plus the curl + headersHelper examples in `commands/cleo-setup.md` and `docs/troubleshooting.md`).
- **Docs**: README gains an explicit Compatibility section and clarifies the plugin is an MCP/commands/skills wrapper bundle (no heavy native runtime).
- **Docs**: `/cleo-classify` and `/cleo-compliance` rendering instructions now require explicit confidence tiers (exact / probable / tentative) so the user-facing answer cannot over-claim relative to model confidence.

## 1.0.0 — 2026-05-20 (initial release)

- 6 slash commands: `/cleo-setup`, `/cleo-search`, `/cleo-classify`, `/cleo-compliance`, `/cleo-doc`, `/cleo-changes`
- 5 skills: `regulatory-research`, `gdpr-data-protection`, `customs-hs-classifier`, `dual-use-export-control`, `sanctions-screening`
- MCP server bundled via `.mcp.json` (66 sources, 100+ countries, 19 tools)
- Published to ClawHub
