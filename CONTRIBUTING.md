# Fixing the docs with Claude Code

This is the workflow for fixing items from the **Docs Site Tracker**: outdated steps, wrong labels, missing pages, and old screenshots. You describe the problem, Claude Code does the legwork, and you make the calls.

| Tool | What it does for you |
|---|---|
| Docs Site Tracker | The list of what to fix. |
| Claude Code | Reads the docs, checks the product, edits the pages, runs the checks, opens the pull request. |
| Claude in Chrome | Lets Claude click through Gainable in your browser to confirm how things really work, and take screenshots. |
| Mintlify | The docs platform. You preview on your computer (`mint dev`), every pull request gets a preview link, and merging publishes to docs.gainable.dev. |
| GitHub | Holds the docs. Every fix is a branch and a pull request, so nothing goes live unreviewed. |

You don't need a Mintlify account. Preview links are public, and publishing happens automatically when a pull request is merged.

---

## One-time setup (about 45 minutes)

### 1. Get access

Ask Malcolm (docs owner) for:

- **GitHub**: write access to `gainable-inc/docs`.
- **Claude**: a Pro, Max, Team or Enterprise plan. Claude in Chrome needs one of these.
- **Gainable**: an account on https://build.gainable.dev with a demo app, for checking steps and taking screenshots. Never use a customer's app.
- **Docs Site Tracker**: access to the app.

### 2. Install the tools

Open **Terminal** (PowerShell) and run these one at a time:

```powershell
winget install --id Git.Git -e
winget install --id OpenJS.NodeJS.LTS -e
winget install --id GitHub.cli -e
irm https://claude.ai/install.ps1 | iex
```

Close Terminal and open it again so it picks up the new tools. Then:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
npm install -g mint
git config --global user.name "Your Name"
git config --global user.email "you@yourcompany.com"
```

The first line lets PowerShell run the `npm` and `mint` commands. Without it you get "running scripts is disabled on this system".

### 3. Get the docs onto your computer

```powershell
gh auth login
```

Choose **GitHub.com**, then **HTTPS**, then **Login with a web browser**, and follow the prompts. Then:

```powershell
mkdir C:\Projects
cd C:\Projects
gh repo clone gainable-inc/docs
```

### 4. Set up Chrome

1. Install the **Claude** extension from the Chrome Web Store: https://chromewebstore.google.com/detail/claude/fcoeoabgfenejglbffodgkkbkcdhcgfn
2. Pin it to the toolbar and sign in with the **same Claude account** you'll use in Claude Code.
3. In the same Chrome window, sign in to https://build.gainable.dev and open your demo app. Claude uses your browser's sign-in, so it sees what you see.

### 5. Connect Claude Code

```powershell
cd C:\Projects\docs
claude
```

The first time, type `/login` and sign in. Then type `/chrome` and choose **Enabled by default**.

### 6. Check it all works

Still in Claude Code, try these three things:

1. Type: `Open https://docs.gainable.dev/quickstart in Chrome and tell me the first heading.` Chrome should open a tab and Claude should answer.
2. Type `/` and check that `docfix` is in the list. Don't run it yet.
3. Open a second Terminal tab and run:

   ```powershell
   cd C:\Projects\docs
   mint dev
   ```

   Open http://localhost:3000. You should see the docs. Press `Ctrl+C` in that tab to stop it.

If all three work, you're set up.

---

## Fixing an item

One item per session:

1. **Pick an item** in the Docs Site Tracker and copy what it says, including the page link.
2. **Start Claude Code** in the docs folder:

   ```powershell
   cd C:\Projects\docs
   claude
   ```

3. **Run the fix:**

   ```
   /docfix https://docs.gainable.dev/prompting/refining says the button is "Revert", but it's "Undo this build" now
   ```

4. **Answer Claude at each checkpoint.** It stops and asks you:
   - **What kind of problem it is.** Docs error, missing docs, unclear docs, product bug, or can't verify. Correct it if you disagree. For a product bug or can't-verify, Claude writes a note for you to paste into the tracker, and that item is done for now.
   - **The plan.** Which pages change and what the new text says. Read it as a user would: is it right, and is it clear?
   - **Screenshots.** Whether one needs updating, and who captures it. See [Screenshots](#screenshots).
   - **Commit and pull request.** A summary of what changed. Say go, and Claude opens the pull request.
5. **Check the preview.** Mintlify posts a **View Preview** link on the pull request within a few minutes, and Claude gives it to you. Open the changed page: read the text, check the screenshot, click the links.
6. **Merge**, or ask for review first. See [Who reviews what](#who-reviews-what). To merge, open the pull request on GitHub, click **Merge pull request**, then **Delete branch**.
7. **Check the live site** a few minutes after merging.
8. **Update the tracker item**: set its status and paste the pull request link.
9. Type `/clear` before starting the next item, so Claude starts fresh.

Nothing reaches docs.gainable.dev until a pull request is merged. If something goes wrong on a branch, tell Claude what happened; the live site is untouched.

---

## Screenshots

Before any screenshot, set the scene:

- Use your **demo app**, never a customer's. No real names, emails, API keys or tokens on screen.
- **Light mode**, browser zoom at 100% (`Ctrl+0`), and the window wide enough that the layout isn't cramped.
- Crop to the part of the screen the text talks about. No browser tabs or address bar.

Then choose one way to capture it:

**Let Claude take it.** Ask for exactly what you want:

```
Take a new screenshot of the Chats panel for prompting/refining.mdx and replace images/chats-panel.png.
```

Claude sets up the screen in Chrome, crops it, saves a sharp PNG, and shows it to you. Check it before you approve.

**Take it yourself.** Do this when it needs arrows or boxes, or a screen Claude can't reach.

1. Press `Win+Shift+S` and drag a box around the area. Windows saves it to `Pictures\Screenshots`.
2. To annotate, click the notification to open it in Snipping Tool, draw in red, and press `Ctrl+S`.
3. Tell Claude: `Use my last screenshot as images/plan-card.png, after step 2.`

Claude checks that the image matches the text, names it, writes the description for screen readers (alt text), and places it on the page.

**To show Claude something without publishing it**, like a bug you ran into, press `Alt+V` to paste the image from your clipboard into the chat. Claude can see it, but it isn't saved as a file, so use one of the two ways above for anything that goes on the site.

When a screenshot is replaced under the same file name, every page that uses it updates. Claude checks which pages those are first.

---

## When not to just fix it

- **The product is broken.** Don't change the docs to describe the bug as normal. Paste Claude's note into the tracker so it reaches the developers. The item stays open until the product is fixed.
- **You can't tell how it's supposed to work.** Ask Rickard (product owner) before writing anything.
- **It's a sensitive area.** See the table below.

## Who reviews what

| The change | Who merges |
|---|---|
| Typo, broken link, wording, or a label you checked in the product | You, after checking the preview |
| Changed steps, or new or replaced screenshots | You, after Malcolm (docs owner) approves |
| Billing, sign-in, security, API or MCP reference, navigation, new or renamed pages | You, after Rickard (product owner) approves |
| A new capability or promise to customers | Rickard (product owner) decides |

To ask for review, add the reviewer on the pull request page on GitHub (**Reviewers**, in the right-hand column).

## What "done" means

- The behavior was checked in the product.
- Every page with the same instruction was fixed.
- `mint broken-links` and `mint validate` pass.
- The preview looks right.
- It's merged, and it's live on docs.gainable.dev.
- The tracker item is updated with the pull request link.

---

## Working with Claude Code

- **Claude asks before it runs a command or edits a file.** Read what it's asking. **Yes, and don't ask again** is fine for `mint`, `git status` and `git diff`. Always read the prompt before `git push` and `gh pr create`.
- **Press `Esc` to stop Claude** mid-task, then tell it what you want instead. Press `Esc` twice to go back to an earlier message.
- **Press `Shift+Tab` to switch modes.** In **plan mode** Claude investigates and proposes but changes nothing. That's useful for big items.
- **Ask plain questions:** "Show me exactly what changed." "Why did you change that?" "Undo that last edit."
- **Rules live in `.claude/CLAUDE.md`.** Claude reads it at the start of every session: how screenshots are named, which components to use, which terms are current. If you catch Claude repeating a mistake, add a line there, in its own pull request.

## Troubleshooting

| Problem | Fix |
|---|---|
| "running scripts is disabled on this system" | Run `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` and try again. |
| `git`, `gh`, `mint` or `claude` is "not recognized" | Close Terminal and open it again. If that doesn't help, rerun its install line. |
| Claude can't reach Chrome | Make sure Chrome is open and the extension is signed in to the same Claude account as Claude Code. Then run `/chrome` and reconnect. |
| Claude sees a Gainable sign-in page | Sign in to build.gainable.dev in that Chrome window, then ask Claude to try again. |
| `mint dev` says port 3000 is in use | Another `mint dev` is still running. Close that Terminal tab, or use the port it offers. |
| No preview link after 10 minutes | Check the **Mintlify Deployment** item under **Checks** on the pull request, and tell Malcolm. |
