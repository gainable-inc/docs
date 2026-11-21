# Docs restructure plan — November 2026

Status (2026-11-21): all seven phases built locally as stacked branches, docs/naming-and-ia (PR #4), then docs/assistant, docs/build-datasets, docs/access-control, docs/apps-grid-and-mobile, docs/developer and docs/fin-refresh. None of the later branches are pushed. See "Product findings" at the end.

## Why

The docs still describe the August product. 26 of the 44 pages mention Gaia Copilot or Gaia Autopilot, and both are retired.
Since August 13, about 1,100 commits have gone into the engine (`gainable-agent-v3`), plus changes in components, Connect, the MCP server, the plugins and www.
The site is organized around product features. It should be organized around **who is reading**.

## What changed, and where I found it

| # | Change | Source | Seen in the product? |
|---|---|---|---|
| 1 | Multi-sheet / multi-source build. The front door takes several sheets (or Google Sheets / Excel Online), analyzes them together into one data model, and links across them | agent commits 11-11 to 11-16; www commits 11-19/20; www.gainable.dev | Yes. The www front door takes several sheets, and the builder has a 4-file dataset under Recent Datasets |
| 2 | The Assistant replaces Ask-AI, Cowork and the autopilots. It has chat history, artifacts with version history, diagrams, scheduled tasks, memory (admin view), record cards, @mentions in comments, and outside MCP tools | release notes 2026-11 §1; components `g-cowork-*` | Partly (release notes). Needs a walkthrough in an app |
| 2b | Live views (`g-view`) inside Assistant artifacts | agent 11-19; components 11-19 | **Not in the Nov 9 release.** Needs checking |
| 3 | App Assistant as MCP. The admin turns it on under Share → "Share this Assistant" → "Copy link to Assistant". Off by default | release notes §6; `cowork/mcp-dialog.js` | Not yet |
| 4 | ACL. Access & sign-in (Open / Invited only; externals: None / Invited externals only / Anyone can sign up; allowed domains; email verification), plus Users / Roles / Groups and row rules in plain words | release notes §7 | **Yes**, in the builder → Users panel |
| 4b | **Data manager** role (per app; can sync the app's data; granted by builders) and **two-way sync** (back to Google Sheets / Excel Online through Connect), app Data page (Sync / Log with Undo / Settings: approval, two-way, schedule) | agent 11-14 to 11-17; Connect 11-14; components `g-data-sources` | **Not in the Nov 9 release.** The Roles tab in prod shows no Data manager |
| 5 | Builder MCP for Claude, ChatGPT, Codex, Gemini and Goose. Tools: `apps_list`, `apps_select`, `import`, `build`, `chat`, `publish`, `share`, `project_setup`, `app_access`, `app_users`, `account_users`, `dataset_*` | live connector; plugins `SETUP.md` | Yes ("MCP server healthy — 10 tools" in the builder) |
| 6 | Plugin for Claude Code / Cowork / Codex v1.2.1. Consent scopes for code / datasets / users; "Author mode" = `code_context` → `code_push` | `gainable-plugins` README, SETUP, SKILL | Install steps are in the repo |
| 7 | Apps grid (`/apps`). Tabs (for example Gallery, General), app groups (folders), widgets (Feed, Files), Add → Apps / Widgets, and an edit mode. Messenger and notification bell in the header | builder UI; "Launcher" commits | **Yes**, at build.gainable.dev/apps |
| 8 | Mobile. The installed launcher (add to home screen) and push notifications | agent commits 09-18 to 09-20 ("installed launcher", "Don't push to a device…") | Not yet. Needs a phone |
| — | Activity log per app, Outside tools page (Data), smarter import (currencies, colours, text dates), live editable tables, folder trees | release notes §3, §4, §6, §8 | Partly |

**Unpublished commits.** The last production release was **Nov 9 (prod v752)**. About 220 commits have landed on `master` since. They include the multi-sheet front door, two-way sync, the Data manager role, the app Data page and live views. Before writing those pages we need to know what is live by the time the docs ship (question 1 below).

## Proposed structure: 4 tabs + a shared front

I'd use Mintlify **tabs** (`navigation.tabs` in `docs.json`), one per audience. The working names are verbs, so the reader picks a job instead of a feature.

Every tab shares a short **front** group: *What is Gainable* and *Who does what* (roles).

### Tab 1 — Build — *"Build apps from your data"*
For builders.

- **Get started**: Quickstart (drop sheets → review decisions → Build), *Build from several sheets*, Build from an idea
- **Datasets**: What a dataset is; Sources (upload, Google Sheets, Excel Online, HubSpot and other connected apps); Combining sources into one model; Keeping data in sync (schedule, approval, two-way); Import rules (currencies, colours, hidden/sensitive columns, dates)
- **Designing the app**: Review decisions; Layouts & theme; Views (tables, boards, trees, dashboards/KPIs, one-page apps); Data models
- **Changing a built app**: refine chat, undo/history, publish (reuses `prompting/*` content)
- **Prompt guide**: reworked from `prompting/*`

### Tab 2 — Own — *"Run and share an app"*
For app owners and admins.

- **Access**: Who can access (Open / Invited only / externals); Users, roles and groups; Row rules ("people only see their own…"); Sensitive columns by role; **Data manager**
- **The app's Assistant**: What it does; Customizing it (name, icon, instructions, starters); Scheduled tasks; Memory; Giving it outside tools (Connectors / Outside tools page); How it respects access
- **Share the Assistant as MCP**: turn it on, copy the link, revoke from the Share tab, what users see
- **Data operations**: the app's Data page, sync log and Undo, approvals
- **Activity log**, **Publishing & domains**

### Tab 3 — Use — *"Work in Gainable"*
For everyone else. This is the biggest gap today, because there is nothing for end users.

- **Your Apps grid**: tabs, groups, widgets; *for admins:* building a company-wide grid
- **Working in an app**: records, comments, @mentions, live tables
- **Working with the Assistant**: asking questions, record cards, documents/artifacts, mentioning it on a record, chat history, team chat
- **Notifications & messaging**: the bell, the messenger, one login for every app
- **Gainable on your phone**: add to home screen (iPhone and Android tabs) and push notifications
- **Use your app's Assistant from Claude / ChatGPT / Gemini**: the end-user side of tab 2's MCP page

### Tab 4 — Develop — *"Build with Gainable from code"*
For developers and power users.

- **Builder MCP**: Overview; Connect from Claude, ChatGPT, Codex, Gemini, Goose and others; Tool reference; Scopes & consent
- **Plugins**: Claude Code & Cowork, Codex (install, SETUP, the skill)
- **Author mode** (code mode): `code_context` / `code_push`
- **Datasets from code**: `dataset_sync` and pipelines (reuses `mcp/datasets`)
- **Bring any source: your AI tool is the connector**: Claude Code etc. read the source and `dataset_sync` it into a dataset, on a schedule
- **Connect any MCP client** (Cursor, Gemini, Goose…): generic setup
- **API reference**, **Troubleshooting**

## My own ideas

1. **A role-picker home page.** `introduction.mdx` becomes four Cards ("I want to build an app / I own an app / I use Gainable / I'm a developer"). That one page carries the whole persona model.
2. **A "Who does what" page.** One table of Account admin, Builder, App admin, Data manager, Member and External, with what each can do. All four tabs link to it. This is where ACL confusion gets solved.
3. ~~What's new page~~ Dropped (2026-11-21). The docs describe the product as it is now, with no history, no "replaces" and no "used to".
4. **Write each feature once and link it from the other tabs.** For example, the Assistant MCP *setup* lives in Own and the *usage* lives in Use, with each page linking to the other.
5. **Retire with redirects, not deletes.** `copilots/*` and `autopilot/*` → Assistant pages; `building/data-connectors` → Datasets; `concepts/agents` → Assistant overview. Every moved URL gets a `redirects` entry.
6. **Rebuild the Fin CSV** after the content lands. Almost every Copilot/Autopilot answer is wrong now, and Fin is answering customers with it today. As a stopgap, update the worst Fin rows in phase 1.
7. **Update `.claude/CLAUDE.md` naming rules first.** They still mandate "Gaia Copilot / Gaia Autopilot", so any `/docfix` session would reintroduce them.
8. **Screenshot pass per tab.** The UI changed everywhere (visual builder, Users panel, Apps grid). Use the existing `images/` conventions and demo data only.
9. **Skills reference** (`skills/chartjs`, `kanban`, `weavy`, `design`): these read as internal. I suggest folding them into the Views pages or dropping them.

## Phasing (one PR each, each passing `mint broken-links` + `mint validate`)

| Phase | PR | Content |
|---|---|---|
| 0 | `docs/naming-and-ia` | Update CLAUDE.md naming; tabs skeleton in `docs.json`; move existing pages into tabs; redirects; role-picker intro; README tree. No new content |
| 1 | `docs/assistant` | Assistant pages (tabs 2+3); remove Copilot/Autopilot pages and links (redirects only, never mentioned on a page); stopgap Fin update |
| 2 | `docs/build-datasets` | Tab 1: multi-sheet build, datasets, sync, review decisions |
| 3 | `docs/access-control` | Tab 2: access, roles/groups, row rules, Data manager, "Who does what" |
| 4 | `docs/apps-grid-and-mobile` | Tab 3: grid, notifications, messaging, mobile |
| 5 | `docs/developer` | Tab 4: Builder MCP for every client, plugins, Author mode |
| 6 | `docs/fin-refresh` | Full Fin CSV rewrite (current product only, no history) |

Phases 2 and 3 include the post-Nov 9 features (Data manager, two-way sync, Data page). They can go first or wait for the next prod release, depending on question 1.

## Decisions (2026-11-21)

1. **Document `master`**, not just the Nov 9 prod release. Phases 2 and 3 are not gated on a release.
2. **Naming rule: "Gaia builds, the Assistant works in your app."** The rule holds even though owners can rename their Assistant. Pages say "the Assistant" and note once that it may carry another name in your app (the test app calls it "Property Ops"). `.claude/CLAUDE.md` gets this rule in phase 0.
3. **"Build your own data connector" is not a Gainable feature you configure.** You build a **dataset**, and the connector is your AI tool. Claude Code (or Codex, Cursor…) reads any source (an API, a database, a file) and pushes it into a dataset through the Builder MCP's `dataset_sync` (`action:"create"` / `createIfMissing:true`, then scheduled re-syncs). In tab 4 this page is **"Bring any source: your AI tool is the connector"**. It sits next to *Datasets from code*, and tab 1's Sources page links to it.
4. **Cursor (and any other MCP client) gets a generic "Connect any MCP client" page** for now, using the URL `https://build.gainable.dev/mcp`, until a Cursor plugin exists.
5. **Test app:** Property Manager (`property-manager-1795261563469.build.gainable.dev`). The sign-in link carries a key, so it must never go into a page, a screenshot or a commit.

## Verified in the test app (2026-11-21)

- **App chrome:** the sidebar toggles between **Browse** and **Assistant**. The header has the messenger, the notification bell, **Data** (`/data-sources`) and **Settings** (pick-list collections).
- **Assistant** (`/chat`): a **Chats** list with **+ New**. The empty state reads "Start a chat with Property Ops — invite people once it's going." So *team chat* means inviting people into an Assistant chat. There are conversation starters. At the bottom are **Scheduled** (3), **Memory** ("Shared knowledge across all chats — the agent reads this") and **Connectors**.
- **Data page** (`/data-sources`) is **live**. Tabs: **Sync / Log / Settings**. Under Sync, each source shows "Rows are matched on …", a file drop, "Current data" download and **Sync** per source, plus **Sync all**. Settings → **Approval** has three options: "Ask every time", "Approve automatically when clean" and "Always approve automatically". Settings → **Schedule** applies only to connected sheets ("This app is fed by files…"). The two-way setting did not show for this file-fed app. It needs a Google Sheets or Excel Online-fed app to verify.
- **Messenger:** account-wide direct and group conversations with real colleagues. Screenshots need a demo account, or this panel must be cropped out.
- **Data manager is verified** (demo account, builder → Users → select a user → Edit User). It is a checkbox next to **Administrator**: "Can sync this app's data from its files and sheets, and approve those syncs. That covers all of the app's data, whatever access rules say. Administrators can do this already." It is a per-user permission, not a Role. (Roles and Groups are labels that "decide what people see, never what they are allowed to administer".) User badges include External, Linked, Admin and Builder.
- **Screenshots come from the demo account** ("Docs" org, demo users Sara Larson and Clark Clarksson). The account is no longer on a trial (changed 2026-11-21), so there is no trial badge in the header shots.
- Still to check: the two-way toggle, @mentioning the Assistant on a record, and phone install plus push (needs a phone).

## Product findings while documenting (2026-11-21)

- **Assigning a role from the builder chat fails.** "Create a Field Staff role and give it to Sara Larson" created the role and the rule, then reported: "I could not set Sara Larson's role to it. Her user record is managed by the platform" (update_records: "User is a platform model"). The release notes say chat assigns roles. The docs tell people to assign roles on the Users tab, which works.
- **The activity log is noisy.** It lists raw requests such as `POST /api/agent-tools/read-dataset` and "Someone using a preview link, acting as …". The release notes say background noise is left out. The docs describe the filters and don't use a screenshot.
- **Apps grid Add menu.** Clicking **Add** a second time sometimes leaves the menu closed, and new account tabs need a click into the name box before typing sticks.
- **Not verified on a device:** the Android steps (Chrome menu labels) and push notifications on a real phone. The iPhone wording comes from the app's own prompt.
- **Not verified in a client:** the ChatGPT, Cursor, Gemini and Goose setup on mcp/other-clients is written generically from the connector's requirements.
- **No way for a coding agent to call an app's own API.** A published app's `/api/*` (`boilerplate/middleware/apiAuthGate.js`) accepts either a signed-in user's `auth_token` JWT or `X-Internal-Key` = the app's `INTERNAL_API_KEY` from its `.env`. The internal key is a server-to-server secret that skips sign-in and access rules, so it must not be handed out. Account API keys (`gak_…`) only work on the builder and the Gaia connector. Suggested fix: a per-app, per-person token that is checked like `auth_token`, so access rules apply, created from the API keys page or the app. Until then the API reference points people to support.
- **The finished-build Undo button is gone.** Undoing a change means selecting Rollback in Commit history. The docs and Fin are updated to match.
